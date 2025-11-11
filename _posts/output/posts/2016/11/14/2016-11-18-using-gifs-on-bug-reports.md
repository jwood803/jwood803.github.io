---
title: "Using GIFs on Bug Reports"
date: 2016-11-18
categories: 
  - "development"
  - "technology"
tags: 
  - "gifs"
  - "testing"
coverImage: "photo-1470790376778-a9fbc86d70e2.jpeg"
---

If you've been developing software for any length of time you've fixed a bug or two that someone has reported. Whether for a customer or a user within your organization, sometime's words of how something is fixed or how to duplicate an issue may not be enough.

Using screenshots is great to show if something is working, especially if it is something easily to show like a UI element working. However, to show that a series of steps is working with the end showing that the fix is there, I believe GIFs are a great way to include in your bug reports after a fix has been implemented.

### But How Do I Make A GIF?

I primarily run a Mac, so I really love how easy [Screenflow](https://store.telestream.net/affiliate.php?ACCOUNT=TELESTRE&AFFILIATE=90688&PATH=http://www.telestream.net/screenflow/) is for this. Sure it's primarily for screencasting, and I use it [for that, too](http://devcenter.wintellect.com/jwood/using-xaml-f-xamarin-forms-screencast), but it's really easy to make GIFs with this.

Of course, there are free alternatives (before Screenflow I was using [Recordit](http://recordit.co/) with good results) and they all are good to use in their own right. One thing I really like with Screenflow is that it is really easy to edit after recording your GIF.

### Why Does Editing GIFs Matter?

The primary reason why I'd want to edit a GIF is to remove unneeded parts of what I was recording. The same thing you'd do with any video, remove any dead or time consuming parts. If you're recording a GIF to show that something is working after a fix is put in, you may have times where you're waiting for a process to execute in order to show the fix is working. Editing your GIF to remove the time it takes for the process to launch and load will reduce the length of the GIF as well as to make it smaller for uploading or viewing.

Another awesome reason I like using Screenflow for GIFs is that you can use all the advanced screencasting features Screenflow provides for you inside of your GIFs. For instance, I can zoom in on places I want to show in the GIF or highlight specific areas of the screen so the viewer can know what I want them to see.

Let's take a look at an example. I did a very quick recording of some F# using VS Code and running a selection in the FSI. Here's the GIF of that using Screenflow's editing tools.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1479474094723-F01S5REUK8261Y91PMLJ/image-asset.gif?format=original)

Not the best edited example, but I hope it helps illustrate some of the things Screenflow can do to make GIFs that much better to include in bug reports.

There are a couple of things of note, though.

- Screenflow allowed me to specifiy what area I wanted to be recorded. I just adjusted the preview window right before recording to the size I needed.
- I did a small zoom to better show the FSI output.
- I did a callout to show to viewers the specific output in FSI of the input I gave it.

I got Screenflow to start doing some screencasting, but since they've introduced GIF support I've been using it much more for caputuring GIFs for projects at work and uploading them to whatever bug tracker we're using (Jira, Visual Studio Team Services, GitHub) and, to me, it's been quite helpful. Often I've recorded a GIF and found something else I needed to work on for that issue before it should have been sent back as fixed.
