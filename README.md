# CROP-PROTECTOR--360
# IT IS A PROJECT USEFUL FOR CULTIVATION LANDS WHEN RAIN IS FALLING
#include <ESP8266WiFi.h>
#include <Servo.h>
#include <DHT.h>

#define DHTPIN D1         // Pin where DHT11 data is connected
#define DHTTYPE DHT11     // Define sensor type
#define RAIN_SENSOR_PIN A0 // Pin where rain sensor analog data is connected

Servo servo1;            // Create servo object 1

DHT dht(DHTPIN, DHTTYPE); // Initialize DHT sensor

int rainSensorValue = 0; // Variable to store the rain sensor value
int rainThreshold = 500; // Set a threshold value to determine rain (adjust based on sensor)

void setup() {
  Serial.begin(115200);
  
  // Initialize the servos
  servo1.attach(D4); // Attach servo 1 to pin D4

  // Initialize DHT sensor
  dht.begin();
  
  // Set up rain sensor input pin
  pinMode(RAIN_SENSOR_PIN, INPUT);
  
  // Initial position of the servos (at 0 degrees)
  servo1.write(0);
}

void loop() {
  // Read the analog value from the rain sensor (0-1023 range)
  rainSensorValue = analogRead(RAIN_SENSOR_PIN);
  
  // Read temperature and humidity from DHT11
  float temperature = dht.readTemperature();  // Read temperature in Celsius
  float humidity = dht.readHumidity();        // Read humidity
  
  // Check if the readings are valid
  if (isnan(temperature) || isnan(humidity)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }
  
  // Print the readings to the Serial Monito
  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.print(" °C  ");
  Serial.print("Humidity: ");
  Serial.print(humidity);
  Serial.println(" %");

  // Print the rain sensor value for debugging
  Serial.print("Rain Sensor Value: ");
  Serial.println(rainSensorValue);
  
  // If rain is detected (if the rain sensor value is above the threshold)
  if (rainSensorValue > rainThreshold) {
    // Rain detected, rotate the servos from 0° to 90°
    servo1.write(90); // Rotate servo 1 to 90°
    Serial.println("Rain detected! Moving servos...");
  } else {
    // No rain detected, set servos back to 0°
    servo1.write(0);  // Rotate servo 1 to 0°
    Serial.println("No rain detected.");
  }
  
  // Delay before the next loop iteration
  delay(1000); // Update every 1 second
}
