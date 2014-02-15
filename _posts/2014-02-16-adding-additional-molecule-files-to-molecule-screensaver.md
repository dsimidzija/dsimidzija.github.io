---
layout: post
title: "Adding additional molecule files to molecule screensaver"
description: ""
category: Linux
tags: [linux, ubuntu, gnome, howto]
---
{% include JB/setup %}

I wanted to add additional molecule definitions to the excellent
[molecule screensaver][molecule]. But in the infinite wisdom of Gnome developers,
it is not possible to configure your screensaver through a simple GUI. Because
that would be too convenient.

<a name="excerpt-continue"></a>

Of course, you can always use xscreensaver daemon which allows you to configure
your screensavers, but xscreensaver daemon breaks screen locking on activation
(i.e. it doesn't work at all) and that's a deal-breaker for me.

Luckily, there is a workaround, although it took a while to find it. First, you'll
need to get some [PDB files][get-pdb-files] and extract them to a folder accesible
to all users on the system. On my system it's `/home/shared/system/molecule`, but
obviously you can use any folder.

Now we're going to "create" a screensaver by creating a new `.desktop` file in
`/usr/share/applications/screensavers`. This folder contains the actual commands
which are executed when the screensaver is to be activated. It's worth mentioning
that I'm on **Ubuntu 10.10**, this folder may have moved in the latest versions.

In any case, copy the existing molecule file and open the copy in
[the editor][vim]:

```bash
$ cd /usr/share/applications/screensavers
$ sudo cp molecule.desktop molecule2.desktop
$ sudo vi molecule2.desktop
```

Change the `Name` and `Exec` line and give molecule the path to your PDB files:

```diff
$ diff molecule.desktop molecule2.desktop 
3,4c3,4
< Name=Molecule
< Exec=/usr/lib/xscreensaver/molecule -root
---
> Name=Molecule 2
> Exec=/usr/lib/xscreensaver/molecule -root -molecule /home/shared/system/molecule
```

The screensaver will now appear as "Molecule 2" in the gnome screensaver
preferences dialog. You can also check `man molecule` for more things you can
customise.

[molecule]: http://harald.ist.org/self-pc/tricks/linux/howto/molecule-screensaver.html
[get-pdb-files]: http://harald.ist.org/self-pc/tricks/linux/howto/molecule-screensaver.html#get-pdb-files
[vim]: http://www.vim.org/
