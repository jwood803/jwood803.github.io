---
title: "Book Review: Python Tricks"
date: 2019-03-30
categories: 
  - "book-review"
  - "python"
tags: 
  - "book-review"
  - "book"
  - "python"
coverImage: "img.jpg"
---

If you've been around Python for a while, then you're probably familiar with [Dan Bader](https://dbader.org/). He likes to help people up their Python game and share his knoweledge of the language. Because of that, he released his own book, [Python Tricks](https://amzn.to/2NDMeE0).

[![](//ws-na.amazon-adsystem.com/widgets/q?_encoding=UTF8&ASIN=1775093301&Format=_SL250_&ID=AsinImage&MarketPlace=US&ServiceVersion=20070822&WS=1&tag=dotnetmeditat-20)](https://www.amazon.com/Python-Tricks-Buffet-Awesome-Features/dp/1775093301/ref=as_li_ss_il?ie=UTF8&qid=1515767023&sr=8-1&keywords=python+tricks&linkCode=li3&tag=dotnetmeditat-20&linkId=930e65e2282584f1897a0024def1cee4)![](https://ir-na.amazon-adsystem.com/e/ir?t=dotnetmeditat-20&l=li3&o=1&a=1775093301)

The book has seven sections of Python goodness:

- Clean Python Patterns
- Effective Functions
- Classes
- Data Structures
- Loops
- Dictionaries
- Productivity

Let's look at an example in each of the above items so you can have an idea of what is in the book. These examples are ones that I have learned from when reading the book. Code examples in this post are my own.

## Clean Python - Assertions

The very first tip in this book goes over using assertions in Python. Assertions, if you aren't familiar, is a way to well, assert if a condition is true. More specifically, a condition in which you expect to be true. If you have an assertion and it turns out to false, it will throw an `AssertionError`.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553979344991-T526NFZVUCVQGZYR24VV/2019-03-30+16_55_19-Clipboard.png?format=original)

If an assertion is caught it's a good idea to see why. Doing so will allow you to catch bugs that you may not have been able to catch before which is why having assertions are helpful.

## Effective Functions - `*args` and `**kwargs`

When starting out in Python you may come across a method like the below:

```
def main(*args, **kwargs):
    print(args)
    print(kwargs)
```

Seeing something like this for the first time and, like me, you'll wonder what in the world that means.

`*args` are extra positional arguments you can pass in. The `*` in front tells it to gather all of those into a tuple. So if we take the method definition from above we can print the `*args`.

```
main("hello", "world")
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553981783987-DATAVQXW7401NOL0PYXI/2019-03-30+17_36_06-Clipboard.png?format=original)

The `**kwargs` parameter does the same thing, but instead of a tuple it will be a dictionary since the `kw` in front of the `args` stands for "key word".

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553981822140-FKH824VW7SWJTOJO3Q2Y/2019-03-30+17_36_50-Clipboard.png?format=original)

## Classes - Named Tuples

Tuples in Python are essentially a collection that is immutable. That is, once it's definied it can't be changed. To easily spot a tuple, the values in it will be enclosed by parentheses.

```
t = (1, 2, 3, 4, "five")
t
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553982558925-LY1OX30CTAAGTPZ2KG30/2019-03-30+17_48_47-Clipboard.png?format=original)

However, if we want to get access to get access to the first item of the tuple, there's no dot notation to give us access to it.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553982671347-ZPF9TCXN26TJDAEU8PLV/2019-03-30+17_50_58-Clipboard.png?format=original)

Though, we can use slicing to access it.

```
t[0]
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553983052987-3K696K8T94SCHQE9EY98/2019-03-30+17_57_26-Clipboard.png?format=original)

With a `NamedTuple` we can create a tuple and access items in it with the dot notation.

```
from collections import namedtuple

t = namedtuple("t", ["one", "two", "three", "four", "five"])

items = t(1, 2, 3, 4, "five")
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553983198823-QC7X0HYHCLQ13NTCQZ93/2019-03-30+17_59_51-Clipboard.png?format=original)

## Data Structures - Sets

Sets are a data structure that simply gives unique values. This can be quite useful if you want to remove duplicates from a list.

```
set([1, 2, 3, 4, 2, 5, 3])
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553983669984-YIQDUAUIH4NDR3ISCLJ1/2019-03-30+18_07_38-Clipboard.png?format=original)

## Loops - Comprehensions

List comprehensions are perhaps one of my favorite things in Python. It is essentially sytactic sugar on top of using a `for` loop and, once understood, can be easier to read since it is only in one line. For example, let's say we have the below loop:

```
item = []
for i in range(0, 10):
    item.append(i + 1)

print(item)
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553985827065-TGPQ5BHJP5YJED1J32OY/2019-03-30+18_43_38-Clipboard.png?format=original)

We can rewrite it using a list comprehension:

```
[i + 1 for i in range(0, 10)]
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553985858780-6G3FQ20AHXFI87RNNFB5/2019-03-30+18_44_09-Clipboard.png?format=original)

What if we need an `if` statement in the loop, like below?

```
item = []
for i in range(0, 10):
    if i % 2 == 0:
        item.append(i + 1)

print(item)
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553985886989-69JXXOF7WP82GQPKFK2B/2019-03-30+18_44_39-Clipboard.png?format=original)

That can also be broken down into a list comprehension.

```
[i + 1 for i in range(0, 10) if i % 2 == 0]
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553985912305-EB7OZVN20OAHW782R4GN/2019-03-30+18_45_05-Clipboard.png?format=original)

## Dictionaries - Default Values

Often, when checking if a value is in a dictionary, it's not always known if that value is in the dictionary in the first place. The `get` method on dictionaries is a good way to get the value if it is in the dictionary or to return a default value if it is not in the dictionary.

For example, let's say we have the below dictionary.

```
d = {"x": 1, "y": 2}
```

We can get the `y` value with the `get` method.

```
d.get("y")
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553986593192-GPBEQSGZUO1DL4K40SAA/2019-03-30+18_56_17-Clipboard.png?format=original)

But if we try to get the value from `z` which doesn't exist in the dictionary, we can give a second parameter to the `get` method to return that value as default if the key doesn't exist.

```
d.get("z", 0)
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553986618367-WLVUTBUACW1ZMYOAEHA3/2019-03-30+18_56_53-Clipboard.png?format=original)

Also, if we don't specify the default value and the key doesn't exist in the dictionary, the `get` method won't return anything.

```
d.get("z")
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553986665529-D9INS3HMLXR2JK5F99PO/2019-03-30+18_57_36-Clipboard.png?format=original)

## Productivity - `dir`

While Python is a great language it is also a bit weird to work with when trying to explore through code what methods or properties a variable can have. That's one of the nice things with C# and it being a typed language. I can use the dot notation and intellisense to see what is available on a variable. With Python, though, that may not always work even in great editors such as PyCharm and Jupyter.

This is where the `dir` method comes to the rescue. Using this method we can see exactly what method and properties are on a variable.

For example, let's say you wanted to see what all is avilable on a list object.

```
x = [1, 2]
dir(x)
```

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1553987263023-ALIJPMH048S8QJ07EU1S/2019-03-30+19_06_30-Clipboard.png?format=original)

The `dir` method has saved me a lot of time trying to find what's available on objects instead of having to hunt down the documentation.

* * *

I consider myself a beginner to intermediate in Python. Some of the tricks in here I've seen before, such as asserts and list comprehensions, but most of the ones in this book I haven't. I definitely learned quite a bit more about using Python. Both for cleaner code and to utilize what the language has to offer whether than defaulting to what I already know from another language.

Whether you're just beginning or a pro in Python, there's something in this book to learn from. Maybe there'll be a second edition soon.
