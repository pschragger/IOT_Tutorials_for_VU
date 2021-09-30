# Setting up an Access Point on Raspberry PI #
In this tutorial we will set up a Raspberry Pi as an IOT Gateway
## Purpose ##
An IOT accesspoint allows IOT devices to connect to RPI base Wireless network. The gateway has the additonal capablity of routing the IOT data to the internet on a second network (wired or wireless)
So the purpose of this tutorial is to set a Raspberry Pi as the access point and gateway.

Do the setup in two phases:

1) boot RPI and login via wifi or ethernet
Headless SSH settings from : https://dietpi.com/phpbb/viewtopic.php?t=5370

2) Setup the Pi as a wifi extender or ethernet hotspot
https://pimylifeup.com/raspberry-pi-wifi-extender/
or setup the PI as a wifi hotspot routing to ethernet

During this tutorial we will:
1. Install a version of a Linux operating system on the PI
2. Setup the wireless accesspoint to allow both the IOT devices and our laptop to connect to the PI
3. Setup  to PI act as gateway with either two wireless networks or a wireless network and an ethernet
4. Test that you can connect to the weatherapi as setup in the wifi_tutorial both with you laptop and your arduino

## Setu p ##
What you will need for this tutorial:
1. A laptop or desktop to act as your development platform
2. A Raspberry PI ( 3 B+ or 4 )
3. A Class 10 microSD 8gb or better (max 32gb) https://www.mymemory.co.uk/blog/the-best-memory-cards-for-raspberry-pi/

For ethernet connection:
4. Ethernet cable
5. Ethernet router

For wifi connection:
4. wifi dongle
5. Wifi router 

## Tutorial Steps ##
### 1. Know your network
  To setup dietpi for the first time you will need access to the internet.
  This can either be done via a WIFI setup or an ethernet setup.
  Before joining a network with your pi test your connection with your laptop and find.
  1. Router ip address
  2. Laptop ip address
     
#### WIFI setup
    1. Connect Laptop to wireless network ( ideally using your own wifi router or my lab router or some other wep or wep2 or wpa  wifi with known ssid name and password.) Note this network connection is temporary until we can get the pi initialized and configured.  Later we will provide an ssid on the pi so it act as router and you will assign the pi a known ip address as the router.
       1. For the inclass lab I have setup a network
       	  SSID: DR-S-IOT-G
	  Password: iotisfun		      
       2. Connect your laptop to the network
       3. Use you laptop wifi network info to obtain your wifi info:
       	  For SSID: DR-S-IOT-G
	  Router: 192.168.10.1
	  Laptop: 192.168.10.2
       4. Choose a fixed ip address Laptop ip + 0.0.0.100
          So I will assign my pi to 192.168.10.102   ( this will assure that we do not steal some elses assigned number. Note the highest you can go is 192.168.10.254   -- So you may have trouble on a public wifi finding an available settings so do this first setup in a known environment. )
#### Ethernet setup
    1. Connect Laptop to an ethernet router ( ideally using your own ethernet router or my lab router.) Note this network connection is temporary until we can get the pi initialized and configured.  Later we will provide an ssid on the pi so it act as router and you will assign the pi a known ip address as the router.
       1. For the in class lab router it shares the managed ip address space with the wifi network
       2. Connect your laptop to the network
       3. Use you laptop wifi network info to obtain your wifi info:
	  Router: 192.168.10.1
	  Laptop: 192.168.10.3
       4. Choose a fixed ip address Laptop ip + 0.0.0.100
          So I will assign my pi to 192.168.10.103   ( this will assure that we do not steal some elses assigned number. Note the highest you can go is 192.168.10.254. You may have trouble on a public wifi finding an available settings so do this first setup in a known environment. )


        
### 2. Setup your working environment on your laptop ###
    1. Prepare your device to with an archiver extractor - on windows I used BreeZip http:www.breezip.com but others are available from https://www.7-zip.org . On MacOS V11 it extracted using tools already installed but an unarchiver is available at  https://theunarchiver.com for your mac if extraction is not working.
   
     2. Prepare you laptop with a sd card writer such as: balenaEtcher https://www.balena.io/etcher/


### 3. Download and configure  DietPI on your laptop

1. Download and extract the DietPi disk image
   1. Go to https://dietpi.com/#download and select the RaspberyPI Images
   2. On the new page choose the download for your targeted pi
      1. How do you know which version will work? - Check https://en.wikipedia.org/wiki/Raspberry_Pi
      Raspberry Pi 3 Model B was released in February 2016 with a 1.2 GHz 64-bit quad core ARM Cortex-A53 processor which is an  ARMv8-A https://en.wikipedia.org/wiki/ARM_Cortex-A53.
      2. So I am choosing to use:
      ARMv8 64-bit image:Download - As of Sep 19, 2021 that link is  https://dietpi.com/downloads/images/DietPi_RPi-ARMv8-Bullseye.7z
   3. Extract the DietPi_RPi-ARMv8-Bullseye.7z -
   4. Write the DietPi image to your SDcard using balenaetcher or other flashing software
      1. open balenaetcher
      2. select the dietpi image
      3. Insert sd card ( do not format it)
      3. Select the targeted card ( that was inserted into your laptop)
      4. Press flash ( you may have to put in your credentials. I also had to  try a second time to make it work)
      5. wait until the flash finished and verifies the copy
      6. close balenaetcher
      
   5. Edit the extracted DietPi_RPi-ARMv8-Bullseye to make a Headless install of Dietpi so installation needs: No Monitor, No LAN,  No router login see 
      Pre Configure WiFiYouTube video #3: https://www.youtube.com/watch?v=vlMpn9u0Y4o for step by step instructions or https://dietpi.com/phpbb/viewtopic.php?f=8&t=273 for text version of instructions
      But follow along here for my take on this setup:
         1. Find the sd card on your machine:
	    1. MACOS - remove card and put it back in to have it mount and use finder to open files in an editor
	    1. WINDOWS - Use File explorer to find mounted sd card and boot
	 2. Open /boot/dietpi.txt or {Drive ID like D}:dietpi.txt
	     1. WINDOWS will probably open in notepad
	     2. MACOS will probably open in textedit
         3. change the following configurations: [example file content](http:src/dietpi-wifi-boot.txt)
	    1. WIFI intialization:
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
	    
### 3. Eject the SD card from your laptop

### 4. Install SD card onto the PI and powerup the PI to run DietPI
      1. You should see the red led turn on and the green led will start to flash as the install is running
      1. Wait until the green light has finished flashing - could take 10 minutes


### 5. Login to the DietPi using the wireless access point
    You will need to find the pi on your accesspoint.  Find the ip address and use ssh to login to your pi.
    using the form:  ssh root@{PI_IPADDR}
    Default password is setup in your dietpi.txt if not changed then it will be: dietpi

 You can change the configuration latter using:
 dietpi-launcher : All the DietPi programs in one place
 dietpi-config   : Feature rich configuration tool for your device
 dietpi-software : Select optimised software for installation
 htop            : Resource monitor
 cpu             : Shows CPU information and stats
 
/var/tmp/dietpi/logs/dietpi-firstrun-setup.log

reboot the pi on the command line using reboot command.



### 6. Setup ethernet and router on the PI

    1. Run dietpi-software
    2. Search or browse for WiFi Hotspot
    3. Use arrow keys and Push spacebar to select (* should show in [])
    4. Tab to ok and press enter key
    5. This will enable the hotspot
       1. This will fail if you are not connected to an ethernet router.
       2. Connect the ethernet to the router
       3. In dietpi-config activate the ethernet
       4. Once ethernet is available start the hotspot installation again after exiting from  from dietpi-config
    6. Once enabled succesfully finish the installation exit from dietpi-software
    7. Restart the machine by bouncing its power
    8. connect your laptop to your new hotspot that should come up in your doetpi.txt configuration
       1. find your routers hotspot IP address
       2. use;  ssh root@{routeripaddr something like 192.168.42.1}
       
    
### 7. Test the internet connection using openweathermap api
 Try your api call ( note the & is escaped using single quotes for bash command line on the dietpi
curl https://api.openwethermap.org/data/2.5/weather?q=Radnor,US-PA'&'appid={YOURAPIKEY}
 If that works now try the wifi on your ardiuno with the weathermap software.
