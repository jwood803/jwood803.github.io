---
title: "Consuming the Jamendo API with AngularJS: Services"
date: 2014-02-25
categories: 
  - "development"
  - "non-net"
  - "client-side"
tags: 
  - "angularjs"
  - "javascript"
  - "jamendo"
  - "projects"
  - "development"
  - "programming"
---

It took a little while to get back to this little project. After watching some nice stuff from [NgConf](http://dotnetmeditations.com/blog/2014/1/18/as-a-virtual-attendee-at-ng-conf) the other week, it helped get me excited again for AngularJs. Plus, I was stuck on a problem for quite a while that I'll explain in more detail below that really slowed me down. After a bit of a break I got back to it and was able to figure it out.

### Services

Before we looked at how to use the [$http get method](http://dotnetmeditations.com/blog/175) from AngularJS, but all of that was within the Angular controller itself. To separate things out and to help us test easier I moved the $http calls into an Angular [service](http://docs.angularjs.org/guide/dev_guide.services.creating_services).

To recap, let's take a look at our controller currently:

<script data-preserve-html-node="true" src="https://gist.github.com/jwood803/6956281.js?file=AlbumController.js"></script>

As mentioned earlier, we have our $http call right inside of our controller. While that definitely works, it's better to separate that out into it's own service. Let's refactor this down a bit...

<script data-preserve-html-node="true" src="https://gist.github.com/jwood803/6956281.js?file=AlbumsController2.js"></script>

Now this looks a bit cleaner. We're first passing in the _albumsService_ as a dependency into the controller. Of course, if Angular can't find that the _albumsService_ is defined, it'll throw an error.

Next, we have the function definition of our _getAlbums_ method. In here we extract out the artist from the scope and then call the _getAlbums_ method on our service. In our call to the service we give it a function as a callback if the $http get was a success that we'll see later when we look at our service. This basically just gets the results and puts them into the scope.

Now let's take a look at our service.

<script data-preserve-html-node="true" src="https://gist.github.com/jwood803/6956281.js?file=albumsService.js"></script>

Pretty straight forward as it's basically the same as what we had in our controller earlier. We inject in the $http Angular object and we create our _getAlbums_ method while prepending it with _this_. 

When we call our success promise from the _get_ method, I just pass in the callback method that we defined in our controller earlier. This makes it a bit easier on our service since the callback method was updating our _$scope_ object and we don't have to worry about that here in our service.

I mentioned earlier that I had a bit of an issue that kept me from finishing this up. I kept getting an _$inject_ error on my service that indicated that it wasn't defined, but I couldn't figure out why that was. After some fiddling around with it, it turns out that I was trying to inject the _$scope_ object into the service and Angular doesn't like that. That's also a reason to use the callback.

### Conclusion

Moving the _$http_ get call to an Angular service is mostly just a small refactoring, but it's a good one to do. We will see later when we start to do testing how much easier it is to have that separated out.

Of course, this was just an introduction to Angular services so there's tons more to learn about them. I'll recommend [Dan Wahlin's](http://weblogs.asp.net/dwahlin/) [AngularJS in 60ish Minutes](http://www.youtube.com/watch?v=i9MHigUZKEM) to anyone. In fact, I've referenced it a few times to remind myself about services. There's also the WintellectNow video from [Jeremy Likness](http://csharperimage.jeremylikness.com/) on [AngularJS Services](http://www.wintellectnow.com/Videos/Watch/angularjs-services) that goes into much greater detail of services.

You can play with the [demo](http://htmlpreview.github.io/?https://github.com/jwood803/JamendoAngular/blob/master/index.html) and, as always, you can play around with the [code](https://github.com/jwood803/JamendoAngular) itself.
