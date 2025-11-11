---
title: "YUI: The Other JavaScript Framework"
date: 2013-09-10
categories: 
  - "development"
tags: 
  - "yui"
  - "javascript"
  - "framework"
---

When you think of JavaScript frameworks, what pops into your mind? JQuery, KnockoutJS, or EmberJS? Did you know that Yahoo! has one of their own? It's [YUI](http://yuilibrary.com/)  and it's actually not too bad of a framework. 

It's definitely not what you'd think of when creating a new web application. For any kind of DOM manipulation, I'm sure the first thought will be to use jQuery. I wouldn't exactly count out Yahoo!, though. They have some of the top engineers of the country and they released YUI as an [open project](https://github.com/yui/yui3) for anyone to fork and contribute to.

Below is a very small example of using YUI. How easily can you read through it? 

<iframe src="http://embed.plnkr.co/1BHCphu3zq7kscZeAAxV/script.js" style="height: 250px; width: 100%;"></iframe>

Fairly simple, right?  You get one instance of the element with the "btn1" ID and on the click event run the function that has the alert.

Let's look at a more complex example of YUI in action. We're going to make a JSONP call to the GitHub API. Let's take a look.

<iframe src="http://embed.plnkr.co/kutQoPXhTNxWo1Nv1khL/script.js" style="height: 500px; width: 100%;"></iframe>

Not too much more here than in our first example. I'm still using the selector and click event handler as we've seen but in the click handler of the button I make a JSONP call to the GitHub API. The real magic happens during the function that handles the response from the JSONP call. The _Y.Lang.sub_  call takes in an HTML template and uses the data from the response to bind the items in the brackets. Of course, the names have to match what you get back in the response.

### Conclusion

These are just very small examples of what you can do with YUI. The [documentation](http://yuilibrary.com/) contains a ton more examples and functionality of what the framework is capable of.

I do believe this is easier to maintain and use than using jQuery. I do have to say, though, since I've found AngularJS, I believe I'll be using that more and more. If I couldn't use that, however, YUI would be my framework of choice.
