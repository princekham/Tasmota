  ## ESP32 RTC

- pin GPIO16 (D16)
- We need to choose two pins for I2C - I2C SDA and I2C SCL
- They are chosen to be GPIO04 (D04) -> I2C SDA and GPIO05 (D05) -> SCL

## To enable RTC
- RtcNtpServer 1

  ## To disable
  - RtcNtpServer 0
 
## Compiling
- I used my raspberry pi 4 to compile (https://github.com/princekham/Tasmota/blob/main/CustomCompiling.md)
  1. sudo docker pull benzino77/tasmocompiler
  2. I chose the following when compiling
 
     ![image](https://github.com/user-attachments/assets/ac273ad2-6cb9-4b8f-8030-089118da1bfe)

  3. I added the following
     ```
     #ifndef USE_RTC_CHIPS 
     #define USE_RTC_CHIPS               // Enable RTC chip support and NTP server
     #endif
     #ifndef USE_DS3231
     #define USE_DS3231                  // [I2cDriver26] Enable DS3231 RTC (I2C address 0x68) (+1k2 code)
     #endif
     ```

