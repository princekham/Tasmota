# LORA

- SX1278 LoRa 433MHz wireless transceiver module

![photo_2026-02-03_14-06-47](https://github.com/user-attachments/assets/7fd7ceab-706b-4254-a93d-38d550aa4fb7)


![photo_2026-02-03_14-06-46](https://github.com/user-attachments/assets/d51fbac9-5835-4306-9377-63a8de74ae1f)

<img width="727" height="508" alt="image" src="https://github.com/user-attachments/assets/fa655207-1eb0-4844-ba90-561faa8ccc59" />


- LoRa VCC -> ESP32 3V3
- LoRa GND -> ESP32 GND
- LoRa MISO -> ESP32 GPIO 19
- LoRa MOSI -> ESP32 GPIO 23
- LoRa SCK -> ESP32 GPIO 18
- LoRa NSS -> ESP32 GPIO 5 (CS)
- LoRa DIO0 -> ESP32 GPIO 26 (Interrupt)
- LoRa RST -> ESP32 GPIO 14 (Reset)

### Antenna Connection
<img width="895" height="402" alt="image" src="https://github.com/user-attachments/assets/c42d8981-99dc-481e-8a99-8f7e7a9a7e7f" />


### Flashing Tasmota
- Use the Tasmota web installer or a similar tool to flash tasmota32.bin onto your ESP32.

### Tasmota Configuration
- Navigate to Configuration > Configure Module.
- Set the module type (usually "Generic 18").
- Assign the GPIO pins used in step 1:
- Set the GPIO connected to NSS as LoRa NSS (usually 5).
- Set the GPIO connected to DIO0 as LoRa DIO0 (usually 26).
- Set the GPIO connected to RST as LoRa RST (usually 14).
- Click Save. The device will reboot. 
### Verification
- Go to the Console and check if the driver initialized correctly. You should see LOR: SX127x initialized or similar confirmation.
## Usage
- Use console commands to send data:
- LoRaSend <payload>: Sends a payload.
- LoRaCommand <command>: Sends a specific LoRa command.
- lorawanbridge must be disabled
- Note: Ensure your Tasmota version supports LoRa, as it is included in tasmota32 binaries
 
### Sending Sensor Data with a Rule

```
Rule1 ON SR04#Distance DO LoRaSend %value% ENDON
Rule1 1
```
- SR04 is the sensor name.
