# Setting up an Access Point on Raspberry PI #
In this tutorial we will set up a Raspberry Pi using DIET_PI

## Purpose ##
Install DietPi on a Raspberry PI via WIFI ready to install other software

1. Install a version of a Linux operating system on the PI

## Setup ##
What you will need for this tutorial:
1. A laptop or desktop to act as your development platform
2. A Raspberry PI ( 3 B+ or 4 )
3. A Class 10 microSD 8gb or better (max 32gb) https://www.mymemory.co.uk/blog/the-best-memory-cards-for-raspberry-pi/
4. WIFI ROUTER and configuration info

## Tutorial Steps ##
### 1. Setup your working environment on your laptop ###
1. Prepare your device to with an archiver extractor - on windows I used BreeZip http:www.breezip.com but others are available from https://www.7-zip.org . On MacOS V11 it extracted using tools already installed but an unarchiver is available at  https://theunarchiver.com for your mac if extraction is not working.
1. Prepare you laptop with a sd card writer such as: balenaEtcher https://www.balena.io/etcher/

### 2. Download and configure  DietPI on your laptop

1. Download and extract the DietPi disk image
   1. Go to https://dietpi.com/#download and select the RaspberyPI Images
   2. On the new page choose the download for your targeted pi
      1. How do you know which version will work? - Check https://en.wikipedia.org/wiki/Raspberry_Pi
      Raspberry Pi 3 Model B was released in February 2016 with a 1.2 GHz 64-bit quad core ARM Cortex-A53 processor which is an  ARMv8-A https://en.wikipedia.org/wiki/ARM_Cortex-A53.
      2. So I am choosing to use:
      ARMv8 64-bit image:Download - As of Sep 19, 2021 that link is  https://dietpi.com/downloads/images/DietPi_RPi-ARMv8-Bullseye.7z
   3. Extract the DietPi_RPi-ARMv8-Bullseye.7z -
   3. Write the DietPi image to your SDcard using balenaetcher or other flashing software
      1. open balenaetcher
      2. select the dietpi image
      3. Select the targeted card ( that was inserted into your laptop)
      4. Press flash ( you may have to put in your credentials. I also had to  try a second time to make it work)
      5. wait until the flash finished and verifies the copy
      6. close balenaetcher
   4. Edit the extracted DietPi_RPi-ARMv8-Bullseye to make an install of Dietpi so installation needs: wifi login
      1.  Login your laptop to the targeted WIFI so that you are sure of the credentials needed
         for the laptop use:
	    SSID: Dr-S-IOT-G
	    Password: iotisfun
	 find your ip address: it will depend on your laptop usually found in the network properties it should be in form of 192.168.10.N
	 for your Raspberry PI we will set it to use a fixed ipaddress using the N from your laptop address and adding 100.
	   for example my laptop is using 192.168.10.3 , so I will configure my PI to 192.168.103
	   Save this ip address to update the config files
       2. Copy the [dietpi.txt](./src/wifi_boot/dietpi.txt) and [dietpi-wifi.txt](./src/wifi_boot/dietpi-wifi.txt) from the [src/wifi_boot](./src/wrifi_boot) directory of this repo

 3. change the following configurations:
	p    1. WIFI intialization:
	       AUTO_SETUP_NET_WIFI_ENABLED=1
	       AUTO_SETUP_NET_HOSTNAME=DietPi_IOT_{YOURINITIALS} so mine is
	          AUTO_SETUP_NET_HOSTNAME=DietPi_IOT_PS
	      CONFIG_WIFI_COUNTRY_CODE=US 
	    1. Auto install
	       AUTO_SETUP_AUTOMATED=1
	    1. Setup Headless
	       AUTO_SETUP_HEADLESS=1
	    1. Auto install software services number found in https://github.com/MichaIng/DietPi/wiki/DietPi-Software-list  ( Adding transmission  )
	       AUTO_SETUP_INSTALL_SOFTWARE_ID=44
            1. Change locale and keyboard and timezone
	       AUTO_SETUP_LOCALE=en_US.UTF-8
	       AUTO_SETUP_KEYBOARD_LAYOUT=us
	       AUTO_SETUP_TIMEZONE=America/New_York
	    1. Turn off serial console
	       CONFIG_SERIAL_CONSOLE_ENABLE=0
	    1. Setup WIFI HOTSPOT
	         # WiFi Hotspot
		 SOFTWARE_WIFI_HOTSPOT_SSID=DietPi-HotSpot-{YOUR_INITIALS}
		 # - Key requires a minimum of 8 characters
		 SOFTWARE_WIFI_HOTSPOT_KEY=dietpihotspot
		 SOFTWARE_WIFI_HOTSPOT_CHANNEL=4 {change}
	4. Edit the dietpi-wifi.txt file by enabling your wifi connection
	       aWIFI_SSID[0]='IOT_AP'
	       aWIFI_KEY[0]='iotclass2021'
	    If you have a home wifi you can use then you could setup the entry 1 as well with your ssid and key

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

