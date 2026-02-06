# SMQTT SERVER

## How to set a fixed IP address
- To set a static IP address, you use the WiFi.config() function before calling WiFi.begin(). 
- Here is a basic example sketch demonstrating the configuration:

```
#include <WiFi.h>

// Network credentials
const char* ssid = "YOUR_SSID";
const char* password = "YOUR_PASSWORD";

// Static IP configuration
IPAddress local_IP(192, 168, 100, 131); // Desired static IP
IPAddress gateway(192, 168, 100, 1);    // Router IP address
IPAddress subnet(255, 255, 255, 0);   // Subnet mask
// Optional: DNS servers (e.g., Google DNS)
IPAddress primaryDNS(8, 8, 8, 8);   
IPAddress secondaryDNS(8, 8, 4, 4); 

void setup() {
    Serial.begin(115200);
    Serial.println();

    // Configure the static IP address, gateway, subnet mask, and DNS
    if(!WiFi.config(local_IP, gateway, subnet, primaryDNS, secondaryDNS)){
        Serial.println("STA Failed to configure");
    }

    // Connect to Wi-Fi network
    Serial.print("Connecting to ");
    Serial.println(ssid);
    WiFi.begin(ssid, password);

    while (WiFi.status() != WL_CONNECTED) {
        delay(500);
        Serial.print(".");
    }

    Serial.println("");
    Serial.println("WiFi connected.");
    Serial.println("IP address: ");
    // Print the assigned IP address (should match local_IP)
    Serial.println(WiFi.localIP()); 
}

void loop() {
    // Your main code here
}
```

### How to Omit DNS
- The WiFi.config() function is structured so that you only provide the parameters you need. If you only provide three values, the ESP32 knows they are the Local IP, Gateway, and Subnet. 
- Correct usage without DNS:

```
IPAddress local_IP(192, 168, 100, 130);
IPAddress gateway(192, 168, 100, 1);
IPAddress subnet(255, 255, 255, 0);

void setup() {
  // Omit DNS entirely from the call
  WiFi.config(local_IP, gateway, subnet); 
  WiFi.begin(ssid, password);
}
```

- ဒီမှာက ap mode ပါတာနဲ့ မပါတာဆိုပြီး ရှိမယ်။ (source: https://www.youtube.com/watch?v=ji_nfVEI25g)
- 
- <img width="2493" height="1294" alt="image" src="https://github.com/user-attachments/assets/31184db9-2a9f-4906-b273-64047e4ab40f" />
-The following is with AP (

```
#include<sMQTTBroker.h>

const char* MQTT_CLIENT_USER = "princekham"; // username for mqtt clients. Set your own value here.
const char* MQTT_CLIENT_PASSWORD = "1qaz2wsx"; // password for mqtt clients. Set your own value here.

class MyBroker:public sMQTTBroker
{
public:
    bool onEvent(sMQTTEvent *event) override
    {
        switch(event->Type())
        {
        case NewClient_sMQTTEventType:
            {
                sMQTTNewClientEvent *e=(sMQTTNewClientEvent*)event;
                // Check username and password used for new connection
                if ((e->Login() != MQTT_CLIENT_USER) || (e->Password() != MQTT_CLIENT_PASSWORD)) {
                  Serial.println("Invalid username or password");  
                  return false;
                  }
            };
            break;
        case LostConnect_sMQTTEventType:
            WiFi.reconnect();
            break;
        case UnSubscribe_sMQTTEventType:
        case Subscribe_sMQTTEventType:
            {
                sMQTTSubUnSubClientEvent *e=(sMQTTSubUnSubClientEvent*)event;
            }
            break;
        }
        return true;
    }
};


MyBroker broker;



void setup()
{
    Serial.begin(115200);
      // Set up Wi-Fi in Access Point mode
    WiFi.softAP("ESP32_Hotspot", "12345678");  // Replace with your SSID and password


    // Display the IP address of the ESP32
    IPAddress IP = WiFi.softAPIP();
    Serial.print("AP IP address: ");
    Serial.println(IP);
   
    const unsigned short mqttPort=9000;
    broker.init(mqttPort);
    // all done
    // your magic code

};
void loop()
{
    broker.update();
    // your code here
};

```

- The following is without AP (source at : https://github.com/HighVoltages/ESP32-MQTT-broker/blob/main/ESP32%20MQTT%20broker/Secure_ESP32_MQTT_broker.ino)
- router ကိုတော့ သွား join မယ်

```
#include<sMQTTBroker.h>


const char* MQTT_CLIENT_USER = "Admin"; // username for mqtt clients. Set your own value here.
const char* MQTT_CLIENT_PASSWORD = "High voltages"; // password for mqtt clients. Set your own value here.


class MyBroker:public sMQTTBroker
{
public:
    bool onEvent(sMQTTEvent *event) override
    {
        switch(event->Type())
        {
        case NewClient_sMQTTEventType:
            {
                sMQTTNewClientEvent *e=(sMQTTNewClientEvent*)event;
                // Check username and password used for new connection
                if ((e->Login() != MQTT_CLIENT_USER) || (e->Password() != MQTT_CLIENT_PASSWORD)) {
                  Serial.println("Invalid username or password");  
                  return false;
                  }
            };
            break;
        case LostConnect_sMQTTEventType:
            WiFi.reconnect();
            break;
        case UnSubscribe_sMQTTEventType:
        case Subscribe_sMQTTEventType:
            {
                sMQTTSubUnSubClientEvent *e=(sMQTTSubUnSubClientEvent*)event;
            }
            break;
        }
        return true;
    }
};


MyBroker broker;



void setup()
{
    Serial.begin(115200);
      // Set up Wi-Fi in Access Point mode
  const char *ssid = "SSID";
const char *password = "Password";
    WiFi.begin(ssid, password);
    while (WiFi.status() != WL_CONNECTED) { // Wait for the Wi-Fi to connect
        delay(1000);
    }
    Serial.println("Connection established!");  
    Serial.print("IP address:\t");
    Serial.println(WiFi.localIP());
   
    const unsigned short mqttPort=1883;
    broker.init(mqttPort);
    // all done
    // your magic code

};
void loop()
{
    broker.update();
    // your code here
};

```

