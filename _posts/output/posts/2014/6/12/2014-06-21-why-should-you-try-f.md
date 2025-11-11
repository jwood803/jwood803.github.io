---
title: "Why F# for the Enterprise"
date: 2014-06-21
categories: 
  - "development"
tags: 
  - "f"
  - "net"
  - "functional"
  - "enterprise"
---

I have gotten bit by the F# bug lately. After helping to release the [F# portion of Exercism](http://dotnetmeditations.com/blog/2014/6/9/exercism-f-track-now-available) I've started to enjoy more and more of what F# has to offer. Of course, F# is a sort of "hybrid" language where you _can_ do object oriented and procedural programming if you make it that way, but out of the box it's trying to get you to go more functional.

Several folks have [posted](http://sergeytihon.wordpress.com/2013/03/24/why-f-by-f-weekly/) as to why learn F# in the first place, but I figured I'd give some reasons why I think F# would be great for scenarios inside enterprise applications. While a few what others have mentioned as good reasons to learn F# in the first place, I believe these would also be the top reasons for enterprise development.

### What's wrong with the current enterprise solutions?

Well, nothing, really. Of course, just because one solution works doesn't mean it's the most productive or easiest to maintain. For example, CSS precompilers like SASS and LESS do this with plain CSS stylesheets.

Why not use something that makes our lives, as developers, easier and helps to bring back the fun in developing again?

## No More NullReferenceExceptions

<blockquote data-preserve-html-node="true" class="twitter-tweet" data-conversation="none" lang="en"><p data-preserve-html-node="true">&gt; <a data-preserve-html-node="true" href="https://twitter.com/davetchepak">@davetchepak</a> "What can C# do that F# cannot?" NullReferenceException :-)</p>— Tomas Petricek (@tomaspetricek) <a data-preserve-html-node="true" href="https://twitter.com/tomaspetricek/statuses/314872258839601152">March 21, 2013</a></blockquote>

<script data-preserve-html-node="true" async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

While the above always tends to make me laugh, I'm quite sure each and every developer has fixed a production defect that involved this error. 

In F# there are is no thought of "null". You'll even get a compiler error if you try to use "null" in a pure F# program. For example, take a peek at the below code:

<script data-preserve-html-node="true" src="https://gist.github.com/jwood803/6091c0ef9d85503a9d27.js?file=FSharpRecordType.fs"></script>

We're just defining some record types - Person and Employee - and let binding the Employee record with some values. Now, what happens if I set Person to null?

<script data-preserve-html-node="true" src="https://gist.github.com/jwood803/6091c0ef9d85503a9d27.js?file=FSharpRecordTypeNullReference.fs"></script>

The compiler won't even let us try that...

\[caption id="" align="alignnone" width="500"\]![ Trying to assign null to a record type. ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1403310570555-OEY4ZOWZIM0XPKS6P3TM/image-asset.png?format=original) Trying to assign null to a record type. \[/caption\]

So this shows that, in pure F# code, the compiler won't allow us to set records as null which greatly reduces, perhaps even eliminates, NullReferenceExceptions.

### What if we still need null?

There are some exceptions where F# will allow you to use null. Both of which involve using the .NET libraries.

- As a parameter when calling one of the .NET libraries.
- Handling if a call to a .NET library returns null.

Even using null in these scenarios can greatly reduce the NullReferenceException occurring in your application. To me, that's just one less thing I need to worry about.

### But I still need it in my F# code

No worries! The language has you covered on that aspect as well. Allow me to introduce the [option type](http://msdn.microsoft.com/en-us/library/dd233245.aspx). This is used if a value might not exist.

Let's take a small example of a bank account.

<script data-preserve-html-node="true" src="https://gist.github.com/jwood803/6091c0ef9d85503a9d27.js?file=FSharpBankAccount.fs"></script>

If the account isn't open yet or has already been closed, we don't expect a value. Otherwise, we do expect some value.

### None vs null

How is this different than assigning to null? Remember that None is an option _type._ Because of that, the compiler will help us out if we may have missed anything. For example, in F# you can do [pattern matching](http://msdn.microsoft.com/en-us/library/dd547125.aspx). Let's see how we can use pattern matching with the option type.

<script data-preserve-html-node="true" src="https://gist.github.com/jwood803/6091c0ef9d85503a9d27.js?file=FSharpPatternMatchingOptions.fs"></script>

Good to go there. But, what if we happened to miss a match on None? We'll get this warning:

\[caption id="" align="alignnone" width="1554"\]![ Pattern match warning. ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1403372849471-L8WTM0WNKMKXQC6XD7HO/image-asset.jpeg?format=original) Pattern match warning. \[/caption\]

## Units of Measure

[Units of Measure](http://msdn.microsoft.com/en-us/library/dd233243.aspx) allow you to explicitly type a certain unit. F# does have predefined unit names and unit symbols in the `Microsoft.FSharp.Data.UnitSystems.SI.UnitNames` and `Microsoft.FSharp.Data.UnitSystems.SI.UnitSymbols` namespaces.

For example, say we wanted to use meters. Instead of assuming our application is using meters throughout all the calculation, which has a probability of being converted to another unit but still being represented as meter, we would just need to do the following:

<script data-preserve-html-node="true" src="https://gist.github.com/jwood803/6091c0ef9d85503a9d27.js?file=FSharpUnitOfMeasure.fs"></script>

Microsoft has [included a few units of measure](http://msdn.microsoft.com/en-us/library/hh289721.aspx) for us, as mentioned earlier, but we can definitely create our own. Say we had an application that dealt with different types of world currencies. We can easily define them as units of measure.

<script data-preserve-html-node="true" src="https://gist.github.com/jwood803/6091c0ef9d85503a9d27.js?file=FSharpCustomUnitOfMeasure.fs"></script>

Now, what does this do for us exactly? Let's try to convert how many euros are in a dollar amount.

<script data-preserve-html-node="true" src="https://gist.github.com/jwood803/6091c0ef9d85503a9d27.js?file=FSharpUnitOfMeasureError.fs"></script>

The first method we defined works just fine since we're using the units of measure. However, the second method generates a compile error since it can't infer what type "0.74" should be.

Since a unit of measure is typed, it will generate compile errors and reduce the amount of defects your application can cause due to a mismatch of units. This type of error [does happen](http://en.wikipedia.org/wiki/Mars_Climate_Orbiter#Cause_of_failure) in real world applications...even to NASA engineers.

## Type Providers

[Type providers](http://msdn.microsoft.com/en-us/library/hh156509.aspx) in F# is essentially a way to access all sorts of data. There are [quite a few of them](http://blogs.msdn.com/b/dsyme/archive/2013/01/30/twelve-type-providers-in-pictures.aspx) all ready for you to use now.

Just the SQL type providers alone are worth it for enterprise applications. Even better, you can even use them to access [Entity Framework](http://msdn.microsoft.com/en-us/library/hh361035.aspx) and [LINQ to SQL](http://msdn.microsoft.com/en-us/library/hh361039.aspx).

Another important one that may be of great use to enterprise applications is the [JSON type provider](http://fsharp.github.io/FSharp.Data/library/JsonProvider.html). 

Type providers provide an easy way to consume data within F# and, if you'd like, use C# to handle that data.

The above are just a few reasons of why I believe F# to be of great use to developing enterprise applications. Using the above, the risks of error would be greatly reduced and productivity would be increased. I know I'll be doing more F# in my applications from now on and I can only see it getting even better in the future as this language evolves.
