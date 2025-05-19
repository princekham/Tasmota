## PZEM-004T Multimeter Module

- copy the following configuration
  
  ``` {"NAME":"HW-655 PZEM","GPIO":[0,62,0,98,0,0,0,0,0,0,0,0,0],"FLAG":0,"BASE":18} ```

### Hardware Configuration
  - GPIO1 (TX) to RX pin of PZEM-0045
  - GPIO3 (RX) to TX pin of PZEM-0045

- I will use ESP32
  - There are three uart ports in ESP32 and I can still use GPIO1 and GPIO3 for TX and RX pins.
  - I used Tasmota Web installer for installing ESP32 firmware.
  - I just chose "Tasmota English" and it automaticalling installed firmware for ESP32.
  - While installing I needed to press the 'Boot' button and it took about 2 minutes to flash.
  - And in the console I entered the follwoings.
  
    ```
    SetOption103 1
    SetOption132 1
    MqttFingerprint2 00
    MqttFingerprint1 00
    ```
- According to (https://www.instructables.com/Smart-Energy-Meter-Using-ESP32-PZEM004T-V30-Config/)
- I set GPIO1 and GPIO3 as follow

  ![image](https://github.com/user-attachments/assets/c23bc85a-4916-43c2-ba20-e962bea2ed16)

- And the following appeared on the home page.

  ![image](https://github.com/user-attachments/assets/7e48ea61-6c56-4bb6-9528-a4f31665f410)

