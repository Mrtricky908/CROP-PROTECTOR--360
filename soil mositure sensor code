# CROP-PROTECTOR-360-IOT-BASED-FARM-DEFENDER
const int sensor_pin = A0;  // Connect Soil moisture analog sensor pin to A0 of NodeMCU
const int relay_pin = D1;   // Define the relay control pin (connected to D1 for irrigation system)

void setup() {
  Serial.begin(9600);  // Define baud rate for serial communication
  pinMode(relay_pin, OUTPUT);  // Set relay pin as an output (for irrigation system)
  digitalWrite(relay_pin, LOW);  // Initially set the relay to off (assuming LOW is off)
}

void loop() {
  float moisture_percentage;

  // Read the soil moisture sensor value and convert it to percentage
  moisture_percentage = (100.00 - ((analogRead(sensor_pin) / 1023.00) * 100.00));

  // Print the moisture percentage to the serial monitor
  Serial.print("Soil Moisture (in Percentage) = ");
  Serial.print(moisture_percentage);
  Serial.println("%");

  // If moisture is less than 50%, turn the relay ON to water the soil
  if (moisture_percentage < 50.0) {
    digitalWrite(relay_pin, HIGH);  // Turn relay ON (active low for irrigation system)
    Serial.println("Relay ON (Watering the soil - Soil is dry!)");
  } else {
    digitalWrite(relay_pin, LOW);   // Turn relay OFF
    Serial.println("Relay OFF (Soil is wet!)");
  }

  delay(1000);  // Wait for 1 second before the next reading
}
