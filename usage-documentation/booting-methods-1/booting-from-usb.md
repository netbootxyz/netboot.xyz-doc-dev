---
title: Booting from USB
description: How to create a USB stick capable of booting into netboot.xyz
date: '2018-04-14T16:32:14.000Z'
weight: 20
---

# Booting from USB

_Warning: Backup your important data before using USB as it will overwrite anything on the USB key._

Download a netboot.xyz USB disk:

* [netboot.xyz](https://boot.netboot.xyz/ipxe/netboot.xyz.usb)

## Creating USB Key on Linux

Insert a USB key in your computer and find the device name. Then use following command:

```text
cat ipxe.usb > /dev/sdX
```

or

```text
dd if=ipxe.usb of=/dev/sdX
```

where sdX is your usb drive.

The USB key should be ready to eject once finished.

### Creating USB Key on MacOS

**Run:**

```text
diskutil list
```

to get the current list of devices

_**Insert the flash media.**_

**Run:**

```text
diskutil list
```

again and determine the device node assigned to your flash media \(e.g. /dev/disk2\).

**Run:**

```text
diskutil unmountDisk /dev/diskN
```

\(replace N with the disk number from the last command; in the previous example, N would be 2\).

**Execute:**

```text
sudo dd if=netboot.xyz.usb of=/dev/rdiskN bs=1m
```

* Using /dev/rdisk instead of /dev/disk may be faster
* If you see the error dd: Invalid number '1m', you are using GNU dd. Use the same command but replace bs=1m with bs=1M
* If you see the error dd: /dev/diskN: Resource busy, make sure the disk is not in use. Start the 'Disk Utility.app' and unmount \(don't eject\) the drive

**Run:**

```text
diskutil eject /dev/diskN
```

and remove your flash media when the command completes.

### Creating USB Key on Windows

Check out [Rufus](https://rufus.akeo.ie/) to install the ISO file to a USB key.

### Booting

Once you've created your key, reboot and set your BIOS to load the USB key first if it's not set for that already. You should see iPXE load up either load up netboot.xyz automatically or prompt you to set your networking information up.

