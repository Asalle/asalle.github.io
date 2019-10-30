---
layout: post
title:  "Installing Windows after Linux"
date:   2019-10-30 19:37:20 +0100
categories: system-administration
tags: windows, linux, fedora, dual-boot
---

# Steps in short

1. Unmount the partition you want to cut.
2. Cut a part of exising ext4 partition in gparted and make it "Unallocated".
3. Install windows in the "unallocated" space. This will create 2 new partitions: boot partition for windows and the actual windows partition.
4. Reboot into linux. Linux will not notice the newly installed windows just yet.
5. Update grub - `sudo grub2-mkconfig -o /boot/grub2/grub.cfg `
6. Reboot and you will see windows in grub menu.


# Story behind

Until this very day I thought that installing windows after linux will make my life a living hell for a good couple of days. Never have I been so wrong. The cutting of my HDD went without any errors, which was probably the most risky part. 

The moral of the story is: installing windows after linux is possible and even easy. Try it for yourself before you say it's not :)
