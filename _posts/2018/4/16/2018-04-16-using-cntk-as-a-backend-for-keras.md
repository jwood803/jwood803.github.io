---
title: "Using CNTK as a Backend for Keras"
date: 2018-04-16
categories: 
  - "deep-learning"
tags: 
  - "cntk"
  - "cognitive-toolkit"
  - "keras"
  - "deep-learning"
coverImage: "pexels-photo-326461.jpeg"
---

I've been learning about [Keras](https://keras.io/) through the [Deep Learning with Python book](https://amzn.to/2IWLC6z) (which I'll be doing a review of), and it can be a bit tricker to get started than just running a single `pip` command to install a package.

[![](//ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=1617294438&Format=_SL160_&ID=AsinImage&MarketPlace=US&ServiceVersion=20070822&WS=1&tag=dotnetmeditat-20)](https://www.amazon.com/Deep-Learning-Python-Francois-Chollet/dp/1617294438/ref=as_li_ss_il?ie=UTF8&linkCode=li2&tag=dotnetmeditat-20&linkId=71695a08234a901959ca949597651a8b)![](https://ir-na.amazon-adsystem.com/e/ir?t=dotnetmeditat-20&l=li2&o=1&a=1617294438)

First make sure you have Keras and CNTK installed.

```
pip install keras
pip install cntk
```

You may see documentation about installing CNTK by using the wheel, but since March, 2018 it is now on [PyPI](https://pypi.org/project/cntk/) so you can directly install it like above.

The easiest way to change it is to go to your user home folder and navigate to the `.keras` folder. There you'll see a `keras.json` file. This file has some configurations for Keras to use when running.

```
{
    "floatx": "float32",
    "epsilon": 1e-07,
    "backend": "tensorflow",
    "image_data_format": "channels_last"
}
```

You can change the `backend` property to `cntk` and then next time you run Keras it will use it.

```
Using CNTK backend
C:\Users\chron\Anaconda3\lib\site-packages\keras\backend\cntk_backend.py:21: UserWarning: CNTK backend warning: GPU is not detected. CNTK's CPU version is not fully optimized,please run with GPU to get better performance.
  'CNTK backend warning: GPU is not detected. '
```
