# Firmware
- the firmware must be -displays.bin
# Oled
- model -SSD1306 OLED display
- connection type - I2C
- D1 (GPIO5) - SCL
- D2 (GPIO4) - SDA
- I2C address - 0x3C (for 128x32)

- ![image](https://github.com/user-attachments/assets/af5e3289-518c-4f51-9897-2c3571ebe481)


- <img width="806" height="1409" alt="image" src="https://github.com/user-attachments/assets/cdfaee78-ed20-4383-afd4-b647645c22f4" />


- run the following command but do not enable it: I got it from (https://github.com/arendst/Tasmota/blob/development/tasmota/displaydesc/SSD1306_128x32_display.ini)
  
`rule3 :H,SSD1306_128x32,128,32,1,I2C,3c,*,*,* :S,0,2,1,0,10,10 :I AE D5,80 A8,1F D3,00 40 8D,14 20,00 A1 C8 DA,02 81,8F D9,F1 DB,40 A4 A6 AF :o,AE :O,AF :D,81 :A,00,10,40,00,00 :i,A6,A7 #`


## To receive sensor data from another Tasmota device

### For the sender
- SetOption3 1
- TelePeriod 20 (desired update time in seconds)

- Rule1 ON SR04#Distance DO Publish cmnd/tasmota_1E6EDE/DisplayText [x10y20] Distance: %value% cm ENDON
- Rule1 1

- I modified the above rule to be
- Rule1 ON SR04#Distance DO Publish cmnd/tasmota_1E6EDE/DisplayText [x0y0] WaterLevel [x10y16]  %value% cm ENDON
### Water Tank
- The depth of the water tank - 58 in (154 cm)
- The water level (in feet) would be (147-sensor reading)*0.0328
- The above rule will be 
- Rule1 ON event#addvar do sub %value% 154 ENDON DO SR04#Distance Publish cmnd/tasmota_1E6EDE/DisplayText [x0y0] WaterLevel [x10y16] %value% cm ENDON
- Finally the following rule works.

- Rule1 on SR04#Distance do var1=154-%value% endon On var1#state do Publish cmnd/tasmota_1E6EDE/DisplayText [x0y0] WaterLevel [x10y16] %value% endon

- Rule1 on SR04#Distance do var1=(154-%value%)*0.0328 endon On var1#state do Publish cmnd/tasmota_1E6EDE/DisplayText [x0y0] WaterLevel [x10y16] %value% ft  endon
#### For the 2nd OLED
- Rule3 on SR04#Distance do var3=(154-%value%)*0.0328 endon On var3#state do Publish cmnd/tasmota_0FE20A/DisplayText [x0y0] WaterLevel [x10y16] %value% ft  endon
  
- Rule3 1
  
#### For the phone mqtt app
- Rule2 on SR04#Distance do var2=(154-%value%)*0.0328 endon On var2#state do Publish stat/tasmota_E27FBB/WaterLevel %value% ft endon

### For the receiver
- no need to configure

## 16x2 LCD Display
### Wire Connection

<img width="1553" height="671" alt="image" src="https://github.com/user-attachments/assets/df70c7d6-d339-459e-ad34-e4113db9c0b6" />

### Configuration

<img width="1353" height="551" alt="image" src="https://github.com/user-attachments/assets/97d7fb60-5557-4946-8f5d-20aefbbd3bd9" />

<img width="1485" height="363" alt="image" src="https://github.com/user-attachments/assets/e6fe0a82-1153-4bc0-b035-7118698dc6a2" />

### Displaying Data (Commands)
- Displaytext1 Hello World: Displays text on line 1.
- Displaytext2 Tasmota: Displays text on line 2.
- DisplayText [z]: Clears the display.
- DisplayText [c]: Turns off the cursor.\
- DisplayText [l1c1]

### For receiving data
- Rule2 on MQTT#Connected do subscribe my_event, tele/tasmota_107EC8/RESULT ENDON
- Rule2 1
- Rule3 on EVENT#my_event Do DisplayText [l1c1] WaterLevel [l2c1] 4 ft ENDON
- Rule3 1

- tele/tasmota_107EC8/RESULT

  #### Command that I need to send

  ```
  DisplayText [l1c1] WaterLevel [l2c1] 4 ft 
  ```

  #### MQTT command that I need to send

  ```
  cmnd/tasmota_12A214/DisplayText [l1c1] WaterLevel [l2c1] 5 ft
  ```

  #### Subscription command seems not working; so I will focus on publishing from the sender

  Rule1 ON SR04#Distance DO LoRaCommand display_unit DisplayText [z][x0y0]Water Level: %value% cm ENDON

  Rule1 on mqtt#connected do backlog Subscribe distance_update, tele/tasmota_107EC8/RESULT ,LoRaReceived Rule1 1

  Rule1 on mqtt#connected do backlog Subscribe dis_update,tele/tasmota_107EC8/RESULT,temperature; Rule1 1 endon on event#dis_update do DisplayText [l1c1] WaterLevel [l2c1] 6 ft endon

  Subscribe MyResult, tele/tasmota_107EC8/RESULT/LoRaReceived


  

