---
layout: post
title:  "3 Reasons to Switch From Native to Hybrid for Mobile App Development"
date:   2014-08-18 11:00:00
categories: dev mobile
tags: [native, hybrid, iOS, Android, Cordova, AngularJS, Ionic]
comments: true
share: true
hide_masthead: true
---

I continually notice a stigma in the tech community that hybrid mobile apps are considered 'second class' and 'inferior' to native apps. As someone who's recently made the switch from native to hybrid ([Cordova](http://cordova.apache.org/) + [AngularJS](https://angularjs.org/) + [Ionic](http://ionicframework.com/)), I wanted to share some reasons why we chose to go with a hybrid stack for [Throwback] and our upcoming projects.

#Reason 1:  On-the-fly Updating
Kevin (my co-founder @kevinchvn) and I have come to despise the ~7 day waiting period to get approved by the iOS App Store. With a mobile app pointed to our hosted Heroku app, we have the ability to make **constant real-time changes** to our production app -- anything from small bug fixes to rolling out new features.

#Reason 2: Unified Clean Codebase
As a developer, **copying and pasting** code makes me **cringe**. It's not just ugly. Having to rewrite the same logic for each platform is a mind numbing process and increases the chance of bugs.  As I was porting one of our earlier projects from iOS to Android, Kevin constantly questioned why we were writing the same code twice. Keeping with the DRY philosophy, it seems like an obvious choice to have a single codebase with native hooks for iOS + Android. *Less code = Less work*

#Reason 3: F*ck Device Fragmentation 
Working with Auto Layout (iOS) and XML (Android) to handle device fragmentation just plain *sucks*. Using a front-end framework like [Ionic](http://ionicframework.com/) lets you build beautiful mobile-optimized UI with just **HTML, CSS, and JS**.

What it comes down to is this: What’s the **fastest** way we can go from the **stroke of our keyboards** to our **user’s phone**? 

Mobile hybrid app development has evolved and continues to evolve to near-native performance. We've observed (to the best of our knowledge) no performance complaints due to using a hybrid stack.

I'm excited to see what the future holds for hybrid apps.

*Shameless Plug*: Check out [Throwback] on [iOS] and [Android] to see our hybrid mobile app in action!

[Throwback]:         http://www.throwbacknow.com
[iOS]:     http://www.throwbacknow.com/#!/ios
[Android]: http://www.throwbacknow.com/#!/android

