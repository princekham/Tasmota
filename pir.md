### PIR sensor
- https://tasmota.github.io/docs/PIR-Motion-Sensors/#tasmota-settings
- The following one is the one I am going to use

- ![image](https://github.com/user-attachments/assets/648d7ad3-8ca5-4ac4-9256-db1a7ff04c8b)


- It has Wide Operating Voltage:
It operates on a 5V to 20V DC power supply, providing flexibility in power options.
- I still need to check the output voltage of the sensor whether mine is output 3.3 V version. I found that to be as follow

![image](https://github.com/user-attachments/assets/25c43248-fb59-44fe-829d-3f2c5796e2ec)


- The above web site chose 'GPIO 13' as 'switch' . We can use 'SwitchMode 13' (Push to on) along with 'PulseTime' for the minimum on-period.
- Or we can broadcast an MQTT message. The message will switch on another Tasmota device switch with a name (******)

  ```
  SwitchMode1 1
  SwitchTopic 0
  Rule1 on Switch1#State=1 Do publish cmnd/tasmota_******/POWER1 ON ENDON endon
  Rule1 1
  ```

- Because we use 'Switch1', so we use 'SwitchMode1' I think.
- 'SwitchMode 1' is to follow the flow, i.e. '0 = Off' and '1= On'
- 'SwitchTopic 0' will not publish MQTT message (default mode)
  
