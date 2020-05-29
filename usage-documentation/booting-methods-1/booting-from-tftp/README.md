---
title: Booting from TFTP
description: Methods of booting into netboot.xyz using TFTP and DHCP
date: '2018-04-14T16:32:10.000Z'
weight: 15
---

# Booting from TFTP

If you want to utilize netboot.xyz from your home or office network, it's relatively easy to set up. It will allow all of your devices on your network to have netboot.xyz available whenever you need it by just changing the boot order on your device, selecting network boot, or manually selecting the device to boot.

## DHCP Server Setup

You will have to tell your DHCP server to provide a "next-server", the address of a TFTP server on your network, and a "filename", the [netboot.xyz boot file](https://boot.netboot.xyz/ipxe/netboot.xyz.kpxe). When your clients boot up, if they are set to network boot, they'll automatically get a valid DHCP address, pull down the netboot.xyz iPXE bootloader and load up the Operating System menu.

Example:

```text
next-server "1.2.3.4"
filename "netboot.xyz.kpxe"
```

If you are using [dnsmasq as your DHCP server](https://wiki.archlinux.org/index.php/dnsmasq#DHCP_server) as well as your TFTP server then setting the next-server option is as simple as adding the following line to `/etc/dnsmasq.conf`:

```text
dhcp-option=66,0.0.0.0
```

`0.0.0.0` is parsed as the address of the machine running dnsmasq.

## TFTP Server Setup

You will need to set up a tftp server to host the iPXE files. There are various types of TFTP servers out there and they all usually work pretty well. You can also use dnsmasq to host the files as well.

If you use dnsmasq you can add this configuration to `/etc/dnsmasq.conf`:

```text
enable-tftp
tftp-root=/var/lib/tftp
dhcp-boot=netboot.xyz.kpxe
```

## Fixing the dnsmasq service

If you are running systemd and you can start dnsmasq fine manually but it fails to start at boot you may need to edit the \[Unit\] section of `/lib/systemd/system/dnsmasq.service` by changing:

```text
After=network.target
```

to

```text
After=network-online.target 
```

## Regular and Undionly Boot Files

If you experiencing issues with the regular [netboot.xyz.kpxe](https://boot.netboot.xyz/ipxe/netboot.xyz.kpxe) bootloader, you can try and use the [netboot.xyz-undionly.kpxe](https://boot.netboot.xyz/ipxe/netboot.xyz-undionly.kpxe) bootloader. The regular bootloader includes common NIC drivers in the iPXE image, while the undionly loader will piggyback off the NIC boot firmware.

