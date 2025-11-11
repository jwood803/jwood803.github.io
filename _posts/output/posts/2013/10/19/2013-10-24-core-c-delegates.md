---
title: "Core C#: Delegates"
date: 2013-10-24
categories: 
  - "development"
tags: 
  - "net"
  - "delegates"
  - "development"
  - "c"
---

Microsoft has made it easier for aspiring developers to easily get started creating something when they came out with the C# language. However, there are definitely some advanced aspects of the language that take quite a bit longer than others to fully grasp. Delegates is one of those types of language features. Hopefully by the end of this article you'll have a deeper understanding of how utilize delegates in your C# code. 

I, myself, had trouble trying to fully understand what the purpose of delegates were. If you were to ask me before what a delegate was, I would just give a small answer indicating that it's similar to a method template. While that's true, it does quite a bit more.

The simplest way I know to go over delegates is to just go through a small example to allow you to also do the same so you can use that to experiment around with the code.

<script src="https://gist.github.com/jwood803/7129103.js?file=DelegateKeyword.cs"></script>

Let's go through this a bit. The first line is where you actually declare the delegate and give it a return type, name, and any parameters if it needs any. In our _Main_ method is where we'll initiate a couple of variables to hold the delegates. The first one, _myDelegate_ , does so using the method syntax. Our second variable, however, we declare it using an anonymous delegate, or a delegate without a function name.

After we've declared the our variables with our delegate, we can then invoke our delegate by calling it just like a method just like in lines 15 and 16 above. We declared our delegate to take in a single string parameter, so that's how we'll invoke it. 

### Built in Delegate Types 

The folks at Microsoft were kind enough to give us a few built in delegates that we can also use within our code. Let's take a quick look at them. 

<script src="https://gist.github.com/jwood803/7129103.js?file=BuiltInDelegates.cs"></script>

The first one, which may be the most commonly used, especially if you've messed quite a bit with the LINQ method syntax, is the _Func_  delegate type. This one will take in zero or more parameters and will return something back to the caller. In the example above I have one parameter of a string and I return back an integer.

The second one is the _Action_ delegate type. Like the _Func_ type it takes in zero or more parameters, however it returns void (returns nothing).

The third one, _Predicate_, is a bit closer to the _Func_ type where it takes in parameters and returns. The difference with this is that it will always return a boolean true or false back to the caller.  

### Invocation Lists

One of the magic things that delegates have is they keep track of all invocations of it. When we instantiate our delegates we are actually adding to the invocation list. Another way to do this is to simply add a delegate to another delegate. 

<script src="https://gist.github.com/jwood803/7129103.js?file=DelegateInvocation.cs"></script>

We'll see the true power of delegates and their invocation lists in the next part, when we talk about events.
