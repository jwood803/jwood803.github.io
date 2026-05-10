---
title: "ML.NET End-to-End: Build Model from Database Data"
date: 2019-03-27
categories: 
  - "machine-learning"
  - "mlnet"
tags: 
  - "mlnet"
  - "machine-learning"
  - "regression"
coverImage: "img.jpg"
---

When doing machine learning on your own data instead of data downloaded from the internet, you'll often have it stored on a database. In this post, I'll show how to use an Azure SQL database to write and read data then use that data to build an ML.NET machine learning model. I'll also show how to save the model into an Azure Blob Storage container so other applications can use it.

The code can be found on [GitHub](https://github.com/jwood803/MLNet-EndToEnd/blob/master/WineRegressionModel/Program.cs). For a video going over the code, check below.

<iframe src="//www.youtube.com/embed/VVekyUMEKsI?wmode=opaque&amp;enablejsapi=1" height="480" width="854" scrolling="no" frameborder="0" allowfullscreen></iframe>

## The Data

The data used will be the [wine quality data](https://www.kaggle.com/rajyellow46/wine-quality) that's on Kaggle. This has several qualities of wine such as its Ph, sugar content, and whether the wine is red or white.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553615980911-CCQ2N2I0KQPZ4CHT3V7L/2019-03-26+11_57_48-Clipboard.png?format=original)

The label column will be "Quality". So we will use the other characteristics of wine to predict this value, which will be from 1 to 10.

# Setup

## Creating Azure Resources

For a database and a place to store the model file for other code, such as an API, can read from, we'll be using Azure.

### Azure SQL Database

To create the SQL database, in the Azure Portal, click `New Resource -> Databases -> SQL Database`.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553616837956-WN80IQJ7NRCNL2J5RMH2/2019-03-26+12_09_04-Clipboard.png?format=original)

In the new page, fill in the required information. If creating a new SQL Server to hold the database into, keep track of the username and password you use to fill it out as it will be needed to connect to it later. Click `Review + Create` and, if validations pass, click `Create`.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553617022632-O33L292RIVICCAX5LZBR/2019-03-26+12_16_35-Clipboard.png?format=original)

### Azure Blob Storage

While the SQL Server and database are being deployed, click on `New Resource -> Storage -> Storage Account`

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553617383504-R5QAPU3IWOIBHHQNGV94/2019-03-26+12_20_02-Clipboard.png?format=original)

Similar to when creating the SQL database, fill in the required items. For a blob container, make sure the `Account kind` is set to `Container`.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553619116606-8X1S460A8IQWWH4NJCES/2019-03-26+12_51_21-Clipboard.png?format=original)

### Creating Database Table

Before we can start writing to the database, the table we'll be writting to needs to be created. You can use Azure Data Studio, which is a light weight version of SQL Server Management Studio, to connect to the database earlier using the user and password and then use the below script to create the table. The script just has columns corresponding to the columns in the data file with an added ID primary key column.

```
CREATE TABLE dbo.WineData
(
    ID int NOT NULL IDENTITY,
    Type varchar(10) not null,
    FixedAcidity FLOAT not null,
    VolatileAcidity FLOAT not null,
    CitricAcid FLOAT not null,
    ResidualSugar FLOAT not null,
    Chlorides FLOAT not null,
    FreeSulferDioxide FLOAT not null,
    TotalSulfurDioxide FLOAT not null,
    Density FLOAT not null,
    Ph FLOAT not null,
    Sulphates FLOAT not null,
    Alcohol FLOAT not null,
    Quality FLOAT not null
)
```

# Code

The code will be done in a .NET Core Console project in Visual Studio 2017.

## NuGet Packages

Before we can really get started the following NuGet packages need to be installed:

- Microsoft.ML
- Microsoft.Extensions.Configuration
- Microsoft.Extensions.Configuration.Json
- System.Data.SqlClient
- WindowsAzure.Storage

## Config File

In order to use the database and Azure Blob Storage we just created, we will have a config JSON file instead of hard coding our connection strings. The config file will look like this:

```
{
  "sqlConnectionString": "<SQL Connection String>",
  "blobConnectionString": "<Blob Connection String>"
}
```

Also, don't forget to mark this file to copy in its properties.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553679359361-SO9DGUN5DR9KO3I0AZNQ/2019-03-27+05_33_19-Clipboard.png?format=original)

The connection strings can be obtained on the resources in the Azure Portal. For the SQL database connection string, go to the `Connection strings` section and you can copy the connection string from there.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553679604299-JRAFUJC5Q4ITPH3Y6M8C/2019-03-27+05_39_25-Clipboard.png?format=original)

You will need to update the connection string with the username and password that you used when creating the SQL server.

For the Azure Blob Storage connection string, go to the `Access keys` section and there will be a key that can be used to connect to the storage account and under that will be the connection string.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553679968417-TLGR01VUKEAFZIS0WY81/2019-03-27+05_44_49-Clipboard.png?format=original)

## Writing to Database

Since we just created the SQL server, database, and table, we need to add the data to it. Since we have the `System.Data.SqlClient` package, we can use `SqlConnection` to connect to the database. Note that, an ORM like Entity Framework can be used instead of the methods from the `System.Data.SqlClient` package.

Real quick, though, let's set up by adding a couple of fields on the class. One to hold the SQL connection string and another to have a constant string of the file name for the model we will create.

```
private static string _sqlConectionString;
private static readonly string fileName = "wine.zip";
```

Next, let's use the `ConfigurationBuilder` to build the configuration object and to allow us to read in the config file values.

```
var builder = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json");

var configuration = builder.Build();
```

With the configuration built, we can use it to pull out the SQL connection string and assign it to the field created earlier.

```
_sqlConectionString = configuration["connectionString"];
```

To write the data into the database we need to read in from the file. I put the file in the solution and make sure it can be read using the same method as the config file to make sure it gets copied over.

Using LINQ, we can read from the file and parse out each of the columns into a `WineData` object.

```
var items = File.ReadAllLines("./winequality.csv")
    .Skip(1)
    .Select(line => line.Split(","))
    .Select(i => new WineData
    {
        Type = i[0],
        FixedAcidity = Parse(i[1]),
        VolatileAcidity = Parse(i[2]),
        CitricAcid = Parse(i[3]),
        ResidualSugar = Parse(i[4]),
        Chlorides = Parse(i[5]),
        FreeSulfurDioxide = Parse(i[6]),
        TotalSulfurDioxide = Parse(i[7]),
        Density = Parse(i[8]),
        Ph = Parse(i[9]),
        Sulphates = Parse(i[10]),
        Alcohol = Parse(i[11]),
        Quality = Parse(i[12])
    });
```

The `WineData` class holds all of the fields that are in the data file.

```
public class WineData
{
    public string Type;
    public float FixedAcidity;
    public float VolatileAcidity;
    public float CitricAcid;
    public float ResidualSugar;
    public float Chlorides;
    public float FreeSulfurDioxide;
    public float TotalSulfurDioxide;
    public float Density;
    public float Ph;
    public float Sulphates;
    public float Alcohol;
    public float Quality;
}
```

There's an additional `Parse` method added to all but one of the fields. That's due to us getting back a string value of the data, but our class says it should be of type `float`. The `Parse` method is fairly straight forward in that it just tries to parse out the field and if it can't it uses the default value of `float`, which is 0.0.

```
private static float Parse(string value)
{
    return float.TryParse(value, out float parsedValue) ? parsedValue : default(float);
}
```

Now that we have the data, we can save it to the database. In a `using` statement, new up an instance of `SqlConnection` and pass in the connection string as the parameter. Inside here, we need to call the `Open` method of the connection and then create an insert statment. Then, loop over each item from the results of reading in the file and add each field from the item as parameters for the insert statement. After that, call the `ExecuteNonQuery` method to execute the query on the database.

```
using (var connection = new SqlConnection(_sqlConectionString))
{
    connection.Open();

    var insertCommand = @"INSERT INTO dbo.WineData VALUES
        (@type, @fixedAcidity, @volatileAcidity, @citricAcid, @residualSugar, @chlorides,
         @freeSulfureDioxide, @totalSulfurDioxide, @density, @ph, @sulphates, @alcohol, @quality);";

    foreach (var item in items)
    {
        var command = new SqlCommand(insertCommand, connection);

        command.Parameters.AddWithValue("@type", item.Type);
        command.Parameters.AddWithValue("@fixedAcidity", item.FixedAcidity);
        command.Parameters.AddWithValue("@volatileAcidity", item.VolatileAcidity);
        command.Parameters.AddWithValue("@citricAcid", item.CitricAcid);
        command.Parameters.AddWithValue("@residualSugar", item.ResidualSugar);
        command.Parameters.AddWithValue("@chlorides", item.Chlorides);
        command.Parameters.AddWithValue("@freeSulfureDioxide", item.FreeSulfurDioxide);
        command.Parameters.AddWithValue("@totalSulfurDioxide", item.TotalSulfurDioxide);
        command.Parameters.AddWithValue("@density", item.Density);
        command.Parameters.AddWithValue("@ph", item.Ph);
        command.Parameters.AddWithValue("@sulphates", item.Sulphates);
        command.Parameters.AddWithValue("@alcohol", item.Alcohol);
        command.Parameters.AddWithValue("@quality", item.Quality);

        command.ExecuteNonQuery();
    }
}
```

We can run this and check the database to make sure the data got added.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553681623415-3Y8FAE94TA9A4LSNFBLO/2019-03-27+06_13_22-Clipboard.png?format=original)

### Reading from Database

Now that we have data in our database let's read from it. This code will be similar than what we used to write to the database by using the `SqlConnection` class again. In fact, the only differences is the query we send to it and how we read in the data.

We do need a variable to add each row to, though, so we can create a new `List` of `WineData` objects.

```
var data = new List<WineData>();
```

Within the `SqlConnection` we can create a select statement that will return all of the columns and execute it with the `ExecuteReader` function. This returns a `SqlDataReader` object and we can use that to extract out the data.

In a `while` loop, which checks that the reader can read the next row, use the `List` variable created earlier to add a new instance of the `WineData` object to it and we can map from the reader to the object using the `reader.GetValue` method. The `GetValue` parameter will be the column position and then we'll do a `ToString` on it. Note that we need the `Parse` method from above again here to parse the strings into a `float`.

```
using (var conn = new SqlConnection(_sqlConectionString))
{
    conn.Open();

    var selectCmd = "SELECT * FROM dbo.WineData";

    var sqlCommand = new SqlCommand(selectCmd, conn);

    var reader = sqlCommand.ExecuteReader();

    while (reader.Read())
    {
        data.Add(new WineData
        {
            Type = reader.GetValue(0).ToString(),
            FixedAcidity = Parse(reader.GetValue(1).ToString()),
            VolatileAcidity = Parse(reader.GetValue(2).ToString()),
            CitricAcid = Parse(reader.GetValue(3).ToString()),
            ResidualSugar = Parse(reader.GetValue(4).ToString()),
            Chlorides = Parse(reader.GetValue(5).ToString()),
            FreeSulfurDioxide = Parse(reader.GetValue(6).ToString()),
            TotalSulfurDioxide = Parse(reader.GetValue(7).ToString()),
            Density = Parse(reader.GetValue(8).ToString()),
            Ph = Parse(reader.GetValue(9).ToString()),
            Sulphates = Parse(reader.GetValue(10).ToString()),
            Alcohol = Parse(reader.GetValue(11).ToString()),
            Quality = Parse(reader.GetValue(12).ToString())
        });
    }
}
```

## Creating the Model

Now that we have our data from the database, let's use it to create an ML.NET model.

First thing, though, let's create an instance of the `MLContext`.

```
var context = new MLContext();
```

We can use the `LoadFromEnumerable` helper method to load the `IEnumerable` data that we have into the `IDataView` that ML.NET uses. In previous versions of ML.NET this used to be called `ReadFromEnumerable`.

```
var mlData = context.Data.LoadFromEnumerable(data);
```

Now that we have the `IDataView` we can use that to split the data into a training set and test set. In previous versions of ML.NET this returned a tuple and it could be deconstructed into two variables (`var (trainSet, testSet) = ...`), but now it returns an object.

```
var testTrainSplit = context.Regression.TrainTestSplit(mlData);
```

With the data set up, we can create the pipeline. The two main things to do here is to set up the `Type` feature, which denotes if the wine is red or white, as [one hot encoded](https://hackernoon.com/what-is-one-hot-encoding-why-and-when-do-you-have-to-use-it-e3c6186d008f). Then we concatenate each of the other features into a feature array. We'll use the `FastTree` trainer and since our label column isn't named "Label", we set the `labelColumnName` parameter to the name of the label we want to predict, which is "Quality".

```
var pipeline = context.Transforms.Categorical.OneHotEncoding("TypeOneHot", "Type")
                .Append(context.Transforms.Concatenate("Features", "FixedAcidity", "VolatileAcidity", "CitricAcid",
                    "ResidualSugar", "Chlorides", "FreeSulfurDioxide", "TotalSulfurDioxide", "Density", "Ph", "Sulphates", "Alcohol"))
                .Append(context.Regression.Trainers.FastTree(labelColumnName: "Quality"));
```

With the pipeline created, we can now call the `Fit` method on it with our training data.

```
var model = pipeline.Fit(testTrainSplit.TrainSet);
```

### Save Model

With our new model, let's save it to Azure Blob Storage so we can retrieve it to build an API around the model.

To start, we'll use the connection string that we put in the config earlier. We then pass that into the `Parse` method of the `CloudStorageAccount` class.

```
var storageAccount = CloudStorageAccount.Parse(configuration["blobConnectionString"]);
```

With a reference to the storage account, we can now use that to create a client and use the client to create a reference to the container that we will call "models". This container will need to be created in the storage account, as well.

```
var client = storageAccount.CreateCloudBlobClient();
var container = client.GetContainerReference("models");
```

With the container reference, we can create a blob reference to a file, which we created earlier as a field.

```
var blob = container.GetBlockBlobReference(fileName);
```

To save the model to a file, we can create a file stream using `File.Create` and inside the stream we can call the `context.Model.Save` method.

```
using (var stream = File.Create(fileName))
{
    context.Model.Save(model, stream);
}
```

And to upload the file to blob storage, just call the `UploadFromFileAsync` method. Note that this method is async, so we need to mark the containing method as `async` and add `await` in front of this method.

```
await blob.UploadFromFileAsync(fileName);
```

After running this, there should now be a file added to blob storage.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553683348197-7HTHISV94VT4NBBJ57ZE/2019-03-27+06_42_08-Clipboard.png?format=original)

* * *

Hope this was helpful. In the next part of this end-to-end series, we will show how to create an API that will load the model from Azure Blob Storage and use it to make predictions.
