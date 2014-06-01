---
layout: post
title: "Reclaiming lost space on ext4 partitions"
description: ""
category: Linux
tags: [linux, fix, ext4, mint, ubuntu]
---
{% include JB/setup %}

After four years of using Ubuntu 10.10 (Maverick), I've finally decided to bite the bullet and upgrade. But then I've noticed that the system is reporting 77GB of "Free space" and only 9GB of "Available space". Turns out I had more _free space_ than I thought...

<a name="excerpt-continue"></a>

Maverick was the last release with usable (i.e. Gnome2) interface, so I was kind of stuck with it for a long time. Today I decided to install [Mint 17 "Qiana"][qiana], as Cinnamon now seems mature enough for daily use. While restoring the balance of natural forces on the machine (i.e. reinstalling and configuring everything) I noticed that the System Monitor is reporting the above numbers.

After ten minutes of reading various google search results, I discovered that ext4 filesystem will reserve a certain amount of free space for the root user _on each partition_. Apparently, if your disk gets filled up, you cannot log in anymore. Therefore, a portion of the disk is reserved for root, so he can still log in and fix the issue even if the regular used can't do a thing.

Problem is, ext4 reserves 5% of each partition by default. This was fine years ago when this feature was introduced, but on a more recent 1.5TB partition, that's some 50+GB of free space wasted. _Especially_ since this reserved space is not needed on non-system partitions, i.e. my `/home` partition.

Luckily, the fix is easy, simply use `tune2fs` to reduce the percentage of reserved space:

```bash
sudo tune2fs -m 1 /dev/sda7
```

In this example, the reserved space is set to 1% on `sda7`.

Two things to note here:

1. Yes, you can set it to zero on a non-system partition, but I'd advise against it. ext4 performance can drop dramatically if it gets near 100% utilisation since the reserved space is also used to reduce disk fragmentation.
2. You need to give `tune2fs` a block device, not a mount point for this to work. In other words, you can't use `sudo tune2fs -m 1 /home`.

It's probably time for ext4 maintainers to refine this percentage for large disks.

[qiana]: http://blog.linuxmint.com/?p=2626
