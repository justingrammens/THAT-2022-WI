This directory contains the source and documentation covered in the hello_world exmaple we covered in the workshop.

# Example

We will be using MAchine Learning to predict the values of a sine wave. This is the class hello_world application of feeding an algorithm data and letting it predict the values of Y vs X.

# Hardware

Our examples in this workshop will be using the Arduino Nano 33 BLE Sense. 

#Tensorflow Lite

We will be running the explanes using TensorFlow Lite for Microcontrollers.

https://www.tensorflow.org/lite/microcontrollers

#Run Tests

1. Setup your environment using Docker
1.1 Launch a new docker container using this:
1.2 `docker run -it --name ubuntu ubuntu:latest bash`

2.1 If you have docker already installed.
2.2 docker start ubuntu
2.3 docker exec -it ubuntu bash

Intall the foloowing packages:

apt-get update
apt-get install git
apt-get install make
apt-get install wget
apt-get install unzip
apt-get install python3
apt-get install python3-pip
Pip3 install Pillow
apt-get install curl
git clone https://github.com/tensorflow/tflite-micro.git
cd tflite-micro

This will make supporting 3rd party libraries
`make -f tensorflow/lite/micro/tools/make/Makefile third_party_downloads`

This will run your tests.
`make -f tensorflow/lite/micro/tools/make/Makefile test_hello_world_test`

Test that is run is here. Testing that output is what you would expect.
https://github.com/tensorflow/tflite-micro/blob/main/tensorflow/lite/micro/examples/hello_world/hello_world_test.cc

Success output would read:

`tensorflow/lite/micro/tools/make/gen/linux_x86_64_default/bin/hello_world_test '~~~ALL TESTS PASSED~~~' linux
Testing LoadModelAndPerformInference
1/1 tests passed
~~~ALL TESTS PASSED~~~`

# Next Steps

Let's go go through the steps of training the model. The output of this process is having a 

# Train the Model

view the source code here:

https://github.com/justingrammens/machine_learning/blob/master/tensorflow/lite/micro/examples/hello_world/train/train_hello_world_model.ipynb

Launch a Google Colab that will allow you to use Google Cloud servers to download the source data and train a neural network to make predicitions.

Note at the end of this exercise you will use TensorFlow lite to output a special, space-efficient format for use on memory-constrained devices.

# Run It On Hardware!

1. Open Arduino IDE
2. Install the TensorFlow - Tools -> Manage Libraries
3. Search for and install "TensorFlow Lite"
4. Open File -> Examples -> Arduino TensorFlow Lite
5. Look at sine_model_data.cpp - this is the quantized model lives.
6. Build and run the application
7. Revew the serial monitor & more interestingly - serial plotter

