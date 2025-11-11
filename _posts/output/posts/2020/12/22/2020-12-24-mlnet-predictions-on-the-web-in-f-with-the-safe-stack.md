---
title: "ML.NET Predictions on the Web in F# with the SAFE Stack"
date: 2020-12-24
categories: 
  - "f"
  - "mlnet"
tags: 
  - "f"
  - "fsadvent"
  - "safe-stack"
  - "mlnet"
coverImage: "jason-dent-3wPJxh-piRw-unsplash.jpg"
---

Building web sites in C# has be something that you could do for quitea while. But, did you know that you can do web sites in F#? Enter the SAFE Stack. An all-in-one framework that allows you to use F# on the server, but also allows you to use F# on the client side. That's right, no more JavaScript for the client side!

For a video version of this post, checkout the video below.

<iframe src="//www.youtube.com/embed/yM6gjErsPT0?wmode=opaque&amp;enablejsapi=1" height="480" width="854" scrolling="no" frameborder="0" allowfullscreen></iframe>

## Introduction to the SAFE Stack

The SAFE Stack is built on top of these components:

- Saturn
- Azure
- Fable
- Elmish

Let's go into each of these in a bit more detail.

### [Saturn](https://saturnframework.org/explanations/overview.html)

Saturn is a backend framework built in F#. Saturn privides several parts to help us build web applications, such as the application lifecycle, a router, controllers, and views.

### [Azure](https://azure.microsoft.com/)

Azure is Microsoft's cloud platform. This is mostly used for hosting our website and any other cloud resources that we may need, such as Azure Blob Storage for files or Azure Event Hub for real time streaming data.

### [Fable](https://fable.io/)

Fable is a JavaScript compiler. Similar to how TypeScript compiles into JavaScript, Fable does the same, except that you write F# and it compiles into JavaScript.

### [Elmish](https://elmish.github.io/elmish/)

The Elmish concept builds on top of Fable to provide a model-view-update pattern that is popularized in the Elm programming language.

## Creating a Project

The best way to create a SAFE Stack project is to follow the steps in the documentation, but I'll highlight them here. By the way, their documentation is great!

There is a .NET template created to make creating a SAFE project much easier than manually putting it together.

To install the template, run the below command.

`dotnet new -i SAFE.Template`

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1608719860517-CKH0EEURD07E70A8DOZ7/1.png?format=original)

Once the template is installed, make a new directory to keep the project files.

`mkdir MLNET_SAFE`

Then, you can use the .NET CLI to create a new project from the template with another command and specify the name of the project.

`dotnet new SAFE -n MLNET_SAFE`

Once that finishes, run the command to restore the tools used for the project. Specifically, the FAKE tool, which is used to build and run the project.

`dotnet tool restore`

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1608719952141-51FNBYN7RNWQM4O4VH08/2.png?format=original)

With that done we can now run the app! To do that run the FAKE command with the run target.

`dotnet fake build --target run`

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1608719983039-DTNRF712CKY6I758IRPA/3.png?format=original)

This is going to perform the following steps (which can be found in the build.fsx file):

- Clean the solution
- Run npm install to install client side dependencies
- Compiles the projects
- Run the projects in watch mode

When that completes, you can navigate to [http://localhost:8080](http://localhost:8080). We now have a running instance of the SAFE Stack!

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1608720078219-OLIMZCHDEFRZBJNDSAL8/4.png?format=original)

The template is a todo app which helps show different aspects of the SAFE Stack. Feel free to explore the app and the code before continuing.

## Adding ML.NET

### The Model

For the ML.NET model, I'll be using the salary model that was created in the below video. It's a simple model with a small dataset to go over more of the F# and ML.NET nuances than working with the data itself.

<iframe src="//www.youtube.com/embed/PVJMX-iD0no?wmode=opaque&amp;enablejsapi=1" height="480" width="854" scrolling="no" frameborder="0" allowfullscreen></iframe>

In the Server project, add a new folder called "MLModel". In there, we can add the model file that was generated from the above video. We would also need to update the properties on the file to allow it to output during build.

Note that this can easily be in Azure Blob Storage instead and use the SDK to retrieve and download it from there.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1608720257710-FK57M1PPI519USBQBX84/5.png?format=original)

Next, for the Server and Shared projects, add the Microsoft.ML NuGet packge. At this time, it's at version 1.5.4.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1608720293277-IS30HAXTZ60G2PW4UBIO/6.png?format=original)

## Updating the Shared File

Now we can update the file in the Shared project. We can put types and methods in this file that we know will be used in more than one other project. For our case, we can use the model input and output schemas.

```
type SalaryInput = {
    YearsExperience: float32
    Salary: float32
}

[<CLIMutable>]
type SalaryPrediction = {
    [<ColumnName("Score")>]
    PredictedSalary: float32
}
```

The `SalaryInput` class has two properties that are both of type `float32`. The `SalaryPrediction` class is special where we need to put the `CLIMutable` attribute on it. That has one property that's also of type `float32`. This property has the `ColumnName` attribute on it to map to the output column from the ML.NET model.

There's one other type we can add to our shared file. We can create an interface that has a method to get our predictions that can be called from the client to the server.

```
type ISalaryPrediction = { getSalaryPrediction: float32 -> Async<string> }
```

In this type, we create a method signature called `getSalaryPrediction` which takes in a paramter of type `float32` and it returns a type of Async of string. So this method is asynchornous and will return a string result.

## Updating the Server

Next, we can update our server file. This file contains the code to run the web server and any other methods that we may need to call from the client.

To run the web app you have the following code:

```
let webApp =
    Remoting.createApi()
    |> Remoting.withRouteBuilder Route.builder
    |> Remoting.fromValue predictionApi
    |> Remoting.buildHttpHandler

let app =
    application {
        url "http://0.0.0.0:8085"
        use_router webApp
        memory_cache
        use_static "public"
        use_gzip
    }

run app
```

The `app` variable creates an application instance and sets some properties of the web app, such as what the URL is, what router to use, and to use GZip compression. You can also add items such as using OAuth, set logging, or enable CORS.

The `webApp` variable creates the API and builds the routing. Both of these are based on the `predictionApi` variable which is based off the `ISalaryPrediction` type we defined in the shared file.

```
let predictionApi = { getSalaryPrediction =
    fun yearsOfExperience -> async {
        let prediction = prediction.PredictSalary yearsOfExperience
        match prediction with
        | p when p.PredictedSalary > 0.0f -> return p.PredictedSalary.ToString("C")
        | _ -> return "0"
    } }
```

The API has the one method we defined in the interface - `getSalaryPrediction`. This is where we implement that interface method. It takes in a variable, `yearsOfExperience`, and it runs an async method defined by the `async` keyword. In the brackets is what it should run.

All we are running in there is to use a prediction variable to call the PredictSalary method on it and pass in the years of experience variable to it. With the value from that we do a match expression and if the PredictedSalary property is greater than 0 we return that property formatted as a currency. If it is 0 or below, we just return the string "0".

But where did the prediction variable come from? Just above the API implementation, a new Prediction type is created.

```
type Prediction () =
    let context = MLContext()

    let (model, _) = context.Model.Load("./MLModel/salary-model.zip")

    let predictionEngine = context.Model.CreatePredictionEngine<SalaryInput, SalaryPrediction>(model)

    member __.PredictSalary yearsOfExperience =
        let predictedSalary = predictionEngine.Predict { YearsExperience = yearsOfExperience; Salary = 0.0f }

        predictedSalary
```

This creates the instance of the `MLContext`. It also loads in the model file, and creates a `PredictionEngine` instance from the model. Remember the `SalaryInput` and `SalaryPrediction` types are from the shared project. And notice that, when we load from the model, it returns a tuple. The first value returns the model whereas the second value returns the `DataViewSchema`. Since we don't need the `DataViewSchema` in our case, we can ignore it using an underscore (`_`) for that variable.

This type also creates a member method called `PredictSalary`. This is where we call the `predictionEngine.Predict` method and give it an instance of `SalaryInput`. Because F# is really good at inferring types, we can just give it the `YearsExperience` property and it knows that it is the `SalaryInput` type. We do need to supply the `Salary` property as well, but we can just set that to 0.0. Then, we return the predicted salary from this method. In F# we don't need to specify the `return` keyword. It automatically returns if it's the last item in the method.

### Updating the Client

With the server updated to do what we need, we can now update the client to use the new information. Everything we need to update will be in the `Index.fs` file.

There are a few Todo items that it's trying to use here from the Shared project. We'll have to update these to use our new types.

First, we have the `Model` type. This is the state of our client side information. For the Todo application, it has two properties, `Todos` and `Input`. The `Input` property is the current input in the text box and the `Todos` property are the currently displayed Todos. So to update this we can change the `Todos` property to be `PredictedSalary` to indicate the currently predicted salary from the input of the years of experience. This property would need to be of type `string`.

```
type Model =
    { Input: string
      PredictedSalary: string }
```

The next part to update is the `Msg` type. This represents the different events that can update the state of your application. For todos, that can be adding a new todo or getting all of the todos. For our application we will keep the `SetInput` message to get the value of our input text box. We will remove the others and add two - `PredictSalary` and `PredictedSalary`. The `PredictSalary` message will initiate the call to the server to get the predicted salary from our model, and the `PredictedSalary` message will initiate when we got a new salary from the model so we can update our UI.

```
type Msg =
    | SetInput of string
    | PredictSalary
    | PredictedSalary of string
```

For the `todosApi` we simply rename it to `predictionApi` and change it to use the `ISalaryPrediction` instead of the `ITodosApi`.

```
let predictionApi =
    Remoting.createApi()
    |> Remoting.withRouteBuilder Route.builder
    |> Remoting.buildProxy<ISalaryPrediction>
```

The `init` method can be updated to use our updated model. So instead of having an array of Todos we just have a string of `PredictedSalary`.

```
let init(): Model * Cmd<Msg> =
    let model =
        { Input = ""
          PredictedSalary = "" }
    model, Cmd.none
```

Next, we update the `update` method. This takes in a message and will perform the work depending on what the message is. For the Todos app, if the message comes in as `AddTodo` it will then call the `todosApi.addTodo` method to add the todo to the in-memory storage. In our app, we will keep the `SetInput` message and add two more to match what we added in our Msg type from above. The `PredictSalary` message will convert the input from a string to a `float32` and pass that into the `predictionApi.getSalaryPrediction` method. The `PredictedSalary` message will then update our current model with the new salary.

```
let update (msg: Msg) (model: Model): Model * Cmd<Msg> =
    match msg with
    | SetInput value ->
        { model with Input = value }, Cmd.none
    | PredictSalary ->
        let salary = float32 model.Input
        let cmd = Cmd.OfAsync.perform predictionApi.getSalaryPrediction salary PredictedSalary
        { model with Input = "" }, cmd
    | PredictedSalary newSalary ->
        { model with PredictedSalary = newSalary }, Cmd.none
```

The last thing to update here is in the `containerBox` method. This builds up the UI. You may have already noticed that there is no HTML in our solution anywhere. That's because Fable is using React behind the scenes and we are able to write the HTML in F#. We'll keep the majority of the UI so there's only a few items to update. The content is what's currently holding the list of todos in the current app. For our case, however, we want it to show the predicted salary so we'll remove the ordered list and replace it with the below div. This sets a label and, if the `model.PredictedSalary` is empty it doesn't display anything. But if it isn't empty it does a formatted string containg the predicted salary.

```
div [ ] [ label [ ] [ if not (System.String.IsNullOrWhiteSpace model.PredictedSalary) then sprintf "Predicted salary: %s" model.PredictedSalary |> str ]]
```

Next, we just need to update the placeholder in the text box to match what we would like the user to do.

```
Control.p [ Control.IsExpanded ] [
                Input.text [
                  Input.Value model.Input
                  Input.Placeholder "How many years of experience?"
                  Input.OnChange (fun x -> SetInput x.Value |> dispatch) ]
            ]
```

And with the button we just need to tell it to dispatch, or fire off a message, to the `PredictSalary` message.

```
Button.a [
   Button.Color IsPrimary
   Button.OnClick (fun _ -> dispatch PredictSalary)
]
```

With all of those updates we can now run the app again to see how it goes.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1608721513173-2QOD9PE764WPQJA2G7UQ/image-asset.gif?format=original)

* * *

Being able to use F# for the client as well as the server is a great way for F# developers to not only build web applications without having to use JavaScript and any of their frameworks, but also so they can utilize their functional programming knowledge to reduce bugs in the code.

If I were building web apps for personal or freelance work, I'll definitely give the SAFE Stack a try. I believe my productivity and efficiency of building the web applications will be much better with it.

To learn more (there is a good bit to learn since we're not only using functional patterns in a web application, we are also using the model view update pattern for the UI) I highly recommend the [Elmish documentation](https://safe-stack.github.io/docs/component-elmish/) and [Elmish book](https://zaid-ajaj.github.io/the-elmish-book/#/chapters/elm/the-architecture) by [Zaid Ajaj](https://github.com/Zaid-Ajaj). I'll be referencing these a lot in the days to come.
