# Connect to Wifi Network

### WiFi Mode : WIFI_STA (Station)

Devices that connect to Wi-Fi networks are called stations (STA). Connection to Wi-Fi is provided by an access point (AP), that acts as a hub for one or more stations. The access point on the other end is connected to a wired network. An access point is usually integrated with a router to provide access from a Wi-Fi network to the internet. Each access point is recognized by a SSID (Service Set IDentifier), that essentially is the name of network you select when connecting a device (station) to the Wi-Fi.

ESP8266 modules can operate as a station, so we can connect it to the Wi-Fi network.

![pinout_arduino_uno](../..//resources/WiFi-station-mode.png)
```c++
#include <Arduino.h>
#include <ESP8266WiFi.h>

#ifndef STASSID
#define STASSID "ssid"
#define STAPSK "password"
#endif

void connectToNetwork(char *ssid, char *password)
{

  Serial.println();
  Serial.println();
  Serial.println();

  for (uint8_t t = 4; t > 0; t--)
  {
    Serial.printf("[Connecting] to %s Please wait %d...\n", ssid, t);
    Serial.flush();
    delay(1000);
  }

  WiFi.mode(WIFI_STA); //configure as stations that connect to wifi-network (AP)

  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP()); //You can get IP address assigned to ESP
};

void setup()
{
  // initialize LED digital pin as an output.
  pinMode(LED_BUILTIN, OUTPUT);

  Serial.begin(115200);
  // Serial.setDebugOutput(true);
  connectToNetwork(STASSID, STAPSK);
}

void loop()
{
 
}

```