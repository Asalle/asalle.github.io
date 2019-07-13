---
layout: post
title: fedora and msi GS43VR 
date:   2019-07-13 21:14:20 +0100
categories: programming
tags: fedora, msi, compatibility
---

I've been using [fedora](https://getfedora.org/) for half a year now and it's great. The most amazing difference between fedora and debian-based distros I've been using so far are:  
- a concise and quick installation process  
- newest versions of software already available for download with `dnf`  
- git, make and etc already pre-installed  
  
My other laptop, dell xps 13 (2018) works with it perfectly, including the graphics/sound card, so I decided to try it out with my msi gs43vr (2017).  

Hardware: [msi gs43vr](https://www.amazon.de/MSI-7RE-064DE-Phantom-Gaming-Laptop-i7-7700HQ/dp/B01MT5BOFS)  
Software: [Fedora 30-1.2](https://getfedora.org/en/workstation/download/)  

# Aaand here goes the list of issues I've encountered.  
1. Graphical install hangs on `Started GNOME Display Manager.`.  
You will need to start the fedora installer in basic graphics mode with `Troubleshooting` -> `Start fedora in basic graphics mode`.  
2. After installation, `gnome-initial-setup` pop ups every time after reboot.  
Just `touch ~/.config/gnome-initial-setup-done`. This will apply to current user only.  
3. When rebooted or shut down and then turned on, fedora froze with blank screen and only a blinking cursor for 2mins.  
At first I thouthg it's graphics driver's problem. Installation of a proper NVIDIA driver didn't work though (I followed [this](https://www.if-not-true-then-false.com/2015/fedora-nvidia-guide/) guide).  
What worked for me is removing `rhbs quiet` from `/etc/default/grub`. Don't forget to run `grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg` to update grub.  
4. Super button not working  
Install `gnome-tweaks` and go to `Keyboard & Mouse` -> `Overview Shortcut` and toggle it.

# Lessons Learned
- [rpmfusion](https://rpmfusion.org/Howto/NVIDIA#Determining_your_card_model) drivers completely smashed my setup.

# Persisting issues I'd like to have some help with:  
1. ~~Same blinking cursor + blank screen on shutdown.~~  
2. ~~Screen brightness regulation buttons (f4&f5) do not work, although volume regulation, keyboard lights and sleep buttons work.~~  
3. ~~Suspend does not work(stuck forever on saving pages), however hibernate does.~~  
All of the above stopped when I installed the NVIDIA drivers according to this [guide](https://medium.com/@gokdeniz91/how-to-install-nvidia-drivers-on-fedora-29-8dd0485aba68).

Still worth it!
