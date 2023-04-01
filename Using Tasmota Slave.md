- To be used with - BlinkSendCommand.ino
- This one needs precompiled binary. For more information, please read at https://tasmota.github.io/docs/TasmotaClient/
- We can use this so that the Arduino can telepost data through Tasmota device.

For custom build, we have to enter the following codes



#ifndef USE_TASMOTA_CLIENT
#define USE_TASMOTA_CLIENT    // Enable the driver
#endif

#ifndef USE_TASMOTA_CLIENT_FLASH_SPEED
#define USE_TASMOTA_CLIENT_FLASH_SPEED 57600    // Configure the baud rate of the bootloader
#endif

#ifndef USE_TASMOTA_CLIENT_SERIAL_SPEED 57600  
#define USE_TASMOTA_CLIENT_SERIAL_SPEED 57600    // Configure the baud rate at which the client microcontroller will be interfacing to Tasmota
#endif



To custom build a Tasmota device 

- go to https://www.gitpod.io/
- register and sign in
- then open the master realease at this page https://tasmota.github.io/docs/Gitpod/
- Ref: https://www.youtube.com/watch?v=WashxTcHiDc&ab_channel=vccground

