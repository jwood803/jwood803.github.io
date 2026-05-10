---
title: "Beginning Ruby for C# Developers"
date: 2014-01-07
categories: 
  - "development"
  - "learning"
  - "technology"
  - "non-net"
tags: 
  - "ruby"
  - "development"
  - "learning"
  - "languages"
  - "programming"
---

Ruby has become quite popular as a programming language over the past few years. Probably mainly due to the popularity of the [Ruby on Rails](http://rubyonrails.org/) framework. Because of its popularity, it makes sense to learn at least the basics of it and what all it can do for you as a developer.

Until I have a better solution for embedding Ruby code and output, I'm just embedding the code and results as Github Markdown for now. Even though it doesn't seem to display properly in the embed, it does when viewing the actual gist on GitHub.

## Getting Started

One of the easiest ways to get started using Ruby is to just use your browser. [Repl.it](http://repl.it/languages/Ruby) is a site that supports many dynamic languages to be run in a browser and it's very handy to just play around in.

I've also seen people use [Sublime Text](http://www.sublimetext.com/3) to edit their Ruby code (I admit, I enjoy using it as well as a light weight IDE. I've had no problems using the beta of version 3, either). For Windows, you'd just need to [install](http://rubyinstaller.org/) Ruby. If you're on a Mac, you already have it installed. In fact, you can just type "ruby -v" in the terminal to see what version you have.

Since Ruby is a dynamic language, there is no compiling. However, everything in Ruby is considered an object, and because of that everything will include some built in functions with it. With everything in Ruby being an object isn't necessarily a new concept to C# developers since every object in C# derives from System.Object and you do get a few predefined functions, Ruby just seems to have gone that extra step. 

For example, say you want to run a statement three times in a row. How would you do that in Ruby? It's actually easier than just using a for-loop similar to what you would do in C# or even creating a way to use a lambda expression.

<script src="https://gist.github.com/jwood803/8290160.js?file=times.md"></script>

Pretty easy and very straight forward, especially for someone who may not even have a programming background.

Let's go on with a few other basics...

### Output

Ruby has a couple of ways to display output back to the console. The main way is to use the "print" statement, though you may also see the "puts" (put string) command as well. The only difference in these is the "puts" command will append a new line. You'll probably see these statements quite a bit, especially if you go through online tutorials.

<script src="https://gist.github.com/jwood803/8290160.js?file=output.md"></script>

### Methods

As mentioned earlier everything in Ruby is an object, and since that's true it can have methods on those objects. Of course, depending on the type Ruby recognizes it during runtime is what methods are available. For example, the "length" method can be used on strings but not on numbers.

<script src="https://gist.github.com/jwood803/8290160.js?file=length.md"></script>

Methods, like in C#, can also be chained together to give some added syntactic sugar and to reduce the overall code needed to accomplish what you need.

<script src="https://gist.github.com/jwood803/8290160.js?file=chaining.md"></script>

One final note about methods that got me confused early on but I thought was really nice is that there is a bit of a naming convention associated with certain methods. Mainly methods that change the object itself and methods that return a boolean.

Take the "[upcase](http://ruby-doc.org/core-2.1.0/String.html#method-i-upcase-21)" method we used earlier. There are actually two versions of this method, "upcase" and "upcase!". The first returns a _copy_ of the string being used, the second (indicated with an exclamation point) changes the instance of the string being used. In other words, the first version causes the string to be immutable.

For methods that return a boolean value a question mark is appended to the end of the method name to indicate to the developer that that method can be used for branching. Take the string's "[empty?](http://ruby-doc.org/core-2.1.0/String.html#method-i-empty-3F)" method, for example. As you can probably tell, it returns if the string is empty or not.

### Program Flow

Like most programming languages Ruby supports the if/else block. However, one noticeable difference is that Ruby also has an "[unless](http://ruby-doc.org/docs/keywords/1.9/Object.html#method-i-unless)"' keyword. It will only execute if the given conditional is false. This was included to help make certain branching more readable and a bit more intuitive.

So we can change the structure of an if/else to an unless block like so...

<script src="https://gist.github.com/jwood803/8290160.js?file=unless.md"></script>

## Further Learning

One of the best resources I can recommend to help get you on the path of learning Ruby much easier is [Code Wars](http://www.codewars.com/). This site has quite a few code [katas](http://dotnetmeditations.com/blog/2013/8/28/code-katas-can-be-fun) that will help in understanding the syntax and included methods that come in with Ruby.

[Codecademy](http://www.codecademy.com/) is recommended a lot, as well, by people and for very good reason...they have a lot of good interactive tutorials for learning Ruby.

## Conclusion

Of course, I definitely don't know everything about the language or all the tools the community uses. I still haven't gotten my head totally around gem files or rake. I'm not even totally sure of the correct structure of a Ruby application! But, hopefully this will at least spark some interest in you C# people to maybe try something different outside of the .NET world. There is plenty of fun to be had with it and I throughly enjoy learning the language and all it has to offer.
