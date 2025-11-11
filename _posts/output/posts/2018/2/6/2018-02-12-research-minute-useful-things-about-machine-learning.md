---
title: "Research Minute: Useful Things about Machine Learning"
date: 2018-02-12
categories: 
  - "data-science"
  - "machine-learning"
  - "research"
tags: 
  - "research-paper"
  - "machine-learning"
  - "data"
  - "data-science"
coverImage: "book-address-book-learning-learn-159751.jpeg"
---

I've done book reviews in the past, but as I mentioned in my post on [deliberate practice](https://jonwood.co/blog/2016/12/21/my-deliberate-practice-plan-to-become-a-better-programmer), I intended to get into more research papers. I don't have any academic experience in reading research papers, I can't quite be able to reach the very math centric or abstract papers (at least not yet), but there are quite a few that, with some patience, anyone can read and learn from.

With that, I'll do my best to summarize the more interesting research papers I read here.

### How to Read a Research Paper

When first getting started with reading research papers is that you don't read the whole thing in one pass through. Read a bit of the Abstract and Introduction sections to make sure it's a paper that looks interesting.

For more on how to read a research paper, [Siraj Raval](https://www.youtube.com/channel/UCWN3xxRkmTPmbKwht9FuE5A) has an interesting video:

<iframe src="//www.youtube.com/embed/SHTOI0KtZnU?wmode=opaque&amp;enablejsapi=1" height="480" width="854" scrolling="no" frameborder="0" allowfullscreen></iframe>

## A Few Useful Things to Know about Machine Learning

For this first Research Minute, I chose the paper [A Few Useful Things to Know about Machine Learning](http://homes.cs.washington.edu/~pedrod/papers/cacm12.pdf) by Pedro Domingos from the University of Washington. This paper has some very practical thoughts on creating your machine learning models. Below are a few of the ones I found the most helpful.

### Generalizing of the model is what matters most

This is the main reason you create a model, but often times you won't have any true test data from the wild to test on. Because of this it is recommended to split your data even before you do any pre-processing on it. `scikit-learn` has a very handy `test_train_split` method just for this purpose.

What we're trying to avoid is overfitting your model (which we explore a bit later in this post). An overfit model won't give you accurate results if you deploy it and it predicts with real data that it hasn't seen before when the model was trained.

### You need more than just data

The author puts this very nicely that doing machine learning is a lot of work. It's not magic that we just give it data and you're done. There's a lot more to it than that. A good analogy from the paper is that creating machine learning models is like farming. Nature does a good bit of the work, but a lot of up-front work has to be done for nature to do its thing. It's the same way with creating machine learning models - a lot of pre-processing and business understanding of the data is needed before starting the modeling process.

It's often said that most of the work in data science and machine learning is cleaning and getting your data in a state that you can feed it into a model and also to evaluate how your model's performance does. Fitting data into a model is easy with the current libraries.

### Be aware of overfitting

[Overfitting](https://en.wikipedia.org/wiki/Overfitting) a model is where the model will score high on training data, but when it sees real data that it hasn't yet seen, it will score low. Ways in which overfitting happens is with [bias and variance](https://en.wikipedia.org/wiki/Bias–variance_tradeoff).

Bias is where the algorithm learns the same wrong thing, which results in the model missing relevant relationships. Recall my [previous post](https://dotnetmeditations.com/blog/2018/2/4/bias-in-machine-learning) about how bias in machine learning is a big topic that we still need to work on. Variance is where the model is sensitive to irrelevant data which can cause it to model random noise in the training data.

A popular way to battle against overfitting is to use [cross validation](https://en.wikipedia.org/wiki/Cross-validation_\(statistics)) on your model. Cross validation will take your data and create subsets of it. In each subset it will take a random portion of the data as test data. It does this with each iteration, or fold, and it compares each score of the sampled test data of all iterations. This is a good way to test your model on different data to make sure you get consistent scores. If you don't, then your model may be sensitive to part of the data.

The image below from the article helps explain bias vs. variance through the game of darts.

\[caption id="" align="alignnone" width="474"\]![ Figure 1 in  A Few Useful Things to Know About Machine Learning by Pablo Domingos  ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1518264505457-099I3MM0LB2JEK7ME4J2/bias-and-variance?format=original) Figure 1 in A Few Useful Things to Know About Machine Learning by Pablo Domingos \[/caption\]

### Prioritize feature engineering

Sometimes the best way to get a more accurate model is to do [feature engineering](https://en.wikipedia.org/wiki/Feature_engineering) on your data. This combines or manipulates columns of your data to create columns of new or a different type of data which may be better for the algorithm to detect a pattern. The author states in this paper that feature engineering is the best thing you can do to increase the accuracy of your model. Though, it is a tough thing to learn and there's [a book](http://amzn.to/2ETySNq) strictly about feature engineering coming out soon.

Feature engineering is tricky. In the book mentioned above, doing feature engineering on your data is more like an art than it is a science. You have to know a lot about your data and have the creativity to think that unrelated parts of data can come together, or that one part of the data can be represented in a different way.

### More data over a more complex algorithm

The author points out here in the paper that there can be a tradeoff of three elements when training a model - the amount of data, computational power, and the time it takes to train athe model. The author then goes into how the most important of those three is the time. Since time is a resource we can never get back or improve upon, unless we can invent the time machine, we need to choose algorithms that generalize well to our data as well as being able to be trained quickly. The best way to get both is to use more data with the simplest algorithm possible.

Wouldn't more complex algorithms generalize better than simpler algorithms? Not necessarily. Take a look at the below image from the paper. Multiple algorithms, some more complex than the others, still can generalize the same as the simpler algorithms. However, these simpler algorithms can be much faster to train and predict on new data.

\[caption id="" align="alignnone" width="507"\]![ Figure 3 in  A Few Useful Things to Know About Machine Learning by Pablo Domingos  ](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1518269294102-KTSBXC1WFHZ052L9AHD5/Model+complexity?format=original) Figure 3 in A Few Useful Things to Know About Machine Learning by Pablo Domingos \[/caption\]

* * *

The paper has several more ideas, thirteen in total, but these are the ones I thought gave the most insights from the projects I've done. I'm sure I'll come back to this paper several times as I do more projects and gain more machine learning experience.
