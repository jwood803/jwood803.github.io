---
title: "How the Machine Learning Process is Like Cooking"
date: 2021-05-17
categories: 
  - "machine-learning"
tags: 
  - "machine-learning"
coverImage: "Machine+learning+is+like+cooking.png"
---

When creating machine learning models it's important to follow the machine learning process in order to get the best performing model that you can into production and to keep it performing well.

But why cooking? First, I enjoy cooking. But also, it is something we all do. Now, we all don't make five course meals every day or aim to be a Michelin star chef. We do follow a process to make our food, though, even if it may be to just heat it up in the microwave.

In this post, I'll go over the machine learning process and how it relates to cooking to give a better understanding of the process and maybe even a way to help remember the steps.

For the video version, check below:

<iframe class="embedly-embed" src="//cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2FHqrkbxd69lM&amp;display_name=YouTube&amp;url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DHqrkbxd69lM&amp;image=https%3A%2F%2Fi.ytimg.com%2Fvi%2FHqrkbxd69lM%2Fhqdefault.jpg&amp;key=61d05c9d54e8455ea7a9677c366be814&amp;type=text%2Fhtml&amp;schema=youtube" width="854" height="480" scrolling="no" title="YouTube embed" frameborder="0" allow="autoplay; fullscreen" allowfullscreen="true"></iframe>

# Machine Learning Process

First, let's briefly go over the machine learning process. Here's a diagram that's known as the cross-industry standard process for data mining, or simply known as [CRISP-DM](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining).

\[caption id="" align="alignnone" width="721"\]![ Kenneth Jensen - Own work based on: ftp://public.dhe.ibm.com/software/analytics/spss/documentation/modeler/18.0/en/ModelerCRISPDM.pdf  (Figure 1) ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1620565377528-SWZ6ZD07Y4D5U4P5CIU2/CRISPDM.png?format=original) Kenneth Jensen - Own work based on: ftp://public.dhe.ibm.com/software/analytics/spss/documentation/modeler/18.0/en/ModelerCRISPDM.pdf (Figure 1) \[/caption\]

The machine learning process is pretty straight forward when going through the diagram:

- Business understanding - What exactly is the problem we are trying to solve with data
    
- Data understanding - What exactly is in our data, such as what does each column mean and how does it relate to the business problem
    
- Data prep - Data preprocessing and preparation. This can also include feature engineering
    
- Modeling - Getting a model from our data
    
- Evaluation - Evaluating the model for performance on generalized data
    
- Deployment - Deploying the model for production use
    

Note that a couple of items can go back and forth. You may do multiple iterations of getting a better understanding of the business problem and the data, data prep and modeling, and even going back to the business problem when evaluating a model.

Notice that there's a circle around the whole process which means you may even have to go back to understanding the problem once a model is deployed.

There are a couple of items I would add to help improve this process, though. First, we need to think about getting our data. Also, I believe can add a separate item in the process for improving our model.

## Getting Data

I would actually add an item before or after defining the business problem, and that's getting data. Sometimes you may have the data already and define the business problem but you may have to get the data after defining the problem. Either way, we need good data. You may have heard an old saying in programming, "Garbage in, garbage out", and that applies to machine learning as well.

We can't have a good model unless we give it good data.

## Improving the Model

Once we have an algorithm we can also spend some time to improve it even further. We can deploy some techniques that can tweak the algorithm to perform better.

Now that we understand the machine learning process a bit better, let's see how it relates to cooking.

# Relating the Machine Learning Process to Cooking

At first glance, you may not see how the machine learning process relates to cooking at all. But let's go into more detail of the machine learning process and how each step relates to cooking.

## Business Understanding

One of the first things to do for the machine learning process is to get a business understanding of the problem.

For cooking, we know we want to make a dish, but which one? What do we want to accomplish with our dish? Is it for breakfast, lunch, or dinner? Is it for just us or do we want to create something for a family of four?

Knowing these will help us determine what we want to cook.

## Getting Data

I would actually add an item before or after defining the business problem, and that's getting data. Sometimes you may have the data already and define the business problem but you may have to get the data after defining the problem. Either way, we need good data. You may have heard an old saying in programming, "Garbage in, garbage out", and that applies to machine learning as well.

We can't have a good model unless we give it good data.

## Data Processing

Data processing is perhaps the most important step after getting good data. Depending on how you process the data will depend on how well your model performs.

For cooking, this is equivalent to preparing your ingredients. This includes chopping any ingredients such as vegetables, but keeping a consistent size when chopping also counts. This helps the pieces cook evenly. If some pieces are smaller they can burn or if some pieces are bigger then they may not be fully cooked.

Also, just like in machine learning there are multiple ways to process your data, there are also different ways to prepare ingredients. In fact, there's a word for processing all of your ingredients before you start cooking - [mise en place](https://en.wikipedia.org/wiki/Mise_en_place) - which is French for "everything in it's place". This is done in cooking shows all the time where they have everything ready to start cooking.

This actually also makes sense for machine learning. We have to have all of our data processing done on the training data before we can give it to the machine learning algorithm.

## Modeling

Now it's time for the actual modeling part of the process where we give our data to an algorithm.

In cooking, this is actually where we cook our dish. In fact, we can relate choosing a recipe to choosing a machine learning algorithm. The recipe will take the ingredients and turn out a dish, and the algorithm will take the data and turn out a model.

Different recipes will turn out different dishes, though. Take a salad, for instance. Depending on the recipe and the ingredients, the salad can turn out to be bright and citrusy like this [kale citrus salad](https://www.foodnetwork.com/recipes/ree-drummond/kale-citrus-salad-2593844). Or, it can be warm and savory like this [spinach salad with bacon dressing](https://www.foodnetwork.com/recipes/alton-brown/spinach-salad-with-warm-bacon-dressing-recipe-1947599).

They're both salads, but they are turned into different kinds of salads because of different ingredients and recipes. In machine learning, you can have similar models from different data and algorithms.

What if you have the same ingredients? There are definitely different ways to make the same recipe. Hummus is traditionally made with chickpeas, tahini, garlic, and lemon like in [this recipe](https://www.foodnetwork.com/recipes/katie-lee/classic-hummus-2333947). But there is also [this hummus recipe](https://www.foodnetwork.com/recipes/alton-brown/hummus-for-real-recipe-2014722) that has the same ingredients but the recipe is just a bit different.

## Optimizing the Model

Depending on the algorithm the machine learning model is using we can give it different parameters that can optimize the model for better performance. These parameter are called [hyperparameters](https://en.wikipedia.org/wiki/Hyperparameter_\(machine_learning\)), which is used during the learning process of the model to your data.

These can be updated manually by you choosing values for the hyperparameters. This can be quite tedious and you never know what value to choose. So, instead, there are ways this can be automated by giving a range of values and running the model multiple times with different values and you can use the best performing model that is found.

How do we optimize a dish, though? Perhaps the best way to get the best taste out of your dish, other than using the best ingredients, is to season it. Specifically, seasoning with salt. In [this video](https://www.youtube.com/watch?v=ITI3J5UWiyQ) by [Ethan Chlebowski](https://www.ethanchlebowski.com/), he suggests

> …home cooks severely under salt the food they are cooking and is often why the food doesn't taste good.

He even quotes this line from the book [Ruhlman's Twenty](https://amzn.to/2Rslw54):

> How to salt food is the most important skill to know in the kitchen.

I've even experienced this in my own cooking where I don't add enough salt. Once I do, the dish tastes 100 times better.

Now, adding salt to your dish is the more manual way of optimizing it with seasoning. Is there a way that this can be automated? Actually, there is! Instead of using just salt and adding other spices to it yourself you can get these seasoning blends that has all the spices in it for you!

## Evaluating the Model

Evaluating the model is going to be one of the most important steps because this tells you how well your model will perform on new data, or rather, data that it hasn't seen before. During training your model have good performance, but giving it new data may reveal that it actually is performing bad.

Evaluating your cooked dish is a lot more fun, though. This is where you get to eat it! You will determine if it's a good dish by how it tastes. Is it good or bad? If you served it to others, what did they think about it?

## Iterating on the Model

Iterating on the model is a part of the process that may not seem necessary, but it can be an important one. Your data may change over time which would then make your model stale. That is, it's relying on data that it used to but due to some process change or something similar it no longer does. And since the underlying data changed the model won't predict as well as it did.

Similarly, you may have more or even better data that you can use for training, so you can then retrain the model with that to make better predictions.

How can you iterate on a dish that you just prepared? First thing is if it was good or bad. If it was bad, then we can revisit the recipe and see if we did anything wrong. Did we overcook it? Did we miss an ingredient? Did we prepare an ingredient incorrectly?

If it was good, then we can iterate on it

A lot of chefs and home cooks like to take notes about recipes they've made. They write in some tricks they've learned along the way but also some different paths from the recipe that they either had to take due to a missing ingredient or preferred to take.

## Conclusion

Hopefully, this helps your better understand the machine learning process through the eyes of cooking a dish. It may even help you understand the importance of each step because, in cooking, if one step is missed then you probably won't be having a good dinner tonight.

And if you're wondering where does AutoML fit into all of this, then you can think of it as the meal delivery kits like Hello Fresh or Blue Apron. They do a lot of the work for you and you just have to put it all together.
