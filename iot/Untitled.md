### Detailed Procedure and Code for Each Task

#### **Experiment 1: Display Distance Readings on Serial Monitor**

**Objective:** Use the HC-SR04 ultrasonic sensor with the ESP32 to measure the distance to an object and display it in two units (e.g., centimeters and inches).

**Procedure:**

1. **Connections:**
    - Connect HC-SR04's `Vcc` to ESP32's `5V`.
    - Connect `Trig` pin to an ESP32 GPIO pin (e.g., GPIO5).
    - Connect `Echo` pin to another GPIO pin (e.g., GPIO18).
    - Connect `GND` to the ESP32's ground.
2. **Steps in Code:**
    - Define `Trig` and `Echo` pins.
    - Configure the pins as input/output.
    - Calculate distance using the time difference of the ultrasonic pulse.
    - Display the calculated distance in centimeters and inches on the serial monitor.

**Code:**

```cpp
#define TRIG_PIN 5
#define ECHO_PIN 18
#define SOUND_SPEED 0.034 // Speed of sound in cm/µs

void setup() {
  Serial.begin(115200);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
}

void loop() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  long duration = pulseIn(ECHO_PIN, HIGH);
  float distance_cm = (duration * SOUND_SPEED) / 2;
  float distance_in = distance_cm / 2.54;

  Serial.print("Distance: ");
  Serial.print(distance_cm);
  Serial.print(" cm, ");
  Serial.print(distance_in);
  Serial.println(" in");

  delay(1000);
}
```

---

#### **Experiment 2A: Send Distance Data Over Bluetooth**

**Objective:** Use ESP32's Bluetooth Classic to send sensor data to the Serial Bluetooth Terminal on a phone.

**Procedure:**

1. **Setup Bluetooth:**
    - Use the `BluetoothSerial` library.
    - Name the device during initialization using `SerialBT.begin()`.
2. **Integration with Experiment 1:**
    - Send distance values to the Bluetooth terminal instead of just printing to the serial monitor.

**Code:**

```cpp
#include "BluetoothSerial.h"

BluetoothSerial SerialBT;

void setup() {
  Serial.begin(9600);
  SerialBT.begin("ESP32-Ultrasonic");
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
}

void loop() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  long duration = pulseIn(ECHO_PIN, HIGH);
  float distance_cm = (duration * SOUND_SPEED) / 2;

  SerialBT.print("Distance: ");
  SerialBT.print(distance_cm);
  SerialBT.println(" cm");

  delay(1000);
}
```

---

#### **Experiment 2B: Proximity Notifier with LED Alert**

**Objective:** Notify the user of proximity when the distance is less than a threshold.

**Procedure:**

1. **Add LED:**
    - Connect an LED to a GPIO pin (e.g., GPIO23) via a resistor.
2. **Set Threshold:**
    - Read the threshold value from the Serial Bluetooth Terminal.
3. **Alert:**
    - Turn on the LED and send a notification when the distance is less than the threshold.

**Code:**

```cpp
#define LED_PIN 23

void setup() {
  Serial.begin(115200);
  SerialBT.begin("ESP32-Proximity");
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);
}

void loop() {
  if (SerialBT.available()) {
    String thresholdStr = SerialBT.readString();
    int threshold = thresholdStr.toInt();

    digitalWrite(TRIG_PIN, LOW);
    delayMicroseconds(2);
    digitalWrite(TRIG_PIN, HIGH);
    delayMicroseconds(10);
    digitalWrite(TRIG_PIN, LOW);

    long duration = pulseIn(ECHO_PIN, HIGH);
    float distance_cm = (duration * SOUND_SPEED) / 2;

    if (distance_cm < threshold) {
      digitalWrite(LED_PIN, HIGH);
      SerialBT.println("Proximity Alert!");
    } else {
      digitalWrite(LED_PIN, LOW);
    }

    SerialBT.print("Distance: ");
    SerialBT.print(distance_cm);
    SerialBT.println(" cm");
  }
}
```

---

#### **Experiment 3: Measure Speed of an Object**

**Objective:** Calculate the speed of a moving object upon a specific command.

**Procedure:**

1. **Command Trigger:**
    - Wait for a special character (e.g., `S`) on the Bluetooth terminal.
2. **Speed Measurement:**
    - Use two distance readings with timestamps to calculate speed.
    - Speed=Distance2−Distance1Time\text{Speed} = \frac{\text{Distance2} - \text{Distance1}}{\text{Time}}
3. **Send Result:**
    - Display the calculated speed on the Bluetooth terminal.

**Code:**

```cpp
void loop() {
  if (SerialBT.available()) {
    char command = SerialBT.read();

    if (command == 'S') { // Trigger for speed calculation
      digitalWrite(TRIG_PIN, LOW);
      delayMicroseconds(2);
      digitalWrite(TRIG_PIN, HIGH);
      delayMicroseconds(10);
      digitalWrite(TRIG_PIN, LOW);

      long start_time = millis();
      long duration1 = pulseIn(ECHO_PIN, HIGH);
      float distance1 = (duration1 * SOUND_SPEED) / 2;

      delay(100); // Short delay for second reading

      digitalWrite(TRIG_PIN, LOW);
      delayMicroseconds(2);
      digitalWrite(TRIG_PIN, HIGH);
      delayMicroseconds(10);
      digitalWrite(TRIG_PIN, LOW);

      long duration2 = pulseIn(ECHO_PIN, HIGH);
      float distance2 = (duration2 * SOUND_SPEED) / 2;
      long end_time = millis();

      float speed = (distance2 - distance1) / ((end_time - start_time) / 1000.0);

      SerialBT.print("Speed: ");
      SerialBT.print(speed);
      SerialBT.println(" cm/s");
    }
  }
}
```
