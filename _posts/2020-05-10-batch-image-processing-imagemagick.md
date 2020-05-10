---
layout: post
title:  "Improve quality of your photos, in batches"
date:   2020-05-10 09:06:20 +0100
categories: tools
tags: image-processing, imagemagick, handy-commands
---

Image magick is an excellent command-line photo editor, it has a lot of features other image editors have, but because of its command-line nature, it can perform them on multiple images at a time.

I had a couple of basic image-improving changes to be performed on 5 of my photos. All of my photos have name of 'IMG_*.jpg'. I put the processed photos into a separate folder just not to mix them up, folder is called `conv`.

## Improve brightness/contrast/threshold:
```
convert ./IMG_*.jpg -level 25%,75%,2.0 ./conv/img.jpg
```  
Command explained:
* `convert` is a utility made for converting images, it's part of the imagemagick.
* `./IMG_*.jpg` is a regular expression for the input files.
* `-level` is imagemagick's option that tells us that we want to change the image's levels (brighness, contrast, ...).
* numbers `25%` and `75%` stand for `white point` and `black point`. white point means "take all pixels that are equal to this value or lighter and make them completely white", and black point means "take all pixels that are equal to this value or darker and make them completely black". This will increase contrast and brightness of the image.
* `2.0` stands for `gamma`, it's for making the image lighter or darker in general, see more [here](http://www.imagemagick.org/Usage/color_mods/#level_gamma).
* `./conv/img.jpg` is the name of the resulting image. We'll get a bunch of images with the names `img_1.jpg` in the folder `conv`.

See more on image levels [here](http://www.imagemagick.org/Usage/color_mods/#levels).

## Crop photos:
```
convert ./IMG_*.jpg -crop 1330x1100+320+0  ./conv/img.png
```  
Command explained:
* `convert` - as we already know is the name of the converting utility.
* `./IMG_*.jpg` are the names on the input files
* `-crop` is the name of the option
* `1330x1100` - is the requested size of the cropped image.
* `+320+0` - is the center of the crop.

See more on crop [here](http://www.imagemagick.org/Usage/crop/#crop).

