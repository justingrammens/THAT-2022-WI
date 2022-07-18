While using custom hardware is the most realisitic exmaple of how intelligence devices for TinyML and AIoT are being deployed, sometimes it's just as easy to use an exisiting off the shelf piece of hardware that contains all of the sensors, visual and audio input and output and controller built together.

One of these devices is right in your pocket and something you use each and every day - your mobile phone!

Getting sesors to be read and hardware to play nice together, connected to the internet and compling code can sometimes not be for the faint of heart and when time and ease is of the essence, a smart phone is perfect piece of hardware to use in development. I would even argue that the more you can use a device already in mass production with no hardware changes, the better off you are.

# Mobile Phone Setup

What's really cool about Edge Impulse is that the type of device used for acquision is generally irrelevant. The interface to push data in is an RESTful API so as long as the device has a standard interface to push data it can be used.

Here's quick totorial on how you setup your phone to push data. No app is needed! It's all loaded through a mobile webview.

[https://docs.edgeimpulse.com/docs/tutorials/image-classification/image-classification-mobile-phone](https://docs.edgeimpulse.com/docs/tutorials/image-classification/image-classification-mobile-phone)

# Let's Get Started!

We will walk through 2 examples.

## Images - Hand Signs

1. Create a new project called "Phone-HandSigns"
2. Select "Image data" as the type of data you will be collecting
3. Select "Devices" on left sidebar
4. Click on the "Connect a New Device" button
5. Choose "Use Your Mobile Phone"
6. You will be presented with QR Code
7. Scan the QR Code
8. Select "Collecting Images?"
9. You will be taken to webpage where you will grant access to your camera

In this example we are going to try and use computer vision to have the model be able to select from a series of multiple fingers on your hand. You will be labeling 3 different image types showing figures on your hand.

* One finger
* Two fingers
* Three fingers

Here are the steps to acquire data.

1. Select the "Label" button on the screen
2. Type in "one"
3. Point the camera at your hand and show your pointer finger
4. Select "Capture"
5. Verify the images are appearing on the "Data Acquision" page in your browser on your computer. It's pretty cool how you cansee these images coming in near realtime
6. Repeat with a label of "two" and a label of "three.

The more images and variaitions you can produce the better! For this exmaple, let's try starting out with 10 for each label. You will likelys ee that this is far too few and as a 2nd part of this exercise we will work on retraining the model with additional images.

Once you have your images loaded, follow the steps like we did prior

1. Select "Impluse Design"
2. Select "Create Impulse"
    1. Select the defaults for image classification - Edge Impulse knows that you are using a mobible phone and will attempt to optimize for this siutation
3. Select the defatuls and generate your features.
4. Finally, select "Transfer Learning" and start training and use the default vision network, [MobileNetV2](http://ai.googleblog.com/2018/04/mobilenetv2-next-generation-of-on.html).
5. When the output is completed we are ready ot start classifying!

Return to your phone and (if neede) refresh the web browser page. 

1. Scroll down on the page and select the button entitled "Switch to Classifcation Mode"
2. You will see a message that that the application is downloading the model
3. After you are prompted to give access to camera, select "Give access to the camera"

Voila! You are now in inferencing mode.

Point the camera at your hand and start showing different digits using your fingers. You will likely find that while, it's close there many times where it returned "uncertain" or it's found one of digits more often than others. How do we fix this? Let's try training more data!

### Retrain Model

Since we are getting not so hot results, let's spend some time retaining the model wit additional images. Details on how Edge Impluse allows for model retraining can be found [here](https://docs.edgeimpulse.com/docs/edge-impulse-studio/retrain-model). In essence, it states that

> "It uses known parameters from your selected DSP and ML blocks then uses them to automatically regenerate new features and retrain the Neural Network model in one single step."

Let's return to the mobile web browser and

1. If needed, refresh the screen
2. Select "Switch to Data Collection Mode"
3. Capture atleast 15 more images for each label

Once you feel like you have accounted for a good number of variations in light, shadows, angels, etc there is nothing else you need to do but retrain your model.

1. Return to the Edge Impulse dashboard
2. Select "Retain Model" from the menu sidebar.
3. Select the "Train Model" button

You will see the build output showing the system going through the default of 20 training cycles and optimizing the output.

Once it is complete

1. Return to your mobile phone browser
2. Refresh the screen (if needed)
3. Change to "Classification Mode"
4. Test using the updated model

If the project is working as expected you should see better returns as you show different hand signs of one, two or three figers to your phone's camera and see the results displayed in realtime.

### Additonal Exercise

As an additonal exercise for the reader. There are free and open source datasets that contain a variety of sign language images.

* [https://www.kaggle.com/datasets/ardamavi/sign-language-digits-dataset](https://www.kaggle.com/datasets/ardamavi/sign-language-digits-dataset)
* [https://www.kaggle.com/datasets/datamunge/sign-language-mnist](https://www.kaggle.com/datasets/datamunge/sign-language-mnist)

Try using one of these datset to see if you can improve your accuracy!

# Continous Motion Recognition

This example will follow the format that was shown in prior workshop by training a model to recognize X,Y and Z sensrs as they sense contunious motion using your mobile phone.

Start here with these steps [https://docs.edgeimpulse.com/docs/tutorials/continuous-motion-recognition](https://docs.edgeimpulse.com/docs/tutorials/continuous-motion-recognition)

Like with the image gesture recognizer:

1. Create a new project named "phone-motion"
2. Select "Acceleromter Data" as the type of data you will be collecting
3. Select "Let's Get Started"
4. Select "Devices" on left sidebar
3. Click on the "Connect a New Device" button
4. Choose "Use Your Mobile Phone"
5. You will be presented with QR Code
6. Scan the QR Code
7. Select "Collecting Motion?"

You will be taken to page where you can begin to start recording motions. In this example we will collect the following data

* Idle - just sitting on your desk while you're working.
* Snake - moving the device over your desk as a snake.
* Wave - waving the device from left to right.
* Updown - moving the device up and down.

Use your phone to enter one of the labels to start loading data. NOTE: MAke sure that you select *"Split Automatically (80/20)"* while uo are training to ensure that 20% of the data is withheld in test dataset.

Once you had completed creating the dataset, you can follow the same steps above to *create an Impulse*.

## Load Sample Data

If you do not want to try and collect data for the select gestures and to save you time and effort, Edge Impulse a created a sample dataset!

You can learn and download it here:

[https://docs.edgeimpulse.com/docs/pre-built-datasets/continuous-gestures](https://docs.edgeimpulse.com/docs/pre-built-datasets/continuous-gestures)

Be sure to follow the steps outlined of downloading it and then using the CLI to run

```
$ edge-impulse-uploader --clean
$ edge-impulse-uploader --category training training/*.cbor
$ edge-impulse-uploader --category testing testing/*.cbor
```

Once it's done, contunie with exploring, classifying and applying anomoly detection while deplying back to your mobile phone.

# Conclusion

There's so many fun and interesting applications that one can apply by bringing Machine Learning to the edge and using Edge Impluse allows anyone with a mobile phone to start testing out concepts with a very barrier to entry. No prior prgramming or Data Science experience is needed. Enjoy!