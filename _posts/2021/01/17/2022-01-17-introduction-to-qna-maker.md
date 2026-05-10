---
title: "Introduction to QnA Maker"
date: 2022-01-17
categories: 
  - "cognitive-services"
tags: 
  - "cognitive-services"
  - "qna-maker"
coverImage: "Microsoft+QnA+Maker.png"
---

Suppose you have a FAQ page that has a lot of data and want to use that as a first line of customer service support for a chat bot on your main page. How can you integrate this with minimal effort? Enter Microsoft Q&A Maker.

For the video verison of this post, check below.

<iframe class="embedly-embed" src="//cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2FqhAPQEE7Gow%3Ffeature%3Doembed&amp;display_name=YouTube&amp;url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DqhAPQEE7Gow&amp;image=https%3A%2F%2Fi.ytimg.com%2Fvi%2FqhAPQEE7Gow%2Fhqdefault.jpg&amp;key=61d05c9d54e8455ea7a9677c366be814&amp;type=text%2Fhtml&amp;schema=youtube" width="854" height="480" scrolling="no" title="YouTube embed" frameborder="0" allow="autoplay; fullscreen" allowfullscreen="true"></iframe>

## What is Microsoft QnA Maker

From the official [docs](https://docs.microsoft.com/en-us/azure/cognitive-services/QnAMaker/Overview/overview) it states that...

> QnA Maker is a cloud-based Natural Language Processing (NLP) service that allows you to create a natural conversational layer over your data.

So, basically, this service allows you to creates question and answering based on the data that you have.

## Creating the Azure Resource

First thing, like for all Cognitive Services items, we need to create the Azure resource. When creating a new resource, you can search for "QnA" and select "QnA Maker".

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/ebe0585c-e14b-4dfe-967c-c3232465cd32/Pasted+image+20211208170245.png?format=original)

After clicking "Create" on that, we're on a screen where we have to enter a few things. First, we will supply the subscription, resource group, name, and pricing tier. Note that this does have a free tier so this can be used for proof of concepts or to simply try out the service to see if it meets your needs.

Next, it will ask for details for Azure Search. This is used to host the data that you give it for the QnA maker and Azure Search is used for that. Only the location and pricing tier is needed for this. This has a free tier, as well.

Last, it will ask details for an App Service. This is used to host the runtime for QnA maker which will be used to make queries against your data. The app name and location is required for this.

You can optionally enable app insights for this service as well, but feel free to disable that since it's not really needed unless you are creating this for actual business purposes and want to see if anything goes wrong.

With all that, we can click the "Review and Create" button to create and deploy the resources.

## Creating a QnA Knowledge Base

With the resource created we can now go to it. Per usual with the Cognitive Services items, we have a "Keys and Endpoint" item on the left where we can get the key and endpoint. This will be used later when using the API.

But first, we need to create our QnA Knowledge Base and to do that we need to go to the "Overview" tab on the left navigation. A bit down it has a link where we can go to the QnA portal to create a new knowledge base.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/80eb7912-c070-4c2c-a65c-46d6d31b3d3f/Pasted+image+20211208172042.png?format=original)

We can skip step one since we already created the Azure resource. In step two, we will connect the QnA portal to our Azure resource. Simply give it the subscription and the Azure service name. Luckily, all of this pre-populates so they are all dropdowns.

In step three, we will give our knowledge base a name. We'll name it "ikea" since we will use the Ikea FAQ to populate the knowledge base.

Step four is where we'll populate the knowledge base. If you already have a FAQ on your website you can put the URL in. Since I'm using the [Ikea FAQ](https://www.ikea.com/us/en/customer-service/faq/) we can do that. You can also add a file for this. If you have neither, you can leave this blank and fill out the questions and answers manually on the next page.

Below this you can customize your bot with a chit-chat. This just helps give your QnA bot a bit more personality based on what you select. Here's a screenshot of an example from [the docs](https://docs.microsoft.com/en-us/azure/cognitive-services/qnamaker/how-to/chit-chat-knowledge-base) of each of the chit-chat items that you can choose from.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/0ead30e5-29d8-474e-abf1-c3e86e978466/Pasted+image+20211211073427.png?format=original)

For step five, we can create our knowledge base by clicking on the button.

## Training and Testing

Once we have our knowledge base created we can look see that it read in the Ikea FAQ quite well. To see just how well QnA Maker is, we can instantly click on the "Save and train" button to train a model on our data.

Once that finishes we can click on the "Test" button to give the model a test. This is an integrated chat bot where we can ask it questions and will receive answers based on the model.

So we can ask "What is the return policy?" and QnA Maker will give us the best answer based on our data.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/59eef268-f07f-4989-a266-fe12bb334009/Pasted+image+20211213175116.png?format=original)

Early on we get some good results from QnA Maker. But what if we want to add a new pair?

## Adding a New QnA Pair

If we want to add new QnA pairs to our existing knowledge base, just click on the **Add QnA Pair** button.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/9573a02d-7358-4902-92e7-a854a1caedff/Pasted+image+20211214121558.png?format=original)

We can add an alternate phrasing such as "This is a question". A phrasing is mostly a question that a user would input into the system that will get sent to QnA Maker. We can input an answer as well, such as "This is an answer". Notice that we have a rich text editor which can be toggled in the upper left. With this, we can add graphics, links, and emojis. Let's add a smile emoji to our answer.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/12d53b3f-0af3-4f62-9e0f-b1d58c7b305b/Pasted+image+20211215135503.png?format=original)

Now we can click the "Save and train" button to train a new model on what we just added. We can then do another test and in put "This is a question" and we should get the answer that we put as the output.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/7e4c8ab8-f9a3-4933-93cb-0340efee0038/Pasted+image+20211215135614.png?format=original)

## Using the API

Before we can actually use the API we need to publish our knowledge base. Simply click the "Publish" tab near the top and then the "Publish" button.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/3c4422f2-841d-489b-b803-f3da8d9c7151/Pasted+image+20211216130209.png?format=original)

Once that completes you can either create a bot that will use this knoweldge base, or you can use the API directly. We'll use the API and the publish page shows how you can call the API using Postman or through curl. We'll use Postman here so we can easily test the API out.

To build the URL for the API, use the "Host" item in the deployment details and then append the first item where it has "POST" after that.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/b89b2b9b-9675-455f-947b-755035c09eb9/Pasted+image+20211218012805.png?format=original)

And since it does say "POST" we will make this as a POST call.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/0f9d72bd-83ba-42b4-a5d0-d439cd1fc342/Pasted+image+20211218012807.png?format=original)

Next, we need to set the authorization header. In the Authorization tab in Postman, set the type to "API Key". The "Key" item will be "Authorization" and the "Value" will be the API key which is the third part of the deployment details.

Now, we can add in the JSON body and the simplest JSON that we can add has only one item, a "question" item. And this is the prompt that a user would send to QnA Maker. Let's add the question that we added earlier.

```
{
 "question":"This is a question"
}
```

Once we hit "Send" in Postman, we will get the below response.

```
{
     "answers": [
     {
         "questions": [
             "This is a question"
         ],
         "answer": "This is an answer😀",
         "score": 100.0,
         "id": 176,
         "source": "Editorial",
         "isDocumentText": false,
         "metadata": [],
         "context": {
             "isContextOnly": false,
             "prompts": []
         }

    }
 ],
 "activeLearningEnabled": false
}
```

The main part to notice here is the "answer", which is what we expect to get back.

* * *

Hopefully, this showed how useful the QnA Maker can be, especially if you already have a FAQ page with questions and answers. With QnA Maker it can be turned into a chat bot or any other automation tool where users or customers may need to ask questions.
