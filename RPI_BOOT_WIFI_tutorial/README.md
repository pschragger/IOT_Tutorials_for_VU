# Setting up dietpi on a Raspberry PI via wifi #

In this tutorial you will perform an initial install of DIET_PI on a device uinsting your WIFI router.

## Purpose ##
Install DietPi on a Raspberry PI via WIFI ready to install other software

1. Install a version of a Linux operating system on the PI

## Prerequisites  ##
To accomplish finishing the setup you will need:
1. A laptop or desktop to act as your development platform
1. A Raspberry PI ( 3 B+ or 4 )
1. A Class 10 microSD 8gb or better with the maximum being a 32gb microsd card ( Check https://www.mymemory.co.uk/blog/the-best-memory-cards-for-raspberry-pi/ for the best cards to use with a PI )
1. WIFI ROUTER and configuration info.  I am using a travel or IOT router to create a local portable network that I can reconfigure to work in various locations using hardwire ethernet or wifi repeater functions to provide wan internet access.  You need your personal routers SSID and your WIFI key info to configure your pi for access. If you need help setting up your WIFI router then follow the [wifi router tutorial](../WIFI_ROUTER_SETUP_tutorial).

## Setup Steps ##

### 1. Setup your working environment on your laptop ###
1. Prepare your device to with an archiver extractor - on windows I used BreeZip http:www.breezip.com but others are available from https://www.7-zip.org . On MacOS V11 it extracted using tools already installed but an unarchiver is available at  https://theunarchiver.com for your mac if extraction is not working.
1. Prepare you laptop with a sd card writer such as: balenaEtcher https://www.balena.io/etcher/


### 2. Download and configure  DietPI on your laptop
1. Download and extract the DietPi disk image
   - Go to https://dietpi.com/#download and select the RaspberyPI Images
   - On the new page choose the download for your targeted pi
     - How do you know which version will work? - Check https://en.wikipedia.org/wiki/Raspberry_PI -  Raspberry Pi 3 Model B was released in February 2016 with a 1.2 GHz 64-bit quad core ARM Cortex-A53 processor which is an  ARMv8-A https://en.wikipedia.org/wiki/ARM_Cortex-A53.
     - So I am choosing to use: ARMv8 64-bit image: Download - As of Sep 19, 2021 that link is  https://dietpi.com/downloads/images/DietPi_RPi-ARMv8-Bullseye.7z  - Extract the DietPi_RPi-ARMv8-Bullseye.7z -
2. Write the DietPi image to your SDcard using balenaetcher or other flashing software
   - Open balenaetcher
   - Select the dietpi image
   - Select the targeted card ( that was inserted into your laptop)
   - Press flash ( you may have to put in your credentials. I also had to  try a second time to make it work. )
   - Wait until the flash finished and verifies the copy
   - Close balenaetcher
3. Configure the boot image on the microSD card of dietpi.
   - copy dietpi.txt and dietpi-wifi.txt to a temporary directory
   - edit dietpi-wifi.txt with your router ssid and creadentials
      Change the lines:
      ```
      aWIFI_SSID[0]=''
      aWIFI_KEY[0]=''
      ```
      adding your SSID and ssid_password for example SSID=IOT-PAS and password=iotlogin would result in the lines:
      ```
      aWIFI_SSID[0]='IOT-PAS'
      aWIFI_KEY[0]='iotlogin'
      ```
   - edit dietpi.txt with your location and wifi network default settings
     For the East Coast US settings change the values of the variables listed below
      ```
      AUTO_SETUP_LOCALE=en_US.UTF-8
      AUTO_SETUP_KEYBOARD_LAYOUT=us
      AUTO_SETUP_TIMEZONE=America/New_York
      AUTO_SETUP_NET_ETHERNET_ENABLED=0
      AUTO_SETUP_NET_WIFI_ENABLED=1
      AUTO_SETUP_NET_WIFI_COUNTRY_CODE=US
      AUTO_SETUP_DHCP_TO_STATIC=1
      AUTO_SETUP_NET_HOSTNAME=DietPi_{YOUR_INITIALS}
      AUTO_SETUP_HEADLESS=1
      AUTO_SETUP_AUTOSTART_TARGET_INDEX=1
      SURVEY_OPTED_IN=0
      CONFIG_SERIAL_CONSOLE_ENABLE=1
      ```
      - be sure to replace {YOUR_INITIALS} with a locally unique string for me I used PAS so my pit is names DietPi_PAS.
      - search for each variable before the = and put in the new value in the file.
      - be sure to save the changes
4. Copy the dietpi.txt and dietpi-wifi.txt you edited to the top level directory of your sd card.

4. Eject the SD card from your laptop

### Install SD card onto the PI and powerup the PI to run DietPI
1. You should see the red led turn on and the green led will start to flash as the install is running
2. Wait until the green light has finished flashing - could take 10 minutes or more depending on the local network load.

### Login to the DietPi using the wireless access point

1. Using the admin website of your router at (if using vanilla openwrt configuration it usually found at 192.168.8.1
   - check the CLIENTS page for the IPADDR of the pi that is booted up.
   - use SSH to login into your PI using
   ```
   ssh root@IPADDR
   password: dietpi
   ```
   be sure to change your root login during setup
   
### 6. Once you are logged into the pi finish setup
1. Using DietPi-Config
   - Change software and user passwords using the "Security Options"

### 7. You are now ready to start install platform software.
1. Continue with other installs such as: [IOT Platform Install](../RPI_IOT_PLATFORM_INSTALL_tutorial)  mostly by using diepi-software
