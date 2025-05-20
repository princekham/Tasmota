# Oled
- model -SSD1306 OLED display
- connection type - I2C
- D1 (GPIO5) - SCL
- D2 (GPIO4) - SDA
- I2C address - 0x3C (for 128x32)

- ![image](https://github.com/user-attachments/assets/af5e3289-518c-4f51-9897-2c3571ebe481)


![image](https://github.com/user-attachments/assets/5eea5598-30d4-401c-b200-b0c219255530)

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
- The depth of the water tank - 58 in (147 cm)
- The water level (in feet) would be (147-sensor reading)*0.0328
- The above rule will be
- 
- Rule1 ON event#addvar do sub %value% 147 ENDON DO SR04#Distance Publish cmnd/tasmota_1E6EDE/DisplayText [x0y0] WaterLevel [x10y16] %value% cm ENDON
- Finally the following rule works.

- Rule1 on SR04#Distance do var1=147-%value% endon On var1#state do Publish cmnd/tasmota_1E6EDE/DisplayText [x0y0] WaterLevel [x10y16] %value% endon

- Rule1 on SR04#Distance do var1=(147-%value%)*0.0328 endon On var1#state do Publish cmnd/tasmota_1E6EDE/DisplayText [x0y0] WaterLevel [x10y16] %value% ft  endon

- 
### For the receiver
- no need to configure
