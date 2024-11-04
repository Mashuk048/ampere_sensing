# ESP8266 MQTT Energy Monitor

## Overview

This project implements an energy monitoring solution using the ESP8266 microcontroller. It connects to a WiFi network and communicates with an MQTT broker to publish energy consumption data and RPM readings. The device can be configured remotely via MQTT commands and supports real-time monitoring of current and RPM values.

## Features

- Monitors current and RPM using an energy sensor.
- Publishes data to specified MQTT topics.
- Subscribes to configuration and command topics for remote control.
- Implements a watchdog timer for stability.
- Provides WiFi configuration via a captive portal using the WiFiManager library.

## Hardware Requirements

- ESP8266 module (e.g., NodeMCU, Wemos D1 Mini)
- Current sensor (compatible with the EmonLib library)
- RPM sensor (e.g., Hall effect sensor)
- Access to a WiFi network
- MQTT broker (e.g., Mosquitto, CloudMQTT)

## Software Requirements

- Arduino IDE with ESP8266 board support
- Required libraries:
  - `ESP8266WiFi`
  - `WiFiManager`
  - `PubSubClient`
  - `ArduinoJson`
  - `EmonLib`

## Setup Instructions

1. **Install Arduino IDE**: Download and install the Arduino IDE if you haven't done so already.

2. **Install ESP8266 Board Support**:
   - Open the Arduino IDE.
   - Go to `File` -> `Preferences`.
   - Add the following URL to the `Additional Board Manager URLs`:
     ```
     http://arduino.esp8266.com/stable/package_esp8266com_index.json
     ```
   - Go to `Tools` -> `Board` -> `Board Manager`, search for `ESP8266`, and install the package.

3. **Install Required Libraries**:
   - Use the Library Manager (`Sketch` -> `Include Library` -> `Manage Libraries`) to install the following libraries:
     - `WiFiManager`
     - `PubSubClient`
     - `ArduinoJson`
     - `EmonLib`

4. **Upload the Code**:
   - Connect your ESP8266 to your computer.
   - Select the appropriate board and port in the Arduino IDE.
   - Copy the provided code into a new Arduino sketch.
   - Update the MQTT credentials and WiFi credentials in the code.
   - Upload the code to the ESP8266.

## Code Explanation

### Key Components

- **MQTT Setup**:
  - Connects to an MQTT broker using the `PubSubClient` library.
  - Publishes messages to specified topics for energy consumption (`autobots/dot/amp`), RPM (`autobots/dot/rpm`), and device responses.

- **WiFi Management**:
  - Connects to a specified WiFi network using the `WiFiManager` library, which can create a captive portal for configuration.

- **Data Publishing**:
  - Publishes current readings (Irms) and RPM values periodically.
  - Supports configuration changes through MQTT messages.

- **Interrupt Handling**:
  - Uses interrupts to monitor RPM sensor readings, ensuring accurate counting of revolutions.

### Main Functions

- `setup()`: Initializes serial communication, configures the energy monitor, sets up pins, attaches interrupts, and connects to the MQTT broker.

- `loop()`: Regularly checks WiFi connection status, MQTT connectivity, and triggers data publishing functions.

- `publishIrms()`: Constructs and publishes the current reading to the MQTT broker.

- `publishRPM()`: Constructs and publishes the RPM count to the MQTT broker.

- `checkWiFi()`: Monitors the WiFi connection and attempts reconnection if necessary.

- `onDemandAP()`: Starts an access point for WiFi configuration if the device cannot connect to the specified network.

## Command and Configuration

### Command Message Format

To send commands to the device, use the following JSON format:

```json
{
  "did": "DOT001",
  "reset": "1",
  "ap": "1"
}


Configuration Change Message Format
To change the device configuration, send a message in the following format:

json
{
  "did": "DOT001",
  "ptime": "30-600",
  "delay": "5-200"
}

Troubleshooting
MQTT Connection Issues: Ensure that the MQTT broker is running and accessible. Check the credentials provided in the code.
WiFi Connection Issues: Verify the WiFi credentials and signal strength. Use the captive portal feature for configuration if needed.
Sensor Issues: Ensure that the current and RPM sensors are connected properly and calibrated.
License
This project is licensed under the MIT License - see the LICENSE file for details.
