By now in this workshop you should have completed the following:

* Created an [Edge Impulse](https://www.edgeimpulse.com/) account
* Verified data is being sent and received from your hardware to the cloud


# Example

In this example we will use the microphone on the [Arduino Nano 33 BLE Sense](https://store-usa.arduino.cc/products/arduino-nano-33-ble-sense) development board to learn how audio classification works and the intelligence that go into edge processing devices to response to "OK Google" and "Hey Siri"

We will be loosely following this example:

[https://docs.edgeimpulse.com/docs/tutorials/responding-to-your-voice](https://docs.edgeimpulse.com/docs/tutorials/responding-to-your-voice)

Buckle-in and ride along as we exercise the power of the Edge Impulse platform.

# Create Project

First we must create project in Edge Impulse

1. In the browser, click your name in the upper right
2. Select "create new project"
3. Give it the name of "Responding Voice"

# Capture Audio

Once the projects setup, let's work on getting data!

1. Select "Devices" link on the left to make sure your device is connected.
2. If the daemon on your board is running you should see the board shows up. 
    1. If you device is not showing up.
    2. You will want to make sure you daemon is running by running the `edge-impulse-daemon --clean` command.
    3. If you have not setup your daemon, please look [here](https://github.com/justingrammens/THAT-2022-WI/tree/main/EdgeImpulse/Nano-33-Sense#user-content-edge-impuse-cli) to go through the toolchain setup.
3. Select the "Data Acquisition" link on the left
4. Choose the "Built-in Microphone" options.
    1. Take note of the defaults regarding the length, quality and label as well.
5. Once you have recorded a sample
    1. Click on the "..." for the sample audio and select "split sample"
    2. Select the pieces of the audio you wish to have sampled
    3. Select "Split" and multiple samples will be created in the collected data list
    
# Reflection

Let's pause here for a moment to understand what we have completed and what we are trying to accomplish.

1. We have built a sounds ingestion mechanism that will allow us to capture keywords, such as "yes" and "no" and tag those specific audio files.
2. The goal of this project is build a system that will then be trained well enough to be able to capture the different classes of sounds that are heard.
3. Training is using a subset of the [Google Speech Commands Dataset](https://ai.googleblog.com/2017/08/launching-speech-commands-dataset.html) so it's faster to train.

Trying to capture enough data by ourselves to distinguish the differences will take a lot of time. Google Speech commands dataset to the rescue! Following the steps on [Keyword Spotting](https://docs.edgeimpulse.com/docs/pre-built-datasets/keyword-spotting) from the Edge Impulse documentation

It contains 25 minutes of data per class, split up in 1 second windows, sampled at 16,000Hz. The dataset contains:

* Yes - one second samples with only the word "yes" in it.
* No - one second samples with only the word "no" in it.
* Unknown - one second samples of other words.
* Noise - one second samples of background or static noise.

**NOTE**: It might be obvious to some, but when you are trying to classify audio into a "yes" or "no", if those are the only classes you have loaded, other random noises and samples will be picked up and put into one of those of those two buckets. To effectively do this classification you actually need 4 classes of audio: "yes", "no", "unknown - i.e. word that are NOT yes or no" and "noise - which are NOT any words."

Let's load the data!

1. Download the [data](https://cdn.edgeimpulse.com/datasets/keywords2.zip)
2. Unzip the file
    1. Upload using the browser OR
    2. Upload using the CLI.
    
```
$ edge-impulse-uploader --clean
$ edge-impulse-uploader --label noise --category split noise/*.wav
$ edge-impulse-uploader --label unknown --category split unknown/*.wav
$ edge-impulse-uploader --label no --category split no/*.wav
$ edge-impulse-uploader --label yes --category split yes/*.wav
```

# Time To Train

Edge Impulse uses the concept of an "impulse" as a predefined method by which the data will be processed and the type of NN to be used. From the documentation, "For this tutorial we'll use the "MFCC" signal processing block. MFCC stands for Mel Frequency Cepstral Coefficients. I will gloss over the details, but basically, you want to select:

1. Create impulse
2. Add a *Time series data*
3. Add an *Audio (MFCC)*
4. Add a *Classification (Keras)*
5. Select *Save Impulse*

# Configure the MFCC block

You can pull images of the spectrogram and details from the [documentation on Edge Impulse](https://docs.edgeimpulse.com/docs/tutorials/responding-to-your-voice#5.-configure-the-mfcc-block)

Further details on Spectrograms can be found [here](https://en.wikipedia.org/wiki/Spectrogram)

From the Edge Impulse Documentation:

```
vertical axis represents the frequencies (the number of frequency bands is controlled by 'Number of coefficients' parameter, try it out!), and the horizontal axis represents time (controlled by 'frame stride' and 'frame length').
```

Try changing the default of "13" to "23" and regenerate see what happens!

# Feature Explorer

One of the really cool features of Edge Impulse is allowing us to explore the features of the dataset from right inside the browser. Looking at the DSP result for each sample is impossible for human to read, but for NN it's no too much work. After your impulse has been created:

1. Click on *MFCC* on the left sidebar
2. Select *Generate Features*

This will give you a graphical representation of the features for the data you are reviewing

# Neural Network Classifier

Steps for this part of the exercise can be taken from [here](https://docs.edgeimpulse.com/docs/tutorials/responding-to-your-voice#6.-configure-the-neural-network)

There's lots of interesting background on neural networks and layers. Much of this was covered in the first exercise as part of the workshop, but if you are jumping in late this is still extremely valuable. The short story is that:

* The system will feed in sample of data, checking how far the network's output is from the correct answer, and adjusting the neurons' internal state to make it more likely that a correct answer is produced next time.
* The default neural network architecture provided by Edge Impulse will work well for our current project, but you can also define your own architectures.
* You'll want to enable 'Data augmentation'. When enabled your data is randomly mutated during training.

Once you have everything set click "Start training".

As you look at the output, does the format of this look familiar? If you worked on the first exercise during the workshop, you should notice that the feedback during training output looks nearly identical to the training he we did in real-time in the [Hello ML World](https://github.com/justingrammens/machine_learning/blob/master/tensorflow/lite/micro/examples/hello_world/train/train_hello_world_model.ipynb) example I created.

# Test our Model

Now that we have the model built we can test it! Edge Impulse by default put aside 20% of our data for the test data set. This is data that the model has not seen and we can use this to validate whether the model is working on unseen data. Perform the following steps:

* Select *Model Testing* in the left menu
* Select the *Classify All* button
* Wait for the system to complete. Testing will typically take a few minutes.

Once you are happy with the model performance, we are ready for the final step! Deploying to device.

# Deploy to Device

The final step in this exercise, is deploying this model to the Arduino Nano 33 BLE Sense. Edge Impulse has a method to do a one click deploy to your device using the *Deployment* option from the web. To accomplish this:

1. Select *Deployment* from the left menu
2. Under the *Build Firmware* section.
    1. Select the *Arduino Nano 33 BLE Sense*
    2. Select "Build Model"
    3. It can take many minutes for the build to complete
3. After building is completed you'll get prompted to download a .zip file.

# Run on Device

To Run on device you will want to flash with the binary you downloaded.

1. Unzip the file.
2. Run the script for your operating system. For me it was:
`./flash_mac.command`
3. You should see the script recognize your board and upload the binary.
4. When complete, you should see something similar to

```
Flashed your Arduino Nano 33 BLE development board.
To set up your development with Edge Impulse, run 'edge-impulse-daemon'
To run your impulse on your development board, run 'edge-impulse-run-impulse'
```

If you want to see it streaming, you can connect to the board's newly flashed firmware over serial.

`$ edge-impulse-run-impulse --continuous`




