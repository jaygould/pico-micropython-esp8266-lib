# Raspberry Pi Pico Micropython ESP8266 Library

## Description
This is a Micropython Library for RaspberryPi-Pico to communicate with ESP8266 using AT command.

## Hardware Connection
<p align="center">
<img src="https://user-images.githubusercontent.com/29272159/134862637-7109bc5b-ac92-4637-8ca6-b4d2fd6d9656.png" width="800">
</p>

## Getting Started with Examples

This is a basic AT Command library for RaspberryPi-Pico, which simplifies the communication process with the ESP8266. 
The best way to understand the library is with the example shown below, This example shows you how to activate access point mode to allow you to connect to the ESP **without an internet connection** and see data directly on the Pico.

The example here is basic at the moment, but can easily be extended to allow data access to sensors and other UART connected modules like GPS:

### Use ESP as a web server

Run the below, and **connect to 

```python
from esp import ESP8266

esp01 = ESP8266()
esp8266_at_ver = None
esp8266_at_ver = esp01.getVersion()
if(esp8266_at_ver != None):
    print("AT version: ",esp8266_at_ver)

print("StartUP: ",esp01.startUP())
print("Echo-Off: ",esp01.echoING())


print("WiFi Current Mode: ",esp01.setCurrentWiFiMode())
print("Set as access point: ",esp01.createAsAccessPoint('Yoshi'))
print("Local IP address: ",esp01.getLocalIpAddress())
print("Create server: ",esp01.createServer())

esp01.listenForConnections()

```

Here's another example of using as a web server and performing HTTP requests to the web:

### Connect ESP to internet and perform HTTP requests

```python
from machine import Pin
from esp8266 import ESP8266
import time, sys

print("~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~")
print("RPi-Pico MicroPython Ver:", sys.version)
print("~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~")

## Create On-board Led object
led=Pin(25,Pin.OUT)

## Create an ESP8266 Object
esp01 = ESP8266()
esp8266_at_ver = None
esp8266_at_ver = esp01.getVersion()
if(esp8266_at_ver != None):
    print(esp8266_at_ver)


print("StartUP",esp01.startUP())
print("Echo-Off",esp01.echoING())
print("\r\n")

print("WiFi Current Mode:",esp01.setCurrentWiFiMode()
  
print("Try to connect with the WiFi..")
while (1):
    if "WIFI CONNECTED" in esp01.connectWiFi("ssid","pwd"):
        print("ESP8266 connect with the WiFi..")
        break;
    else:
        print(".")
        time.sleep(2)


print("\r\n\r\n")
print("Now it's time to start HTTP Get/Post Operation.......\r\n")

while(1):    
    led.toggle()
    time.sleep(1)
    
    httpCode, httpRes = esp01.doHttpGet("www.httpbin.org","/ip","RaspberryPi-Pico", port=80)
    print("------------- www.httpbin.org/ip Get Operation Result -----------------------")
    print("HTTP Code:",httpCode)
    print("HTTP Response:",httpRes)
    print("-----------------------------------------------------------------------------\r\n\r\n")
    
    
    post_json="{\"name\":\"Noyel\"}"
    httpCode, httpRes = esp01.doHttpPost("www.httpbin.org","/post","RPi-Pico", "application/json",post_json,port=80)
    print("------------- www.httpbin.org/post Post Operation Result -----------------------")
    print("HTTP Code:",httpCode)
    print("HTTP Response:",httpRes)
    print("--------------------------------------------------------------------------------\r\n\r\n")
```