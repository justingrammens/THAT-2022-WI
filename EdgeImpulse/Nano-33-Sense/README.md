This directory contains the source and documentation covered in the EdgeImpulse example that we covered in the hands-on workshop.

# Exmaple

In this example we will use the [Arduino Nano 33 BLE Sense](https://store-usa.arduino.cc/products/arduino-nano-33-ble-sense) to listen to audio using the on-board microhone and then classifiy audio, train a model and then deploy it back to the device.

# What Is Edge Impuse?

Edge Impulse provides a cloud hosted develpoment platform where anyone can build advanced embedded machine learning applications without a PhD. They are making the process of building, deploying, and scaling embedded ML applications easier and faster than ever. Best of all, they offer a freemium subscription where anyone can get started!

# Edge Impulse

If have not already, create a free Edge Impuse account at [https://www.edgeimpulse.com/](https://www.edgeimpulse.com/)

After logging in:

1. Click on your name in the upper right
2. Create a new project called "Nano-Sense-Accel"

We will now work off the steps of their pre-defined examples here:

[https://docs.edgeimpulse.com/docs/development-platforms/officially-supported-mcu-targets/arduino-nano-33-ble-sense](https://docs.edgeimpulse.com/docs/development-platforms/officially-supported-mcu-targets/arduino-nano-33-ble-sense)

# Edge Impuse CLI

Edge Impulse has a CLI which will install a deamon to send data right from the Nano 33 Sense to your account. Follow the steps to install on your operating system here:

[https://docs.edgeimpulse.com/docs/edge-impulse-cli/cli-installation](https://docs.edgeimpulse.com/docs/edge-impulse-cli/cli-installation)

# Arduino CLI

Install the Arduino CLI right from the the IDE. Instructions are covered:
[https://arduino.github.io/arduino-cli/0.25/installation/](https://docs.edgeimpulse.com/docs/edge-impulse-cli/cli-installation)

I just installed it via homebrew on my MAC with the commands:

```
brew update
brew install arduino-cli
```

# Firmware

In order to run this example you will need to load firmware created by Edge Impulse.

Download the Edge Impulse firmware:

[https://cdn.edgeimpulse.com/firmware/arduino-nano-33-ble-sense.zip](https://cdn.edgeimpulse.com/firmware/arduino-nano-33-ble-sense.zip)

and then run the script for your particular OS.

This script uses the arduino CLI ( arduino-cli ) to install the arduino:mbed package and then upload the:
`arduino-nano-33-ble-sense.ino.bin` file that has been built in the current directory

Once this is complete your hardware will now be able to talk with the Edge Impulse cloud.

#Install Keys & Verify

Need to now authenticate to your account and project. Run the following:

`edge-impulse-daemon --clean`

and follow the prompts

using the --clean will force you to re-login and select a different project.

then verify the device is connected by going to the URL that is outputted. Likely something like this:

[https://studio.edgeimpulse.com/studio/select-project?autoredirect=1](https://studio.edgeimpulse.com/studio/select-project?autoredirect=1)

# Test Receiving Values

You now can start collecting data by moving the board and observing the X,Y & Z values changing!


# Alternative Uploader

Note! We leave this as an exercise for the reader, but there is also a way to have *any* Arduino program forward the data to the Edge Impulse by following [these steps](https://docs.edgeimpulse.com/docs/edge-impulse-cli/cli-data-forwarder).

For now however, we will not use this generic data forwarder.