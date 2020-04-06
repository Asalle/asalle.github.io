---
layout: post
title:  "Android setup woes"
date:   2019-02-28 21:06:20 +0100
categories: programming
tags: android, sdk, linux
---

So I wanted to fix some things in an open source android project.  
I looked it up briefly how to work with android sdk on emacs and decided that it's going to be
easier to start with Android Studio, so I  
- installed android [studio](https://developer.android.com/studio/) to /mnt/opt (my `/` filesystem is almost full :sweat_smile:)
- installed sdk
- installed adb  (`sudo dnf install android-tools`)
- created `plugdev` group (``)
- added myself to `plugdev` group: ``

Connected my phone to my computer, turned on USB debugging and all and there was... nothing.  

Then I did `lsusb` to see if my phone is even reconizable via usb: no.  
Chaned usb cable, ran lsusb again -> yes!  
Ran `adb devices` - not shown.
I have Fedora, so the udev rules are not originally where they should be so I liked them:  
- copied udev rules: `sudo cp /usr/share/doc/android-tools/51-android.rules /etc/udev/rules.d`
- restarted udev: `sudo udevadm control --reload-rules`
- found phone on `adb devices` -> yeah!, but android studio still does not see it :thinking_face: 
# - started android studio with sudo - still does not work
# - started a new blank project - works!
# - I tought it was root who made it work - opened orgzly with root android studio - does not works
# - installed Android api 28 - works on root
- works only if I start Android studio with root
`export ANDROID_HOME=<the Android/Sdk folder>` in `~/.profile` and `source ~/.profile` - many thanks to [Yu Shen](https://stackoverflow.com/a/51432157/3528539).
