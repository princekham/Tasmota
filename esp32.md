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

D2 GPIO4 to I2C SDA
D1 GPIO5 to I2C SCL

  ![image](https://github.com/user-attachments/assets/224be1b0-c4e7-4396-bccf-955b40f58a3d)

#### SHT30 Wire Connection
- Red Wire:VCC
- Black Wire：GND
- White Wire：SDA
- Yellow Wire：SCL
