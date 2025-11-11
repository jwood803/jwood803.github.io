---
title: "Xamarin Evolve - Day 1 Summary"
date: 2014-10-09
categories: 
  - "xamarin"
  - "events"
tags: 
  - "conference"
  - "xamarin"
---

Considering this is my first time going to an actual conference, especially one at this size, I believe I could be easily impressed by anything done at [Xamarin Evolve](https://evolve.xamarin.com/). And indeed, I was impressed! From all the the quality of the presentations to just how well this event is being run, this is definitely an event I'll remember and will want to come back to next year.

I admit, I haven't done much with Xamarin and their tools other than getting excited over this conference and some small playing around with [Xamarin Studio](https://xamarin.com/studio). After this first day, though, I'm excited to play around with it a lot more and utilize it for mobile apps.

### Keynote

Things started off with the co-founders on stage giving a keynote that contained quite a few surprises.

#### Profiler

First thing announced was a [profiler](http://xamarin.com/profiler) for Android and iOS apps to help developers dig deeper into performance issues. This is apparently helpful since there isn't really an alternate solution for profiling mobile apps. Especially one as nice as they have made.

#### Mobile UI Tests

This one really got me excited the more I saw demos of it in use. The Xamarin team has created a library to create unit tests that can launch the app and perform actions within the app. If certain items on the screen can't be found or an action can't be performed, it will fail the test.

Now this by itself can be useful for an integration test suite, but what's really exciting is this is integrated into the [Xamarin Test Cloud](http://xamarin.com/test-cloud) so the tests can be run on all devices that it needs to be run on!

#### Android Player

I'm sure that if you ask any Android developer what the biggest pain point is in developing an Android app is that the emulator take a really long time to start up when debugging/running an application. Really...you can probably go on a 30 minute walk and it _may_ be finished loading the OS and start your app.

What Xamarin has done was to create a way better experience for this. During the demos the [Android Player](http://xamarin.com/android-player) took way over half the time to launch, start the OS, and launch the app than with the emulator. This by itself definitely got most people excited.

#### Sketches

Another interesting thing they announced is [Sketches](http://developer.xamarin.com/guides/cross-platform/sketches/) which is a live coding environment that executes the code as you type. The interesting thing with this, though, is that they've integrated an analysis pane. Xamarin has allowed graphing within Sketches. If prototyping a small UI, you can even use Android or iOS components right within Sketches.

#### Xamarin Insights

Xamarin also announced a way to monitor apps when they have been published and out for the public to use.

[Xamarin Insights](http://xamarin.com/insights) looks like it has a lot to offer than the other services that do this. Just by adding only a few lines of code in your app you can get real time notifications of errors that includes stack traces and even show how users will interact with your app.

This and the UI tests together will make a world of a difference in the quality of your mobile apps.

### Day One Conclusion

Just after day one I'm glad I decided to come to this conference. I've already learned a ton and got back some excitement from coding that I used to have. I can't wait for what's in store for day two.
