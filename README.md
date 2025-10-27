#define BLYNK_TEMPLATE_ID "TMPL3pC2wHqf-"
#define BLYNK_TEMPLATE_NAME "Agriculture Monitoring System"
#define BLYNK_AUTH_TOKEN "VvCqK1otLq20gdCV4-mnwfhM8AWDScbt"

#include <WiFi.h>
#include <BlynkSimpleEsp32.h>
#include <DHT.h>

#define DHT_PIN 4
#define SOIL_PIN 34
#define LIGHT_PIN 35
#define RAIN_PIN 32
#define RELAY_PIN 27
#define DHT_TYPE DHT11

DHT dht(DHT_PIN, DHT_TYPE);
BlynkTimer timer;

float humidity = 0;
float temperature = 0;
int soilMoisture = 0;
int lightIntensity = 0;
bool isRaining = false;
bool pumpState = false;
bool autoMode = true;

#define VPIN_HUMIDITY V0
#define VPIN_TEMPERATURE V1
#define VPIN_SOIL V2
#define VPIN_LIGHT V3
#define VPIN_RAIN V4
#define VPIN_PUMP V5
#define VPIN_AUTO V6
#define VPIN_RAIN_LED V7
#define VPIN_AUTO_LED V8
#define VPIN_PUMP_LED V9

char ssid[] = "WiFi";
char pass[] = "wordpass";

void setup() {
  Serial.begin(115200);
  delay(500);
  pinMode(RAIN_PIN, INPUT_PULLUP);
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, LOW);
  dht.begin();
  Blynk.begin(BLYNK_AUTH_TOKEN, ssid, pass);
  timer.setInterval(5000L, sendSensorData);
  timer.setInterval(3000L, updateLEDs);
  timer.setInterval(10000L, controlPump);
}

void loop() {
  Blynk.run();
  timer.run();
}

void readSensors() {
  float newHum = dht.readHumidity();
  float newTemp = dht.readTemperature();
  if (!isnan(newHum) && !isnan(newTemp)) {
    humidity = newHum;
    temperature = newTemp;
  }
  int soilRaw = analogRead(SOIL_PIN);
  soilMoisture = map(soilRaw, 4095, 1200, 0, 100);
  soilMoisture = constrain(soilMoisture, 0, 100);
  int lightRaw = analogRead(LIGHT_PIN);
  lightIntensity = map(lightRaw, 0, 4095, 0, 100);
  lightIntensity = constrain(lightIntensity, 0, 100);
  isRaining = digitalRead(RAIN_PIN) == LOW;
}

void controlPump() {
  readSensors();
  if (autoMode) {
    if (soilMoisture < 30 && !isRaining) {
      if (!pumpState) {
        digitalWrite(RELAY_PIN, HIGH);
        pumpState = true;
      }
    } else {
      if (pumpState) {
        digitalWrite(RELAY_PIN, LOW);
        pumpState = false;
      }
    }
  }
}

void updateLEDs() {
  Blynk.virtualWrite(VPIN_RAIN_LED, isRaining ? 255 : 0);
  Blynk.virtualWrite(VPIN_AUTO_LED, autoMode ? 255 : 0);
  Blynk.virtualWrite(VPIN_PUMP_LED, pumpState ? 255 : 0);
}

void sendSensorData() {
  readSensors();
  Blynk.virtualWrite(VPIN_HUMIDITY, humidity);
  Blynk.virtualWrite(VPIN_TEMPERATURE, temperature);
  Blynk.virtualWrite(VPIN_SOIL, soilMoisture);
  Blynk.virtualWrite(VPIN_LIGHT, lightIntensity);
  Blynk.virtualWrite(VPIN_RAIN, isRaining ? 1 : 0);
  Blynk.virtualWrite(VPIN_PUMP, pumpState ? 1 : 0);
  Blynk.virtualWrite(VPIN_AUTO, autoMode ? 1 : 0);
}

BLYNK_WRITE(VPIN_PUMP) {
  int val = param.asInt();
  if (!autoMode) {
    pumpState = val == 1;
    digitalWrite(RELAY_PIN, pumpState ? HIGH : LOW);
  }
}

BLYNK_WRITE(VPIN_AUTO) {
  autoMode = param.asInt() == 1;
  if (!autoMode) {
    digitalWrite(RELAY_PIN, LOW);
    pumpState = false;
  }
  Blynk.virtualWrite(VPIN_PUMP, pumpState ? 1 : 0);
}
