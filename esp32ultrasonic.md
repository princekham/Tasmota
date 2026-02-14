# Ultrasonic Sensor with ESP32
- I changed many pins but finally ok with pins 16&17 as below
- 16 - Echo; 17 - Trigger
- one thing to beware is that the sensors cannot touch with the casing box.

- <img width="1848" height="976" alt="image" src="https://github.com/user-attachments/assets/8481a7de-d031-41e8-8e8a-ac6b6ceca5d8" />


  ![image](https://github.com/user-attachments/assets/1b778872-5ec0-4f82-b117-4964feb102c5)

### With Lora
- I will try the following with Lora

- LoRa VCC -> ESP32 3V3
- LoRa GND -> ESP32 GND
- LoRa MISO -> ESP32 GPIO 19
- LoRa MOSI -> ESP32 GPIO 23
- LoRa SCK -> ESP32 GPIO 18
- LoRa NSS -> ESP32 GPIO 5 (CS)
- LoRa DIO0 -> ESP32 GPIO 26 (Interrupt)
- LoRa RST -> ESP32 GPIO 14 (Reset)

### Template
- {"NAME":"ESP32-DevKit","GPIO":[1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,0,1,1,1,0,1,1,1,0,0,0,0,1,1,1,1,1,0,0,1],"FLAG":0,"BASE":1}

<img width="971" height="1482" alt="image" src="https://github.com/user-attachments/assets/4087e9c9-0323-446d-a87a-c4e63a4b3df7" />

### setting Frequency and Bandwidth

```
Loraconfig {"Frequency":433.0,"Bandwidth":125.0}
```
### setting Spreading Factor and Output Power
- the Spreading Factor (SF7–SF12) determines the speed, range, and time-on-air by controlling how many chirps represent a data symbol, with higher SFs (e.g., 12) offering longer range but lower data rates. 
- Output Power (TX Power) represents the transmit strength, dictating how far the signal travels, typically regulated up to 14 dBm in many regions. 

```
Loraconfig {"SpreadingFactor":12, "OutputPower":17}
```
