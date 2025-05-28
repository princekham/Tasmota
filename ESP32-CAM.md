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
- GPIO27	CAM_SIOC //change to GPIO22
- GPIO26	CAM_SIOD //change to GPIO21
- GPIO25	CAM_VSYNC
- GPIO23	CAM_HREF
- GPIO21  CAM_RESET // change to GPIO26
- GPIO22	CAM_PCLK  //change to GPIO27
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
