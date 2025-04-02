# Overview of C++ for Arduino

C++ is the primary programming language used for **Arduino development**, with a simplified framework designed for embedded systems. Arduino uses a **subset of C++** with additional built-in functions and libraries to facilitate hardware control.

## Arduino Programming Basics

Arduino programs are written in **sketches** (files with a `.ino` extension) using a simplified C++ syntax. Each sketch consists of:

- **`setup()` function** – Runs once at startup for initialization.
- **`loop()` function** – Runs repeatedly for continuous execution.

Example:

```cpp
void setup() {
    pinMode(LED_BUILTIN, OUTPUT); // Set LED pin as output
}

void loop() {
    digitalWrite(LED_BUILTIN, HIGH); // Turn LED on
    delay(1000);                     // Wait for 1 second
    digitalWrite(LED_BUILTIN, LOW);  // Turn LED off
    delay(1000);                     // Wait for 1 second
}
```

## Key Features of C++ for Arduino

- **Simplified I/O Handling**: Functions like `pinMode()`, `digitalWrite()`, and `analogRead()` abstract low-level operations.
- **Preprocessor Directives**: Uses `#include` for libraries and `#define` for constants.
- **Classes and Objects**: Supports object-oriented programming (OOP).
- **Interrupts**: Allows handling of external hardware events.
- **Memory Management**: No dynamic memory allocation by default to avoid fragmentation.

## Differences from Standard C+

| Feature           | Standard C++                | Arduino C++                     |
| ----------------- | --------------------------- | ------------------------------- |
| Standard Library  | Uses STL (vectors, lists)   | No STL, limited use of `String` |
| Dynamic Memory    | `new`, `delete`, `malloc()` | Avoided due to limited RAM      |
| File Handling     | Uses `<fstream>`            | Not supported                   |
| Multithreading    | `std::thread`               | No direct support               |
| Real-time Control | Not designed for real-time  | Optimized for real-time         |

## Using Classes in Arduino

Although Arduino sketches don’t require explicit OOP, you can create custom classes.

Example:

```cpp
class LED {
  private:
    int pin;

  public:
    LED(int p) { pin = p; pinMode(pin, OUTPUT); }
    void on() { digitalWrite(pin, HIGH); }
    void off() { digitalWrite(pin, LOW); }
};

LED myLED(13);

void setup() {}

void loop() {
    myLED.on();
    delay(1000);
    myLED.off();
    delay(1000);
}
```

## Libraries and Hardware Interaction

Arduino has a vast ecosystem of libraries, making it easy to integrate hardware components.

Example: Using **Wire.h** for I2C communication:

```cpp
#include <Wire.h>

void setup() {
    Wire.begin(); // Initialize I2C
}

void loop() {
    Wire.beginTransmission(0x68); // Communicate with device at address 0x68
    Wire.write(0x00); // Send a command
    Wire.endTransmission();
    delay(1000);
}
```

## Advanced Features

- **Interrupts**: Respond to external events asynchronously using `attachInterrupt()`.
- **Timers**: Use hardware timers for precise timing.
- **EEPROM**: Store non-volatile data using `EEPROM.h`.
- **Serial Communication**: Communicate with the PC using `Serial.print()`.

Arduino leverages a **simplified version of C++** tailored for embedded systems. It removes unnecessary complexities while retaining object-oriented features, making it a powerful yet accessible tool for electronics and IoT development.
