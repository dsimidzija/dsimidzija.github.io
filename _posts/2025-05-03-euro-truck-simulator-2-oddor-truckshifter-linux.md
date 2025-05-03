---
layout: post
title: "ETS2, ODDOR Truckshift, and USB drivers on linux"
description: "How I learned to stop worrying and love the USB."
category: Gaming
tags: [gaming, linux, truckshift, oddor, programming, rust]
---

## Intro

In my [previous post about Euro Truck Simulator 2 and headtracking][ets2-headtrack] we got a working setup for
headtracking and relatively comfortable driving. One thing that kept bugging me was the fact that the G29 shifter just
wasn't meant for trucking, as using the wheel buttons for range selection and gear splitting is just cumbersome and
unintuitive. So I decided to buy a truck shifter from Amazon, specifically [this one][amazon-oddor-truckshift] that can
be mounted on top of the G29 shifter base.

Unfortunately, it doesn't work on Linux out of the box. So in order to try to save some money and dignity, I decided to
try to make it work.

Given that you're reading this, it worked:

![ETS2 devices](/assets/img/euro-truck-simulator-2-oddor-truckshifter-linux/oddor-ets2-devices.png)
![ETS2 shifter toggles](/assets/img/euro-truck-simulator-2-oddor-truckshifter-linux/oddor-ets2-shifter-toggles.png)

This specific shifter is branded as "colortree", but the hardware underneath is made by Labtec, so there is a chance
that other brands might work if they use the same hardware.

[![GitHub](https://badgen.net/badge/icon/github?icon=github&label)][main-repo]
[![GitHub](https://badgen.net/github/license/dsimidzija/rust-oddor-truckshift)][main-repo]
[![GitHub](https://badgen.net/github/issues/dsimidzija/rust-oddor-truckshift)][main-repo]
{: style="text-align: center" }

## USB Communication

Since I had no idea where to begin, I started investigating how USB devices work, and found [this wonderful
website][usb-descriptor] which describes how USB devices establish communication with your PC. Long story short, a USB
device has "descriptors" for the interface it provides, through which the communication is defined: which
configuration/interface/endpoint to use, and how. The endpoint zero is the control endpoint, which determines how is
your machine going to communicate with the device, while the rest can be customised.

Without going into too many details, it boils down to the following (using `libusb`):

1. Enumerate all the devices, and look for a device with a specific vendor ID and product ID you're interested in. In
   our case, it is `1020:8863`, which you can check via `lsusb` with your device plugged in.

      ```bash
      $ lsusb
      ...
      Bus 001 Device 019: ID 1020:8863 Labtec ODDOR-TRUCKSHIFT
      ```

2. Open the device, and look for the first readable endpoint. Note that this works for ODDOR truckshift because it has
   only one non-control endpoint, if it were a more complex device, this step would probably be more complex as well. As
   a side note, you can get the descriptor info via `lsusb` too:

      ```bash
      $ lsusb -vv -d 1020:8863
      Bus 001 Device 019: ID 1020:8863 Labtec ODDOR-TRUCKSHIFT
      Device Descriptor:
        bLength                18
        bDescriptorType         1
        bcdUSB               2.00
        bDeviceClass          255 Vendor Specific Class
        bDeviceSubClass       255 Vendor Specific Subclass
        bDeviceProtocol       255 Vendor Specific Protocol
        bMaxPacketSize0        64
        idVendor           0x1020 Labtec
        idProduct          0x8863 ODDOR-TRUCKSHIFT
        bcdDevice            1.14
        iManufacturer           1 ZSC
        iProduct                2 ODDOR-TRUCKSHIFT
        iSerial                 3 
        bNumConfigurations      1
        Configuration Descriptor:
          bLength                 9
          bDescriptorType         2
            <...snip...>
            Item(Global): Logical Maximum, data= [ 0xff 0x7f ] 32767
            Item(Global): Report Size, data= [ 0x10 ] 16
            Item(Global): Report Count, data= [ 0x02 ] 2
            Item(Main  ): Input, data= [ 0x02 ] 2
                            Data Variable Absolute No_Wrap Linear
                            Preferred_State No_Null_Position Non_Volatile Bitfield
            Item(Main  ): End Collection, data=none
            Item(Main  ): End Collection, data=none
        Endpoint Descriptor:
          bLength                 7
          bDescriptorType         5
          bEndpointAddress     0x81  EP 1 IN
          bmAttributes            3
            Transfer Type            Interrupt
            Synch Type               None
            Usage Type               Data
          wMaxPacketSize     0x0040  1x 64 bytes
          bInterval              10   wTotalLength       0x0029
      ```

3. Read raw bytes from the endpoint and decode them. This was the part that scared me the most, since I had no idea how
   the button presses are going to be represented, but it turned out quite simple: the shifter has only three buttons,
   and their states were being stored in the first three bits of the first byte (even though the device returns 5 bytes
   for some reason).

The initial prototype for all of this was quickly hacked in Python, just to confirm that it is actually doable, but the
additional challenge was to make the actual driver in Rust (as a challenge for my &lt;dadjoke&gt;rusty
brain&lt;/dadjoke&gt;). The Python prototype is not going to be published, because I'd like to save some dignity, since
this is my first Rust project ever, I'm probably going to need all the dignity I can muster if anyone fluent in Rust
opens it.

## System preparation

> Note: Everything written here is from [Arch][arch] perspective, adapt to other distributions as necessary.
> _(I use Arch btw.)_

In order to read the USB device, you will need to set up some udev rules, otherwise you'll get access denied errors:

`/etc/udev/rules.d/99-oddor-truckshift.rules`:

```
SUBSYSTEM=="usb", ATTRS{idVendor}=="1020", ATTRS{idProduct}=="8863", GROUP="users", MODE="0660"
KERNEL=="event*", SUBSYSTEM=="input", ATTRS{id/vendor}=="1020", ATTRS{id/product}=="8863", GROUP="users", MODE="0660" SYMLINK+="oddor_truckshift" RUN+="/bin/chmod 0660 /dev/oddor_truckshift"
```

The two rules grant the `users` group read access to the USB device, _and_ to the virtual `libevdev` device we'll be
creating. If your machine doesn't have the `users` group, change the rules accordingly.

## Implementation details

The driver works in two different modes: with and without [hotplug][libusb-hotplug]. Virtually all modern systems should
support hotplug, but unfortunately I had to take into account the option of not having it.

If hotplug is not available, and the truckshift is not detected upon startup, we abort the execution immediately. The
alternative is to keep polling the USB for it, which I decided against - there is no point in doing this for a device
which is going to be used intermittently for the vast majority of cases, and we're already in an obscure situation where
hotplug is not available.

After `libusb` notifies us that a new device has been plugged in, we enumerate all the available devices and try to find
the shifter. If it was found, we open it, locate the appropriate endpoint, and start listening for events. That was the
_relatively_ easy part, since it required only figuring out how to read a USB device and decode the output.

The tricky part was figuring out how to actually make a virtual input device visible in ETS2, which is where `libevdev`
and `libinput` come in. We create a virtual device with the same vendor/product ID as our USB device, and assign it
properties of a "buttonbox" with three buttons.

On each change of buttons/switches, we cache a persistent state of all the switches, and if it has changed, we forward
these events to our new virtual device. Keeping the state is probably optional, but in theory it reduces the number of
unnecessary events being sent.

There were quite a few gotchas during this whole process. One was discovering that by default you don't have the correct
permissions to read from a USB device, or even a `libinput`/`evdev` device, as noted above. Second one was discovering that
you need to wait a bit to let `udev` to set the correct permissions, otherwise you'll get silent access denied errors
even though your permissions seem correct when you check them.

```rust
// this seems to be necessary to allow udev enough time to set the correct permissions,
// otherwise adding the device to libinput fails silently
thread::sleep(Duration::from_millis(100));
```

Another one is that reading from USB and forwarding this to an `evdev` device will work, but your device won't be
visible in the game. To expose it to your game via `libinput`, you have to implement `LibinputInterface`, and in order
to that, you have to pull in `libc` as a dependency, just for some `fopen` flags? Long story short, it's a bit of a
mess.

```rust
struct UdevInterface;

impl LibinputInterface for UdevInterface {
    fn open_restricted(&mut self, path: &Path, flags: i32) -> Result<OwnedFd, i32> {
        OpenOptions::new()
            .custom_flags(flags)
            .read((flags & O_RDONLY != 0) | (flags & O_RDWR != 0))
            .write((flags & O_WRONLY != 0) | (flags & O_RDWR != 0))
            .open(path)
            .map(|file| file.into())
            .map_err(|err| err.raw_os_error().unwrap())
    }
    fn close_restricted(&mut self, fd: OwnedFd) {
        let _ = File::from(fd);
    }
}
```

Since I am a complete Rust noob, the most difficult part to get my head around was of course the concept of ownership
and things relating to it. Luckily the docs are pretty well written, so they can sometimes be helpful even if the error
message is completely bonkers. The thing that didn't help was trying to do multithreading on my first Rust experiment,
but luckily there is `crossbeam`, which offers a very clean API for [scoped
threads](https://docs.rs/crossbeam/latest/crossbeam/thread/index.html) and cross-thread messaging.

In the end, I was pretty lucky that all the necessary ~~libraries~~ _crates_ already exist in the Rust ecosystem,
otherwise I'd probably still be looking to get off the Mr. Bones' Wild Ride.

## Links

* <https://github.com/dsimidzija/rust-oddor-truckshift>
* <https://www.amazon.de/-/en/gp/product/B09C4YKB2B>
* <https://www.beyondlogic.org/usbnutshell/usb5.shtml>
* <https://docs.rs/evdev/latest/evdev/index.html>
* <https://docs.rs/rusb/latest/rusb/index.html>
* <https://docs.rs/input/latest/input/index.html>
* <https://docs.rs/signal-hook/latest/signal_hook/index.html>
* <https://docs.rs/crossbeam/latest/crossbeam/index.html>
* <https://docs.rs/crossbeam-channel/latest/crossbeam_channel/index.html>
* <https://github.com/emberian/evdev/tree/main/examples>
* <https://github.com/a1ien/rusb/tree/master/examples>
* <https://rust-cli.github.io/book/in-depth/signals.html>

*[ETS2]: Euro Truck Simulator 2

[main-repo]: https://github.com/dsimidzija/rust-oddor-truckshift
[ets2-headtrack]: /posts/euro-truck-simulator-2-headtracking-linux/
[amazon-oddor-truckshift]: https://www.amazon.de/-/en/gp/product/B09C4YKB2B
[usb-descriptor]: https://www.beyondlogic.org/usbnutshell/usb5.shtml
[arch]: https://archlinux.org/
[libusb-hotplug]: https://libusb.sourceforge.io/api-1.0/libusb_hotplug.html
