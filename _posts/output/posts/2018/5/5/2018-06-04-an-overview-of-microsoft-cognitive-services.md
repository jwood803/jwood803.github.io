---
title: "An Overview of Microsoft Cognitive Services"
date: 2018-06-04
categories: 
  - "cognitive-services"
tags: 
  - "cognitive-services"
  - "deep-learning"
  - "artificial-intelligence"
coverImage: "anatomy-1751201_640.png"
---

A big thing around apps these days are that they are much more intelligent than they used to be. And with that users are expecting more and more from apps. Usually, the way to do this is to create machine learning or deep learning models yourself and deploy that within your applications. But now, Microsoft can do the heavy lifting for you with their suite of [Cognitive Services](https://azure.microsoft.com/en-us/services/cognitive-services/).

These services can do everything you need from sentiment analysis on text to face and emotion detection. In this post, we'll go over each service that is provided so you can see what all is available and decide which one is best for your application.

Cognitive Services are divided up into several areas:

- [Vision](https://azure.microsoft.com/en-us/services/cognitive-services/directory/vision/)
- [Speech](https://azure.microsoft.com/en-us/services/cognitive-services/directory/speech/)
- [Knowledge](https://azure.microsoft.com/en-us/services/cognitive-services/directory/know/)
- [Search](https://azure.microsoft.com/en-us/services/cognitive-services/directory/search/)
- [Language](https://azure.microsoft.com/en-us/services/cognitive-services/directory/lang/)

There's also a separate experimental area, called [Cognitive Service Labs](https://labs.cognitive.microsoft.com/), where Microsoft puts newer services out for people to try and give feedback on so they can improve on them. Let's take a deeper dive into each of these services and what they offer.

## Vision

### Computer Vision

Perhaps one of the first of the cognitive services, the Computer Vision API does quite a lot. It can give you the following:

- Text description of what it thinks is happening in a photo
- Tags of what it thinks it finds in a photo
- Content moderator scores in terms of adult content
- How it best things to categorize the photo
- And if it recognizes any faces within the photo such as any celebrities

That's a lot for one API to return and some of these things, such as the faces and content moderator, are their own API that you can implement separately.

### Content Moderator

Do you have a site where people can upload images and videos but don't want to go through each upload to make sure there's no adult content? This API will do all of that for you.

The API can look at images to detect if it has a high confidence that it is adult content, but it can also detect if the image is racy. That is, if it is midly sexual but not considered adult. It's no limited to just images, though. It can also do these checks with video.

This can also look at text to see if there is any profanity in it. But it goes beyond just profanity. It can also help check if there is any personal identifiable information (PII) in the text to make sure no personal information is published to your site. In these times of digital privacy being even more important, this is going to be very helpful.

### Face API

The Face API lets you do a few things in terms of finding and identifying people based on their face in a photo. This includes verifying faces from two photos match, recognizing emotion from a person's face, and detecting faces in a photo.

The emotion detection is interesting in that the API response will give you scores and the highest scored emotion is the one it thinks the face is giving.

```
"scores": {
      "anger": 0.09557262,
      "contempt": 0.003917685,
      "disgust": 0.684764564,
      "fear": 4.03712329E-06,
      "happiness": 8.999826E-08,
      "neutral": 0.002147009,
      "sadness": 0.213587672,
      "surprise": 6.34691469E-06
    }
```

In this response, "disgust" was the highest with a score of almost 70%.

### Video Indexer

The Video Indexer API is really interesting. It combines a few of the other APIs including content moderation, speech, and sentiment analysis to give a complete look at a video. For a complete walkthrough at what it can do, I have a [Wintellect post](https://www.wintellect.com/get-insights-video-microsoft-video-indexer/) that details the video indexer.

### Custom Vision

The Customer Vision API is where you get to upload your own sets of images, tag them, and then train a model to classify those images into the tags. This is a lot more powerful than just using the regular Computer Vision API where you can use your own images to train a model. This gives you or your business a more specialized model that is much easier to train and deploy than doing it by hand.

## Speech

### Speech to Text and Text to Speech

These are similar and are mostly self explanatory, but there are some subtlties that Microsoft gives you with them. For instance, the APIs can be customized to detect specific vocabularies and accents for a more accurate detection of the speech.

### Speaker Recognition

This API can recognize who is speaking of an unknown speaker and gives you the speaker's name. This works by sending in different recordings of a person's voice and associating them with that voice. Then the API knows how to recognize their voice and it works quite well.

This API can also do speaker verification. Similar to enrolling the voice for recognition, you'd have to train the API to know who is speaking so it can recognize the voice.

### Speech Translation

A fairly simple API but a powerful one. This adds real time and multi-language translation to speech in your apps. So if your application needs to support this type of feature, then just implement this API instead of spending the resources to create this model on your own, which may not even be as accurate as Microsoft's.

## Knowledge

### QnA Maker

This API is unique and serves to solve a specific problem - can automate responses to commonly asked questions about my site or services? QnA Maker makes this really easy by having a web interface where you can put the questions and their answers in and then train a model to detect the question being asked and output the correct answer to it.

This API is mainly used in conjunction with other services, specifically LUIS, that will be highlighted in the language section, and the [Bot Framework](https://dev.botframework.com/) to create bot to answer those types of questions for your users.

### Custom Decision Service

This API is a newer offering and is still in preview, but is also one to look out for. It is a way to offer custom recommendations on certain items. Not only that, but it does this in real time through [reinforcement learning](https://en.wikipedia.org/wiki/Reinforcement_learning).

## Search

### Bing Web Search

This simple yet powerful API gives your applications the ability to search the web through a keyword. The API brings back search results of web pages, images, video, and even news with a single API call.

### Bing Visual Search

This API allows you to search with an image and it will return any results that match that image. For example, if a customer wants to purchase a similar couch to replace their current one, your application can give results based off of a photo of their couch that they take and give product recommendations based on the results.

### Bing Entity Search

This API can give some extra context about people, places, and things that your application may refer to based off what is available out on the web about them. As an example, suppose your application gives recommendations on local places to visit depending on where your user is. With this API you can give more context about the items that your application finds to your user so they can make a more informed decision on where to go or what to do next.

### Bing Video Search

This video search API does the same thing as the web search API, but it only returns videos. However, this API gives a lot more in the results than just links to videos; you also get information such as the embed HTML, view count, and video length for starters. This is a perfect API if your application content is all about videos from around the web.

### Bing News Search

Another specific API, this one returns news articles. Similar to the Video Search API, this one returns a lot of additional information you can use within your application. For instance, along with the news article URL, you get the date it was created, related news and categories to the current news article, and specific mentions within the article.

### Bing Image Search

You can't have specific search on news and videos without having one that's for images. Not only does this API give you the full URL to the image, but you also get metadata on each image, thumbnails, and information on the website that published the image.

### Bing Custom Search

While the other search APIs are for the entire web, the Custom Search API will allow you to have search capabilities on your own custom domain. This means that search results will only be relevant to your domain and nothing else unless you specify other web pages to include. An example for this API can be if you have a huge knowledge base of FAQ on your site about all the products your company uses, then incorporating the Custom Search API will let users search through the entire knowledge base so they can quickly find an answer instead of browsing through all of the content.

### Bing Autosuggest

This is a unique API in all it does is, as you type in a search box, the API gets called giving suggestions on what it thinks you're typing. You've seen this if you've done a Google search, but to help illustrate, and see some results this API gives, here's a gif:

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1527253112724-VH1NXOGUKMKGJRWIELPY/autosuggest.gif?format=original)

## Language

### Text Analytics

This API is probably the most essential when you want to get information from text. That's due to what all this API gives you. You get sentiment analysis, it detects what language the text is in, and it extracts key phrases.

The API has actually been updated to also include a way to identify entities within the text. What this gives you is a way to tell if the text refers to a specific place, person, or organization.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1527253637034-Y5MLJQ3A41J83XYJ554J/2018-05-25+09_06_54-Clipboard.png?format=original)

### Bing Spell Check

Another interesting API offering, the Spell Check API will analyze your text and give any suggestions on what it thinks should be fixed.

For instance, if I give the API the text of `He has fixed there enviornment`, it will come back with the following JSON:

```
{
  "_type": "SpellCheck",
  "flaggedTokens": [
    {
      "offset": 13,
      "token": "there",
      "type": "UnknownToken",
      "suggestions": [
        {
          "suggestion": "their",
          "score": 0.786016486722212
        },
        {
          "suggestion": "the",
          "score": 0.702565790712439
        }
      ]
    },
    {
      "offset": 19,
      "token": "enviornment",
      "type": "UnknownToken",
      "suggestions": [
        {
          "suggestion": "environment",
          "score": 0.786016486722212
        }
      ]
    }
  ]
}
```

### Translator Text

This API can translate text between, as of right now, over 60 languages. Since it is REST based, there are an endless amount of applications for this API, such as helping you to localize your applications, communication and messaging applications, or you can mix in the Vision API to read text from images and translate that to another language with this API, and use the Speech API to speak the translation out from your application.

### Content Moderator

This simple API looks at images and video to determine if they have adult content in it. This is a very useful API to help you make sure no adult images get uploaded to your website if you have that feature.

This API also includes a flag in the response if the image or video is racy or not. If it is racy it isn't necessarily adult, but somewhat close to it. For example, this API may flag a photo as racy if it is of a person in a bathing suit.

### Language Understanding

Language Understanding Intelligent Service, or LUIS, is a nice part of the Cognitive Services family that allows applications to understand natural language. I've also talked more in detail about LUIS in a [Wintellect post](https://www.wintellect.com/building-language-intelligent-apps-microsofts-luis/) as well as how to [integrate LUIS](https://www.wintellect.com/integrating-microsoft-luis-bot-framework/) into the bot framework.

## Cognitive Service Labs

### Gesture

This API allows you to implement custom gestures into your application. And by gestures, it lets you implement hand gestures. Using hand gestures in your application can allow your users to be more productive and your applications be more intuitive to use.

<iframe src="//www.youtube.com/embed/k38ygfiAqVg?wmode=opaque&amp;enablejsapi=1" height="480" width="854" scrolling="no" frameborder="0" allowfullscreen></iframe>

### Ink Analysis

Ink Analysis allows you to not just allow a user to use a digital pen within your application, but it can understand what they wrote so you can take action on it.

This API can understand several things, such as:

- Shapes
- Handwritten text in over 60 languages
- Layout of text

### Local Insights

This fairly simple API can give you scores on locations around you by the different types of aminities that those places have. You can also tell it to score based on the proximity to you, or by other custom criteria.

### Event Tracking

This simple API will return events based off of a Wikipedia entry. For example, if I give a query of "Microsoft" I'll get a response like the below:

```
{
      "relatedEntities": [
        "Microsoft",
        "Surface 3"
      ],
      "latestDocument": {
        "documentId": "ceeddb607592dc8260110855489fa45b",
        "source": "Neowin",
        "publishDate": "2018-06-01T21:22:01Z",
        "title": "Surface 3 gets its first firmware update in a year and a half",
        "summary": "Microsoft today released a firmware update for its Surface 3 tablet, and it's the first update that the device has received since September 27, 2016.It contains security and display fixes.",
        "url": "https://www.neowin.net/news/surface-3-gets-its-first-firmware-update-in-a-year-and-a-half",
        "entities": [
          "Microsoft",
          "Surface 3"
        ]
      }
    },
    {
      "relatedEntities": [
        "Paul Allen",
        "Bill & Melinda Gates Foundation",
        "Government of India"
      ],
      "latestDocument": {
        "documentId": "ceeddc75d6a8971302b102291211f79f",
        "source": "UW CSE News",
        "publishDate": "2018-06-01T20:08:03Z",
        "title": "Thank you to our state legislators!",
        "summary": "On Thursday the Paul G. Allen School was honored to host UW’s annual reception thanking our state legislators for their investments in education.In the case of the Allen School, recent investments include substantial support for the Bill & Melinda Gates Center – a second building that will double our space when it opens in January – and multiple years of funding for enrollment growth that have more than doubled our degree capacity.",
        "url": "https://news.cs.washington.edu/2018/06/01/thank-you-to-our-state-legislators/",
        "entities": [
          "Paul Allen",
          "Bill & Melinda Gates Foundation",
          "Government of India"
        ]
      }
    }
```

### Answer Search

This API is designed to make search in your application much more efficient and faster. It does this by recognizing entities with search queries to determine what the query is about and allows your users to get quicker answers.

### Personality Chat

This will easily enable your bots to have more small talk, or conversation about unimportant subjects. Specifically, Personality Chat can add the following:

- Remove fallback answers such as "I don't know", or "I didn't get that".
- Add personality to the small talk.
- Add customizable small talk responses.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1527903150511-QL5Z1K4ID1VT9QLOPVZK/Untitled+Project.gif?format=original)

### Knowledge Exploration

This API can help enable search experiences over data with natural language queries. If you've used the Q&A feature in [Power BI](https://powerbi.microsoft.com/en-us/) then you've seen an example of Knowledge Exploration.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/1527904272378-ZU2CR7STCV8MARJ6BR2I/Untitled+Project.gif?format=original)

> If you're curious about more Power BI, feel free to checkout [this post](https://www.wintellect.com/getting-quick-insights-on-sales-data-with-powerbi/) on the Wintellect blog.

### Academic Knowledge

This API will allow you to get insights of academic content, mainly researach papers. Similar to Knowledge Exploration this API can interpret natural language queries to return more accurate results.

### URL Preview

This API is specific in that it provides a URL's title, description, and a relevent image. This API will also let you know if the URL is flagged for adult content.

### Conversation Learner

Similar to LUIS, Conversation Learner allows you to train models based on your own criteria for dialogs. Mainly to be used for the Bot Framework, this will definitely help people develop better bots to help their customers.

<iframe src="https://www.youtube.com/embed/9DJcWyRkqBI?ecver=2" width="640" height="360" frameborder="0" allow="autoplay; encrypted-media" style="position:absolute;width:100%;height:100%;left:0" allowfullscreen></iframe>

### Anomaly Finder

This API will help you find anomalies, or outliers, in your data. Mainly used for time series data it will return back which of your data points is considered an anomaly.

### Custom Decision

This API is interesting as it will allow a recommendation engine as a sevice. Using reinforcement learning, it gives personalized content based on the information that you provide to it.

* * *

It was a long journey, but we have went through each of the current offerings from Microsoft's Cognitive Services. Feel free to try any of these as they will really help to make your applications much more intelligent.
