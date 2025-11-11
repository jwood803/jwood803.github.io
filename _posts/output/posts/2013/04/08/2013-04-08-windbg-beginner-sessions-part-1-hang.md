---
title: "WinDBG Beginner Sessions: Part 1 - Hang"
date: 2013-04-08
categories: 
  - "debugging"
tags: 
  - "-net"
  - "debugging"
  - "windbg"
---

We got through the exhibition [post](/blog/2013/04/07/windbg-beginner-sessions-part-0), but now it’s time to get serious. I figured the quickest way to get going here is to go through [Tess Ferrandez’s](http://blogs.msdn.com/b/tess/) series of debugging [labs](http://blogs.msdn.com/b/tess/archive/2008/02/04/net-debugging-demos-information-and-setup-instructions.aspx). I’ve heard that these are actually some of the best tutorials and hands-on experience you can get with WinDBG, so I’m excited to get started.

Of course, one of the reasons I’ve waited so long to do this is due to the labs having the demo site setup in IIS. I’m definitely not the best at deploying and configuring IIS, so instead here I used IIS Express. This seems to work quite well so I’ll be sticking with that throughout the labs. So let’s get started with [lab 1 – the hang](http://blogs.msdn.com/b/tess/archive/2008/02/04/net-debugging-demos-lab-1-hang.aspx).

The first part of the lab was to recreate the issue. Since TinyGet appears to be dead, I just manually hit refresh on the browser windows. While that was going I ran the following command to get a dump file while going to the page in question:

```
.\procdump.exe -n 2 iisexpress.exe
```

Unfortunately, I forgot to generate the dump using the -ma switch, so no memory information will be available. I swear to remember to use this from now on!

Based on the suggestion of the lab, running the ~\* kb 2000 command. Ok, so after some searches, I'm still not exactly sure what this command is saying, but here's a shot in the dark:

- ~\* will execute the following command towards all threads.
- kb will print out the native (non-.NET) call stack.
- 2000…well, here’s where I'm lost. If anyone out there happens to know, please let me know.

And the results of this is the below. Let's see, it appears that most of these threads are just in a waiting state.

[![clip_image001](http://static.squarespace.com/static/521545a6e4b0734032a27076/52154697e4b0fe422ebb9ca8/5215469ae4b0fe422ebb9ce2/1365453601000/clip_image001_thumb.png?format=original "clip_image001")](http://static.squarespace.com/static/521545a6e4b0734032a27076/52154697e4b0fe422ebb9ca8/5215469ae4b0fe422ebb9cdf/1365453596000/clip_image001.png?format=original)

Oh, but after some scrolling, thread 28 has something interesting.

[![clip_image002](http://static.squarespace.com/static/521545a6e4b0734032a27076/52154697e4b0fe422ebb9ca8/5215469ae4b0fe422ebb9cee/1365453609000/clip_image002_thumb.png?format=original "clip_image002")](http://static.squarespace.com/static/521545a6e4b0734032a27076/52154697e4b0fe422ebb9ca8/5215469ae4b0fe422ebb9cea/1365453603000/clip_image002.png?format=original)

Let's take a closer look at this thread and switch the debugger to it with the ~28s command. Then, we'll check out the managed call stack with the !ClrStack command.

[![clip_image003](http://static.squarespace.com/static/521545a6e4b0734032a27076/52154697e4b0fe422ebb9ca8/5215469ae4b0fe422ebb9cf4/1365453618000/clip_image003_thumb.png?format=original "clip_image003")](http://static.squarespace.com/static/521545a6e4b0734032a27076/52154697e4b0fe422ebb9ca8/5215469ae4b0fe422ebb9cf1/1365453614000/clip_image003.png?format=original)

See anything here? Talk about interesting, this thread is sleeping! Well, that's about all the commands I can think to run for this. Tess' lab suggests to run the !syncblk command, however, when I ran it I get the below.

[![clip_image004](http://static.squarespace.com/static/521545a6e4b0734032a27076/52154697e4b0fe422ebb9ca8/5215469ae4b0fe422ebb9cfa/1365453620000/clip_image004_thumb.png?format=original "clip_image004")](http://static.squarespace.com/static/521545a6e4b0734032a27076/52154697e4b0fe422ebb9ca8/5215469ae4b0fe422ebb9cf7/1365453619000/clip_image004.png?format=original)

Perhaps that's due to the fact that it isn't a full dump? Regardless, I think we have more than enough information from our dump here. Now, let's check the code that the !ClrStack command has given us. It's telling us to check out the DataLayer.GetFeaturedProducts() method, so let's take a look.

And look what we have.

[![clip_image005](http://static.squarespace.com/static/521545a6e4b0734032a27076/52154697e4b0fe422ebb9ca8/5215469be4b0fe422ebb9d00/1365453622000/clip_image005_thumb.png?format=original "clip_image005")](http://static.squarespace.com/static/521545a6e4b0734032a27076/52154697e4b0fe422ebb9ca8/5215469be4b0fe422ebb9cfd/1365453621000/clip_image005.png?format=original)

Of course, looking back at the actual [walkthrough](http://blogs.msdn.com/b/tess/archive/2008/02/06/net-debugging-demos-lab-1-hang-review.aspx) of the lab, I missed a bit without the !synblk command. I’m definitely going to redo this with an actual full dump to see if I can find the thread that is being blocked and the thread that’s doing the blocking. I got a bit lucky on this one, I think.
