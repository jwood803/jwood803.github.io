---
title: "Git Commands I Can't Live Without"
date: 2017-01-18
categories: 
  - "development"
  - "technology"
tags: 
  - "git"
  - "command-line"
coverImage: "pexels-photo-177598+(1).jpeg"
---

[Git](https://git-scm.com/) has definitely become the main version control system developers use lately, and I admit, I got on that bandwagon when I got more into [GitHub](https://github.com/jwood803). I wanted to be more familiar with Git since I was doing more and more with GitHub.

I'll also admit, and I may be dating myself here, my first version control system that I used was [StarTeam](https://en.wikipedia.org/wiki/StarTeam). Never heard of it? I don't blame you. Just look at the interface it had:

\[caption id="" align="alignnone" width="1280"\]![ StarTeam interface ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1484658809488-U4YEKUNCJJ8C3WOSYNZP/starteam.jpg?format=original) StarTeam interface \[/caption\]

I used that for a few years until the company got rid of that and went to [Team Foundation Server](https://www.visualstudio.com/tfs/). Definitely a step up from StarTeam, but then I got started using Git. The more I use it the more I enjoyed using it.

However, I didn't get an appreciation for how powerful it was at first. I first started using Git when GitHub had their [GitHub Desktop](https://desktop.github.com/) applications and used that for a while. I also used [SourceTree](https://www.sourcetreeapp.com/) on one project I was on.

While these applications are great, I still wanted to learn more about the commands these applications are running in the background. That's when I started to _only_ use Git through the command line. From that challenge I set myself, I realized there was _way_ more to Git than these applications may show. Below is a list of commands I've been using quite often, but feel they may not be as well known.

## Stash

If you ever need to quickly put your current changes away so you can help work on something more important than your current task? Well, git [stash](https://git-scm.com/docs/git-stash) is here to the rescue!

Just type in `git stash` and your changes are "stashed" away.

\[caption id="" align="alignnone" width="800"\]![ Stash newly tracked file ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1484668028466-F93I8ZK6O7FU35M4GGJH/git-stash.jpg?format=original) Stash newly tracked file \[/caption\]

Now I can move on to working on something else while keeping my changes away for me to come back to.

You can have many as many stashes as you need, as well. If you're not sure of the ones you have, just do a `git stash list` and you'll see them all.

\[caption id="" align="alignnone" width="500"\]![ List of git stashes ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1484668174157-77CSC5IXGE4ABF4G2I6Y/image-asset.png?format=original) List of git stashes \[/caption\]

Oh, but you want a meaningful message when you stash to you have an idea what changes are in it? Git has you covered there, too! Just do a `git stash save <message here>` and git will save the stash with your message.

\[caption id="" align="alignnone" width="800"\]![ Save git stash with a message ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1484668504555-7IZ6CPE8XVETK27OU8CN/image-asset.jpeg?format=original) Save git stash with a message \[/caption\]

So we can save stashes in a couple of different ways, but what if we want to go back to a stash to continue our work? We have a couple of ways to do that, as well.

If you want to just apply the latest stash, just do a `git stash apply`.

\[caption id="" align="alignnone" width="500"\]![ Applying the latest stash ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1484668639657-FDP3FMUH2ZFWDOP6EBF5/image-asset.jpeg?format=original) Applying the latest stash \[/caption\]

If we want to apply a specific stash we can just do `git stash apply <stash name>`.

\[caption id="" align="alignnone" width="750"\]![ Apply stash by name ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1484668762617-DWS1901HKCEOF1NVVOUH/image-asset.jpeg?format=original) Apply stash by name \[/caption\]

> “Note that, if you’re in PowerShell and try this command it won’t work. To get it to work, you’d have to put the stash name in quotes.”

## Diff

Sometimes, you just want to double check the changes you've done before adding and commiting. In that case `git diff` is there for that.

\[caption id="" align="alignnone" width="500"\]![ Git diff results ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1484669326501-23O8KWANNYLG3EWOX204/image-asset.jpeg?format=original) Git diff results \[/caption\]

No need to go to a separate application just to see what the changes are. You can just look at them in the command line.

* * *

With all this said, though, there is still a ton more commands and uses that Git has that I probably have never even heard of. Because of that, as y'all could probably guess, I got a book for it!

[![](//ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=111928497X&Format=_SL250_&ID=AsinImage&MarketPlace=US&ServiceVersion=20070822&WS=1&tag=dotnetmeditat-20)](https://www.amazon.com/Professional-Git-Brent-Laster/dp/111928497X/ref=as_li_ss_il?ie=UTF8&qid=1484659539&sr=8-1&keywords=professional+git&linkCode=li3&tag=dotnetmeditat-20&linkId=0e21f7d7477ba84a6a090f4f212a56a6)![](https://ir-na.amazon-adsystem.com/e/ir?t=dotnetmeditat-20&l=li3&o=1&a=111928497X)

I'll definitely be reviewing this after I go through it to let y'all know how it is overall. Just going through the table of contents indicates that it covers a lot of really useful commands.

What git commands can you not live without?
