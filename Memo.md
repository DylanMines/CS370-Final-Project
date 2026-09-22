## The User
Our user is any person who lives in a home, big or small. The place: any entrance of a home. There are many who hear a strange sound in the night, and it gives them a fright. 

## The Problem

## Why Such a Device?

## The Sensors

<!-- DISPLAY https://www.adafruit.com/product/398 -->
<!-- Raspberry Pi 3 https://www.adafruit.com/product/3055 -->
<!-- Matrix Keypad https://www.adafruit.com/product/3845 -->
<!-- Micro SD memory https://www.adafruit.com/product/5249 -->

### Ultrasonic Distance Sensor

> https://www.adafruit.com/product/4007

This will be responsible for detecting presence in front of it. Paired with another, they can cooperate to discern whether an individual entered or left a space (direction).

### Microphone

> https://www.adafruit.com/product/1063

This will be responsible for detecting volume that crosses a certain threshold. Useful for when the UDS is not in line-of-sight of the disturbance.

#### How they pair

Both sensors individually can determine whether a disturbance is present. By leveraging ultrasonic data, we can determine whether volume that crosses the threshold is solely due to an entity within the residence or one that entered.

## The Mechanisms

| Mechanism | Description |
| --------- | ----------- |
|     B     | Keypad used for interrupt-driven input which serves to configure the system with threshold data and for navigation. |
|     D     | Logging history, which includes a UNIX timestamp (year, month, day, hour, minute, second), along with ultrasonic distance and microphone voltage. In addition, a it will store the current volume and distance threshold. |
|     F     | Both the ultrasonic distance sensor and microphone amplifier are real-time and relay data back to the Pi. |

## The Risk

There are two categories of risk for this project, one being implementation and the other being practical. From an implemention side, our final plan involves several sensors, data processing, and output. This is not a simple goal. Orchestrating these factors together is a large determining factor of whether this project will succeed. Similarly, the pratical aspect of the project involves general population interest. Will people at the end of the day, despite all of our convincing that this product does what it's supposed to do, want to use this?
