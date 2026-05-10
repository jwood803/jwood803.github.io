---
title: "Mastering .NET Debugging….or Trying To"
date: 2013-04-03
categories: 
  - "debugging"
tags: 
  - "-net"
  - "debugging"
  - "training"
---

For two days this week I’ve had the privilege of taking another training [course](http://www.wintellect.com//Training/Courses/Mastering-.NET-Debugging/) from [Wintellect](http://www.wintellect.com/) and [John Robbins](http://www.wintellect.com/cs/blogs/jrobbins/default.aspx) (author of a still very relevant [debugging book](http://www.amazon.com/Debugging-Microsoft-NET-2-0-Applications/dp/0735622027/ref=sr_sp-atf_title_1_1?s=books&ie=UTF8&qid=1365215166&sr=1-1&keywords=debugging+applications+.net+2)) was the instructor. Based on the title of this post, the course was on .NET debugging. I thought I’d give my thoughts on this course and my experiences with Wintellect’s training in general.

## Day 1

This day was mainly focusing on debugging through Visual Studio and slightly changing your processes in how you do daily development and coding, such as more advanced breakpoints. One of the things that I didn’t quite expect to learn was about code analysis. I have used FxCop as a stand-alone tool before a few times, but now it seems that as of Visual Studio 2012 (that I’ve noticed) it’s built into the IDE. This is definitely the quickest thing to incorporate into your daily routine. Once I was back in the office turning on code analysis was the first thing I started changing. Another aspect from this day was how versatile the watch window is. I used to use the immediate window if I needed to run a quick statement, but switching to the watch window seems to be a better choice. It’s much easier to explore the object I evaluated and it’s members.

## Day 2

Now this is where it was getting more fun. I’ve been interested in learning more about how to use WinDBG plenty of times in the past but, in all honesty, each time I’ve tried it’s just gotten so overwhelming that it didn’t last very long. Sure there are actually tons of resources now to help, but if you get to a point where you have no idea how the author was able to find how they got a memory address or explained how they knew how to go from one command to the other, you’re just going to get lost. With this course, however, if I got to that point, all I had to do was just ask and it was explained very clearly. That’s one of the reasons I like these so much, it’s like having your own personal mentor for a day or two. Now I feel like I have a decent understanding of at least what I can do to start doing some of this myself. Take a random memory dump and see what I can find by exploring. John even went over a few interesting items related to performance, as well.

This course definitely makes me feel much more confident in my debugging skills and I hope to use all the items learned from here to their full extent. Of course, one thing I’ve learned recently – if John Robbins tells me how to code, that’s how I’m going to code. :\]
