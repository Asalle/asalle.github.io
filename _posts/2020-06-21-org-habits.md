---
layout: post
title:  "Atomitc habits, org-habit and... habits!"
date: 2020-06-21 18:42:20 +0100
categories: self-tracking 
tags: emacs, spacemacs, habits, org-habit, atomic habits
---

After reading an incredible book "[Atomic Habits](https://jamesclear.com/atomic-habits)" by James Clear I started to put more attention to my own habits. The analogy of changing the plane's course 3.5 degrees got to me:  

![3.5-degree course chage landed the plane in Washington instead of NY]({{site.baseurl}}/assets/habits/plane.jpg)

So, the first rule of consious habits: "Make it visible". As a happy user of `org-mode` I couldn't imagine a better place to put them, then `org-habit`.  
To use `org-habit` with `spacemacs` you need to enable it first as an org-module. Thankfully, there is a handy shortcut to display the contents of the variable, in our case - it's `org-modules`:  
`SPC h d v` - then type `org-modules`
_I remember this mnemonic shortcut as `SPC` + `Help` + `Describe` + `Variable`_.   
Scroll all the way down to the `Customize variable` line and click it.
After you've seen the contents of `org-modules` variable and actually see that `org-habit` is not enabled - put the tick in front of it and save (C-x C-s).  

![list of org-modules]({{site.baseurl}}/assets/habits/org-modules.png)  

Now all is left is to write down your habits. Let's say I want to wear my posture corrector every day. Let's look at this example habit:  
```
     ** TODO 15mins with posture corrector
        SCHEDULED: <2020-06-20 Sat .+1d>
        :PROPERTIES:
        :STYLE:    habit
        :END:  
```
It's just a regular TODO entry, with a couple of changes:  
1) it's scheduled to the time I want it to repeat (in my case, daily, for more details on scheduling see [this](https://orgmode.org/manual/Deadlines-and-Scheduling.html))
2) it includes a special tag `:STYLE: habit`  

Now, that we've created an actual habit, let's take a look at it in the agenda view:  
`, a a` - to open agenda  

![habits in agenda view]({{site.baseurl}}/assets/habits/consistency_graph.png)

This funny blue-yellow-red stripe is called "consistency graph" - at first I didn't get what it means, but then I understood that it depicts a row of several days in the past and in the future. Green (or blue if it's a new habit) on the left are days when checking the habit as `DONE` meant that you did it on time.  
One yellow thingy is likely today (means that habit is risking being late - org has so little faith in me :sigh:).  
  
And the rest of red pieces depic all those days in the future when ticking the habit off would mean that you ~~lost~~ were late.  
The amount of days before and after today is changable by chaning the variables `org-habit-presceding-days` and `org-habit-following-days`.  
I guess they are there to make you motivated?.. (Don't break the chain!..) Anyway, I don't enjoy seeing so much red in my agenda so I changed the content of `org-habit-following-days` to 0.  
  

Alright, now my agenda will bully me into following my habits every day! One pity is that in spacemacs+evil you cannot use K to toggle habits in agenda view,  I have to invoke the command by clicking `M-x org-habit-toggle-habits`. Also it is likely not supported by [Orgzly](https://github.com/orgzly/orgzly-android) - an outliner that is semi-equal to org-mode on my phone.  

What habits do you want to form? What do you want to get rid of? What are your ways to track habits? Letme on know via email.

P.S. I hightly recommend James' newsletter - it really delivers what it promises.
