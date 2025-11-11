---
title: "Introduction to Paket"
date: 2014-10-22
categories: 
  - "f"
  - "technology"
  - "development"
tags: 
  - "paket"
  - "f"
  - "tools"
  - "introduction"
---

[Paket](http://fsprojects.github.io/Paket/) has been quite the talk lately in the .NET community, and for very good reason. It is essentially another way to manage dependencies in your .NET projects.

#### But why replace NuGet?

Paket is basically a NuGet replacement so I'm sure you're wondering, "[why would we need to replace NuGet?](http://fsprojects.github.io/Paket/faq.html#I-don-t-understand-why-I-need-Paket-to-manage-my-packages-Why-can-t-I-just-use-NuGet)" Paket essentially just takes the functionality of NuGet and adds some extra nice features.

For one thing, Paket makes it able for you to control exactly what's happening with your package dependencies. No more conflict between different packages if those packages reference different versions of the same dependent package.

Another really cool thing Paket does is that it can reference a [single file from GitHub](http://fsprojects.github.io/Paket/github-dependencies.html). How many times have you needed that and just wound up downloading what you needed and using it that way? If a new version of that file comes along, you'll have to repeat that process.

#### But I'm already using NuGet

No problem at all! Paket has a nifty [`convert-from-nuget`](http://fsprojects.github.io/Paket/convert-from-nuget.html) command to get you up and going.

#### I'm hooked...but how do I get started?

First, you need to include a [`.paket`](https://github.com/jwood803/Demos/tree/master/Demos/PaketDemo/.paket) folder in the root of your solution. This will include `paket.exe` that will be used to install and restore packages.

Once that folder and its contents are there, you'll need to create a [`paket.dependencies`](http://fsprojects.github.io/Paket/dependencies-file.html) file in the root directory of your solution. This file will be similar to the following:

`source http://nuget.org/api/v2   nuget FSharp.Data   nuget FAKE`

This file tells Paket what the sources are (NuGet or GitHub) and the package/file names so it can be downloaded.

You can then use a [`build.cmd`](https://github.com/jwood803/Demos/blob/master/Demos/PaketDemo/build.cmd) file or manually call `paket.exe` like below.

`\.paket\paket.exe install`

This will create a `packages` folder that will include all the libraries.

From here you can always manually reference the libraries that you want, but Paket makes this easy as well. In each of the folders where you have a project file, create a [`paket.references`](http://fsprojects.github.io/Paket/references-files.html) file that contain the names of each library you want to be referenced, like below.

`FSharp.Data`

> Note that `FAKE` isn't in the file since it won't get referenced. The `paket.references` file will only add to the project if the library is in a `lib` folder. `FAKE` is in a `tools` folder. This isn't a problem since it can be referenced manually in the `build.fsx` file.

To get Paket to use the references file, simply rerun the install command with the `--hard` switch.

`\.paket\paket.exe install --hard`

This will look at the `paket.references` file and use that to automatically reference the project with the appropriate libraries.

After that, you're good to get started on your project.

#### Conclusion

Hopefully this walkthrough will help you get started with using Paket to make your package management easier than before. This is still a young and very active project so I wouldn't be surprised if there are tons of things that this can do for all of our .NET projects.
