# Developing ESP32 as lwm2m client #

Most of this tutorial is based on [Anjay Lwm2m client instructions](https://github.com/AVSystem/Anjay-esp32-client)

Documentation about the software for esp32 development can be found at:
https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/tools/idf-tools.html#xtensa-esp32-elf

## Prerequisites ##

1. laptop to work a console
2, Rasperry PI SBC with sshd running 
3. wifi router as connection between laptop and Raspberry PI SBC

##  Instructions ##

1. ssh to your Raspberry pi using "ssh dietpi@PI_IPADDR "
1. Clone Anjay-esp32-client repo to raspberry pi
   ```
   cd projects
   git clone --recurse-submodules --remote-submodules https://github.com/pschragger/Anjay-esp32-client
   ```
   should take less then a minute
2. Install build tools for ESP-IDF
   ```
   sudo apt-get install git wget flex bison gperf python3 python3-pip python3-setuptools cmake ninja-build ccache libffi-dev libssl-dev dfu-util libusb-1.0-0
   ```
   You will need to answer "Y" as the install calculates the size of the code.
   This install should take less then a minute.

3. Download esp-idf repo
   ```
   mkdir -p ~/esp
   cd ~/esp
   git clone -b v4.4 --recursive https://github.com/espressif/esp-idf.git
   ```
   This install will take a few minutes to complete since this is doing recursive install of several repos
4. Install esp-idf
   ```
   cd ~/esp/esp-idf
   ./install.sh all
   ```
5. Setup environment variables
   ```
   . ~/esp/esp-idf/export.sh
   ```
6. Test with Hello-world project
   - setup project directory
   ```
   cd ~/esp
   cp   -r $IDF_PATH/examples/get-started/hello_world .
   ```
  - Connect your ESP32 to your pi using the usb to mini usb connector
  -   Build the hello_world project
  
  ```
  . ~/esp/esp-idf/export.sh
  cd $IDF_PATH/examples/get-started/hello_world 
  idf.py set-target esp32
  idf.py fullclean
  idf.py menuconfig
  ```
  check out all the options but do not change anything yet
  Save [S] then quit [Q] the config menu
  now build the hello-world program

  ```
  idf.py build
  ```
  
  - find the usb that the esp is connected to
  
  ```
  lsusb
  ```
  
  my result shows:
  
  ```
  Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
  Bus 001 Device 003: ID 1a86:7523 QinHeng Electronics CH340 serial converter
  Bus 001 Device 002: ID 2109:3431 VIA Labs, Inc. Hub
  Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
  ```
  
  So I choose the CH340 serial converter
  
  -  find the port number /dev/ttyUSB[PORTNUMBER]
  
  ```
  ls -l /dev/ttyUSB*
  ```
  
  showed
  
  ```
  crw-rw---- 1 root dialout 188, 0 Jul  6 16:08 /dev/ttyUSB0
  ```
  
  So I am using port = 0 in the command  idf.py -p port flash
  
  ```
  sudo chmod 666 /dev/ttyUSB0
  idf.py -p 0 flash
  ```
  this will load the code onto the esp device.

  Now run the monitor to see its communications
  ```
  idf.py -p /dev/ttyUSB0 monitor
  ```
  



Install ESP-IDF and its dependencies on your computer. Please follow the instructions at https://docs.espressif.com/projects/esp-idf/en/v4.4/esp32/get-started/index.html up to and including the point where you call . $HOME/esp/esp-idf/export.sh
The project has been tested with ESP-IDF v4.4, but may work with other versions as well.
3. Run idf.py set-target esp32 in the project directory
4. Run idf.py menuconfig
5. Navigate to Component config/anjay-esp32-client:
select one of supported boards or manually configure the board in Board options menu
configure Anjay in Client options menu
configure WiFi in Connection configuration menu
Run idf.py build to compile
Run idf.py flash to flash
NOTE: M5StickC-Plus does not support default baudrate, run idf.py -b 750000 flash to flash it
The logs will be on the same /dev/ttyUSB<n> port that the above used for flashing, 115200 8N1
You can use idf.py monitor to see logs on serial output from a connected device, or even more conveniently idf.py flash monitor as one command to see logs right after the device is flashed


## Install C compiler ##

1. Use dietpi-software tool on the PI to install gcc on the pi

## build install utils and libraries to build anjay

``` 
sudo apt-get install git build-essential cmake libmbedtls-dev zlib1g-dev
```
## Build and install the avs_commons system ##

1. clone the avs_commons repo
```
cd ~/projects
git clone https://github.com/AVSystem/avs_commons
```
2. Build and install the avs_commons library
```
cd ~/projects/avs_commons
cmake . && make && sudo make install
```

## Build and install the ANJAY Library ##



1. retrieve the anjay code
```
cd ~/projects
git clone https://github.com/AVSystem/Anjay
```
2. build the anjay code
```
cd ~/projects/Anjay
git submodule update --init
cmake . -DDTLS_BACKEND="mbedtls"
make -j
```
3. try the anjay demo

-- view the leshan test server at https://leshan.eclipseprojects.io/#/clients
-- run the demo and see your machine listed in the leshsn server
```
./output/bin/demo --endpoint-name $(hostname) --server-uri coap://leshan.eclipseprojects.io:5683
```


### Installing Anjay library objects
https://avsystem.github.io/Anjay-doc/Compiling_client_applications.html
```
cd ~/projects/Anjay
cmake -DCMAKE_INSTALL_PREFIX=/home/dietpi/projects/Anjay-esp32-client . && make &&  make install
```

## Build the ANJAY client ##

1. move to the anjay-esp32-client directory that was clone earlier
```
cd ~/projects/Anjay-esp32-client
```

2. Setup the local enironment for using the esp tools
```
cd ~/projects/Anjay-esp32-client
. $HOME/esp/esp-idf/export.sh
idf.py set-target esp32 
```
3. setup the device requirements
     ```
     cd ~/projects/Anjay-esp32-client
     idf.py menuconfig
     ```
     - navigate to "Component->" and select config/anjay-esp32-client:

     Setup your config to be:
     (anjay-esp32-client) Endpoint name
     (coap://{LESHAN_SERVER_IP}:5683) Server URI
     Choose socket (UDP)  --->
     Choose security mode (Non-secure connection)  --->


     - navigate to "Board - > "

     -  navigate to "Client options ->"
          - Change Server URI from coaps://try-anjay.avsystem.com:5684 to
	    coaps://YOUR_LESHAN_SERVER_IP_ADDR:
	    I used coaps://192.168.8.224:5684
	    Your IP my be different
     -  navigate to "WiFi ->"
4. Build the code for the device using
     ```
     cd ~/projects/Anjay-esp32-client
     idf.py build
     ```

5. find the port and flash the device

   - find port number by using
   ```
   ls -l /dev/ttyUSB*
   ```
   it returns something like
   crw-rw---- 1 root dialout 188, 0 Jul  8 06:17 /dev/ttyUSB0
   so the port number is 0

   - change the port number in this line to load the code
    idf.py -p  port_number flash
    with port nubmer = 0 I used:
     ```
     cd ~/projects/Anjay-esp32-client
     sudo chmod 666 /dev/ttyUSB0
     idf.py -p 0 flash
     ```
     You should see flashing


   



   
    
