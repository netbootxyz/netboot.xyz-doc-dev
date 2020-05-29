---
title: FAQ
description: Frequently Asked Questions about the netboot.xyz project
weight: 20
date: '2018-04-14T16:40:17.000Z'
---

# FAQ

## What is this?

netboot.xyz is tool that allows you to boot your favorite Operating System's installer or various utilities over the network with minimal overhead and all from a single menu system. It's similar to various tools netbooting tools of the past like boot.kernel.org with a lot more flexibility. The boot loader is very light weight being under 1MB in size which translates into a very quick time to create a USB key.

## How does this work?

netboot.xyz uses an open source tool called iPXE. The bootloader used calls to a webserver that hosts that the iPXE source files. The iPXE source files contain menus and logic that understand how the various Linux installers operate. When you select an Operating System, netboot.xyz retrieves the images from the project directory when possible or known and trusted performant mirrors. The location the file is pulled from is always displayed during retrieval.

## What is PXE Booting?

PXE stands for **P**re-Boot e**X**ecution **E**nvironment. PXE booting has been used for years to allow for clients to boot from a server over the network. It gives you an oportunity to automate a system inside the BIOS before it boots off its hard drive which opens up the door for running stateless machines without having to use storage in the system. PXE booting is used in many applications but it's most common use is automating the installation of bare metal or a virtual machine.

## Will my favorite distribution work with netboot.xyz?

Usually you need three things to boot up an OS, the vmlinuz, the initramfs, and the rootfs. Distributions that support an installer kernel hosted on a mirror are typically the easier ones to implement. Distributions that only release as ISOs typically are harder to implement as we have to use memdisk to load it up into memory.

From [syslinux - memdisk](http://www.syslinux.org/wiki/index.php/MEMDISK): The majority of Linux based CD images will also fail to work with MEMDISK ISO emulation. Linux distributions require kernel and initrd files to be specified, as soon as these files are loaded the protected mode kernel driver\(s\) take control and the virtual CD will no longer be accessible. If any other files are required from the CD/DVD they will be missing, resulting in boot error\(s\). Linux distributions that only require kernel and initrd files function fully via ISO emulation, as no other data needs accessing from the virtual CD/DVD drive once they have been loaded. The boot loader has read all necessary files to memory by using INT 13h, before booting the kernel.

You can read more about my thoughts and possible solutions [here](https://www.reversengineered.com/2016/01/07/booting-linux-isos-with-memdisk-and-ipxe/).

## My distribution uses ISOs for delivery, how can I see if they work?

You can do a quick check by loading up netboot.xyz in a virtual environment baremetal. Make sure you have plenty of RAM as you are loading the ISO into RAM. Then select the iPXE command line and enter the following;

```text
kernel https://boot.netboot.xyz/memdisk iso raw
initrd http://url/to/iso
boot
```

That should load the ISO and if you make it all the way into the installer, great, your OS may work. If it fails during initramfs load trying to load the CD device, then it has the issue of not being able to find the ISO in memory.

## Can I create my own configurations?

Yes! You can fork [netboot.xyz-custom](https://github.com/netbootxyz/netboot.xyz-custom) and create your own menu. You can then set your Github user from the Utility menu and your menu will show up in the main menu. If you don't want to set your user every time, you can custom compile the netboot.xyz iPXE code and include your github\_user during the compile. This allows you to create your own menu without the maintenance of everything else.

## What Operating Systems are currently available on netboot.xyz?

### Operating Systems

| Name | URL | Installer Kernel | Live OS |
| :--- | :--- | :--- | :--- |
| Alpine Linux | [https://alpinelinux.org](https://alpinelinux.org) | Yes | No |
| Arch Linux | [https://www.archlinux.org](https://www.archlinux.org) | Yes | No |
| Backbox | [https://www.backbox.org](https://www.backbox.org) | No | Yes |
| BlackArch Linux | [https://blackarch.org](https://blackarch.org) | ISO - Memdisk | No |
| CentOS | [https://centos.org](https://centos.org) | Yes | No |
| Debian | [https://debian.org](https://debian.org) | Yes | Yes |
| Deepin | [https://www.deepin.org](https://www.deepin.org) | No | Yes |
| Devuan | [https://devuan.org](https://devuan.org) | Yes | No |
| Elementary OS | [https://elementary.io](https://elementary.io) | No | Yes |
| Fedora | [https://fedoraproject.org](https://fedoraproject.org) | Yes | Yes |
| Feren OS | [https://ferenos.weebly.com/](https://ferenos.weebly.com/) | Yes | No |
| Flatcar Linux | [https://www.flatcar-linux.org](https://www.flatcar-linux.org) | Yes | No |
| FreeBSD | [https://freebsd.org](https://freebsd.org) | Yes, disk image | No |
| FreeDOS | [http://www.freedos.org](http://www.freedos.org) | ISO - Memdisk | No |
| Gentoo | [https://gentoo.org](https://gentoo.org) | Not Working | No |
| IPFire | [https://www.ipfire.org](https://www.ipfire.org) | Yes | No |
| Kali Linux | [https://www.kali.org](https://www.kali.org) | Yes | Yes |
| KDE Neon | [https://neon.kde.org](https://neon.kde.org) | No | Yes |
| Linux Lite | [https://www.linuxliteos.com](https://www.linuxliteos.com) | No | Yes |
| Mageia | [https://www.mageia.org](https://www.mageia.org) | Yes | No |
| Manjaro | [https://manjaro.org](https://manjaro.org) | No | Yes |
| Mint | [https://linuxmint.com](https://linuxmint.com) | No | Yes |
| Microsoft Windows | [https://www.microsoft.com](https://www.microsoft.com) | User supplied media | No |
| MirOS | [https://www.mirbsd.org](https://www.mirbsd.org) | Yes | No |
| NixOS | [https://nixos.org](https://nixos.org) | Yes | No |
| OpenBSD | [https://openbsd.org](https://openbsd.org) | Yes | No |
| OpenSUSE | [https://opensuse.org](https://opensuse.org) | Yes | No |
| Parrot Security | [https://www.parrotsec.org](https://www.parrotsec.org) | ISO | No |
| Peppermint | [https://peppermintos.com](https://peppermintos.com) | No | Yes |
| Pop OS | [https://system76.com/pop](https://system76.com/pop) | No | Yes |
| Q4OS | [https://q4os.org](https://q4os.org) | No | Yes |
| RancherOS | [https://rancher.com/rancher-os](https://rancher.com/rancher-os) | Yes | No |
| Red Hat Enterprise Linux | [https://www.redhat.com](https://www.redhat.com) | User supplied media | No |
| Regolith | [https://regolith-linux.org](https://regolith-linux.org) | No | Yes |
| Scientific Linux | [https://scientificlinux.org](https://scientificlinux.org) | Yes | No |
| Slackware | [https://www.slackware.com](https://www.slackware.com) | Yes | No |
| Tails | [https://tails.boum.org](https://tails.boum.org) | No | Yes |
| Tiny Core Linux | [https://tinycorelinux.net](https://tinycorelinux.net) | ISO - Memdisk | No |
| Ubuntu | [https://www.ubuntu.com](https://www.ubuntu.com) | Yes | Yes |
| Voyager | [https://voyagerlive.org](https://voyagerlive.org) | No | Yes |
| Zoris OS | [https://zorinos.com](https://zorinos.com) | No | Yes |

## Utilities

| Name | URL | Installer Kernel | Live OS |
| :--- | :--- | :--- | :--- |
| ALT Linux Rescue | [https://en.altlinux.org/Rescue](https://en.altlinux.org/Rescue) | ISO - Memdisk | - |
| Breakin | [http://www.advancedclustering.com/products/software/breakin/](http://www.advancedclustering.com/products/software/breakin/) | Yes | Yes |
| Clonezilla | [http://www.clonezilla.org/](http://www.clonezilla.org/) | - | Yes |
| DBAN | [http://www.dban.org/](http://www.dban.org/) | Yes | Yes |
| GParted | [http://gparted.org](http://gparted.org) | ISO - Memdisk | - |
| Grml | [http://grml.org](http://grml.org) | ISO - Memdisk | - |
| Memtest | [http://www.memtest.org/](http://www.memtest.org/) | Yes | - |
| Super Grub2 Disk | [http://www.supergrubdisk.org](http://www.supergrubdisk.org) | ISO - Memdisk | - |
| Ultimate Boot CD | [http://www.ultimatebootcd.com](http://www.ultimatebootcd.com) | ISO - Memdisk | - |

## What are some good resources for learning more about network booting?

* [The iPXE Project](http://ipxe.org/)
* [NetworkBoot.org](http://networkboot.org/)
* [Syslinux Project](http://www.syslinux.org/wiki/index.php?title=The_Syslinux_Project)

