---
title: "On Better Developer Testing"
date: 2013-04-06
categories: 
  - "development"
tags: 
  - "-net"
  - "development"
  - "testing"
---

I read an interesting [post](http://haacked.com/archive/2013/03/04/test-better.aspx) by [Phil Haack](http://haacked.com/) a little while ago on having developers do better testing themselves instead of relying solely on their QA team to test. While it’s definitely a great read in itself, I felt I had to add my opinion to it.

Don’t get me wrong with this, though. I think the more experience and skill a developer has on testing their own code, the more valuable they actually are. Also, it’ll mean their code is that much more reliable and stable upon initial release all the way through to production. No developer wants to find that their code somehow broke in production the morning after it was deployed and spend that whole day fixing it.

While I definitely agree with Phil on most of his arguments, there was one thought about the whole thing that kept coming to mind about myself, personally, when testing stuff that I’ve written. I know I don’t have the experience to think of a good amount of scenarios the code will be hit at. Now, this may not be an issue for the more experienced developers out there, but I know it’s a weak point of mine. I’ve definitely been bit by not doing more of this in the past and, indeed, it wasn’t fun admitting it was my fault when the manager is asking what happened. In fact, when this happened to me it gave me a very real benefit of using unit tests. Of course, unit tests can’t test everything that a user would do as input or processes on a screen. While Phil notes that Test Driven Design is mainly for the initial design of the code we’re ultimately testing, it definitely helps to make sure any refactorings for readability, maintainability, performance, etc. didn’t break anything we’ve already shown and trust to be correct.
