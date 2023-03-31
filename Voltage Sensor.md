Voltage Sensor

Items used
- ZMPT101B
- ESP8266
- AC to 5V transformer (0.6 A)
- LD1117AV33

ZMPT101B
 - input voltage 3.3 V

LD1117AV33
 - PINOUT - G | O/p | I/p
![image](https://user-images.githubusercontent.com/16104631/228725944-08bd25c6-a7d9-4b36-9d20-3c4ae6031139.png)

I will use one ADC avaiable on the board for reciving Voltage input

The input voltage accepted by ESP8266 is 1 V max for the ADC. So I will have to use voltage divider circuit 10k and 6.8 or 6.2 K for reducing 1.6 V to 1V.

