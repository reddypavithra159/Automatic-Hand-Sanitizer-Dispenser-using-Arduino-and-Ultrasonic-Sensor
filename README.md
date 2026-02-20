# Automatic Hand Sanitizer Dispenser using Arduino and Ultrasonic Sensor

Automatic hand sanitizer dispenser developed using Arduino, an ultrasonic sensor (HC-SR04), and a servo motor. The system detects the presence of a hand and automatically actuates the sanitizer lever without physical contact.

## Project Description

This system uses an ultrasonic sensor to measure the distance between the sensor and a user’s hand. When the measured distance is less than 10 cm, the Arduino activates a servo motor that presses the sanitizer lever. After dispensing for a fixed duration, the servo returns to its original position. A delay is added to prevent repeated triggering.

## Working Principle

The ultrasonic sensor generates a pulse and measures the echo return time. The distance is calculated using:

Distance (cm) = (Duration × 0.034) / 2

where:
Duration = Echo pulse time in microseconds
0.034 cm/µs = Speed of sound
Division by 2 accounts for forward and return travel of the wave

If the calculated distance is less than 10 cm, the servo rotates to 90 degrees to press the sanitizer lever and then returns to 0 degrees.

## Pin Configuration

| Component       | Arduino Pin |
| --------------- | ----------- |
| Ultrasonic TRIG | 9           |
| Ultrasonic ECHO | 8           |
| Servo Motor     | 7           |

## Arduino Code

```cpp
#include <Servo.h>

#define TRIG 9
#define ECHO 8
#define SERVO_PIN 7

Servo sanitizerServo;

void setup() {
  pinMode(TRIG, OUTPUT);
  pinMode(ECHO, INPUT);
  sanitizerServo.attach(SERVO_PIN);
  sanitizerServo.write(0); // Initial position
  Serial.begin(9600);
}

void loop() {
  long duration;
  int distance;

  // Trigger ultrasonic pulse
  digitalWrite(TRIG, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG, LOW);

  // Calculate distance
  duration = pulseIn(ECHO, HIGH);
  distance = duration * 0.034 / 2; // cm

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // Check if hand is near (< 10 cm)
  if (distance < 10) {
    sanitizerServo.write(90);  // Press sanitizer lever
    delay(1500);               // Hold for 1.5 sec
    sanitizerServo.write(0);   // Return back
    delay(2000);               // Avoid multiple triggers
  }

  delay(200);
}
```

## Features
* Touch-free operation
* Automatic sanitizer dispensing
* Low-cost hardware implementation
* Simple embedded control logic
* Suitable for public environments

## Applications
* Hospitals
* Educational institutions
* Offices
* Public hygiene stations

## Future Scope
* Integration of IR sensor for improved detection
* LCD display for usage count
* IoT-based refill monitoring
* Rechargeable battery system

## Author
Pavithra Ravula
B.Tech – Electronics and Instrumentation Engineering


