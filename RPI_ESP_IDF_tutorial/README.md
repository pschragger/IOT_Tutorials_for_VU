# Install software to build ESP32 code #

Most of this tutorial is based on [Anjay Lwm2m client instructions](https://github.com/AVSystem/Anjay-esp32-client).  The actual building of the LWM2M clients will be done in [RPI_BUILD_LWMWM_DEVICE](../RPI_BUILD_LWM2M_DEVICE) tutorial

Documentation about the software for esp32 development can be found at:
[docs.espressif.com](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-guides/tools/idf-tools.html#xtensa-esp32-elf)
and
[Intro to esp-idf](https://www.espressif.com/en/products/sdks/esp-idf)


Most of this tutorial is based on [Anjay Lwm2m client instructions](https://github.com/AVSystem/Anjay-esp32-client).

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
  
  ### RESULT
  A message counts down from 10 seconds and then prints:
Hello world!
This is esp32 chip with 2 CPU core(s), WiFi/BT/BLE, silicon revision 1, 2MB external flash

This indicates that your idf build code is complete and your esp32 was flashed with the hello world code.


To stop the monitor service open another command window on your laptop and ssh to your pi as dietpi

then run 

```
ps aux | grep monitor
```
it should return the process that is running on your pi
mine returned
```
dietpi     19169  0.1  1.0  50616 39504 pts/0    S+   21:13   0:01 python /home/dietpi/esp/esp-idf/tools/idf.py -p /dev/ttyUSB0 monitor
dietpi     19172  2.2  0.4 391560 16176 pts/0    Sl+  21:13   0:15 /home/dietpi/.espressif/python_env/idf4.4_py3.9_env/bin/python /home/dietpi/esp/esp-idf/tools/idf_monitor.py -p /dev/ttyUSB0 -b 115200 --toolchain-prefix xtensa-esp32-elf- --target esp32 --revision 0 /home/dietpi/esp/esp-idf/examples/get-started/hello_world/build/hello_world.elf -m '/home/dietpi/.espressif/python_env/idf4.4_py3.9_env/bin/python' '/home/dietpi/esp/esp-idf/tools/idf.py' '-p' '/dev/ttyUSB0'
dietpi     34593  0.0  0.0   7396   624 pts/1    S+   21:24   0:00 grep monitor
```
In this case the process id (pid) is 19172, so I will run :  "kill  19172" to stop the process in the other command window.


## Blink project ##

1. copy the blink code to a new directory 
   ```
   . ~/esp/esp-idf/export.sh
   cd ~/esp
   cp   -r $IDF_PATH/examples/get-started/blink .
   ```
2. Connect your ESP32 to your pi using the usb to mini usb connector
  -   Build the blink project
  
  ```
  . ~/esp/esp-idf/export.sh
  cd $IDF_PATH/examples/get-started/blink 
  idf.py set-target esp32
  idf.py fullclean
  idf.py menuconfig
  ```
  check out all the options but do not change anything yet
  Save [S] then quit [Q] the config menu
  now build the blink program
  
3   Build the blink code using
    ```
    cd $IDF_PATH/examples/get-started/blink 
    idf.py build
    ```
    
4. flash the esp32 as you did with the hello_world project

5. check the monitory 

6. change the blink period by looking at the code and changing the sleep times.
    
  
  a. Look at the code
  ```
    cd ~/esp/esp-idf/examples/get-started/blink/main
    nano blink_example_main.c
   ```
   what we find is a constant called    CONFIG_BLINK_PERIOD
   
  b. now we will use the menuconfig to change the settings
   ```
   cd ~/esp/esp-idf/examples/get-started/blink
   idf.py menuconfig
   ```
    i. navigate to the "Example Configuration" and hit enter
    ii. there are two settings Blink GPIO number and Blink Perion in ms
    iii. change the blink period in ms to 5 seconds ( 5000 ) 
    iv. save the results to the sdkconfig (S then Q )
7 remember to rebuild and reflash 


   
    


   
    
