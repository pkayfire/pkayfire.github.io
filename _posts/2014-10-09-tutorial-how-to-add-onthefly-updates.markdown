---
layout: post
title:  "Tutorial: How to Add On-The-Fly Updating to a Hybrid Mobile App"
date:   2014-10-09 11:00:00
categories:
tags: [native, hybrid, Cordova, AngularJS, Ionic, NodeJS, Heroku]
comments: true
share: true
hide_masthead: true
---

I've been getting emails regarding how to *specifically* add on-the-fly updating capability to a hybrid app after my [previous post] so I thought I would write up a quick tutorial on how we do it for [Throwback]. 

What do I mean by **On-The-Fly Updating**? For us, OTFU[^1] means the ability to change any *HTML, CSS, or JS* in our app and push the update to our users without having to submit the app through *any app store approval process*. It's saved us significant time in our development cycle, and has helped us push out **crucial bug fixes/features without friction** to our users. I can go on and on about the benefits, but I'm sure everyone is more interested in the tutorial.

#Precursor

We use [Apache Cordova] + [AngularJS] + [NodeJS] + [Ionic] + [Parse] in our hybrid tech stack. Although my tutorial will be oriented to this particular tech stack, it should be trivial to implement this method to other frameworks.

This method works by configuring your hybrid app to point to a **remote server hosting your files** instead of relying on the local copy of your files packaged with your app when it releases to the app store.

#Setup

We will be working with the following files/directories found in an Apache Cordova project:

* config.xml
* www/

#Step 1: Host www/ on a Remote Server
We first need to host our www/ directory holding all our AngularJS code on a remote server so we can push new changes to our code to the remote server, which the hybrid app will use. We use [NodeJS] + [Express] + [Heroku] to host our www/ directory.

I've put below our configuration for Express:
<pre><code>
var express = require('express');
var app = module.exports = express();

app.configure(function(){
  app.use(express.static('www/'));
  app.use(app.router);
});

app.get('*', function(req, res){
  res.sendfile('www/index.html');
});

app.listen(process.env.PORT || 9000);
console.log("Starting Express Server...");
</code></pre>

You can access a [hosted version of Throwback here].

#Step 2: Configure config.xml to point to remote server

We now need to configure config.xml in our Apache Cordova project in order to make the hybrid app load from our remote server.

I've put below a shortened version of our config.xml:

```
<widget id="com.throwback.throwbackapp" version="1.1.3" xmlns="http://www.w3.org/ns/widgets" xmlns:cdv="http://cordova.apache.org/ns/1.0">
  <name>throwback_mobile</name>
  <description>
        See exactly what you and your friends did today in history! Travel through time and view photos and tweets that you forgot you posted. Connect now and join in on the fun!
    </description>
  <author email="trythrowback@gmail.com" href="http://www.throwbacknow.com/">
      Team Throwback
    </author>
  <content src="mobile.throwbacknow.com"/>
  <access origin="*"/>
  <preference name="webviewbounce" value="false"/>
  <preference name="UIWebViewBounce" value="false"/>
  <preference name="DisallowOverscroll" value="true"/>
  <preference name="BackupWebStorage" value="none"/>
</widget>
```

Notice how ``` <content src="xxx"/> ``` has a remote url as a source rather than index.html, which points to a local file in your project. 

#Step 3: Set Up Native Hook to Refresh Web View On Update

The last thing we need to is make sure our app refreshes its web view every time we push a new update to our remote server. The way we accomplish is to make our app ping our API for the latest version number of our app. If the version of the app on an user's phone is lower than the version returned by the API, the web view refreshes and downloads the latest version from our remote server.

I've put below how we accomplish this in an iOS app using Parse:
<pre><code>
// In AppDelegate.m

    [PFCloud callFunctionInBackground:@"getMostRecentVersion" withParameters:@{} block:^(NSNumber* result, NSError *error) {
        if (!error) {
            NSLog(@"Current Version: %@", [result stringValue]);
            __block NSNumber* versionNumber = result;
            PFInstallation *currentInstallation = [PFInstallation currentInstallation];

            if (currentInstallation) {
                [currentInstallation fetchIfNeededInBackgroundWithBlock:^(PFObject *object, NSError *error) {
                    NSLog(@"Current version on PFInstallation: %@", [object objectForKey:@"version"]);
                    if ([versionNumber doubleValue] != [[object objectForKey:@"version"] doubleValue]) {
                        NSLog(@"The current version on the PFInstallation is NOT equal with the current available version!");
                        [object setObject:versionNumber forKey:@"version"];
                        [object saveInBackground];
                        
                        [self.viewController.webView reload];
                    } else {
                        NSLog(@"The current version on the PFInstallation is equal with the current available version.");
                    }
                }];
            }
        }
    }];
</code></pre>

There you have it. On-the-fly updating for your hybrid app. AFAIK, this method is approved by the various app stores as long as your app feels and behaves like a native app. 

Don't hesitate to contact me if you have any comments or questions. Happy hacking!

*Shameless Plug*: Check out [Throwback] on [iOS] and [Android] to see our hybrid mobile app in action!

Like this post? [Follow me on Twitter](http://www.twitter.com/pkayfire) to stay updated!

[previous post]: http://blog.peterkim.co/3-reasons-to-switch-from-native-to-hybrid

[Apache Cordova]: http://cordova.apache.org
[AngularJS]: http://angularjs.org
[NodeJS]: http://http://nodejs.org
[Ionic]: http://www.ionicframework.com
[Parse]: https://parse.com/

[Express]: http://expressjs.com/
[Heroku]: http://heroku.com

[hosted version of Throwback here]: http://mobile.throwbacknow.com

[Throwback]:         http://www.throwbacknow.com
[iOS]:     http://www.throwbacknow.com/#!/ios
[Android]: http://www.throwbacknow.com/#!/android

[^1]: My nifty abbreviation for On-The-Fly Updating