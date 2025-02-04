
## Setup and Loop Functions

Every Arduino program (called a sketch) requires two essential functions:

- `void setup()`: Runs once at startup or reset. Used for initializing settings like configuring pin modes or starting serial communication.
- `void loop()`: Runs continuously after `setup()` finishes, forming the core of the program's execution.

```cpp
void setup() {
    pinMode(LED_BUILTIN, OUTPUT); // Set built-in LED pin as output
}

void loop() {
    digitalWrite(LED_BUILTIN, HIGH); // Turn on LED
    delay(1000); // Wait 1 second
    digitalWrite(LED_BUILTIN, LOW);  // Turn off LED
    delay(1000); // Wait 1 second
}
```

The `setup()` function is where you prepare the environment, and the `loop()` function is where you define behaviors that need to repeat indefinitely.

## Digital I/O

- `pinMode(pin, mode)`: Configures a pin as `INPUT`, `OUTPUT`, or `INPUT_PULLUP`. The `INPUT_PULLUP` mode uses an internal resistor to simplify hardware setups.
- `digitalWrite(pin, value)`: Sets the pin state to `HIGH` or `LOW`.
- `digitalRead(pin)`: Reads the state of a pin configured as `INPUT`, returning `HIGH` or `LOW`.

**Example: Button Control**
```cpp
const int buttonPin = 2;
const int ledPin = 13;

void setup() {
    pinMode(buttonPin, INPUT_PULLUP); // Enable internal pull-up resistor
    pinMode(ledPin, OUTPUT);
}

void loop() {
    int buttonState = digitalRead(buttonPin);
    if (buttonState == LOW) {
        digitalWrite(ledPin, HIGH); // Turn on LED
    } else {
        digitalWrite(ledPin, LOW); // Turn off LED
    }
}
```

In this example, a pull-up resistor simplifies the circuit by ensuring a default `HIGH` state for the button.

## Analog I/O

- `analogRead(pin)`: Reads a value between 0 and 1023 from an analog input pin. This is useful for sensors like potentiometers and temperature sensors.
- `analogWrite(pin, value)`: Outputs a PWM signal ranging from 0 to 255 to simulate an analog output.

**Example: Light Sensor**
```cpp
const int sensorPin = A0;
const int ledPin = 9;

void setup() {
    pinMode(ledPin, OUTPUT);
}

void loop() {
    int sensorValue = analogRead(sensorPin);
    int brightness = map(sensorValue, 0, 1023, 0, 255);
    analogWrite(ledPin, brightness);
}
```

This demonstrates how to adjust LED brightness based on sensor readings, translating analog input to output.

## Timing and Delays

- `delay(ms)`: Pauses the program for the specified number of milliseconds.
- `millis()`: Returns the number of milliseconds since the program started, useful for non-blocking timing.
- `micros()`: Returns the number of microseconds since the program started, ideal for precise timing tasks.

**Example: Non-blocking Delay**
```cpp
unsigned long previousMillis = 0;
const long interval = 1000;

void setup() {
    pinMode(LED_BUILTIN, OUTPUT);
}

void loop() {
    unsigned long currentMillis = millis();
    if (currentMillis - previousMillis >= interval) {
        previousMillis = currentMillis;
        digitalWrite(LED_BUILTIN, !digitalRead(LED_BUILTIN));
    }
}
```

This approach avoids the blocking behavior of `delay()` and allows concurrent tasks.

## Serial Communication

- `Serial.begin(baudRate)`: Initializes communication at the specified baud rate.
- `Serial.print(value)`: Sends data to the serial monitor without a newline.
- `Serial.println(value)`: Sends data with a newline.
- `Serial.read()`: Reads incoming data, returning `-1` if no data is available.
- `Serial.available()`: Returns the number of bytes available for reading.

**Example: Echo Input**
```cpp
void setup() {
    Serial.begin(9600);
}

void loop() {
    if (Serial.available() > 0) {
        char input = Serial.read();
        Serial.print("You typed: ");
        Serial.println(input);
    }
}
```

This simple program echoes user input from the serial monitor, useful for debugging.

## External Libraries

Libraries extend functionality, allowing easier control of hardware components or performing complex tasks.

- Install libraries through **Sketch > Include Library > Manage Libraries**.
- Include them in your program using `#include <LibraryName.h>`.

**Example: Using an LCD**
```cpp
#include <LiquidCrystal.h>

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

void setup() {
    lcd.begin(16, 2); // Initialize the LCD with 16 columns and 2 rows
    lcd.print("Hello, World!");
}

void loop() {}
```

This example demonstrates the use of an LCD to display messages, a common requirement in projects.

## Interrupts

Interrupts provide a way to handle asynchronous events.

- `attachInterrupt(digitalPinToInterrupt(pin), ISR, mode)`: Attaches an interrupt service routine (ISR) to a pin.
- `detachInterrupt(digitalPinToInterrupt(pin))`: Removes the interrupt from the pin.

**Modes:**
- `LOW`, `CHANGE`, `RISING`, `FALLING` determine when the ISR is triggered.

**Example: Button Interrupt**
```cpp
volatile bool ledState = false;

void toggleLED() {
    ledState = !ledState;
}

void setup() {
    pinMode(LED_BUILTIN, OUTPUT);
    attachInterrupt(digitalPinToInterrupt(2), toggleLED, FALLING);
}

void loop() {
    digitalWrite(LED_BUILTIN, ledState);
}
```

This approach is highly responsive and suitable for real-time applications.

---

## EEPROM

EEPROM provides non-volatile storage for saving data that persists across resets or power cycles.

- `EEPROM.read(address)`: Reads a byte of data from the specified address.
- `EEPROM.write(address, value)`: Writes a byte of data to the specified address.

**Example: Store and Retrieve Data**
```cpp
#include <EEPROM.h>

void setup() {
    Serial.begin(9600);
    EEPROM.write(0, 42);
    int value = EEPROM.read(0);
    Serial.print("Value: ");
    Serial.println(value);
}

void loop() {}
```

This demonstrates how to use EEPROM to store critical data.

---

## **Common Sensors and Actuators**

### Ultrasonic Sensor

Ultrasonic sensors measure distance using sound waves.

```cpp
const int trigPin = 9;
const int echoPin = 10;

void setup() {
    pinMode(trigPin, OUTPUT);
    pinMode(echoPin, INPUT);
    Serial.begin(9600);
}

void loop() {
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);

    long duration = pulseIn(echoPin, HIGH);
    int distance = duration * 0.034 / 2;
    Serial.println(distance);
    delay(500);
}
```

This code calculates the distance to an object based on sound wave reflections.