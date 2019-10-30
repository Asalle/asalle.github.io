---
layout: post
title:  "ccze is awesome!"
date:   2019-10-30 19:37:20 +0100
categories: tools
tags: journalctl, colors, ccze
---

I was skimming though journalctl logs of my project and I found it unfortunate that journalctl stripped all the colors... So I installed `ccze` to colorize them back!  

`sudo dnf install ccze`  

Using `ccze` couldn't have been easier!  

`journalctl -u <unitname> -f | ccze`  

You can even colorize files with `tail`:  

`tail -f -n 50 file.txt | ccze`  

The only drawback is that you cannot input the enter lines in log, which was pretty handy in `journalctl`... Also, the colors that I have in `ccze` are not original colors from my logger, `ccze` reparsed by logs and added its own colors.  


Still looks better than plain white text :)

