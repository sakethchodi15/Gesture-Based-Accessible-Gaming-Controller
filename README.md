# Gesture-Based-Accessible-Gaming-Controller

The GestureController project implements a Bluetooth Low Cost (BLE) keyboard interface using an ESP32, an Adafruit MMA8451 accelerometer, and a flex sensor.  
The system interprets arm orientation and finger flexion as discrete keyboard inputs, enabling hands‑free or gesture‑based control for accessibility, human and computer interaction research, and experimental input devices.



1. Overview
The firmware establishes a BLE keyboard profile and continuously monitors accelerometer and flex‑sensor data.  
Based on calibrated baseline values and predefined thresholds, the device issues keyboard press and release events corresponding to directional gestures and flex‑based activation.

A fallback **mock mode** is automatically enabled if the MMA8451 sensor is not detected, allowing BLE functionality to be tested without hardware.



2. Features
- BLE keyboard emulation using the ESP32  
- Real‑time gesture detection via the MMA8451 accelerometer  
- Automatic baseline calibration on startup  
- Flex sensor integration for discrete activation (spacebar)  
- Serial output for debugging and threshold tuning  
- Mock mode for development without hardware  



3. Hardware Requirements
- ESP32 development board  
- Adafruit MMA8451 accelerometer module  
- Flex sensor (analog)  
- 10 kΩ resistor (for voltage divider)  
- Breadboard and jumper wires  



4. Wiring Specifications

MMA8451 → ESP32
- VIN → 3.3 V  
- GND → GND  
- SDA → GPIO 21  
- SCL → GPIO 22  

Flex Sensor → ESP32
- One terminal → 3.3 V  
- Other terminal → GPIO 12  
- 10 kΩ resistor from GPIO 12 → GND  



5. Gesture‑to‑Key Mapping

Gesture | Condition | Keyboard Output 

Right tilt: X > 3.5 = “D”
Left tilt: X < –3.5 = “A”
Arm raised: Z > 5.0 = “W”|
Arm lowered: Z < –5.0 = “S”
Strong flex: FlexValue == 3500 = Space

Thresholds may be adjusted depending on sensor placement and user movement characteristics.



6. System Behavior

6.1 Initialization
- BLE keyboard service is started.  
- The MMA8451 is initialized and configured for ±4 g range.  
- If detected, the system collects 20 samples to compute baseline X and Z acceleration values.  
- If not detected, the system enters **mock mode**, using simulated gesture values.

6.2 Runtime Loop
Each cycle performs the following operations:

1. Read accelerometer values (or simulated values in mock mode).  
2. Compute ΔX and ΔZ relative to baseline.  
3. Read flex sensor value.  
4. Issue keyboard press or release events based on threshold logic.  
5. Output diagnostic data over Serial.  



7. Software Dependencies
- Adafruit_MMA8451
- Adafruit_Sensor
- BleKeyboard
- Arduino IDE

These libraries must be installed via the Arduino Library Manager or manually added to the project.



8. Serial Debug Output
The firmware prints real‑time sensor deltas and flex readings, for example:


X: 0.42 | Z: -6.03 | Flex: 3480


This output assists with calibration, tuning, and verifying gesture detection.



9. Notes and Recommendations
- Ensure the ESP32 variant supports BLE functionality.  
- Flex sensor readings vary significantly by model; threshold adjustments may be required.  
- Gesture thresholds should be tuned based on how the device is worn and the user’s natural movement range.  
- For stable calibration, the user should hold their arm in a neutral position during startup.
