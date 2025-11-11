---
title: "Clustering in ML.NET"
date: 2019-03-09
categories: 
  - "machine-learning"
tags: 
  - "mlnet"
  - "machine-learning"
  - "clustering"
coverImage: "img.jpg"
---

Clustering is a well known type of unsupervised machine learning algorithm. It is unsupervised since there isn't usually a known label in the data to help the algorithm know how to train on a known value. Instead of training on the data point to see a pattern in how it got a label value, an unsupervised algorithm will find patterns among each of the data points themselves. In this post, I'll go over how to use the clustering trainer in ML.NET.

This example will be using ML.NET version 0.11. Sample code is on [GitHub](https://github.com/jwood803/MLNetExamples/tree/master/MLNetExamples/SeedClustering).

For a video version of this example, check out the video below.

<iframe src="//www.youtube.com/embed/NzkLDX5k_V0?start=1&amp;wmode=opaque&amp;enablejsapi=1" height="480" width="854" scrolling="no" frameborder="0" allowfullscreen></iframe>

## The Data

The data I'll be using is the [wheat seed](https://www.kaggle.com/dongeorge/seed-from-uci) data that can be found on Kaggle. This data has properties of wheat seeds such as area, perimeter, length and width of each seed, etc. These properties measure what variety of wheat the seed is. Whether it is the variety of Kama, Rosa, or Canadian.

## Project Setup

For the code, I'll create a new .NET Core Console project and bring in ML.NET as a NuGet package. For the data, I like to put them in the project itself so it can be easier to work with. When doing that, don't forget to mark the file to copy or copy if newer so it can be read when running the project.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1552157846567-CHFD1WADYQ30AEXOESJ2/2019-03-09+13_57_16-Clipboard.png?format=original)

## Loading Data

To start off, instantiate an instance of the `ML Context`.

```
var context = new MLContext();
```

To read in the data, use the `CreateTextLoader` method on the `context.Data` peroperty. This will take in an array of `TextLoader.Column` objects. In each of these object's constuctor pass in the name of the column, what data type it is which all of ours will be `DataKind.Single` to represent a float, and the position in the file where the column is. Then, as other parameters to the `CreateTextLoader` method, pass in that it has a header and that the separator is a comma.

```
var textLoader = context.Data.CreateTextLoader(new[]
{
    new TextLoader.Column("A", DataKind.Single, 0),
    new TextLoader.Column("P", DataKind.Single, 1),
    new TextLoader.Column("C", DataKind.Single, 2),
    new TextLoader.Column("LK", DataKind.Single, 3),
    new TextLoader.Column("WK", DataKind.Single, 4),
    new TextLoader.Column("A_Coef", DataKind.Single, 5),
    new TextLoader.Column("LKG", DataKind.Single, 6),
    new TextLoader.Column("Label", DataKind.Single, 7)
},
hasHeader: true,
separatorChar: ',');
```

With our data schema defined we can use it to load in the data. This is done by calling the `Load` method on the loader we just created above and pass in the file location.

```
IDataView data = textLoader.Load("./Seed_Data.csv");
```

Now that the data is loaded let's use it to get a training and test set. We can do that with the `context.Clustering.TrainTestSplit` method. All this takes in is the `IDataView` that we got when we loaded in the data. Optionally, we can specify what fraction of the data to get for our test set.

```
var trainTestData = context.Clustering.TrainTestSplit(data, testFraction: 0.2);
```

This returns an option that has `TrainSet` and `TestSet` properties.

## Building the Model

Now that the data is loaded and we have our train and test data sets, let's now create the pipeline. We can start simple by creating a features vector and then passing that into a clustering algorithm of our choosing. Since all of the data are float columns there's no need to do any other processing to it.

```
var pipeline = context.Transforms.Concatenate("Features", "A", "P", "C", "LK", "WK", "A_Coef", "LKG")
    .Append(context.Clustering.Trainers.KMeans(featureColumnName: "Features", clustersCount: 3));
```

Using the `context.Transforms` property we have access to several transformations we can perform on our data. The one we'll do here is the Concatenate transform. The first parameter is the name of the new column that it will create after concatenating the specified columns. The next parameter(s) are [`params`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/params) of all the columns to be concatenated.

Appended to the transform is the trainer, or algorithm, we want to use. In this case we'll use the [K-Means](https://en.wikipedia.org/wiki/K-means_clustering) algorithm. The parameters here are the column name of all the features, which we specified in the Concatenate transform as "Features". This is actually defaulted to "Features" so we don't need to specify it. We can also define the number of clusters the algorithm should try to create.

To get a preview of the data so far, we can call the `Preview` method on any instance of `IDataView`.

```
var preview = trainTestData.TrainSet.Preview();
```

To create the model, we simply just call the `Fit` method on the pipeline and pass in the training set.

```
var model = pipeline.Fit(trainTestData.TrainSet);
```

## Evaluating the Model

With a model built, we can now do a quick evaluation on the model. To do this for clustering, use the `context.Clustering.Evaluate` method. We can pass in the test data set. However, we would need to transform that data set similar to what we did in the pipeline. To do this we can use the `Transform` method on the model and pass in our test data set.

```
var predictions = model.Transform(trainTestData.TestSet);
```

Now we can use the test data set to evaluate the model and give some metrics.

```
var metrics = context.Clustering.Evaluate(predictions);
```

We get a few metrics for clustering on the `metrics` object but the one I'll care about is the average minimum score. This tells us the average distance from all examples to their center point of their cluster. So the lower the number here the better the clustering is.

```
Console.WriteLine($"Average minimum score: {metrics.AvgMinScore}");
```

## Predicting on Model

To make a prediction on our model we first need to create a prediction engine. To do that, call the `CreatePredictionEngine` method on the model. This is generic and it does specify the data input schema and the prediction classes so it knows what object to read in for a new prediction and what object to use when it makes a prediction.

```
public class SeedData
{
    public float A;
    public float P;
    public float C;
    public float LK;
    public float WK;
    public float A_Coef;
    public float LKG;
    public float Label;
}

public class SeedPrediction
{
    [ColumnName("PredictedLabel")]
    public uint SelectedClusterId;
    [ColumnName("Score")]
    public float[] Distance;
}
```

The `ColumnName` attribute tells the prediction engine what fields to use for those columns. This is under the `Microsoft.ML.Data` namespace.

To create the prediction engine, which used to be done using the `CreatePredictionFunction` method in previous versions of ML.NET, call it on the model and pass in the context as the parameter.

```
var predictionFunc = model.CreatePredictionEngine<SeedData, SeedPrediction>(context);
```

Now we can use the prediction engine to make predictions.

```
var prediction = predictionFunc.Predict(new SeedData
{
    A = 13.89F,
    P = 15.33F,
    C = 0.862F,
    LK = 5.42F,
    WK = 3.311F,
    A_Coef = 2.8F,
    LKG = 5
});
```

And we can get the selected cluster ID, or what cluster the model predicts the data would belong to.

```
Console.WriteLine($"Prediction - {prediction.SelectedClusterId}");
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1552160977816-9J7BDYM7W3YYOM4Z5NRZ/2019-03-09+14_49_30-Clipboard.png?format=original)
