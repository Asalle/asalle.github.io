---
layout: post
title:  "Figure out the amount of free space on your disk"
date:   2019-11-17 19:37:20 +0100
categories: tools
tags: du, space, hdd
---

# Steps in short

`df -h` will show you disk usage per filesystem.  
`du -h <foldername>/*` will show you file sizes in the specified folder.  
`docker system prune` may free astonishing amount of disk space sometimes.  
