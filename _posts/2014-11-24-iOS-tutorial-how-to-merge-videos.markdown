---
layout: post
title:  "iOS Tutorial: Merging Videos via AVFoundation"
date:   2014-11-24 09:00:00
categories:
tags: [iOS, AVFoundation]
comments: true
share: true
hide_masthead: true
---

I've recently gotten back into iOS programming. In October, I built the iOS app + API for [Bartndr], a fully automated bartending solution that won the Kleiner Perkins Caufield Byers Fellows Prize at [Cal Hacks]. More recently, I've been working on a collaborative video recording app in my spare time. One of the issues I've faced was merging **multiple video assets into a single video client-side**. I searched online for a good tutorial to achieve this using the AVFoundation framework, but I didn't find anything **substantial**. Well here's a quick tutorial.

#Setup
I've placed the below code snippets inside a static method within my video model. 

#Step 1: Create AVMutableComposition + AVMutableCompositionTracks

<pre><code>
            AVMutableComposition *mixComposition = [[AVMutableComposition alloc] init];
            AVMutableCompositionTrack *mutableCompVideoTrack = [mixComposition addMutableTrackWithMediaType:AVMediaTypeVideo preferredTrackID:kCMPersistentTrackID_Invalid];
            AVMutableCompositionTrack *mutableCompAudioTrack = [mixComposition addMutableTrackWithMediaType:AVMediaTypeAudio preferredTrackID:kCMPersistentTrackID_Invalid];
</code></pre>

The above code instantiates an AVMutableComposition object, which will be used to hold our merged videos, and AVMutableCompositionTrack objects, which will be used to hold the video and audio tracks of our videos. Note that you only need *two* AVMutableCompositionTrack objects for multiple video/audio tracks.

#Step 2: Convert Video Files To AVAssets

<pre><code>
            CMTime currentCMTime = kCMTimeZero;
            
            for (int i = 0; i < getMNVideoFilesTasks.count; i++) {
                NSData *videoFileData = [getMNVideoFilesTasks[i] result];
                NSURL *randomVideoFileURL = [self getRandomVideoFileURL];
                
                [videoFileData writeToURL:randomVideoFileURL options:NSAtomicWrite error:nil];
                AVAsset *videoAsset = [AVAsset assetWithURL:randomVideoFileURL];
        
                [mutableCompVideoTrack insertTimeRange:CMTimeRangeMake(kCMTimeZero, videoAsset.duration) ofTrack:[[videoAsset tracksWithMediaType:AVMediaTypeVideo] objectAtIndex:0] atTime:currentCMTime error:nil];
                [mutableCompAudioTrack insertTimeRange:CMTimeRangeMake(kCMTimeZero, videoAsset.duration) ofTrack:[[videoAsset tracksWithMediaType:AVMediaTypeAudio] objectAtIndex:0] atTime:currentCMTime error:nil];
                
                currentCMTime = CMTimeAdd(currentCMTime, videoAsset.duration);
            }
</code></pre>

The above code takes the video file data (NSData) and writes the data to a temporary random file url, which is then turned into an AVAsset. These AVAsset objects are written to the AVMutableCompositionTrack by the following method *insertTimeRange: ofTrack: objectAtIndex: atTime: error:*. I use *currentCMTime* to keep track of the correct next starting time for *atTime*.

#Step 3: Export Merged Video File

<pre><code>
            NSURL *randomFinalVideoFileURL = [self getRandomVideoFileURL];
            AVAssetExportSession *exportSession = [[AVAssetExportSession alloc] initWithAsset:mixComposition presetName:AVAssetExportPreset640x480];
            exportSession.outputFileType=AVFileTypeQuickTimeMovie;
            exportSession.outputURL = randomFinalVideoFileURL;
            
            CMTimeValue val = mixComposition.duration.value;
            CMTime start = CMTimeMake(0, 1);
            CMTime duration = CMTimeMake(val, 1);
            CMTimeRange range = CMTimeRangeMake(start, duration);
            exportSession.timeRange = range;
            
            [exportSession exportAsynchronouslyWithCompletionHandler:^{
                switch ([exportSession status]) {
                    case AVAssetExportSessionStatusFailed:
                    {
                        NSLog(@"Export failed: %@ %@", [[exportSession error] localizedDescription],[[exportSession error]debugDescription]);
                    }
                    case AVAssetExportSessionStatusCancelled:
                    {
                        NSLog(@"Export canceled");
                        break;
                    }
                    case AVAssetExportSessionStatusCompleted:
                    {
                        NSLog(@"Export complete!");
                        [getAllMNVideosCompletionSource setResult:exportSession.outputURL];
                    }
                    default:
                    {
                        NSLog(@"default");
                    }
                }
            }];
</code></pre>
The last code snippet above creates an AVAssetExportSession, which will be used to export the AVMutableComposition of our merged videos. We once again create a random file url to assign to the AVAssetExportSession and perform the export asynchronously. In the completion block, we can handle different statuses via [exportSession status]. I personally use the [Bolts Framework] to make the organization of my asynchronous code easier.[^1] 

Don't hesitate to contact me if you have any comments or questions. Thanks!

Like this post? [Follow me on Twitter](http://www.twitter.com/pkayfire) to stay updated!

[Bolts Framework]: https://github.com/BoltsFramework/Bolts-iOS
[Bartndr]:         http://www.bartndr.me
[Cal Hacks]:    http://www.calhacks.io

[^1]: The Bolts Framework introduces the concept of a Javascript Promise into handling asynchronous code in iOS.
