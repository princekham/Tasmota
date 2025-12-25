## Latest method (10/5/2014)
### It seems Gitpos is not working anymore
- goto Gitpod (https://arendst-tasmota-q1eu6x7zswt.ws-us110.gitpod.io/)
- if not ok go to https://gitpod.io/start/#arendst-tasmota-ngfc5w75hvh
- And then goto tasmota and then to "user config override .h" file
- နောက်ဆုံး endif ရှေ့မှာ ကိုယ့်ရဲ့ codes ကို ထည့်
- then run ```platformio run -e tasmota ```
- Then go to buildoutput -> firmware -> use the bin version
______________________________________________________________________________________
### The last time I compiled was by using docker I think

- I use https://github.com/benzino77/tasmocompiler 
- I set up on my server from above website
![image](https://github.com/princekham/Tasmota/assets/16104631/1f0f380d-dd1c-4f3b-8e54-01fb64bc72a1)


  - I use different port ```docker run --rm --name tasmocompiler -p 8080:3000 benzino77/tasmocompiler```
  - Please run the following to lines for custom compilation

``` sudo docker pull benzino77/tasmocompiler```

``` sudo docker run --rm --name tasmocompiler -p 8080:3000 benzino77/tasmocompiler```

or I can use use port 3000

``` sudo docker run --rm --name tasmocompiler -p 3000:3000 benzino77/tasmocompiler```



  ![image](https://github.com/princekham/Tasmota/assets/16104631/61afe9a0-8f6f-4d2c-9fe6-8c5cbfee7c98)

- To install docker on Raspberry Pi 4 ( https://linuxconfig.org/how-to-install-docker-on-raspberry-pi)
- https://github.com/tasmota/docker-tasmota

## For ESP32 Node MCU V3
- Driver installation (https://sparks.gogo.co.nz/ch340.html)


