### Connecting to InfluxDB with Arduino

- https://github.com/tobiasschuerg/InfluxDB-Client-for-Arduino
- 
### My Arduino File for connecting InfluxDB (with SHT31 Sensor)
- I needed to install required library files before compiling
  
```

#if defined(ESP32)
  #include <WiFiMulti.h>
  WiFiMulti wifiMulti;
  #define DEVICE "ESP32"
  #elif defined(ESP8266)
  #include <ESP8266WiFiMulti.h>
  ESP8266WiFiMulti wifiMulti;
  #define DEVICE "ESP8266"
  #endif
  
  #include <InfluxDbClient.h>
  #include <InfluxDbCloud.h>

#include <Arduino.h>
#include <Wire.h>
#include <ArtronShop_SHT3x.h>

ArtronShop_SHT3x sht3x(0x44, &Wire); // ADDR: 0 => 0x44, ADDR: 1 => 0x45

  
  // WiFi AP SSID
  #define WIFI_SSID "Your WiFi SSID" // Your WiFi
  // WiFi password
  #define WIFI_PASSWORD "Your WiFi password"
  
  #define INFLUXDB_URL "Your InfluxDB URL without port no"
  #define INFLUXDB_TOKEN "Your InfluxDB Token"
  #define INFLUXDB_ORG "Your InfluxDB Token"
  #define INFLUXDB_BUCKET "Your InfluxDB Bucket"
  
  // Time zone info
  #define TZ_INFO "UTC6.5"
  
  // Declare InfluxDB client instance with preconfigured InfluxCloud certificate
  InfluxDBClient client(INFLUXDB_URL, INFLUXDB_ORG, INFLUXDB_BUCKET, INFLUXDB_TOKEN, InfluxDbCloud2CACert);
  
  // Declare Data point
  Point sensor("my_sensor");
  
  void setup() {
    Serial.begin(115200);
  
    // Setup wifi
    WiFi.mode(WIFI_STA);
    wifiMulti.addAP(WIFI_SSID, WIFI_PASSWORD);
  
    Serial.print("Connecting to wifi");
    while (wifiMulti.run() != WL_CONNECTED) {
      Serial.print(".");
      delay(100);
    }
    Serial.println();
  
    // Accurate time is necessary for certificate validation and writing in batches
    // We use the NTP servers in your area as provided by: https://www.pool.ntp.org/zone/
    // Syncing progress and the time will be printed to Serial.
    timeSync(TZ_INFO, "pool.ntp.org", "time.nis.gov");
  
    Wire.begin();
  while (!sht3x.begin()) {
    Serial.println("SHT3x not found !");
    delay(1000);
  }
  
    // Check server connection
    if (client.validateConnection()) {
      Serial.print("Connected to InfluxDB: ");
      Serial.println(client.getServerUrl());
    } else {
      Serial.print("InfluxDB connection failed: ");
      Serial.println(client.getLastErrorMessage());
    }

//    sensor.addTag("device", DEVICE);
//    sensor.addTag("SSID", WiFi.SSID());

  }
  void loop() {

   if (sht3x.measure()) {
    Serial.print("Temperature: ");
    Serial.print(sht3x.temperature(), 1);
    Serial.print(" *C\tHumidity: ");
    Serial.print(sht3x.humidity(), 1);
    Serial.print(" %RH");
    Serial.println();
  } else {
    Serial.println("SHT3x read error");
  }

 // Clear fields for reusing the point. Tags will remain the same as set above.
    sensor.clearFields();

   
  
    // Store measured value into point
    // Report RSSI of currently connected network
    sensor.addField("temperature", sht3x.temperature());
    sensor.addField("humidity", sht3x.humidity());
    // Print what are we exactly writing
    Serial.print("Writing: ");
    Serial.println(sensor.toLineProtocol());
  
    // Check WiFi connection and reconnect if needed
    if (wifiMulti.run() != WL_CONNECTED) {
      Serial.println("Wifi connection lost");
    }
  
    // Write point
    if (!client.writePoint(sensor)) {
      Serial.print("InfluxDB write failed: ");
      Serial.println(client.getLastErrorMessage());
    }
  
    Serial.println("Waiting 3 minute");
    delay(180000);

  }

```
