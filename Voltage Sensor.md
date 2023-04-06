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

I will use Arduino Pro Mini (5V) to connect with Tasmota device with TasmotaClinet to get data from the voltage sensor (ZMPT101B)

- I will use codes from the following web page : https://electropeak.com/learn/interfacing-zmpt101b-voltage-sensor-with-arduino/ 
- And teleport it with the following codes : 

```
/*
  To send command - cmnd/<device name>_fb/ClientSend <payload>  // Added by Sao
  
  BlinkRelay.ino - Example for TasmotaClient receiving the FUNC_EVERY_SECOND
                   callback and respond by toggling the LED as configured.
                   This example also extends sending commands back to the
                   Tasmota device by which could be any command normally
                   executed from the console but in this particular example
                   we just use a simple publish command.
  Copyright (C) 2019  Andre Thomas
  This program is free software: you can redistribute it and/or modify
  it under the terms of the GNU General Public License as published by
  the Free Software Foundation, either version 3 of the License, or
  (at your option) any later version.
  This program is distributed in the hope that it will be useful,
  but WITHOUT ANY WARRANTY; without even the implied warranty of
  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
  GNU General Public License for more details.
  You should have received a copy of the GNU General Public License
  along with this program.  If not, see <http://www.gnu.org/licenses/>.
*/

#include <Arduino.h>
#include <TasmotaClient.h>

TasmotaClient client(&Serial);

bool ledstate = false;

/*******************************************************************\
 * Function which will be called when Tasmota sends a
 * FUNC_EVERY_SECOND command
\*******************************************************************/

void user_FUNC_EVERY_SECOND(void)
{
  if (ledstate) {
   // ledstate = false;
    digitalWrite(LED_BUILTIN, LOW);
  } else {
   // ledstate = true;
    digitalWrite(LED_BUILTIN, HIGH);
  }
  if (ledstate) {
    client.ExecuteCommand((char*)"publish tele/mytopic/power on");
  } else {
    client.ExecuteCommand((char*)"publish tele/mytopic/power off");
  }
}

void user_FUNC_RECEIVE(char *data)
{
  if (!strcmp(data, "ON")) { // SlaveSend ON
    digitalWrite(LED_BUILTIN, HIGH);
    client.ExecuteCommand((char*)"publish tele/mytopic/power on");
  }
  if (!strcmp(data, "OFF")) { // SlaveSend OFF
    digitalWrite(LED_BUILTIN, LOW);
    client.ExecuteCommand((char*)"publish tele/mytopic/power off");
  }
}

/*******************************************************************\
 * Normal setup() function for Arduino to configure the serial port
 * speed (which should match what was configured in Tasmota) and
 * attach any callback functions associated with specific requests
 * or commands that are sent by Tasmota.
\*******************************************************************/

void setup() {
  // Configure serial port
  Serial.begin(57600);
  // We're going to use the builtin LED so lets configure the pin as OUTPUT
  pinMode(LED_BUILTIN, OUTPUT);
  // Attach the callback function which will be called when Tasmota requests it
  // client.attach_FUNC_EVERY_SECOND(user_FUNC_EVERY_SECOND);

  client.attach_FUNC_COMMAND_SEND(user_FUNC_RECEIVE);
}

void loop() {
  // Call the client loop function every so often to process incoming requests
  client.loop();
  
  ```
  
  I can remove 'user_FUNC_RECEIVE' function because I will not be receiving data from the Tasmota device.
  
  So the final codes will be as follow:
  
  ```
  /*
  To send command - cmnd/<device name>_fb/ClientSend <payload>  // Added by Sao
  
  BlinkRelay.ino - Example for TasmotaClient receiving the FUNC_EVERY_SECOND
                   callback and respond by toggling the LED as configured.
                   This example also extends sending commands back to the
                   Tasmota device by which could be any command normally
                   executed from the console but in this particular example
                   we just use a simple publish command.
  Copyright (C) 2019  Andre Thomas
  This program is free software: you can redistribute it and/or modify
  it under the terms of the GNU General Public License as published by
  the Free Software Foundation, either version 3 of the License, or
  (at your option) any later version.
  This program is distributed in the hope that it will be useful,
  but WITHOUT ANY WARRANTY; without even the implied warranty of
  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
  GNU General Public License for more details.
  You should have received a copy of the GNU General Public License
  along with this program.  If not, see <http://www.gnu.org/licenses/>.
*/

#include <Arduino.h>
#include <TasmotaClient.h>

TasmotaClient client(&Serial);

bool ledstate = false;

/*******************************************************************\
 * Function which will be called when Tasmota sends a
 * FUNC_EVERY_SECOND command
\*******************************************************************/

/*
  ZMPT101B - AC Voltage sensor
  Calculate Voltage

  modified on 7 Sep 2020
  by Mohammad Reza Akbari @ Electropeak
  Home
*/


double sensorValue1 = 0;
double sensorValue2 = 0;
int crosscount = 0;
int climb_flag = 0;
int val[100];
int max_v = 0;
double VmaxD = 0;
double VeffD = 0;
double Veff = 0;


void user_FUNC_EVERY_SECOND(void)
{

  if (ledstate) {
    client.ExecuteCommand((char*)"publish tele/mytopic/power on");
  } else {
    client.ExecuteCommand((char*)"publish tele/mytopic/power off");
  }
}


/*******************************************************************\
 * Normal setup() function for Arduino to configure the serial port
 * speed (which should match what was configured in Tasmota) and
 * attach any callback functions associated with specific requests
 * or commands that are sent by Tasmota.
\*******************************************************************/

void setup() {
  // Configure serial port
  Serial.begin(57600);
  // We're going to use the builtin LED so lets configure the pin as OUTPUT
  pinMode(LED_BUILTIN, OUTPUT);
  // Attach the callback function which will be called when Tasmota requests it
  // client.attach_FUNC_EVERY_SECOND(user_FUNC_EVERY_SECOND);

  client.attach_FUNC_COMMAND_SEND(user_FUNC_RECEIVE);
}

void loop() {
  // Call the client loop function every so often to process incoming requests
  client.loop();
  
   for ( int i = 0; i < 100; i++ ) {
    sensorValue1 = analogRead(A0);
    if (analogRead(A0) > 511) {
      val[i] = sensorValue1;
    }
    else {
      val[i] = 0;
    }
    delay(1);
  }
  
  max_v = 0;

  for ( int i = 0; i < 100; i++ )
  {
    if ( val[i] > max_v )
    {
      max_v = val[i];
    }
    val[i] = 0;
  }
  if (max_v != 0) {


    VmaxD = max_v;
    VeffD = VmaxD / sqrt(2);
    Veff = (((VeffD - 420.76) / -90.24) * -210.2) + 210.2;
  }
  else {
    Veff = 0;
  }
  Serial.print("Voltage: ");
  Serial.println(Veff);
  VmaxD = 0;

  delay(100);
  
   }
```

I used the following to convert double interger to char

```     String myString = "publish tele/voltage/";
    myString += String(myDouble, 2); // append the double value with two decimal places
    client.ExecuteCommand(myString.c_str()); // send the formatted string as a char array
    ```
And the final codes for the Arduino Pro mini (5V) is as follow

```
/*
  To send command - cmnd/<device name>_fb/ClientSend <payload>  // Added by Sao
  
  BlinkRelay.ino - Example for TasmotaClient receiving the FUNC_EVERY_SECOND
                   callback and respond by toggling the LED as configured.
                   This example also extends sending commands back to the
                   Tasmota device by which could be any command normally
                   executed from the console but in this particular example
                   we just use a simple publish command.
  Copyright (C) 2019  Andre Thomas
  This program is free software: you can redistribute it and/or modify
  it under the terms of the GNU General Public License as published by
  the Free Software Foundation, either version 3 of the License, or
  (at your option) any later version.
  This program is distributed in the hope that it will be useful,
  but WITHOUT ANY WARRANTY; without even the implied warranty of
  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
  GNU General Public License for more details.
  You should have received a copy of the GNU General Public License
  along with this program.  If not, see <http://www.gnu.org/licenses/>.
*/

#include <Arduino.h>
#include <TasmotaClient.h>

TasmotaClient client(&Serial);

bool ledstate = false;

double myDouble = 123.45;
char myChar[50]; // create a char array to hold the formatted string

/*
  ZMPT101B - AC Voltage sensor
  Calculate Voltage

  modified on 7 Sep 2020
  by Mohammad Reza Akbari @ Electropeak
  Home
*/


double sensorValue1 = 0;
double sensorValue2 = 0;
int crosscount = 0;
int climb_flag = 0;
int val[100];
int max_v = 0;
double VmaxD = 0;
double VeffD = 0;
double Veff = 0;



/*******************************************************************\
 * Function which will be called when Tasmota sends a
 * FUNC_EVERY_SECOND command
\*******************************************************************/

void user_FUNC_EVERY_SECOND(void)
{


   for ( int i = 0; i < 100; i++ ) {
    sensorValue1 = analogRead(A0);
    if (analogRead(A0) > 511) {
      val[i] = sensorValue1;
    }
    else {
      val[i] = 0;
    }
    delay(1);
  }

  max_v = 0;

  for ( int i = 0; i < 100; i++ )
  {
    if ( val[i] > max_v )
    {
      max_v = val[i];
    }
    val[i] = 0;
  }
  if (max_v != 0) {


    VmaxD = max_v;
    VeffD = VmaxD / sqrt(2);
    Veff = (((VeffD - 420.76) / -90.24) * -210.2) + 210.2;
  }
  else {
    Veff = 0;
  }
 // Serial.print("Voltage: ");
//  Serial.println(Veff);



    String myString = "publish tele/voltage ";
    myString+=String(Veff, 2); // append the double value with two decimal places
    client.ExecuteCommand(myString.c_str()); // send the formatted string as a char array

    VmaxD = 0;
    
}



/*******************************************************************\
 * Normal setup() function for Arduino to configure the serial port
 * speed (which should match what was configured in Tasmota) and
 * attach any callback functions associated with specific requests
 * or commands that are sent by Tasmota.
\*******************************************************************/

void setup() {
  // Configure serial port
  Serial.begin(57600);

  // Attach the callback function which will be called when Tasmota requests it
   client.attach_FUNC_EVERY_SECOND(user_FUNC_EVERY_SECOND);

 // client.attach_FUNC_COMMAND_SEND(user_FUNC_RECEIVE);
}

void loop() {
  // Call the client loop function every so often to process incoming requests
  client.loop();

}
```
 
- for adjusting voltage CW - lower the voltage and CCW - raise the voltage
