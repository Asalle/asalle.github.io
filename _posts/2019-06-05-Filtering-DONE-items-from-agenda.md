---
layout: post
title:  "Filtering DONE items from agenda"
date:   2019-06-05 09:23:20 +0100
categories: programming
tags: org-mode, org-agenda, emacs
---

# Steps in short
In `.spacemacs`:
```
(defun dotspacemacs/user-config ()
  "Configuration function for user code.
This function is called at the very end of Spacemacs initialization after
layers configuration.
This is the place where most of your configurations should be done. Unless it is
explicitly specified that a variable should be set before a package is loaded,
you should place your code here."
;; some other setq ...

  (setq org-agenda-skip-scheduled-if-done t)
  (setq org-agenda-skip-deadline-if-done t))
```

# Story behind
After install this magnificent calendar [view](https://github.com/kiwanami/emacs-calfw) to spacemacs I firgured, that my calendar is littered with `DONE` items, at least in the month view. `calfw` allows you filter entries with `cfw:org-agenda-schedule-args`, but they concern only the types of entries (you can filter out all deadlines, scheduled or sexps), but does not allow one to filter out by `DONE` or `TODO`.

Luckily, a quick search with `SPC h d v` found that agenda can actually skip done items.

God bless developers of org-mode.
