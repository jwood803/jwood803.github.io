---
title: "What's New in the ML.NET CLI"
date: 2023-01-22
categories: 
  - "mlnet"
  - "tools"
tags: 
  - "mlnet-updates"
  - "mlnet"
  - "mlnet-cli"
---

The [ML.NET](http://ML.NET) CLI has gotten some interesting updates. This post will go over the main items that are new.

For a video version of this post, check below.

<iframe width="200" height="113" src="https://www.youtube.com/embed/ZApDu_Q_0-8?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen title="What's New in the ML.NET CLI"></iframe>

# New Install Name

The first thing to make note of is that there is a new name when installing the newer versions of the [ML.NET](http://ML.NET) CLI. Since the file size got too big for a single .NET tool, it is now split up into multiple installs depending on what operating system and CPU architecture you're running.

![](https://t1253902.p.clickup-attachments.com/t1253902/754024c3-082a-4088-bd67-22e82c56b0dc/image.png)

So getting the newest version will require a new install even if you have the older version installed. Actually, I would recommend to go ahead and uninstall the older version of the CLI if you already have it installed. This can be done with the `dotnet tool uninstall mlnet --global` command.

So depending on your machine is what you will install. I have a M1 MacBook Pro, so I would install the `mlnet-osx-arm` version. If you're on Windows, you will probably be installing the `mlnet-win-x64` version.

If you want to update a previously installed newer version, you can use the `dotnet tool update` command.

# Train with a `mbconfig` File

With the new CLI release, it comes with a couple of new command. The first we'll go over is the `train` command. This takes in a single required argument, which is a `mbconfig` file. This will use the information in the `mbconfig` file and will perform another training run.

This can be good for a few scenarios, including continuous integration where the `mbconfig` file is checked into version control and can be run each day to see if a new model can be discovered.

# Forecasting

Along with the `train` command a new scenario has been added - forecasting. Forecasting is primarily used for time series data to forecast values in the future. Similar to the other scenarios, we have a few arguments we can pass in.

![](https://t1253902.p.clickup-attachments.com/t1253902/cab9c525-5135-423c-9186-ea6ec313c31d/image.png)

The `dataset` and `label-col` arguments are similar to the other scenarios, but forecasting has a couple of others that are required - `horizon` and `time-col` .

The `horizon` argument is simply the number of items in the future you want the forecasting algorithm to predict.

The `time-col` argument is just the column that has the time or dates that the algorithm can use.

And we can run this like other scenarios with the below command. We'll let it run only for 10 seconds with the `--train-time` argument. The data can be found [here](https://github.com/rishabh89007/Time_Series_Datasets/blob/main/Wind%20Gen.csv) if you want to run it as well.

```
mlnet forecasting --dataset C:/dev/wind_gen.txt --horizon 3 --label-col 1 --time-col 0 --train-time 10
```

![](https://t1253902.p.clickup-attachments.com/t1253902/34d1edb2-7548-4e7f-84cf-a4e6de01c022/image.png)

* * *

A couple of big additions to the CLI and I'm sure more are coming. It is nice to see that the [ML.NET](http://ML.NET) team is continuing to keep the CLI's features on par with Model Builder.
