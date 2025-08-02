# ESP32-CAM

![image](https://github.com/user-attachments/assets/fb59e1f6-dd8e-4782-8a87-3f55fde74072)

- I think I need to connect based on this (https://templates.blakadder.com/ai-thinker_ESP32-CAM.html)

- GPIO00	CAM_XCLK
- GPIO05 CAM_DATA 1
- GPIO18	CAM_DATA 2
- GPIO19	CAM_DATA 3
- GPIO21	CAM_DATA 4
- GPIO22	CAM_PCLK
- GPIO23	CAM_HREF
- GPIO25	CAM_VSYNC
- GPIO26	CAM_SIOD
- GPIO27	CAM_SIOC
- GPIO32	CAM_PWDN
- GPIO34	CAM_DATA 7
- GPIO35	CAM_DATA 8
- GPIO36	CAM_DATA 5
- GPIO39	CAM_DATA 6

- I tried to change some pins and the following is my pin connections
- {"NAME":"MY ESP32-CAM","GPIO":[1,1,1,1,4992,1,1,1,5090,5091,5092,5093,1,1,5094,4928,0,5024,5056,5152,0,5120,4960,4992,0,0,0,0,5088,5089,5095,1,1,0,0,1],"FLAG":0,"BASE":1}
- 
- GPIO22	CAM_SIOC 
- GPIO21	CAM_SIOD 
- GPIO25	CAM_VSYNC
- GPIO23	CAM_HREF
- GPIO26  CAM_RESET 
- GPIO27	CAM_PCLK  
- GPIO04	CAM_XCLK
- GPIO34	CAM_DATA 8
- GPIO18	CAM_DATA 7
- GPIO15	CAM_DATA 6 
- GPIO14	CAM_DATA 5 
- GPIO13	CAM_DATA 4 
- GPIO12	CAM_DATA 3 
- GPIO33	CAM_DATA 2 
- GPIO32	CAM_DATA 1 
- GPIO19	CAM_PWDN

- The following pin connection was suggested by DeepSeek

![image](https://github.com/user-attachments/assets/118a0737-0da5-43bf-a9ad-a9e9006e4c0b)

### Using the web cam
-  the MJPEG stream URL (e.g., http://<ESP32-CAM_IP_ADDRESS>:81/stream).
-  You can find the snapshot URL (e.g., http://<ESP32-CAM_IP_ADDRESS>/wc.jpg or snapshot.jpg)
-  
