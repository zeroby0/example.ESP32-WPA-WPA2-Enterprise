# Connect ESP32 to WPA/WPA2-Enterprise
[![License: Unlicense](https://img.shields.io/badge/license-Unlicense-blue.svg)](http://unlicense.org/)
[![](https://img.shields.io/github/issues/zeroby0/example.ESP32-WPA-WPA2-Enterprise.svg?color=blue)](https://github.com/zeroby0/example.ESP32-WPA-WPA2-Enterprise/issues)



This uses the [Arduino core for ESP32](https://github.com/espressif/arduino-esp32).

If you haven't already, I highly recommend checking out [Platformio IDE](https://platformio.org/platformio-ide). The complete tool chain for ESP32 and several other microcontrollers is available with it out of the box.

ESP-8266 doesn't support WPA2-Enterprise as of 9th April 2019. The following code may or may not work for you with an ESP8266.

``` C
#include <Arduino.h> 
#include <WiFi.h>

#include "esp_wifi.h"
#include "esp_wpa2.h"

#define EAP_IDENTITY "org\\user.name"
#define EAP_PASSWORD "Password"
const char* ssid = "WIFI-NAME";

void setup() {
  WiFi.mode(WIFI_MODE_STA); 
  esp_log_level_set("*", ESP_LOG_DEBUG);
  
  WiFi.disconnect(true);
  
  esp_wifi_sta_wpa2_ent_clear_ca_cert();
  esp_wifi_sta_wpa2_ent_clear_identity();
  esp_wifi_sta_wpa2_ent_clear_username();
  esp_wifi_sta_wpa2_ent_clear_password();

  esp_wifi_sta_wpa2_ent_set_identity((uint8_t *)EAP_IDENTITY, strlen(EAP_IDENTITY));
  esp_wifi_sta_wpa2_ent_set_username((uint8_t *)EAP_IDENTITY, strlen(EAP_IDENTITY));
  esp_wifi_sta_wpa2_ent_set_password((uint8_t *)EAP_PASSWORD, strlen(EAP_PASSWORD));

  esp_wpa2_config_t config = WPA2_CONFIG_INIT_DEFAULT();
  
  esp_wifi_sta_wpa2_ent_enable(&config);  
  WiFi.begin(ssid); // Connect
  
  while (WiFi.status() != WL_CONNECTED) { // Wait till you're connected.
    delay(200);
  }
}

void loop() {

}
```
