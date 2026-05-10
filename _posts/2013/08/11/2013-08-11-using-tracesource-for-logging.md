---
title: "Using TraceSource for Logging"
date: 2013-08-11
categories: 
  - "debugging"
tags: 
  - "-net"
  - "debugging"
  - "logging"
  - "tracesource"
---

How often have you been in a project and you wanted to create a simple log file to log out errors and, if debugging an annoying error, parameter and variable values? I've wanted to do this a few times but there are a handful of references out there on how best to use TraceSource for logging. Of course you have your best resources of [MSDN](http://msdn.microsoft.com/en-us/library/ms228984.aspx) and [StackOverflow](http://stackoverflow.com/questions/4376699/how-to-use-tracesource-across-classes), but I figured a bit more of an understandable example would be most helpful to everyone looking on how to easily implement this to their projects.

To get started in the simplest way, all you need to do is to update the config file to include the System.Diagnostics section and then update the code you want to log out the information that you want. Below is a similar example to what is shown on the previously mentioned MSDN article.

<script src="https://gist.github.com/jwood803/6329425.js?file=TraceSourceConfig.xml"></script>

You may have noticed that I'm calling the Flush() and Close() methods on the TraceSource object. This is done to make sure the file gets written to and is closed properly. If you aren't getting everything written to your listener, these calls may be all that's needed. However, I've seen cases where it's not always required.

<script src="https://gist.github.com/jwood803/6329425.js?file=TraceSourceLogging.cs"></script>

TraceSource can be used across layers of the application as well and you can create a source for each of the layers to do a bit of a separated logging. So if you have a wrapper class you wouldn't want to log as much as you might a business class that has all your business logic.

Of course, if you push to production and don't want all this logging until you need it, you can turn it off per the switch inside of the specific source.

<script src="https://gist.github.com/jwood803/6329425.js?file=SetTraceSourceConfigOff.xml"></script>

To show how it's used for different classes, I'll use a standard Person/Employee class hierarchy. So below we have our two new classes:

<script src="https://gist.github.com/jwood803/6329425.js?file=CustomLoggingWithTraceSource.cs"></script>

Fairly basic classes here with some similarly basic methods. As you can tell with the methods, I’m logging some fairly verbose information out. So if there any problems with certain data, I can just look at the logs to see what the methods are using and their return value. This can be crucial in debugging a certain item when you have a decent sized data set. If this wasn’t available you’d then have to go through each item in the data set by hand.

To separate out the log for each class or even a whole layer in your app, just create a new source for each layer you want to be logged separately. I’m using the same listener so they’ll all go to the same place, but you’re free to have each source have it’s own listener.

<script src="https://gist.github.com/jwood803/6329425.js?file=MultipleTraceSourceConfig.xml"></script>

And now you have your information logged: 

This is just a basic use of TraceSource for logging information. I used a file to log out because I feel that it’s fairly easy to use and you have one location for all your log statements. If you’re used to log4net with the different additional appenders (listeners in terms of using TraceSource), then the [Essential Diagnostics](http://essentialdiagnostics.codeplex.com/) library will be a great addition to the project to further enhance using TraceSource for logging.

The full source for this example can be viewed on GitHub – [TraceSourceLogging](https://github.com/Chrono803/TraceSourceLogging).
