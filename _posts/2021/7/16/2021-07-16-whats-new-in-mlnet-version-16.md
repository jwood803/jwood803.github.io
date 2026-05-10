---
title: "What's New in ML.NET Version 1.6"
date: 2021-07-16
categories: 
  - "mlnet"
tags: 
  - "mlnet"
  - "mlnet-updates"
coverImage: "Copy+of+cinema+is+a+matter+of+what's+in+the+frame+and+what's+out+(4).png"
---

Another new release of [ML.NET](http://ML.NET) is now out! The [release notes](https://github.com/dotnet/machinelearning/blob/main/docs/release-notes/1.6.0/release-1.6.0.md) for version 1.6 has all the details, but this post will highlight all of the more interesting updates from this version. I'll also include the pull request for each item in case you want to see more details on it or learn how something was implemented.

There were a lot of things added to this release, but they did make a note that there are no breaking changes from everything that was added.

For the video version of this post, check below.

<iframe class="embedly-embed" src="//cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2FUJqEYGcwNhU%3Ffeature%3Doembed&amp;display_name=YouTube&amp;url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DUJqEYGcwNhU&amp;image=https%3A%2F%2Fi.ytimg.com%2Fvi%2FUJqEYGcwNhU%2Fhqdefault.jpg&amp;key=61d05c9d54e8455ea7a9677c366be814&amp;type=text%2Fhtml&amp;schema=youtube" width="854" height="480" scrolling="no" title="YouTube embed" frameborder="0" allow="autoplay; fullscreen" allowfullscreen="true"></iframe>

## Support for ARM

Perhaps the most exciting part of this update is the new [support for ARM architectures](https://github.com/dotnet/machinelearning/pull/5789). This will allow for most training and inference in [ML.NET](http://ML.NET).

Why is this update useful? Well, ARM architectures are almost everywhere. As mentioned in the [June update blog post](https://devblogs.microsoft.com/dotnet/ml-net-june-updates-model-builder/#ml-net-on-arm) this ARM architectures are included on mobile and embedded devices. This can open up a whole world of opportunities for ML.NET for mobile phones and IoT devices.

## DataFrame Updates

The [DataFrame API](https://www.nuget.org/packages/Microsoft.Data.Analysis/) is probably one of the more exciting packages that's currently in the early stages. Why? Well, .NET doesn't have much in terms of competing with pandas in Python for data analysis or data wrangling to handle some preprocessing that you may need before you send the data into [ML.NET](http://ML.NET) to make a model.

Why am I including DataFrame updates in a [ML.NET](http://ML.NET) update? Well, the DataFrame API has been [m](https://github.com/dotnet/machinelearning/pull/5641)[oved into the ML.NET repository](https://github.com/dotnet/machinelearning/pull/5641)! The code used to be in the [CoreFx Lab repository](https://github.com/dotnet/corefxlab) as an experimental package, but now it's no longer experimental and now part of [ML.NET](http://ML.NET). This is great news since it is planned to have many more updates to this API.

Other DataFrame updates include:

- [GroupBy operation extended](https://github.com/dotnet/machinelearning/pull/5821) - While the DataFrame API already had a GroupBy operation, this update adds new property groupings and makes it act more like LINQ's GroupBy operation.
- [Improved CSV parsing](https://github.com/dotnet/machinelearning/pull/5711) - Implemented the `TextFieldParser` that can be used when loading a CSV file. This allows the handling of quotes in columns.
- [Convert](https://github.com/dotnet/machinelearning/pull/5712)[`IDataView`](https://github.com/dotnet/machinelearning/pull/5712)[to](https://github.com/dotnet/machinelearning/pull/5712)[`DataFrame`](https://github.com/dotnet/machinelearning/pull/5712) - We've already had a way to convert a `DataFrame` object into an `IDataView` to be able to use data loaded with the DataFrame API into [ML.NET](http://ML.NET), but now we can do the opposite where we can load data in [ML.NET](http://ML.NET) and convert it into a DataFrame object to perform further analysis on it.
- [Improved DateTime parsing](https://github.com/dotnet/machinelearning/pull/5834) - This allows for better parsing of date time data.
- Improvements to the [Sort](https://github.com/dotnet/machinelearning/pull/5776) and [Merge](https://github.com/dotnet/machinelearning/pull/5778) methods - These updates allow for better handling of null fields when performing a sort or merge.

By the way, if you're looking for a way to help contribute to the [ML.NET](http://ML.NET) repository, helping with the DataFrame API is a great way to get involved. They have quite a few issues already that you can take a look at and help out with. It would be awesome if we got this package on par with pandas to help make C# a great ecosystem to perform data analysis.

You can use the [Microsoft.Data.Analysis label](https://github.com/dotnet/machinelearning/issues?q=is%3Aopen+is%3Aissue+label%3AMicrosoft.Data.Analysis) on the issues to filter them out so you can see what all they need help with.

## Code Enhancements

Quite a few of the enhancement updates were code quality updates. In fact, [feiyun0112](https://github.com/feiyun0112) did several pull requests that improved the code quality of the repo helping to make it easier for folks to read and maintain it.

## Miscellaneous Updates

There were also quite a lot of updates that didn't really tie in to a single theme. Here are some of the more interesting ones.

- [Saving Tensorflow models in the SavedModel format](https://github.com/dotnet/machinelearning/pull/5797) - Allows you to save Tensorflow models to use the [SavedModel format](https://www.tensorflow.org/guide/saved_model) instead of freezing the graph to save it.
- [Ability to specify a temp path](https://github.com/dotnet/machinelearning/pull/5782) - You can now specify the temp path location instead of it always going to the default location. This is specified in the `MLContext`.
- [Update LightGBM to version 2.3.1](https://github.com/dotnet/machinelearning/pull/5851) - Using this new version can give better results when using the LightGBM algorithms.
- [Label column name suggestions in AutoML](https://github.com/dotnet/machinelearning/pull/5624) - If you may have mistyped the label column name when using the AutoML API, this update will give suggestions for fixing it.
- Fixed [several CI issues](https://github.com/dotnet/machinelearning/blob/main/docs/release-notes/1.6.0/release-1.6.0.md#build--test-updates) - Some tests would sometimes fail in the CI builds so these updates fixed their stability so that you can have more confidence in your pull request.
- [Updated doc for cross compiling on ARM](https://github.com/dotnet/machinelearning/pull/5811) - Adds a docker image that can be used.
- [Updated contribution doc with help wanted tags](https://github.com/dotnet/machinelearning/pull/5815) - Helps direct anyone looking to contribute on where they can find issues.

These are just a few of the changes in this release. Version 1.6 has a lot of stuff in it so I encourage you to go through the [full release notes](https://github.com/dotnet/machinelearning/blob/main/docs/release-notes/1.6.0/release-1.6.0.md) to see all the items that I didn't include in this post.

* * *

What was your favorite update in this release? Was it ARM support or the new DataFrame enhancements? Let me know in the comments!
