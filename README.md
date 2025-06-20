

# 🚨 Motion-Activated Security Alert System

A simple embedded project using **Arduino Uno** and **ESP32**, designed to trigger an alert when motion is detected. Ideal for learning basic sensor integration, alarm systems, and GPIO control.

---

## 🔧 Features

* **Microcontrollers**: Arduino Uno & ESP32
* **Sensors & Actuators**:

  * HC-SR501 PIR Motion Sensor
  * HC-SR04 Ultrasonic Distance Sensor
  * Buzzer (for audible alert)
  * Red LED (for visual alert)
* **Core Skills**:

  * GPIO Input/Output
  * Sensor integration
  * Alarm logic
  * Delay and reset logic
  * (Bonus) IR Remote to enable/disable system

---

## 📋 Description

When motion or proximity is detected (closer than 24 cm), the **buzzer sounds** and the **LED flashes**. The system then resets after a short delay, waiting for the next detection.

> 💡 *Bonus Idea*: Extend functionality using an **IR remote** to toggle the system ON/OFF (e.g., for arming/disarming the alarm).

---

## 🧩 Arduino Uno Implementation

### 🧭 Circuit Diagram

> [View the interactive Wokwi simulation](https://wokwi.com/projects/434304672270491649)
>
> <img width="650" alt="Wokwi Simulation Screenshot" src="https://github.com/user-attachments/assets/b4887193-775f-4bdb-9eda-69f995e289a2" />

---

### 📸 Real Hardware Setup

![Real hardware setup](https://github.com/user-attachments/assets/833fa088-cad5-470f-aed1-62f327abf71a)

---

### 🧠 Arduino Code

```cpp
const int buzzerPin = 12;
const int echoPin = 10;
const int trigPin = 9;      
const int ledPin = 11;

long duration;
int distance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(ledPin, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // Trigger ultrasonic sensor
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Measure distance
  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  // Display distance
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // Trigger alarm if object is close
  if (distance < 24) {
    tone(buzzerPin, 1000);
    digitalWrite(ledPin, HIGH);
    delay(100);
    digitalWrite(ledPin, LOW);
    noTone(buzzerPin);
    delay(100);
  } else {
    digitalWrite(ledPin, LOW);
    noTone(buzzerPin);
  }

  delay(100); // Small delay for stability
}
```

---

### 💬 Code Explanation

* **Ultrasonic Sensor (HC-SR04)**:

  * Sends out a sound wave using `trigPin`.
  * Waits for the echo on `echoPin`.
  * Calculates distance based on time delay.

* **LED + Buzzer**:

  * If an object is within **24 cm**, the LED flashes and buzzer sounds.
  * System resets every cycle to re-check the distance.

---

## 🧩 TODO / Extensions

* [ ] Add IR remote to enable/disable the system.
* [ ] Replace distance sensor with PIR motion sensor (HC-SR501).
* [ ] Upload version for ESP32.
* [ ] Add notification or logging feature (e.g., via Wi-Fi with ESP32).

---


