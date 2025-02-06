# Solar Tracker Project Using Arduino UNO

A dual-axis solar tracker designed to maximize solar energy harvesting by dynamically adjusting the position of a solar panel to follow the sun's movement. Built with Arduino UNO, light-dependent resistors (LDRs), and servo motors.

## Table of Contents
- [Project Overview](#project-overview)
- [Features](#features)
- [Components Used](#components-used)
- [Installation & Setup](#installation--setup)
- [How It Works](#how-it-works)
- [Code Explanation](#code-explanation)
- [Contributing](#contributing)
- [Acknowledgments](#acknowledgments)

---

## Project Overview
This project automates solar panels to maintain optimal alignment with the sun, enhancing energy efficiency by up to 40% compared to fixed panels. The system uses **4 LDR sensors** to detect light intensity and **2 servo motors** (horizontal and vertical) to adjust the panel's position. The Arduino UNO processes sensor data in real time to control the servos.

---

## Features
- **Dual-Axis Tracking**: Adjusts both azimuth (horizontal) and elevation (vertical) angles.
- **Real-Time Light Sensing**: Utilizes LDRs to measure sunlight intensity.
- **Energy Efficiency**: Maximizes solar panel output by maintaining direct sunlight exposure.
- **Open-Source**: Fully customizable code and hardware design.
- **Low-Cost Design**: Built with affordable, widely available components.

---

## Components Used
1. **Arduino UNO**: Microcontroller for processing sensor data and controlling servos.
2. **Servo Motors (x2)**: SG90 motors for horizontal (180° rotation) and vertical (180° rotation) movement.
3. **LDR Sensors (x4)**: Light-dependent resistors to detect sunlight direction.
4. **10kΩ Resistors (x4)**: Used in voltage divider circuits with LDRs.
5. **Solar Panel**: Small-scale panel (optional for demonstration).
6. **Breadboard & Jumper Wires**: For circuit prototyping.
7. **9V Battery/Power Supply**: Powers the Arduino and servos.

---

## Installation & Setup

### Prerequisites
- Arduino IDE installed ([Download here](https://www.arduino.cc/en/software)).
- Basic knowledge of Arduino programming and electronics.

### Steps
1. **Clone the Repository**:
   bash
   git clone https://github.com/Abram-Emad/Solar-Tracker-Project-Using-Arduino-UNO.git
   
2. **Hardware Setup**:
   - Connect LDRs in voltage divider configuration with 10kΩ resistors (see [Circuit Diagram](#circuit-diagram)).
   - Attach servos to the solar panel mount.
3. **Upload the Code**:
   - Open `SolarTracker.ino` in Arduino IDE.
   - Install the `Servo` library (if not already installed).
   - Connect the Arduino UNO and upload the sketch.
4. **Calibration**:
   - Adjust LDR positions for accurate light detection.
   - Modify servo angle thresholds in the code if needed.

---

## How It Works
1. **Light Sensing**: Four LDRs (placed in cardinal directions: top-left, top-right, bottom-left, bottom-right) measure ambient light.
2. **Data Processing**: The Arduino reads analog voltages from the LDRs and calculates differences to determine the sun's position.
3. **Servo Control**:
   - **Horizontal Movement**: The horizontal servo adjusts based on left/right LDR differences.
   - **Vertical Movement**: The vertical servo responds to top/bottom LDR differences.
4. **Continuous Adjustment**: The system updates every 500ms for real-time tracking.

---

## Code Explanation
Key parts of the Arduino sketch (`SolarTracker.ino`):
cpp
#include <Servo.h>
Servo horizontalServo, verticalServo;

// LDR Pins
const int ldrTopLeft = A0, ldrTopRight = A1, ldrBottomLeft = A2, ldrBottomRight = A3;

void setup() {
  horizontalServo.attach(9);
  verticalServo.attach(10);
}

void loop() {
  int tl = analogRead(ldrTopLeft);   // Top-left LDR
  int tr = analogRead(ldrTopRight);  // Top-right LDR
  int bl = analogRead(ldrBottomLeft); // Bottom-left LDR
  int br = analogRead(ldrBottomRight); // Bottom-right LDR

  // Calculate average light intensities
  int horizontalDiff = (tl + bl) - (tr + br); // Horizontal balance
  int verticalDiff = (tl + tr) - (bl + br);   // Vertical balance

  // Adjust servos based on differences
  if (abs(horizontalDiff) > 10) { // Threshold to prevent jitter
    horizontalServo.write(horizontalServo.read() + (horizontalDiff > 0 ? -1 : 1));
  }
  if (abs(verticalDiff) > 10) {
    verticalServo.write(verticalServo.read() + (verticalDiff > 0 ? -1 : 1));
  }

  delay(500); // Update every 0.5 seconds
}


---

## Contributing
Contributions are welcome! 
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/improvement`).
3. Commit changes (`git commit -m 'Add new feature'`).
4. Push to the branch (`git push origin feature/improvement`).
5. Open a Pull Request.

---

## Acknowledgments
- Developed by **[Abram Emad](https://github.com/Abram-Emad)**.
- Inspired by open-source solar tracking projects.
- Thanks to the Arduino community for continuous support.
