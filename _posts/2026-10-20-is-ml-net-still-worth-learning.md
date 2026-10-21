---
layout: post
title: Is ML.NET Still Worth Learning?
featured: true
description: Is ML.NET still worth learning? An honest look at where it beats an LLM,
  where it falls short, and a cheat sheet for choosing the right .NET AI tool.
image: "/images/mlnet-thumb-strike.png"
tags:
- mlnet
- ".NET"
- machine-learning
- ai
- C#
date: 2026-10-20 21:19 -0400
---
In our new age of generative AI with LLMs such as [ChatGPT](https://chatgpt.com/) and [Claude](https://claude.ai/), one might assume that ML.NET is now obsolete. However, I'd argue that it's still worth learning even though AI continues to advance including ML.NET itself. In fact, it has distinct advantages that none of the LLMs do. ML.NET didn't die and is still under active development. However, it did grow into two different variations - the classic `Microsoft.ML` library to train and consume machine learning algorithms, and the `Microsoft.ML.Tokenizers` package that is plumbing for LLMs. 

In this post, we will go over how you can do AI in .NET and where the state of ML.NET is today, where it still wins, where it doesn't, and how you can choose what Microsoft AI solution is best for you.

## "AI in .NET" Has Different Meanings Now
Microsoft has invested a good bit in integrating your apps with AI, and in several ways. There are different ways you can call any model you need, to train your own models, and to run different models locally depending on your specific scenario.

### Calling Models
Microsoft offers several ways in which we can call different types of AI models:

- `Microsoft.Extensions.AI` - This is the base code layer. As collections in .NET have `IEnumerable`, AI providers have `IChatClient` that is available from `Microsoft.Extensions.AI`. It provides interfaces that help with calling AI models, as well as caching and telemetry.
- Agent Framework - This framework builds on `Microsoft.Extensions.AI` to enable you to orchestrate multiple agents with conversation states.
- Microsoft Foundry - Foundry has its own portal in which you can deploy and host models as well as give you evaluations and governance on your models.

### Training Models
Training your own models is where ML.NET shines. Having your own data and using it to train your own models for your situation is what ML.NET is made for you to do within .NET. As with several of my [posts on ML.NET]({{ '/tags/?tag=mlnet' | relative_url }}) it offers a lot more than just training models, as well, such as retrieving data, [building feature pipelines]({% post_url 2018/7/11/2018-07-11-create-your-first-machine-learning-pipeline-in-mlnet %}), evaluating models, and hosting your models to make inference calls on it.

Honestly, not all problems that can be solved with AI need to use a sophisticated LLM. There are still plenty of business problems that classical machine learning is perfect for. Problems such as forecasting, recommendations, and classification are fairly common across businesses. The reason for this is that businesses still have their own data that can help solve these problems. Below are a couple of real-life scenarios businesses may have and can see where classical machine learning can fit in better than using an LLM.

- **Customer Churn** - No business wants to lose customers and being able to try to find those customers that are most likely to leave your product or service can be helpful so you can try to retain that customer. By using [your customer data]({% post_url 2019/3/17/2019-03-27-mlnet-end-to-end-build-model-from-database-data %}), such as demographics, billing, and how much activity they spend with customer support, a customer churn model can be built. An LLM has never seen your customer history, so the best it can do is make an educated guess. That's a separate API call for every customer, each one taking a few seconds, costing tokens, and possibly giving a different answer the next time you ask. A trained model scores that same customer in milliseconds, right inside your app, and gives the same answer every time.
- **Fraud Detection** - You've probably at least once gotten a text message or call from your bank after a transaction saying it thought that transaction was fraudulent and to verify it was you who actually made the purchase. This is your bank or credit card company implementing fraud detection to make sure your account or card number didn't get compromised. Again this uses the data from each customer including their demographics and purchase history to find any outlier transaction that may seem unlikely that you actually purchased them. This could be from an outlier based off other transactions you've done or classification based off of already known fraudulent transactions. The same problem applies here, but speed matters even more.

### Running Models
Models can be made by a variety of different frameworks other than ML.NET. There have been a lot of models created from Python frameworks such as scikit-learn, PyTorch, and even have them hosted on Hugging Face. We've [touched on ONNX before](https://youtu.be/Yf7VXj1etdw) as to how to use ONNX exported models in ML.NET. There are two ways to do it. You can call the ONNX Runtime directly, which gives you the most control, but it's up to you to turn your data into the tensors the model expects and to make sense of the tensors it returns. Or you can use ApplyOnnxModel to make the ONNX model one step in an ML.NET pipeline, so ML.NET handles loading your data and any transforms before and after it, such as normalizing values or mapping the output back to labels. If your data is already in the shape the model wants, call the runtime directly. If it needs preparing first, let the pipeline do that work for you.

## The State of ML.NET Today
You may be wondering if ML.NET is still [getting updates](https://github.com/dotnet/machinelearning/releases), what's going on with it? The main thing to keep in mind is that the ML.NET version updates happen at the same time as .NET itself is updated, which is usually in November of each year during .NET Conf. So for 2026, ML.NET version 6 is scheduled to be released. Take a look at the below table and you can get an idea of the release schedule for ML.NET and that work is still going into it.

| ML.NET Version | Release Date |
|:----------:|:-------------:|
| 3.0 | November 2023 |
| 4.0 | November 2024 |
| 5.0 | November 2025 |
| 6.0 | November 2026 (tentative) |

Another thing to keep in mind is that the updates for ML.NET aren't going to always be for the machine learning side itself. The changes are going into the `Microsoft.ML.Tokenizers` area. This is essentially a plumbing package to use for LLMs. For instance, in the [preview of version 6](https://github.com/dotnet/machinelearning/releases/tag/v6.0.0-preview1) there are new features for tokenizer support for different GPT models.

## Where ML.NET Still Wins (And Where It Doesn't)
ML.NET still shines when you need to train and/or make predictions on a machine learning model within the .NET ecosystem. Classical machine learning itself as a whole still offers a lot of advantages over calling an LLM:
- Cost - Not only are you saving in costs by not using tokens for each call, but you wouldn't even need a subscription to an LLM for it at all. While it will take some time and resources to train a model that works best for your needs, once it's built the costs associated with it can be substantially smaller than having the same calls through an LLM.
- Offline Use - Hosted LLMs are API calls, so they only work while the device is connected. Using your own trained ML.NET model can run on the device so no connection is even needed. There are ways to host models locally (Foundry Local, Ollama) with smaller models, but they do tend to be less capable in what they can do compared to the big models like Fable from Anthropic.
- Privacy - Calling a hosted LLM means you call into another company's APIs and you don't necessarily know what all they may do with your data. Having your own trained models you can run them on the device for full privacy or, if it still needs to perform an API call for predictions, you can control the backend where the calls are going to so you know the data isn't going anywhere else.
- Specialized - While it's nice having these general LLMs where you can ask it anything, they won't know your data very well. Training a specialized machine learning model strictly on your data means you can train it to be optimized to that data. A classical machine learning model runs on hardware a local LLM can't so there is no need to always have a GPU.

Of course, like with everything, there are pros and cons and ML.NET has its disadvantages as well:
- Lack of deep learning - While there are some ways to [use deep learning models with ML.NET]({% post_url 2021/9/13/2021-09-13-the-mlnet-deep-learning-plans %}), it is mostly geared towards classical machine learning algorithms. 
- Samples and community - Since machine learning and AI in general is highly geared towards Python, the amount of samples (though I try to help with ML.NET by giving examples through this blog) and community members are much smaller.
- Need labeled data - Most classical machine learning algorithms need labeled data in order to perform their training. First, you would need the data for it to train on. And second, you would need to take the time to make sure they are labeled correctly. Both of these could take time and resources to gather, but LLMs wouldn't need all of that data.
- Initial setup costs - Not only do classical machine learning algorithms need its structured data, the initial training, evaluating, and deploying of those models would take initial resources to get going. LLMs wouldn't need that. In addition, those models would need to be retrained so they don't go stale as new data starts coming in that could change the predictions. 

## Which Solution to Choose?
There is a Microsoft AI solution that can fit your needs, but it will depend on what you need it for. Feel free to use the below table to see which tool to use for your specific situation.

| If you need… | Use | Why |
|---|---|---|
| Token counts, cost estimates, chunking | `Microsoft.ML.Tokenizers` | One package, no ML involved |
| Generation, summarization, open-ended language | An LLM via `Microsoft.Extensions.AI` (e.g. Microsoft Foundry) | Nothing classical comes close |
| Multi-step, tool-using agents | Agent Framework (`Microsoft.Agents.AI`) | Built for orchestrating agents |
| A model trained elsewhere | ONNX Runtime | Runs any exported model in-process |
| Predictions from structured data | ML.NET | In-process, no per-call cost, same output every time |
| A technique with no .NET path | Python | The research ecosystem lives there |

Here's the table as a shareable image.

![Which .NET AI tool do you actually need? cheat sheet]({{ '/images/microsoft-ai-cheat-sheet.png' | absolute_url }})

{: .tip}
If you want a downloadable version of the cheat sheet, here's a [PDF.]({{ '/assets/files/microsoft-ai-cheat-sheet.pdf' | relative_url }})

---

This post went over different ways you can use AI in .NET, the state of ML.NET and how it has evolved, especially during this LLM era. It also highlighted how you can work with AI in the Microsoft ecosystem. Is ML.NET still worth learning? Depending on your scenario for AI use, it just might be what you need.