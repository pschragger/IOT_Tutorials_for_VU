# BLINK TUTORIAL #
Microcontroller First Tutorial -  Blink and LED
## Purpose ##
This tutorial outlines how to :
1. Setup a working development environment on a personal computer or Raspberry PI
1. Wire up your experimental setup on a solderless prototype board or use direct wiring
1. Connect the pc or RPI to the microcontroller
1. Compile software and flash compiled code onto the microcontroller
1. Run the software and view console
1. Change code Recompile and run again
## Setup ##
What you will need for this tutorial:
1. A laptop or a rpi working as a desktop  with connection to the internet (WIFI or ethernet)
1. 1 esp8266 nodemcu or other arduino equivalent
1. Solderless Protype board
1. 2 Wires 
1. 1 USB to usb mini connector for connecting to arduino 

## Tutorial Steps
1. Setup a working development environment
  1. Install arduino IDE
  
    On your laptop/desktop install arduini ide from the arduino.cc software URL https://www.arduino.cc/en/software
    Once installed run the app.  It should look like:
    
  2. Install esp board support into your IDE:
    1. In your Arduino IDE, go to Arduino> Preferences for MACOS and File>Preferences for Windows.
    2. Enter http://arduino.esp8266.com/stable/package_esp8266com_index.json into the “Additional Boards Manager URLs” field as shown in the figure below. Then, click the “OK” button

      Note: if you are using an ESP32 board you can install both URLS with a comma as follows:
      > https://dl.espressif.com/dl/package_esp32_index.json,    
      > http://arduino.esp8266.com/stable/package_esp8266com_index.json 
    3. Open the Boards Manager. Go to Tools > Board > Boards Manager
    4. Search for ESP8266 and press install button for the “ESP8266 by ESP8266 Community“, be sure to check to see that “ESP8266 by ESP8266 Community” is marked as installed.
2. Wiring your test board
Connect an LED to your ESP8266, as shown in the following schematic diagram. The LED should be connected to GPIO 2 (D4).

![Blink Circuit Diagram](/blink_tutorial/images/blink_circuitdiagram.png)

Once wired it should look something like:

![Blink Wiring Photo](/blink_tutorial/images/blink_wiring_cropped.png)
