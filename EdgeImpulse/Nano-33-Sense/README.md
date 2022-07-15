#Exmaple

In this example we will use the Arduino Nano 33 Sense to listed to audio using the oboard microhone and then classifiy audio, train the model and then deply back to the device.

#Edge Impulse

If have not already, create a free account at https://www.edgeimpulse.com/

After logging:
1. Click on your name in the upper right
2. Create a new project called "Nano-Sense-Accel"

We will work off the steps defined here:

https://docs.edgeimpulse.com/docs/development-platforms/officially-supported-mcu-targets/arduino-nano-33-ble-sense

# Edge Impuse CLI

Edge Impulse has a CLI which will install a deamon to send data right from the Nano 33 Sense to your account. Follow the steps to instal on your operating system here:
https://docs.edgeimpulse.com/docs/edge-impulse-cli/cli-installation

# Arduino CLI

Install the Arduino CLI right from the the IDE. Instructions are covered:
https://arduino.github.io/arduino-cli/0.25/installation/

I just installed it via homebrew on my mac.

# Firmware

Download the Edge Impulse firmware:
https://cdn.edgeimpulse.com/firmware/arduino-nano-33-ble-sense.zip
and then run the script for your particual OS.

This script uses the arduino CLI ( arduino-cli ) to install the arduino:mbed package and then ujpload the:
`arduino-nano-33-ble-sense.ino.bin` file that has been built in the current directory

#Install Kyys & Verify

run `edge-impulse-daemon --clean`
and follow the prompts
using the --clean will force you to relogin and select a differnet project.
then verify the device is connected by going to the URL that is outputted. Likely something like this:
https://studio.edgeimpulse.com/studio/select-project?autoredirect=1

# Test

You now can start collecting data by moving the board and observing the X,Y & Z values changing!


# Alternative Uploader
Note! Thaere is also a way to have any Arduino program forward the data to the cloud by following these steps.
https://docs.edgeimpulse.com/docs/edge-impulse-cli/cli-data-forwarder
For now however, we will not use this generic data forwarder.
