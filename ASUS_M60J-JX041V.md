---
title: ASUS M60J-JX041V
permalink: /ASUS_M60J-JX041V/
---

This page is a work in progress.

Overview
========

comming ...

Final Laptop Configuration
==========================

not yet.

Problems & Solutions
====================

No Ethernet
-----------

    [root@nixos:]# lspci -v | sed -n '/Ethernet/ { :loop p; n; s/^$//; T loop; q}'
    07:00.0 Ethernet controller: Attansic Technology Corp. Device 1063 (rev c0)
            ...
            Kernel driver in use: atl1c
            Kernel modules: atl1c

While searching on the web you find that this ethernet device need the kernel module `atl1e` instead of `atl1c`.

    [root@nixos:]# rmmod atl1c
    [root@nixos:]# modprobe alt1e

Grub entries & BIOS boot order
------------------------------

Be aware that changing the BBS (?) order of hard drive is changing the apparent mapping of partitions. Thus `hd0` and `hd1` are inverted. I assume that this is also true for devices like /dev/sda and /dev/sdb.