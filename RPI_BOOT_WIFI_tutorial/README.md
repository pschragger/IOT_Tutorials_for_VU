# Setting up dietpi on a Raspberry PI via wifi  static ip #
In this tutorial we will set up a Raspberry Pi using DIET_PI

## Purpose ##
Install DietPi on a Raspberry PI via WIFI ready to install other software

1. Install a version of a Linux operating system on the PI

## Setup ##
What you will need for this tutorial:
1. A laptop or desktop to act as your development platform
1. A Raspberry PI ( 3 B+ or 4 )
1. A Class 10 microSD 8gb or better (max 32gb) https://www.mymemory.co.uk/blog/the-best-memory-cards-for-raspberry-pi/
1. WIFI ROUTER and configuration info

## Tutorial Steps ##
### 1. Setup your working environment on your laptop ###
1. Prepare your device to with an archiver extractor - on windows I used BreeZip http:www.breezip.com but others are available from https://www.7-zip.org . On MacOS V11 it extracted using tools already installed but an unarchiver is available at  https://theunarchiver.com for your mac if extraction is not working.
2. Prepare you laptop with a sd card writer such as: balenaEtcher https://www.balena.io/etcher/

### 2. Download and configure  DietPI on your laptop

1. Download and extract the DietPi disk image
1. Go to https://dietpi.com/#download and select the RaspberyPI Images
1. On the new page choose the download for your targeted pi
    1. How do you know which version will work? - Check https://en.wikipedia.org/wiki/Raspberry_PI -  Raspberry Pi 3 Model B was released in February 2016 with a 1.2 GHz 64-bit quad core ARM Cortex-A53 processor which is an  ARMv8-A https://en.wikipedia.org/wiki/ARM_Cortex-A53.
    2. So I am choosing to use:
      ARMv8 64-bit image:Download - As of Sep 19, 2021 that link is  https://dietpi.com/downloads/images/DietPi_RPi-ARMv8-Bullseye.7z
1. Extract the DietPi_RPi-ARMv8-Bullseye.7z -
1. Write the DietPi image to your SDcard using balenaetcher or other flashing software
      1. open balenaetcher
      2. select the dietpi image
      3. Select the targeted card ( that was inserted into your laptop)
      4. Press flash ( you may have to put in your credentials. I also had to  try a second time to make it work)
      5. wait until the flash finished and verifies the copy
      6. close balenaetcher
1. Edit the extracted DietPi_RPi-ARMv8-Bullseye to make an install of Dietpi so installation needs: wifi login
1.  Login your laptop to the targeted WIFI so that you are sure of the credentials needed for the laptop use:
>  SSID: Dr-S-IOT-G
>  Password: iotisfun
find your ip address: it will depend on your laptop usually found in the network properties it should be in form of 192.168.10.N
then set your Raspberry PI to use a fixed ipaddress using the N from your laptop address and adding 100.
As an example: my laptop is using 192.168.10.3 , so I will configure my PI to 192.168.103
	   Save this ip address to update the config files
2. Copy the [dietpi.txt](./src/wifi_boot/dietpi.txt) and [dietpi-wifi.txt](./src/wifi_boot/dietpi-wifi.txt) from the [src/wifi_boot](./src/wifi_boot) directory of this repo
3. open the new dietpi.txt from my repo for editing.  update the line

> AUTO_SETUP_NET_STATIC_IP=192.168.10.{HOSTNUMBER}
to
> AUTO_SETUP_NET_STATIC_IP=192.168.10.(your host number)
example for my setting
> AUTO_SETUP_NET_STATIC_IP=192.168.10.103

> AUTO_SETUP_NET_HOSTNAME=DietPi_{NAME}
to
>AUTO_SETUP_NET_HOSTNAME=DietPi_(YOUR INITIAL HERE)
example
>AUTO_SETUP_NET_HOSTNAME=DietPi_PAS

1. Find the sd card on your machine:
1. MACOS - remove card and put it back in to have it mount and use finder to open files in an editor
2. WINDOWS - Use File explorer to find mounted sd card and boot
2. Copy the dietpi.txt and dietpi-wifi.txt that you edited to the sd card (Replacing the old versions)  
       	    
### 3. Eject the SD card from your laptop

### 4. Install SD card onto the PI and powerup the PI to run DietPI
      1. You should see the red led turn on and the green led will start to flash as the install is running
      1. Wait until the green light has finished flashing - could take 10 minutes


### 5. Login to the DietPi using the wireless access point
    You will need to the ip address you set in the dietpi.txt file
    using the form:  ssh root@{PI_IPADDR}
    Default password is setup in your dietpi.txt if not changed then it will be: dietpi

### 6. Once you are logged into the pi you can continue with other

