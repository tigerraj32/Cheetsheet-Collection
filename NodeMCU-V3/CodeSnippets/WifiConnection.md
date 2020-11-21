# Connect to Wifi Network

## WiFi Mode : WIFI_STA (Station)

Devices that connect to Wi-Fi networks are called stations (STA). Connection to Wi-Fi is provided by an access point (AP), that acts as a hub for one or more stations. The access point on the other end is connected to a wired network. An access point is usually integrated with a router to provide access from a Wi-Fi network to the internet. Each access point is recognized by a SSID (Service Set IDentifier), that essentially is the name of network you select when connecting a device (station) to the Wi-Fi.

ESP8266 modules can operate as a station, so we can connect it to the Wi-Fi network.

![pinout_arduino_uno](../..//resources/WiFi-station-mode.png)
<br>
    [Documentation](https://arduino-esp8266.readthedocs.io/en/latest/esp8266wifi/station-class.html)
<br>

```c++
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

  //WiFi.mode(WIFI_STA); //configure as stations that connect to wifi-network (AP)

  IPAddress staticIP(192, 168, 100, 161);
  IPAddress gateway(192, 168, 100, 1);
  IPAddress subnet(255, 255, 255, 0);
  WiFi.config(staticIP, gateway, subnet);

  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi connected with following info");

  Serial.printf(" Wifi SSID: %s\n Wifi PSK: %s\n Wifi AP MAC Address: %s\n RSSI: %d dBm\n", WiFi.SSID().c_str(), WiFi.psk().c_str(), WiFi.BSSIDstr().c_str(), WiFi.RSSI());
  Serial.printf(" DNS #1: %s, DNS #2: %s\n", WiFi.dnsIP().toString().c_str(), WiFi.dnsIP(1).toString().c_str());
  Serial.printf(" IP Address: %s\n Gataway IP: %s\n Subnet mask: %s\n", WiFi.localIP().toString().c_str(), WiFi.gatewayIP().toString().c_str(), WiFi.subnetMask().toString().c_str());
};

};

void setup()
{
 
  Serial.begin(115200);
  // Serial.setDebugOutput(true);
  connectToNetwork(STASSID, STAPSK);
}

void loop()
{
 
}

```


<br>
<br>

## Soft Access Point

An access point (AP) is a device that provides access to a Wi-Fi network to other devices (stations) and connects them to a wired network. The ESP8266 can provide similar functionality, except it does not have interface to a wired network. Such mode of operation is called soft access point (soft-AP). The maximum number of stations that can simultaneously be connected to the soft-AP can be set from 0 to 8, but defaults to 4.


### Uses

- The soft-AP mode is often used and an intermediate step before connecting ESP to a Wi-Fi in a station mode. This is when SSID and password to such network is not known upfront. ESP first boots in soft-AP mode, so we can connect to it using a laptop or a mobile phone. Then we are able to provide credentials to the target network. Then, the ESP is switched to the station mode and can connect to the target Wi-Fi.

- Another handy application of soft-AP mode is to set up mesh networks. The ESP can operate in both soft-AP and Station mode so it can act as a node of a mesh network.

![](../../resources/esp8266-soft-access-point.png)

### Notes:

- The network established by softAP will have default IP address of 192.168.4.1. This address may be changed using softAPConfig (see below).
Even though ESP8266 can operate in soft-AP + station mode, it actually has only one hardware channel. Therefore in soft-AP + station mode, the soft-AP channel will default to the number used by station. For more information how this may affect operation of stations connected to ESP8266’s soft-AP, please check this FAQ entry on Espressif forum.

<br>

[Documentation](https://arduino-esp8266.readthedocs.io/en/latest/esp8266wifi/soft-access-point-class.html)


### Sample Code

```c++
#include <Arduino.h>
#include <ESP8266WiFi.h>

#ifndef STASSID
#define STASSID "aarop_wlink"
#define STAPSK "Aa9808907D"
#endif

void configureSoftAP()
{
  IPAddress local_IP(192, 168, 4, 22);
  IPAddress gateway(192, 168, 4, 9);
  IPAddress subnet(255, 255, 255, 0);

  Serial.print("Setting soft-AP configuration ... ");
  Serial.println(WiFi.softAPConfig(local_IP, gateway, subnet) ? "Ready" : "Failed!");

  Serial.print("Setting soft-AP ... ");
  Serial.println(WiFi.softAP("NodeMCU_WIFI", "1234567890") ? "Ready" : "Failed!");

  Serial.print("Soft-AP IP address = ");
  Serial.println(WiFi.softAPIP());
}



void setup()
{
  // initialize LED digital pin as an output.
  pinMode(LED_BUILTIN, OUTPUT);

  Serial.begin(115200);
  // Serial.setDebugOutput(true);

  //connectToNetwork(STASSID, STAPSK);
  configureSoftAP();
}

void loop()
{
  Serial.printf("Stations connected = %d\n", WiFi.softAPgetStationNum()); // no of count connected to this ap.
}
```