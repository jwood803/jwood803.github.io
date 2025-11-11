---
title: "Learning Python Using IronPython"
date: 2013-08-21
categories: 
  - "development"
tags: 
  - "dynamic"
  - "iron-python"
  - "pythin"
---

Not too long ago when browsing the [Coursera](https://www.coursera.org/) courses I found an interesting [one](https://www.coursera.org/#course/programming1) about learning to program using the Python programming language. Once I saw this I jumped right on it and installed [IronPython](http://ironpython.net/), a .NET library that lets you use Python to interact with .NET applications. Unfortunately, I never really finished all the assignments. I got a bit stuck with converting a .NET List object to a Python List object. I believe I finally got the program to actually do the conversion, but my test would still fail for some reason (any insights on this would be awesome).

The code for IronPython that really does the work in my project is in the PythonEngine class.

<script src="https://gist.github.com/jwood803/6302038.js"></script>

As you can see, it’s taking in a parameter for the location of the Python script file for it to execute. From there the engine compiles the file and executes it. I get the variable (or method in this case) by the name and then I send it the list of parameters and invoke it getting back the results that are returned as a dynamic type.

One of the cool things you can do with IronPython and using the dynamic keyword is that, if your app has a different set of rules, you can create the rules within the Python script and use the C# to invoke them. This way only the Python script will need to be updated for the logic of the rules and no compiling of the C# is needed to execute the new rule set.

This course was a fairly easy introduction to the Python language and I believe it’ll be offered again fairly soon on the Coursera site, if you’re interested in taking it. Using it with IronPython also shows how powerful it can be in enterprise applications as well as how much power using the dynamic keyword can be.
