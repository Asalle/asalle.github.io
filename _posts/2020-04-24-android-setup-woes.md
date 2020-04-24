---
layout: post
title:  "Android Studio does not see my phone, but adb does!"
date:   2020-04-24 09:06:20 +0100
categories: programming
tags: android, sdk, linux
---

So I wanted to fix some things in an open source android project.  
I looked it up briefly how to work with android sdk on emacs and decided that it's going to be
easier to start with Android Studio, so I  
- installed android [studio](https://developer.android.com/studio/) to /mnt/opt (my `/` filesystem is almost full :sweat_smile:)
- installed sdk
- installed adb  (`sudo dnf install android-tools`)
- created `plugdev` group (`sudo groupadd plugdev`)
- added myself to `plugdev` group (`sudo usermod -aG plugdev $LOGNAME`)

Connected my phone to my computer, turned on USB debugging and all and there was... nothing.  

Then I did `lsusb` to see if my phone is even reconizable via usb: no.  
Changed usb cable, ran lsusb again -> yes!  
Ran `adb devices` - my device was not shown.
I have Fedora, so the udev rules are not originally where they should be so I liked them:  
- copied udev rules: `sudo cp /usr/share/doc/android-tools/51-android.rules /etc/udev/rules.d`
- restarted udev: `sudo udevadm control --reload-rules`
- found phone on `adb devices` -> yeah!, but android studio still does not see it :thinking_face: 
- started android studio with sudo - still does not work
- works only if I start Android studio with root
- works if I do this: `export ANDROID_HOME=<the Android/Sdk folder>` in `~/.profile` and `source ~/.profile` - many thanks to [Yu Shen](https://stackoverflow.com/a/51432157/3528539).
It seems that my "non-standard" path to sdk/android studio (`/mnt/opt`) was in the way.
But in the end it worked out 🐱.
