# Building an LWM2M clients on RPI for ESP32


What we are doing in this tutorial is building an LWM2M Device using idf and the ANJAY Libraries

To perform this tutorial is is expected that your will start by logining onto your PI as dietpi.


## Install C compiler ##

1.  Test to see if the c compiler is already installed  using 

```
gcc -v
```

2. If installed then go to the next step if not then use 

```
sudo dietpi-software`
```
on the PI to install gcc.

## Install utils and libraries to build anjay ###

You can peform this install in any directory.  YOu are using the linux apt-get command a root using sudo.

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

You have now built and installed the avs_commons library into share directories

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
          - Change Server URI from coaps://try-anjay.avsystem.com:5684 to  coaps://YOUR_LESHAN_SERVER_IP_ADDR:5684
	    I used coaps://192.168.8.224:5684
	    Your IP my be different
	    
     -  navigate to "WiFi ->"
         To enter your IOT ROUTER WIFI SSID and key to allow the esp32 acccess to your router and PI.
	 
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

  6. check your leshan server for the registration of your esp32.


