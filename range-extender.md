# Range Extender 
- source (https://eddyds-smarthome.de/tasmota-range-extender/)
- ESP 32 achieves more upload and download speed than esp8266
- I am not sure yet I need to compile it or not
### Custom compilation (source : https://github.com/princekham/Tasmota/blob/main/CustomCompiling.md) 
- Wifi connection (ကိုယ်ချိတ်မယ့် ဝိုင်ဖိုင်)
  
```
#undef  STA_SSID1
#define STA_SSID1         "Hust24"           // [Ssid1] Wifi SSID

#undef  STA_PASS1
#define STA_PASS1         "Geheim0815xx"     // [Password1] Wifi password
```

- Range extender settings (Range extender လွင့်မယ့် ဝိုင်ဖိုင် နဲ့ အိုင်ပီ လိပ်စာ)

```
#define WIFI_RGX_SSID "Hust24K"
#define WIFI_RGX_PASSWORD "12345678"
#define WIFI_RGX_IP_ADDRESS "192.168.2.1"
#define WIFI_RGX_SUBNETMASK "255.255.255.0"
#define WIFI_RGX_STATE 1
#define WIFI_RGX_NAPT 1

#define USE_TELEGRAM
```
- From the web site, for ESP32
```
#ifndef USE_WIFI_RANGE_EXTENDER
#define USE_WIFI_RANGE_EXTENDER
#endif
// Optionally, to enable NAT/NAPT
#ifndef USE_WIFI_RANGE_EXTENDER_NAPT
#undef USE_WIFI_RANGE_EXTENDER_NAPT
#endif

#define WIFI_RGX_SSID "Hust24K"
#define WIFI_RGX_PASSWORD "12345678"
#define WIFI_RGX_IP_ADDRESS "192.168.2.1"
#define WIFI_RGX_SUBNETMASK "255.255.255.0"
#define WIFI_RGX_STATE 1
#define WIFI_RGX_NAPT 1

```
- From the web site, for ESP8266

```





- compile ပြီးရင် web installer ကနေ သွင်း
- အဆင်ပြေမပြေကို RgxSSid နဲ့ ရိုက်ထည့်ပြီး စစ်နိုင်
- ချိတ်နေတဲ့ client ကို သိဖို့

```
RgxClients
```
- အင်တာနက် သုံးဖို့ အတွက်ပဲ ဆိုရင် port forward လုပ်စရာမလိုဘူး
- tasmota device ချိတ်မယ်ဆိုလိုတယ်
- portforwar အတွက် ရိုက်ထည့်ရမယ် ဟာ

```
RgxPort tcp,8080,192.168.2.100,80
```
- ip က Tasmota ရဲ့ ip
- 192.168.2.100 ဆိုရင် range extender ကို ရောက်မယ် 192.168.2.100:8080 ဆိုရင် tasmota device ကို ရောက်မယ်။
- Tasmota Device ကိုတော့ static ip address ပေးထားရမယ်

  ```
  ipaddress1 192.168.2.100
  ```
  - Port forwarding အတွက် rule လုပ်ထားပေးရမယ်။ restart လုပ်ရင် RgxPort command က ပျောက်သွား

```
  rule1 on system#boot do RgxPort tcp,8080,192.168.2.100,80 endon
  rule1 ON
```
- custom compile မလုပ်ဘူးဆိုရင် အောက်က လိုင်းကို ရိုက်ထည့်ပေးရမယ်။

- ```
  RgxSSID Hust24K
  RgxPassword 12345678
  RgxAddress 192.168.2.1
  RgxSubnet 255.255.255.0
  RgxState ON
  RgxNAPT ON
```
  
