---
title: "Creating an Object Detection Model with Model Builder and Azure ML"
date: 2021-08-22
categories: 
  - "azure"
  - "mlnet"
tags: 
  - "model-builder"
  - "mlnet"
  - "object-detection"
  - "azure-ml"
coverImage: "dazzle.png"
draft: true
---

Object detection models can be quite useful for providing more information about an image than image classification can do. Not only can it give you multiple items in the image but it can also tell you where in the image that those objects are.

![Computer vision sample in Simón Bolivar Avenue, Quito - Object detection - Wikipedia](https://upload.wikimedia.org/wikipedia/commons/3/3b/Computer_vision_sample_in_Sim%C3%B3n_Bolivar_Avenue%2C_Quito.jpg) By Comunidad de Software Libre Hackem \[Research Group\] - [https://www.youtube.com/watch?v=ZmMFsL1ahI4](https://www.youtube.com/watch?v=ZmMFsL1ahI4), CC BY-SA 3.0, [https://commons.wikimedia.org/w/index.php?curid=92687514](https://commons.wikimedia.org/w/index.php?curid=92687514)

Object detection is very helpful in several industries. In health care we can have an x-ray photo and the model can detect if there is cancer in the image, but also where in the image that it is. With that information the doctors can act on it and give the patient the treatment that they need.

There's also a lot of applications in manufacturing. A part or a complete component that was made can have a photo of it taken and an object detection model can determine if there are any defects. If there are, and since it tells you where the defect is, it can be fixed a lot quicker.

## Data

The data we will use is from a Microsoft tutorial that goes into object detection and uses the stop sign data set. We will use this data to create a model to determine if an image incluces a stop sign and where in the image the stop sign is located.

## Generating Data with VoTT

However, the trickiest thing about creating an object detection model is getting the data. The data needs to have the bounding boxes, or the coordinates, of where the objects are in the image. Those bounding boxes aren't very trivia to get on your own. Luckily, we have an application from Microsoft that can help - Visual Object Tagging Tool or [VoTT](https://github.com/Microsoft/VoTT).

There's a great doc on the official Microsoft docs site, but we'll also go over how to use VoTT in this post.

While there is an online version of VoTT, I will use the downloadable version to generate the data even while offline.

### Create new VoTT project

Once you download the latest version of VoTT and open it you will be greeted by a screen to create a new project or to open one either locally or on the cloud. We will create a new project.

!\[\[Pasted image 20210826130923.png\]\]

Once we click to create a new project we have the following options that we need to fill in:

- **Display name**: The name of the project.
- **Source Connection**: This is where the images are located. Since we have them on our hard drive, I have this set as `LocalConnection`.
- **Target Connection**: This is where VoTT will output files.

For the other options we can leave the defaults. Let's fill in the three items that we need.

The Display Name is set as `StopSignObjectDetection`. For the Source Connection, we will slick **Add Connection**.

We have another screen to add the connection with a couple of items to fill out. First, we need to give it a name. We can set this as **StopSignInput**. For the "Provider" section, this is what we will tell VoTT where the images are located. There are a couple of options here but we will select **Local File System**. This displays a new field where we give it the folder path where our images are. If you downloaded the stop sign dataset the images will be in the "stop signs\\stop-sign-dataset\\Stop-Signs" folder from where you extracted it at. Now we can click **Save Connection**.

With the new connection saved we can now set the **Source Connection** field to the connection we just created.

The **Target Connection** will be similar where we click the **Add Connection** button and give it the same fields. We can set the display name as `StopSignOutput` and set the "Provider" to the local system, again. To help keep the files separated, I tend to create an "output" folder so the output won't be in the same folder as the input.

With those filled out, the Project Settings will look something like this: !\[\[Pasted image 20210829080114.png\]\]

Now we can click on the **Save Project** button to start tagging our images.

### Tagging Images in VoTT

At this point VoTT should display the images on the left side and the currently selected one in the middle. On the right there is the tagging area. It should look similar to this:

!\[\[Pasted image 20210829080711.png\]\]

#### Creating a Tag

Before we can start tagging we should create at least one tag. To created one, click on the plus icon in the tagging area toolbar.

!\[\[Pasted image 20210829080915.png\]\]

We can now type in the name of our tag, which we will call `stop-sign`, and click Enter to accept it.

We will only have this one tag, but you can create as many tags as you need. You can also create tags later on if you feel that you need more than what you initially created.

#### Tagging Images

With our tag created we can now do the manual task of tagging all of your images. Even though this task is manual it's still a lot easier to do this than trying to figure out the bounding boxes yourself.

You should see a screen like this: !\[\[Pasted image 20210829172214.png\]\]

You can scroll through all of the images on the left. The middle is where you'll do the tagging. You can use your mouse to drag the region that you want to become the bounding box. Once you do that to all of the items in the image you want to tag you can click on the tag on the tag page on the right

However, a nice shortcut is to bypass clicking on the tag that you want. If you look closer at the tag there is a number on the right of it.

!\[\[Pasted image 20210829172716.png\]\]

This is the keyboard shortcut that you can use to quickly tag your objects. So you can use the numbers corresponding to the tag to tag it instantly once you got the bounding box.

Here's a gif to show this in action for a couple of objects.

!\[\[Untitled Project.gif\]\]

Now you may noticed the images on the left has an icon that changes when going to an image and when tagging.

The orange icon indicates that the image has been visited but hasn't been tagged yet. !\[\[Pasted image 20210829173508.png\]\]

The green icon indicates tha tthe image has been tagged. !\[\[Pasted image 20210829173639.png\]\]

This is actually nice because you can quickly see which images haven't been tagged or even seen in VoTT yet.

Now that we have all of our images tagged we can export the data into a JSON format. This format will be used within Model Builder to send to Azure ML to build our object detection model.

On the top toolbar the last item will be the export button.

!\[\[Pasted image 20210829174725.png\]\]

Once you click this it will export all of the data to the Target Location that we defined earlier when setting up our project.

If you go into the Target Location you will see a lot of JSON files. Each JSON file will correspond to each image. However, the JSON file we care about is in the "vott-json-export" folder. This is the file that Model Builder will use. I'll reiterate this again during the data step of Model Builder since this can be easy to miss. I know because I've missed what JSON file to use several times earlier on.

This finishes what we need from VoTT. Now we'll jump to Visual Studio to create a Model Builder project to build our object detection model for us.

### Creating the Model Builder Project

In Visual Studio, we can create a blank Console project. This is so we can have a project to right click and go to Add -> Machine Learning.

From there we can create a new `mbconfig` file. If you haven't seen the `mbconfig` file before it's fairly new in Model Builder. It stores all the information for each step so if you need to close the Model Builder window or need to come back to it a few days later to tweak a model, you can double click on it and it keeps the same state that it had before.

We'll name our Model Builder project "StopSignDetection" and continue.

With the project created we now have to tell Model Builder what scenario we want. We'll select the Object Detection scenario. Notice that this scenario can only be trained in Azure ML. Currently, there is no local option.

!\[\[Pasted image 20210829180728.png\]\]

Since we can only train in Azure ML, we will have to set up a workspace. This is actually great because we can utilize GPUs if our machine doesn't have one. Of course my machine doesn't so the training will go much quicker than training on the CPU.

Model Builder really helps us out by having the workspace creation all within the GUI. If there's anything we need to create we can do it right there.

!\[\[Pasted image 20210829181110.png\]\]

If you need to create a new compute, I believe it will be best if the priority is set to "Dedicated".

!\[\[Pasted image 20210829181941.png\]\]

With the enviornment created we can click on the "Next Step" button to move on to the data step.

In this step we will use the JSON file from VoTT. Remember, this will be the JSON file that is in the "vott-json-export" folder. Once that's uploaded you should see a preview of the images and the names of the tags.

!\[\[Pasted image 20210829182252.png\]\]

From here we can click "Next Step" and this will take us to the training step. From here we just click the "Start Training" button to start the training process. This will prepare our Azure ML workspace, upload all of the images to Azure, and Azure ML will take those images and train the model for us.

Warning: After you're done training it is highly recommended to delete the compute that you created for Model Builder. This is to help you not incur any other costs from having the compute on after the training.

Once training is complete you should see something similar to the following output.

!\[\[Pasted image 20210829184615.png\]\]

You may notice that the accuracy is 100%. This is a bit of a red flag and may indicate overfitting, but we can do some testing to see how it performs. We'll continue with what we have, though, to show the rest of the process.

In fact we can do a test on a new image now. We can go to [Unsplash](https://unsplash.com/) and search for "stop sign" and get a new image that has a stop sign in it.

I found this image that we can do a quick test with from Unsplash: !\[\[Pasted image 20210829190512.png\]\]

Let's see how this performs on our model in the "Evaluate" step.

!\[\[Pasted image 20210829190615.png\]\]

This did really good and predicted the stop sign at a 98% confidence. That's pretty good for an image the model has yet to see.

#### Consume Model in Console App

The next step is the "Consume" step in Model Builder. This is optional but the templates here offer a very quick and easy way to consume the model. Especially since it shows how you can calculate the bounding boxes for the image. Let's add a console app and see how it looks.

To add the console app, click the **Add to Solution** link for it.

!\[\[Pasted image 20210830035507.png\]\]

This will show a popup asking what you want to name the product. We can name it "StopSignDetection\_Console" and click the button to add it to our current solution.
