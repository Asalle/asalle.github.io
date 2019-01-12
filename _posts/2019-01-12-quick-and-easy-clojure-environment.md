---
layout: post
title:  "Quick and easy clojure environment setup"
date:   2019-01-12 12:39:20 +0100
categories: programming
tags: clojure, spacemacs, emacs
---

## Steps in short
1. Install latest [emacs](https://www.gnu.org/software/emacs/): `sudo apt install emacs`.
2. Install latest [spacemacs](http://spacemacs.org/):
`git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d`.
3. Install [lein](https://leiningen.org/#install).
4. Add [clojure](spacemacs.org/layers/+lang/clojure/README.html) layer:
 - `SPC f e d`  to open the `~.spacemacs` file;
 - add clojure to the `dotspacemacs-configuration-layers variable`;
 - `SPC f e R` to download layer and apply changes.

## Story behind
So I started `Clojure for brave and true` by Daniel Higginbotham and the first chapters of it are dedicated
to setting up REPL (I guess you can call it clojure shell) and development environment in general. The instructions given are great
except they do not work. I tried to install manually CIDER and nREPL and it was terrible, so I gave up on emacs and just started a project
in IntelliJ IDEA with Cursive plugin, only to realize that it's hideous and too heavy of an environment for such a small task. A friend of mine recommended me
spacemacs, primarily because it has evil mode, which I, as a devoted vim user, loved absolutely.

Who knew spacemacs had this amazing layer mechanism that helped you install plugin bundles in a second!

After installing it I just used those simple spacemacs key bindings for REPL:
`SPC m s b` to evaluate buffer
`SPC m s s` to switch between REPL and buffer I was before
and I was happily coding ever after!


Also, check out these incredible clojure resources:
 - Spacemacs + clojure special from [practical.li](https://practicalli.github.io/spacemacs/).
 - Spacemacs + clojure without manual configuration ([youtube](https://www.youtube.com/watch?v=Uuwg-069NYE))
