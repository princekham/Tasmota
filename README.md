# Tasmota

Initial Configuration
- connect to the power
- look for tasmota device in the wifi list
- log lin to 192.168.4.1
- add WiFi credentials
- configure other -> rename
- copy and note the MAC address
- configure pwoer on state - console -> poweronstate 0 or 1
- configure time zone - console -> Backlog Latitude 17.1750495; Longitude 95.9999652; TimeZone +06:30
- set ip address - console ->ipaddress1 192.168.
- SetOption65 1 (to disable power interruptions to reset the device - 1 to disable; 0 to enable)

Download the image (tasmota.bin.gz file) from http://ota.tasmota.com/tasmota/release/
Installing Tasmota with Tasmotizer, it worked perfectly fine. Get the Tasmotizer at https://github.com/tasmota/tasmotizer
There is an exe file for window users.
For setting timezone, get the command at https://tasmota-tz.netlify.app/

For setting up a swith -> configure module -> choose generic (18) -> then make the setting up

For Raspberry part
Setting up static IP for Raspberry PI - https://polargeek.com/static-ip-raspberry-pi/

Some notes on using ESP8266 (more at https://wiki.politick.ca/pages/viewpage.action?pageId=8454628)

![image](https://user-images.githubusercontent.com/16104631/200354673-5c4d0114-e3ee-41ad-9bbf-06a5f9adaa65.png)

![image](https://user-images.githubusercontent.com/16104631/200355128-d8e0e11d-7e60-405a-823a-c895de02e63e.png)


![image](https://user-images.githubusercontent.com/16104631/212595891-340ca250-fc7a-40d0-92af-64f1599359be.png)


Setting UP MQTT in Tasmota

![image](https://user-images.githubusercontent.com/16104631/213329857-0160d4da-11f7-4918-a354-ab12b696af8c.png)

1. Put username and password
2. put friendly name in the topic field
3. and save

For more than one button, we use topic and payload

Ref: https://www.youtube.com/watch?v=9j4fua0cuWU&ab_channel=digiblurDIY


HTTP command

![image](https://user-images.githubusercontent.com/16104631/218124778-e7933d13-1806-4dd3-aa21-46dab7724699.png)


![image](https://user-images.githubusercontent.com/16104631/218124942-d42926b6-a15c-4916-a4e6-4e59ef273329.png)

MQTT command

![image](https://user-images.githubusercontent.com/16104631/218125085-6450002e-d749-4a8b-9759-a7d37c5b9184.png)


Setting Group Topic
- grouptopic pressurepump
- setoption85 1

Tasmota Client

![image](https://user-images.githubusercontent.com/16104631/218308700-4d1c740b-6ffe-46df-9761-1bf8563f1ad9.png)


- Go to - https://tasmota.github.io/docs/Compile-your-build/#compiling-tools
- Then open Gitpod - https://tasmota.github.io/docs/Gitpod/
- Then choose tasmota, then open -  user_config_override.h
- make changes
- Custom Build - platformio run -e tasmota (at gitpodio)
- then go to buildoutput
- under firmirmware, download tasmota.bin file
