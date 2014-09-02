---
layout: post
title:  "3 Observations on Why Hackers Should Embrace Analytics"
date:   2014-09-02 10:37:00
categories:
tags: [hacking, analytics, shipping, product]
comments: true
share: true
hide_masthead: true
---

We get it. You're known as the *progamming genius* or *coding ninja* among your peers. You're the guy (or gal!) that everyone comes to when they want advice on how to start programming. You're the person that can architect front-end, back-end, tri-end[^1] with their eyes closed.

You don't give a *second thought* to **analytics**.

I'm not saying I'm anywhere near the skill level or experience of a *certified hacker*. Our team at [Throwback] have, however, observed the following in the past year while working on several projects:

#Observation 1: Analytics Exposes Critical Bugs
When we released our first version of Throwback to the app store, we noticed that there was a significant discrepancy between the **# of Downloads** vs **# of Signed Up Users**. We decided from the advice of our mentor to add analytics to track a *visitor's flow* through our onboarding process. We ended up uncovering a number of critical bugs where a *potential user* dropped off due to various edge cases we forgot to cover -- edge cases that would've been **painstakingly hard** to uncover with no idea where to look.

#Observation 2: Analytics Helps You Know Your Users
It's hard to predict who your users are going to be and how they're going to use your product. How can you decide what to fix + build next if you have *no data on your users*? Sure, you can reach out to user's directly, but we have found that getting direct user feedback to be time-consuming and sometimes inaccurate[^2]. We've observed that making **data-driven** product decisions have always turned out better versus making decisions on a whim.

#Observation 3: Analytics Helps You Ship Faster + Smarter
We're hackers. We love to ship fast and watch shit break (then fix it!). Analytics gives you full control of your product. If set up correctly, analytics will provide you with the data to decide *what to ship next*, if you *broke anything* while shipping, and if what you shipped **actually provided value to users**. You'll always have a **complete** and **user-centric** overview of your product.

The common perception among the hacker community is to leave analytics to the *business folk* and to concentrate solely on architecture + implementation. As we start to concentrate more on product and less on technical details, I sincerely hope we don't throw analytics into the **non-essential** bucket.

*Shameless Plug*: Check out [Throwback] on [iOS] and [Android] to see our analytics in action!

Like this post? [Follow me on Twitter](http://www.twitter.com/pkayfire) to stay updated!

[Throwback]: http://www.throwbacknow.com/
[iOS]: http://www.throwbacknow.com/#!/ios
[Android]: http://www.throwbacknow.com/#!/android

[^1]: My sorry attempt at a joke. No such thing exists AFAIK
[^2]: Sometimes users would ask for features they ended up not using.