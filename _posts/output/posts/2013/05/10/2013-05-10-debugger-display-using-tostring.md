---
title: "Debugger Display using ToString()"
date: 2013-05-10
categories: 
  - "development"
tags: 
  - "-net"
  - "debugging"
  - "development"
---

One of the things as a .NET developer that I tend to do most while debugging is to traverse through objects and their properties to see what values they have. Of course, the Base Class Library has something to help us do this even faster and more efficient – DebuggerDisplay attribute. Let’s say you’re tasked to help an insurance company write an updated application to determine the rate based off certain attributes on the car, such as age and number of collisions. It may look something like the following:

<script src="https://gist.github.com/jwood803/6329778.js?file=CarClass.cs"></script>

\[caption id="" align="alignnone" width="337"\]![ Showing regular object debugging. ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1377126049126-JOSSSM183KW3KJWCMLD6/image-asset.png?format=original) Showing regular object debugging. \[/caption\]

And while debugging you want to see the properties, Visual Studio will show you something similar to the below.  Not bad, but we can do a bit better with the DebuggerDisplay attribute. Let’s see how this gets used. With the same class definition that we had earlier, we just add the DebuggerDisplay attribute to the class. To access any of the properties, just enclose them in “{ }”.

<script src="https://gist.github.com/jwood803/6329778.js?file=DebuggerDisplay.cs"></script>

And our result is the below. Note that you can still expand out the object for other properties as well.

\[caption id="" align="alignnone" width="302"\]![ Debugging the object with DebuggerDisplay ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1377126049245-9B7C82TPDG7EBI0RV2G6/image-asset.png?format=original) Debugging the object with DebuggerDisplay \[/caption\]

Now, this alone can help out, but we can also run methods in the DebuggerDisplay attribute. Let’s override our ToString method of our class and use that. Note that the “nq” in the attribute is for “no quote” which will remove the quotes from the resulting string.

<script src="https://gist.github.com/jwood803/6329778.js?file=CarWithToString.cs"></script>

And, as you can see, we get the ToString() result in our debugger tool tip.

\[caption id="" align="alignnone" width="308"\]![ Debugging object using DebuggerDisplay with ToString() ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1377126049207-1X4SAC3J0S4GKRMKR4OJ/image-asset.png?format=original) Debugging object using DebuggerDisplay with ToString() \[/caption\]

Of course, there are other reasons why you would want to override the ToString method in your own classes, but hopefully now you know just one more way you can use it. Have fun debugging, everyone!
