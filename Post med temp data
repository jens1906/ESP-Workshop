#include <max6675.h>
#include <WiFi.h>
#include <HTTPClient.h>
#include <ArduinoJson.h>


int soPin = 33;// SO=Serial Out
int csPin = 25;// CS = chip select CS pin
int sckPin = 26;// SCK = Serial Clock pin
MAX6675 Module(sckPin, csPin, soPin);

const char* ssid = "ANDET";
const char* password = "SkoleUdstyrBGE";

void setup() {
  Serial.begin(115200);
  delay(4000);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.println("Connecting to WiFi..");
  }
  Serial.println("Connected to the WiFi network");
}

void loop() {
  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;
    http.begin("http://espworkshop.ml/62205b18-4b3b-4383-bdeb-033f5bbd42bb/");
    http.addHeader("Content-Type", "text/plain");
    Serial.print("C = ");
    Serial.println(Module.readCelsius());
    String temp = String(Module.readCelsius(), 0);
    int httpResponseCode = http.POST((temp));
    if (httpResponseCode > 0) {
      String response = http.getString();
      Serial.println(httpResponseCode);
      Serial.println(response);
      DynamicJsonBuffer  jsonBuffer(200);
      JsonObject& root = jsonBuffer.parseObject(response);
      if (!root.success()) {
        Serial.println("parseObject() failed");
        return;
      }
      int Data = root["data"];
      Serial.println(Data);
      Serial.println("C");

      //int DataN = map(Data, 0, 100, 0, 1000);
      //Serial.println(DataN);
    } else {
      Serial.print("Error on sending POST: ");
      Serial.println(httpResponseCode);
    }
    http.end();
  } else {
    Serial.println("Error in WiFi connection");
  }
  delay(10000);
}
