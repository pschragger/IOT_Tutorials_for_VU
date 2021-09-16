# WIFI and WeatherMap API Tutorial #
Microcontroller testing WIFI with Weathermap API 
## Purpose ##
This tutorial outlines how to :
1. Register and obtain weathermap api keys
1. Setup microcontroller to connect to local wifi
1. Send API request to weathermap API
1. Parse response
1. Display returned results
## Setup ##
What you will need for this tutorial:
1. A laptop or a rpi working as a desktop  with connection to the internet (WIFI or ethernet)
1. 1 esp8266 nodemcu or other arduino equivalent
1. 1 USB to usb mini connector for connecting to arduino 

## Tutorial Steps
1. Register and obtain weathermap api keys
   1. Register with https://openweathermap.org/ to obtain api keys for free access.
   2. Once registered copy your  default key locally from: https://home.openweathermap.org/api_keys  (Or you can make a specific key for this project)
   3. Choose the api request that you will use from https://openweathermap.org/current The API format should look like:
         > api.openweathermap.org/data/2.5/weather?q={city name},{state code}&appid={API key}
   4. Fill the variables in the {} with the city, state and API key it should look like:
 > api.openweathermap.org/data/2.5/weather?q=Villanva,US-PA&appid=090302390234920  5. Try from a browser on you computer 
2. Setup microcontroller to connect to local wifi
   1. In ArduinoIDE start a create a new FILE using : FILE->Examples->ESP8266Wifi->HTTPSRequest
   2. Note the fingerprint has expired if you want to retrieve a new one then follow: https://maakbaas.com/esp8266-iot-framework/logs/https-requests/  Otherwise replace the line: client->setFingerprint(fingerprint); as follows to disable security
     
   > //client->setFingerprint(fingerprint);
  
  >  client->setInsecure();


3. Send API request to weathermap API
   Now replace the URL being retrieved with your weathermap api
   Flash the ESP and see the results in the monitor.
   
4. Parse response
   Load in ArduinoJson Library under sketch->include Library->Manage Libraries
   Search fore ArduinoJason and install
   use https://arduinojson.org/v6/assistant/ with a sample to get the parsing text using a sample json from your weathermap api call.

5. Display returned results
 something like:
 >> Serial.println(String(name)+ " Weather is "+ String(weather_0_main));
