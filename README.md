# ESP-wake
en:
Code to enable ESP32 Through an optocoupler or WOL.
ru: это лишь моя идея для включения сервера на ESP32, через WOL или потопару.
with WOL: 
```
#include <WiFi.h>
#include <WiFiUdp.h>
#include <WakeOnLan.h>
#include <ESP32Ping.h>
#define PIN_LEDR 13 //можете здесь заменить на любой вам угодный
#define PIN_LEDG 14 //можете здесь заменить на любой вам удобный
WiFiUDP udp;
WakeOnLan wol(udp);

void setup() {
    Serial.begin(115200);
pinMode(PIN_LEDR, OUTPUT);
pinMode(PIN_LEDG, OUTPUT);
    WiFi.begin(your wifi ssid, wifi password);
    while (WiFi.status() != WL_CONNECTED) delay(500);
    wol.setRepeat(3, 100);
}

void loop() {
    Serial.print(" checking server");
    if (Ping.ping(your server serverip)) {
        Serial.println(" server is on");
digitalWrite(PIN_LEDG, HIGH);
digitalWrite(PIN_LEDR, LOW);
    } else {
        Serial.println(" server off, turning on");
digitalWrite(PIN_LEDG, LOW);
digitalWrite(PIN_LEDR, HIGH);
        wol.sendMagicPacket(your macaddress)
        delay(300000); //ожидание 5-ти минут, можете поставить сколько нужно
}

```
with optocoupler:
```
#include <WiFi.h>
#include <WiFiUdp.h>
#include <ESP32Ping.h>
#define PIN_LEDR 13 //можете здесь заменить на любой вам угодный
#define PIN_LEDG 14 //можете здесь заменить на любой вам удобный
#define PIN_ON 15 //можете заменить на любой чам вам удобный
WiFiUDP udp;

void setup() {
    Serial.begin(115200);
pinMode(PIN_LEDR, OUTPUT);
pinMode(PIN_LEDG, OUTPUT);
pinMode(PIN_ON, OUTPUT);
    WiFi.begin(your wifi ssid, wifi password);
    while (WiFi.status() != WL_CONNECTED) delay(500);
    wol.setRepeat(3, 100);
}

void loop() {
    Serial.print(" checking server");
    if (Ping.ping(your server serverip)) {
        Serial.println(" server is on");
digitalWrite(PIN_LEDG, HIGH);
digitalWrite(PIN_LEDR, LOW);
    } else {
        Serial.println(" server off, turning on");
digitalWrite(PIN_LEDG, LOW);
digitalWrite(PIN_LEDR, HIGH);
digitalWrite(PIN_ON, HIGH);
delay (500);
digitalWrite (PIN_ON, LOW);
delay(300000); //ожидание 5-ти минут, можете поставить сколько нужно
}
```
To connect the optocoupler, the following circuit is used: 
```
GND ESP-------|katode--emmiter  |-----button server
              |                 |
PIN5 ESP------|anod----collector|-----button server
```
