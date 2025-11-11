---
title: "Use Bing Image Search to Get Training Image Data"
date: 2021-12-06
categories: 
  - "cognitive-services"
tags: 
  - "bing-image-search"
  - "cognitive-services"
coverImage: "dazzle.png"
---

When going through the FastAI book, [Deep Learning for Coders](https://amzn.to/3FIWUsw), I noticed that in one of the early chapters they mention using the [Bing Image Search API](https://docs.microsoft.com/en-us/bing/search-apis/bing-image-search/overview) to retrieve images for training data. While they have a nice wrapper for the API, I thought I'd dive into the API as well and use it to build my own way to download training image data.

Let's suppose we need to make an image classification model to determine what kind of car is in an image. We'd need quite a bit of different images for this model, so let's use the Bing Image Search to gather images of the Aston-Martin car so we can start getting our data.

Check out the below for a video version of this post.

<iframe class="embedly-embed" src="//cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2FlKCxQ6mxuy0%3Ffeature%3Doembed&amp;display_name=YouTube&amp;url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DlKCxQ6mxuy0&amp;image=https%3A%2F%2Fi.ytimg.com%2Fvi%2FlKCxQ6mxuy0%2Fhqdefault.jpg&amp;key=61d05c9d54e8455ea7a9677c366be814&amp;type=text%2Fhtml&amp;schema=youtube" width="854" height="480" scrolling="no" title="YouTube embed" frameborder="0" allow="autoplay; fullscreen" allowfullscreen="true"></iframe>

## Why Bing Image Search

Before going into the technical side of Bing Image Search, let's go over why use this in the first place. Why not just download the images ourselves?

Bing Images Search has a few features in it that we can utilize in our code when getting our images. Some of these features are important to take into account.

### Automation

I'll be honest, I'm lazy and if I can script something to do a task for me then I'll definitely spend the time to build the script rather than do the task manually. Rather than manually finding images and downloading them, we can use the Bing Image Search API to do this for us.

### Image License

We can't always just take an image from a website and use it however we want. A lot of images that are online are copyrighted and if we don't have a license to use the copyright we are actually in violation of the creator's copyright. If they find out we use their image without a license or permission then they can, more than likely, take legal action against us.

However, with Bing Image Search, we have an option to specify what license the images has that get returned to us. We can do this with the `licenseType` query parameter in our API call. This utilizes [Creative Commons](https://creativecommons.org/) licenses. We can specify exactly what type of license our images has. We can specify that want images that are public where the copyright is fully waived, which is what we will do. There are many Creative Commons license types that the Bing Image Search supports and there's a full list [here](https://docs.microsoft.com/en-us/bing/search-apis/bing-image-search/reference/query-parameters#license).

### Image Type

There are quite a few images types that we could download from Bing Image Search. For our purposes, though, we only want photos of Aston Martin cars. Due to that, we can specify the image type in our API calls to just `photo`. If we don't specify this we could get back animated GIFs, clip art, or drawings of Aston Martin cars.

### Safe Content

When downloading images from the internet you never really know what you're going to get. Bing Image Search can help ease that worry by specifying that you want only safe content to be returned.

Bing can do this filtering for us so we don't have to worry about it when we do our API call. This is one less thing we have to worry about and, because it's the internet, it's definitely something to worry about when download images.

## Create Azure Resource

Before we can use the Bing Image Search API we need to create the resource for it. In the Azure Portal create a new resource and search for "Bing Search". Then, click on the "Bing Search v7" resource to create it.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/88d0d438-613b-48a0-bdae-79183fdea0fb/Pasted+image+20211127081402.png?format=original)

When creating the resource give it a name, what resource group it will be under, and what subscription it will be under. For the pricing tier, it does have a free tier to allow you to give the service a try for evaluation or for a proof of concept. Once that is complete, click "Create".

When that completes deployment, we can explore a bit on the resource page. One thing to note is that there are a few things we can look at here. There's a "Try me" tab where we can try the Bing Search API and see what results we get. There is some sample code to see real quick how to use the Bing Search API. And there are a lot of tutorials that we can look at if we want to look at something more specific, such as the image or video search APIs.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/992b1ab5-e64b-4e5c-a392-0c1b4705b4a8/Pasted+image+20211127083005.png?format=original)

## Retrieve Key and Endpoint

To use the API in our code we will need the API key and the endpoint to call. There are a couple of ways we can get to it. First, on the "Overview" page of the resource there's a link that says to "click here to manage keys". Clicking that will take you to another page where you can get the API keys and the endpoint URL.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/6f75c579-89c9-4226-92ec-c0e4b98cd2d6/Pasted+image+20211128053009.png?format=original)

You can also click on the "Keys and Endpoint" section on the left navigation.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/0b347e5f-60a2-4054-a7f9-ae5c67729e67/Pasted+image+20211128052911.png?format=original)

Now save the API key and the endpoint since we'll need those to access the API in the code.

## Using the API

Now we get to the fun stuff where we can get into some code. I'll be using Python, but you're very welcome to use the language of your choice since this is a simple API call. I'm also using Azure ML since it's very easy to get a Jupyter Lab instance running plus most machine learning and data science packages already installed.

### Imports

First, we need to import some modules. We have four that we will need to import.

- **JSON**: This will be used to read in a config file for the API key and endpoint
- **Requests**: Will be used to make the API calls. This is pre-installed in an Azure ML Jupyter instance, so you may need to run `pip install requests` if you are using another envrionment.
- **Time**: Used to delay API calls so the server doesn't get hit too much by requests.
- **OS**: Used to saved and help clean image data on the local machine.
- **PPrint**: Used to format JSON when printing.

```
import json
import requests
import time
import os
import pprint
```

### The API Call

Now, we can start building and making the API call to get the image data.

#### Building the Endpoint

To start building the call, we need to get the API key which is kept in a JSON file for security reasons. We'll use the `open` method to open the file to be able to read it and use the `json` module to load the JSON file. This creates a dictionary where the JSON keys are the key names of the dictionary where you can get the values.

```
config = json.load(open("config.json"))

api_key = config["apiKey"]
```

Now that we have the API key we can build up the URL to make the API call. We can use the endpoint that we got from the Azure Portal and help build up the URL.

```
endpoint = "https://api.bing.microsoft.com/"
```

With the endpoint, we have to add some to it to tell it that we want the Image Search API. To learn more about the exact endpoints we're using here, [this doc](https://docs.microsoft.com/en-us/bing/search-apis/bing-image-search/reference/endpoints) has a lot of good information.

```
url = f"{endpoint}v7.0/images/search"
```

#### Building the Headers and Query Parameters

Some more information we need to add to our call are the headers and the query parameters. The headers is where we supply the API key and the query parameters detail what images we want to return.

Requests makes it easy to specify the headers, which is done as a dictionary. We need to supply the `Ocp-Apim-Subscription-Key` header for the API key.

```
headers = { "Ocp-Apim-Subscription-Key": api_key }
```

The query parameters are also done as a dictionary. We'll supply the license, image type, and safe search parameters here. Those are optional parameters, but the `q` parameter is required which is what query we want to use to search for images. For our query here, we'll search for aston martin cars.

```
params = {
    "q": "aston martin", 
    "license": "public", 
    "imageType": "photo",
    "safeSearch": "Strict",
}
```

#### Making the API Call

With everything ready, we can now make the API call and get the results. With `requests` we can just call the `get` method. In there we pass in the URl, the headers, and the parameters. We use the `raise_for_status` method to throw an exception if the status code isn't successful. Then, we get the JSON of the response and store that into a variable. Finally, we use the pretty print method to print the JSON response.

```
response = requests.get(url, headers=headers, params=params)
response.raise_for_status()

result = response.json()

pprint.pprint(result)
```

And here's a snapshot of the response. There's quite a bit here but we'll break it down some later in this post.

```
{'_type': 'Images',
 'currentOffset': 0,
 'instrumentation': {'_type': 'ResponseInstrumentation'},
 'nextOffset': 38,
 'totalEstimatedMatches': 475,
 'value': [{'accentColor': 'C6A105',
            'contentSize': '1204783 B',
            'contentUrl': '[https://www.publicdomainpictures.net/pictures/380000/velka/aston-martin-car-1609287727yik.jpg](https://www.publicdomainpictures.net/pictures/380000/velka/aston-martin-car-1609287727yik.jpg)',
            'creativeCommons': 'PublicNoRightsReserved',
            'datePublished': '2021-02-06T20:45:00.0000000Z',
            'encodingFormat': 'jpeg',
            'height': 1530,
            'hostPageDiscoveredDate': '2021-01-12T00:00:00.0000000Z',
            'hostPageDisplayUrl': '[https://www.publicdomainpictures.net/view-image.php?image=376994&amp;picture=aston-martin-car](https://www.publicdomainpictures.net/view-image.php?image=376994&amp;picture=aston-martin-car)',
            'hostPageFavIconUrl': '[https://www.bing.com/th?id=ODF.lPqrhQa5EO7xJHf8DMqrJw&amp;pid=Api](https://www.bing.com/th?id=ODF.lPqrhQa5EO7xJHf8DMqrJw&amp;pid=Api)',
            'hostPageUrl': '[https://www.publicdomainpictures.net/view-image.php?image=376994&amp;picture=aston-martin-car](https://www.publicdomainpictures.net/view-image.php?image=376994&amp;picture=aston-martin-car)',
            'imageId': '38DBFEF37523B232A6733D7D9109A21FCAB41582',
            'imageInsightsToken': 'ccid_WTqn9r3a*cp_74D633ADFCF41C86F407DFFCF0DEC38F*mid_38DBFEF37523B232A6733D7D9109A21FCAB41582*simid_608053462467504486*thid_OIP.WTqn9r3aKv5TLZxszieEuQHaF5',
            'insightsMetadata': {'availableSizesCount': 1,
                                 'pagesIncludingCount': 1},
            'isFamilyFriendly': True,
            'name': 'Aston Martin Car Free Stock Photo - Public Domain '
                    'Pictures',
            'thumbnail': {'height': 377, 'width': 474},
            'thumbnailUrl': '[https://tse2.mm.bing.net/th?id=OIP.WTqn9r3aKv5TLZxszieEuQHaF5&amp;pid=Api](https://tse2.mm.bing.net/th?id=OIP.WTqn9r3aKv5TLZxszieEuQHaF5&amp;pid=Api)',
            'webSearchUrl': '[https://www.bing.com/images/search?view=detailv2&amp;FORM=OIIRPO&amp;q=aston+martin&amp;id=38DBFEF37523B232A6733D7D9109A21FCAB41582&amp;simid=608053462467504486](https://www.bing.com/images/search?view=detailv2&amp;FORM=OIIRPO&amp;q=aston+martin&amp;id=38DBFEF37523B232A6733D7D9109A21FCAB41582&amp;simid=608053462467504486)',
            'width': 1920}]
```

A few things to note from the response:

- `nextOffset`: This will help us page items to perform multiple requests.
- `value.contentUrl`: This is the actual URL of the image. We will use this URL to download the images.

### Paging Through Results

For a single API call we may get around 30 items or so by default. How do we get more images with the API? We page through the results. And the way to do this is to use the `nextOffset` item in the API response. We can use this value to pass in another query parameter `offset` to give the next page of results.

So if I only want at most 200 images, I can use the below code to page through the API results.

```
new_offset = 0

while new_offset <= 200:
    print(new_offset)
    params["offset"] = new_offset

    response = requests.get(url, headers=headers, params=params)
    response.raise_for_status()

    result = response.json()

    time.sleep(1)

    new_offset = result["nextOffset"]

    for item in result["value"]:
        contentUrls.append(item["contentUrl"])
```

We initialize the offset to 0 so the initial call will give the first page of results. In the `while` loop we limit to just 200 images for the offset. Within the loop we set the `offset` parameter to the current offset, which will be 0 initially. Then we make the API call, we sleep or wait for one second, and we set the `offset` parameter to the `nextOffset` from the results and save the `contentUrl` items from the results into a list. Then, we do it again until we reach the limit of our offset.

### Downloading the Images

In the previous API calls all we did was capture the `contentUrl` items from each of the images. In order to get the images as training data we need to download them. Before we do that, let's set up our paths to be ready for images to be downloaded to them. First we set the path and then we use the `os` module to check if the path exists. If it doesn't, we'll create it.

```
dir_path = "./aston-martin/train/"

if not os.path.exists(dir_path):
    os.makedirs(dir_path)
```

Generally, we could just do the below code and loop through all of the content URL items and for each one we create the path with the `os.path.join` method to get the correct path for the system we're on, and open the path with the `open` method. With that we can use `requests` again with the `get` method and pass in the URL. Then, with the `open` function, we can write to the path from the image contents.

```
for url in contentUrls:
    path = os.path.join(dir_path, url)

    try:
        with open(path, "wb") as f:
            image_data = requests.get(url)

            f.write(image_data.content)
    except OSError:
        pass
```

However, this is a bit more complicated than we would hope it would be.

### Cleaning the Image Data

If we print the image URLs for all that we get back it would look something like this:

```
https://www.publicdomainpictures.net/pictures/380000/velka/aston-martin-car-1609287727yik.jpg
https://images.pexels.com/photos/592253/pexels-photo-592253.jpeg?auto=compress&amp;cs=tinysrgb&amp;h=750&amp;w=1260
https://images.pexels.com/photos/2811239/pexels-photo-2811239.jpeg?cs=srgb&amp;dl=pexels-tadas-lisauskas-2811239.jpg&amp;fm=jpg
https://get.pxhere.com/photo/car-vehicle-classic-car-sports-car-vintage-car-coupe-antique-car-land-vehicle-automotive-design-austin-healey-3000-aston-martin-db2-austin-healey-100-69398.jpg
https://get.pxhere.com/photo/car-automobile-vehicle-automotive-sports-car-supercar-luxury-expensive-coupe-v8-martin-vantage-aston-land-vehicle-automotive-design-luxury-vehicle-performance-car-aston-martin-dbs-aston-martin-db9-aston-martin-virage-aston-martin-v8-aston-martin-dbs-v12-aston-martin-vantage-aston-martin-v8-vantage-2005-aston-martin-rapide-865679.jpg
https://c.pxhere.com/photos/5d/f2/car_desert_ferrari_lamborghini-1277324.jpg!d
```

Do you notice anything in the URLs? While most of then end in `jpeg` there are a few with some extra parameters on the end. If we try to download with those URLs we won't get the image. So we need to do a little bit of data cleaning here.

Luckily, there are two patterns we can check, if there is a `?` in the URL and if there is a `!` in the URL. With those patterns we can update our loop to download the images to the below to get the correct URLs for all images.

```
for url in contentUrls:
    split = url.split("/")

    last_item = split[-1]

    second_split = last_item.split("?")

    if len(second_split) > 1:
        last_item = second_split[0]

    third_split = last_item.split("!")

    if len(third_split) > 1:
        last_item = third_split[0]

    print(last_item)
    path = os.path.join(dir_path, last_item)

    try:
        with open(path, "wb") as f:
            image_data = requests.get(url)
            #image_data.raise_for_status()

            f.write(image_data.content)
    except OSError:
        pass
```

With this cleaning of the URLs we can get the full images.

![](https://images.squarespace-cdn.com/content/v1/521545a6e4b0734032a27076/4a3cd388-977b-4aaa-9670-1cfe7d366417/Pasted+image+20211129131529.png?format=original)

### Conclusion

While this probably isn't as sophisticated as the wrapper that FastAI has, this should help if you need to get training images from Bing Image Search manually. You can also tweak this if needed.

Using Bing Image Search is a great way to get quality and license appropriate images for training data.
