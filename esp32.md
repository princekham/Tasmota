### Installing Firmware
- I use the web installer (https://tasmota.github.io/install/)
- I had to hold the boot button when installing firmware

- In console the following commands are entered
  ```
  SetOption103 1
  SetOption132 1
  MqttFingerprint2 00
  MqttFingerprint1 00
  ```

  The following is from https://tasmota.github.io/docs/SHT30/#tasmota-settings

  Tasmota Settings~
In the Configuration -> Configure Module page assign:

D4 GPIO4 to I2C SDA
D5 GPIO5 to I2C SCL

operating voltage range (2.15 to 5.5 V)

  ![image](https://github.com/user-attachments/assets/224be1b0-c4e7-4396-bccf-955b40f58a3d)
  https://tasmota.github.io/docs/_media/wemos_sht30_config_marked.jpg

#### SHT30 Wire Connection
- Red Wire:VCC (3.3V)
- Black Wire：GND (GND)
- White Wire：SDA (GPIO4 - D4)
- Yellow Wire：SCL (GPIO5 - D5)
Because it is I2C no resistors required when connecting to the microcontroller

#### SHT30 Specs
- Product Attribute	Attribute Value
- Full Temp Accuracy:	+/- 0.3 C
- Operating Supply Current:	800 uA
- Supply Voltage - Min:	2.4 V
- Supply Voltage - Max:	5.5 V

![image](https://github.com/user-attachments/assets/831f42c3-3241-401e-862d-2905f0414f83)
https://lastminuteengineers.com/wp-content/uploads/iot/ESP32-Pinout.png

### Steps 
#### Tasmota
- mqtt broker: mqtt.flespi.io
- username: token no from Flespi
- password: no password

#### Flespi
- create a token (master token)
- create a device (which is under 'Telematics Hub')
  - device name - as you wish 
  - device identifier - tasmota topic name
- create a channel (which is under 'Telematics Hub')
  - Protocol Id - mqqt
  - flespi MQTT broker - token no
  - topic - use topic from Tasmota console
  - JSON Payload
    - Device Identification field - tasmota topic name
    - parameter
      - Temperature - SHT3X/Temperature
      - etc,
        
  ![image](https://github.com/user-attachments/assets/82504be1-19ff-43f9-a096-65eb2535274d)

