---
layout: post
title:  "LaTex in spacemacs"
date:   2020-02-16 19:37:20 +0100
categories: file-editing
tags: latex, tex, emacs, spacemacs
---

# Steps in short
1. Install latexmk: `sudo dnf install latexmk`
2. Add [latex](https://develop.spacemacs.org/layers/+lang/latex/README.html) layer to spacemacs:
 - `SPC f e d`  to open the `~.spacemacs` file;
 - add `latex` to the `dotspacemacs-configuration-layers variable`;
 - `SPC f e R` to download layer and apply changes.
3. Open a .tex file and compile a pdf with `, b` or `latex/build`

If you, like me, enjoy working with 2 windows with code in the first one and the resulting pdf in the second one then do this:
1. Open a .tex file: `C-x C-f`
2. Divide your window in two: `SPC w /`
3. Go to the second window and open a pdf that you just build there.
If you see pdf as plan text - enable `doc-view-mode`
4. Change the .tex, press `, b` to rebuild and see changes applied in the second window.

![two windows with latex and pdf]({{site.baseurl}}/assets/latex.png)
<p style="text-align: center;"><i>it's a png so zoon in</i></p>

Pro tip: regulate the width of both screens with going into `window-transient-state` (`SPC w ,`) and pressing `[` and `]`.

# Story behind

I like compiling things and having my documents version-controlled, that's why I like tex.
I tried TexMaker and TexStudio but they both have funny UI.
I use spacemacs for my day-to-day, so I tried to give it a shot.
Turned out it's quite decent.
