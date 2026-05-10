---
title: "Small Introduction to AngularJS"
date: 2013-09-01
categories: 
  - "development"
tags: 
  - "angularjs"
  - "javascript"
---

I know what you're probably thinking, "Oh, another JavaScript library/framework for me to learn." Well, yes, that's correct. However, I believe this is one worth learning. 

Why this one over the many other ones available to you? AngularJS has a different approach. Instead of providing a library, it's goal is to make HTML dynamic like most modern web sites are instead of being the old, boring static HTML we're all used to.

Of course, when you first see AngularJS in action, it can look pretty weird. That can be said of a lot of libraries, frameworks, or technologies until you get used to it. I was the same thing when I first started messing with lambdas in .NET.

Below is a small example for AngularJS mostly showing controllers and some basic data binding.

<script src="https://gist.github.com/jwood803/6405598.js?file=Index.html"></script>

At first, you may notice all the _ng-\*_ attributes in the HTML. This is all AngularJS and, as mentioned above, it extends HTML to be more dynamic. The double curly braces ( {{ }} ) tell Angular that it's a two way binding. Inside the braces is the name on the model to use. You can also tell that most of the functions and properties are in the EmployeeController JavaScript file, which we'll take a look at next.

<script src="https://gist.github.com/jwood803/6405598.js?file=EmployeeController.js"></script>

The first line tells what application Angular will be using. This is the same as the _ng-app_  attribute in the HTML. The empty array will be for any dependencies we may need, such as additional JavaScript files. The next section is where we're defining the actual EmployeeController, which sets our initial Employee object and defines the add and remove methods. These methods get called in their appropriate places inside the HTML. You may notice we also pass in the _$scope_ object into our controller function. This is mainly for AngularJS and can be used for dependency injection later on, if you need it.

Below is the full demo in action. Note that JSFiddle automatically puts in the _html_ tag, so I had to wrap the _body_ tag with the _ng-app_ attribute for Angular to work correctly.

<iframe width="100%" height="300" src="http://jsfiddle.net/jwood803/WGsey/5/embedded/js,html,result" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### Additional Resources

The one tutorial that got me started on the path of Angular is [Dan Wahlin's](http://weblogs.asp.net/dwahlin/) [AngularJS in 60ish Minutes](http://youtu.be/i9MHigUZKEM).

Another good tutorial is the one from [Thinkster](http://www.thinkster.io/).  They combine a lot of the articles and tutorials from all over and give them all to you piece by piece along with their own videos.

### Conclusion

This was just a _very_  small introduction to AngularJS. While there are tons of other things you can do, this should be good to get something up and going. Once a little bit is understood, the tutorials and all will make so much more sense and you will be able to learn a bit more than you did before. Hope you enjoy learning AngularJS as much as I have!
