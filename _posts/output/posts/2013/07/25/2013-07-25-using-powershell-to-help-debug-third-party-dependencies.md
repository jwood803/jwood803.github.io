---
title: "Using PowerShell to Help Debug Third Party Dependencies"
date: 2013-07-25
categories: 
  - "development"
tags: 
  - "debugging"
  - "powershell"
---

I hate when an application has a third party dependency (web service, url, ftp, etc.) mainly because it’s hard to test or even see what data you’re getting back. However, this is one awesome reason to love PowerShell. PowerShell can be used to make those calls manually so you can see what comes back and, if needed, use that data further while doing tests. For example, you have a call that gives XML has a response and use PowerShell to traverse the response using the new Invoke-WebRequest cmdlet in PowerShell v3. I’ll use the MSDN blog RSS feed as a small example here.

<script src="https://gist.github.com/jwood803/6329806.js?file=ThirdPartyCall.ps1"></script>

However, PowerShell makes it easier for us to traverse the XML by putting all the nodes into properties. For that, we need to put the results into an XML typed variable.

<script src="https://gist.github.com/jwood803/6329806.js?file=UsingWebCallResults.ps1"></script>

And with the above we get these results:

\[caption id="" align="alignnone" width="1033"\]![ image ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1377126049231-H8ZNGYTDH939Q2CLZD7X/image_thumb.png?format=original) image \[/caption\]

And from there you can use the results anyway you need to to help test and/or debug that part of your application. This recently just helped me out of a bit of a bind.
