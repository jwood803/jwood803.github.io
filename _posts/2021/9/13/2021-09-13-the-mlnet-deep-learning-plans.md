---
title: "The ML.NET Deep Learning Plans"
date: 2021-09-13
categories: 
  - "mlnet"
tags: 
  - "deep-learning"
  - "mlnet"
coverImage: "Copy+of+cinema+is+a+matter+of+what's+in+the+frame+and+what's+out+(5).png"
---

One of the most requested features for ML.NET is the ability to create neural networks models from scratch to perform deep learning in ML.NET. The ML.NET team has taken that feedback and the feedback from the [customer survey](https://devblogs.microsoft.com/dotnet/ml-net-june-updates-model-builder/#ml-net-survey-results) and has come out with a plan to start implementing this feature.

## Current State of Deep Learning in ML.NET

Currently, in ML.NET, there isn't a way to create neural networks to have deep learning models from scratch. There is great support for taking an existing deep learning model and using it for predictions, however. If you have a TensorFlow or ONNX model then those can be used in ML.NET to make predictions.

There is also great support for transfer learning in ML.NET. This allows you to take your own data and train it against a pretrained model to give you a model of your own.

However, as mentioned earlier, ML.NET does not yet have the capability to let you create your own deep learning models from scratch. Let's take a look at what the plans are for this.

## Future Deep Learning Plans

In the ML.NET GitHub repo there is an [issue](https://github.com/dotnet/machinelearning/issues/5918) that was fairly recently created that goes over the plans to implement creating deep learning models in ML.NET.

There are two reasons for this:

1. Communicate to the community about what the plans are and that this is being worked on.
2. Get feedback from the community on the current plan.

While we'll touch on the main points in the issue in this post, I would highly encourage you to go through it and give any feedback or questions about the plans you may have to help the ML.NET team in their planning or implementation.

The issue details three parts in order to deliver creating deep learning models in ML.NET:

1. Make consuming of ONNX models easier
2. Support TorchSharp and make it production ready
3. Create an API in ML.NET to support TorchSharp

Let's go into each of these in more detail.

### Easier Use of ONNX Models

While you can currently use ONNX models in ML.NET right now, you do have to know the input and output names in order to use it. Right now we rely on the [Netron](https://netron.app/) application to load the ONNX models to give us the input and output names. While this isn't bad, the team wants to expose an internal way to get these instead of having to rely on a separate application.

Of course, along with the new way to get the input and output names for ONNX models, the documentation will definitely be updated to reflect this. I believe, not only documentation, but examples would follow to show how to do this.

### Supporting TorchSharp

[TorchSharp](https://github.com/dotnet/TorchSharp) is the heart of how ML.NET will implement deep learning. Similar to how [Tensorfow.NET](https://github.com/SciSharp/TensorFlow.NET) supports scoring TensorFlow models in ML.NET, this will provide access to the PyTorch library in Python. [PyTorch](https://pytorch.org/) is starting to lead the way in building deep learning models in research and in industry so it makes sense to implmement in ML.NET.

In fact, one of the popular libraries to build deep learning models is [FastAI](https://course.fast.ai/). Not only is FastAI one of the best courses to take when learning deep learning, but the Python library is one of the best in terms of building deep learning models. Under the hood, though, FastAI uses PyTorch to actually build the models that it produces. This isn't by accident. The FastAI developers decided that PyTorch was the way to go for this.

TensorFlow is great to support for predicting existing models, but for building new ones from scratch I really think PyTorch and TorchSharp is the preferred way. To do this, TorchSharp will help ML.NET lead the way.

### Implementing TorchSharp into ML.NET

The final stage is, once TorchShap has been made production ready, create a high-level API in ML.NET to train deep learning models from scratch.

This will be like when Keras came along for TensorFlow. It was an API on top of TensorFlow to help make building the models much easier. I believe ML.NET can do that for TorchSharp.

This will probably be a big undertaking but definitely worth doing. This will be the API people will use to build their models so taking the time to get this the best way possible. will be worth it in the long run to let us build our models the most trivial way possible which will make us more productive in the long run.

## Conclusion

Creating deep learning models from scratch is, by far, one of the most requested features for ML.NET and their plan to do this is definitely going to reach this goal. In fact, I think it will surpass this goal since it will use PyTorch on the backend which is where research and the industry is leaning towards.

If you have any feedback or questions, definitely feel free to comment on the [GitHub issue](https://github.com/dotnet/machinelearning/issues/5918).
