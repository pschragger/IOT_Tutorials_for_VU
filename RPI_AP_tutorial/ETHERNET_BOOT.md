 Copy the configs in the src directory to you laptop and Edit the ip addresses in the dietpi.txt file and make edits to the dietpi-wifi.txt.

   The changes for the dietpi.txt are setup to make this a headless install
   The extracted DietPi_RPi-ARMv8-Bullseye to make a Headless install of Dietpi so installation needs: No Monitor, No LAN,  No router login see 
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
	       aWIFI_SSID[0]='Dr-S-IOT-G'
	       aWIFI_KEY[0]='iotisfun'
	    If you have a home wifi you can use then you could setup the entry 1 as well with your ssid and key