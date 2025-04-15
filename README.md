# Smart Helmet for Industrial Safety

## Project Overview

The Smart Helmet for Industrial Safety project introduces an innovative approach to enhancing worker safety in hazardous industrial environments. Unlike traditional Personal Protective Equipment (PPE), which primarily provides passive physical protection, this project integrates real-time monitoring and proactive alert systems to tackle dynamic workplace risks. The helmet is equipped with sensors that continuously monitor environmental factors such as temperature and gas levels, providing critical data that allows supervisors to take timely action to prevent accidents.

### Objective

The goal of this project is to create a **smart helmet system** that:
1. Monitors the worker's physiological state (temperature).
2. Monitors hazardous environmental conditions (e.g., carbon monoxide, hydrogen sulfide, and flammable gases).
3. Sends **SMS alerts** to supervisors or emergency responders when danger thresholds are breached.

By leveraging a **LPC2148 microcontroller**, **analog sensors**, and **wireless communication** via **GSM**, the system aims to improve safety protocols in industries with high exposure to dangerous conditions, such as mining, manufacturing, construction, and oil and gas.

## Key Features

- **Continuous Monitoring**: Measures the worker's body temperature and environmental gas levels, including carbon monoxide (CO), hydrogen sulfide (H2S), and flammable gases.
- **Real-time Alerts**: When hazardous conditions exceed predefined safety thresholds, the system immediately triggers SMS notifications via a GSM module.
- **Embedded System Design**: The helmet uses the **LPC2148 microcontroller** with built-in Analog-to-Digital Converter (ADC) channels, optimizing data processing.
- **Low-Cost Solution**: The use of affordable components makes the system accessible and scalable for deployment in various industrial settings.
- **Power Efficiency**: The system is designed with power-saving mechanisms to ensure long operational life on a single charge.

## Components

### 1. **Microcontroller**

- **LPC2148 Microcontroller (ARM7TDMI-S)**: 
  - A powerful 32-bit microcontroller that integrates an ARM7 core with built-in **ADC** and **UART** peripherals.
  - Ideal for real-time data processing and communication tasks.

### 2. **Sensors**

- **Temperature Sensor (LM35 or TMP36)**:
  - **LM35**: Analog output (10 mV/°C), measures temperature.
  - **TMP36**: Analog output with a 500mV offset, also measures temperature but with higher accuracy.
  - Both sensors provide data that is processed via the microcontroller's ADC.

- **Gas Sensors**:
  - **MQ-7**: Measures **Carbon Monoxide (CO)** in the range of 10-1000 ppm.
  - **SPEC ULPSM-H2S**: Measures **Hydrogen Sulfide (H2S)** in the range of 0-50 ppm.
  - **MQ-2**: Measures **Flammable gases** (e.g., methane, propane) in the range of 200-10000 ppm.
  - These sensors provide analog signals that are digitized by the microcontroller’s ADC channels.

### 3. **GSM Module**

- **SIM800L GSM Module**:
  - Used for sending SMS alerts when sensor thresholds are exceeded.
  - Communicates with the microcontroller via UART.
  - Powered by a 3.7V to 4.2V supply to ensure proper transmission of messages.

### 4. **Power Supply and Accessories**

- **Power Supply**: 
  - The system is powered using a **5V power source** for the microcontroller and sensors.
  - A **3.7V to 4.2V** lithium-ion battery powers the GSM module.
- **Voltage Regulators**: Ensure stable voltage for the components.
- **Rugged Enclosures**: Designed to protect sensitive electronics from harsh industrial environments.

### 5. **Additional Components**

- **Resistors, Capacitors, Diodes, and Connectors**: For ensuring proper functioning of the circuit, voltage regulation, and protection of the components.

### Software

1. **Keil μVision 4**:
   - The Integrated Development Environment (IDE) used for writing and compiling the embedded C code that runs on the LPC2148 microcontroller.
   - It supports debugging, simulation, and provides useful tools to streamline embedded development.

2. **Proteus 8 Professional**:
   - Used for designing and simulating the circuit before physical implementation.
   - Validates the interaction between components such as the microcontroller, sensors, and GSM module.

## Working Procedure

The system continuously monitors the worker's environment and physiological conditions. Here’s a breakdown of how the system works:

### 1. **System Initialization**

- **Power-Up**: Upon powering the system, the LPC2148 initializes all peripherals and sets up the necessary communication interfaces (ADC, UART).
  - **ADC Configuration**: The system configures the microcontroller’s ADC channels to read temperature and gas sensor outputs.
  - **UART Communication**: The microcontroller sets up UART for communication with the GSM module for sending SMS alerts.
  - **GPIO Pins**: Configure pins to manage sensor functions such as heater cycles for gas sensors.

### 2. **Sensor Data Acquisition**

- **Temperature Monitoring**: The system acquires temperature data from the LM35 or TMP36 sensor connected to the ADC. The raw ADC data is converted into temperature values using specific formulas.
- **Gas Monitoring**: The gas sensors (MQ-7, MQ-2, and SPEC ULPSM-H2S) provide analog outputs corresponding to the concentration of gases in the environment. These outputs are also digitized by the ADC.

### 3. **Threshold Evaluation**

- **Temperature Check**: The system checks if the worker’s body temperature exceeds predefined thresholds (e.g., 38°C for high temperature or 15°C for low temperature). If the temperature is outside the normal range, the system triggers an SMS alert.
- **Gas Check**: The gas sensor outputs are compared to predefined thresholds (e.g., 100 ppm for CO, 50 ppm for H2S, 1000 ppm for flammable gases). If any gas level exceeds the threshold, the system sends an SMS alert.

### 4. **Sending SMS Alerts**

- If any of the sensor readings exceed the safe threshold, the system formats a message (e.g., “High Temperature Alert: 40°C”) and sends it to a preconfigured phone number via the GSM module.
- The system uses AT commands to communicate with the SIM800L GSM module and transmit the SMS:
  - `AT+CMGF=1` sets the message mode to text.
  - `AT+CMGS="phone_number"` specifies the recipient.
  - The message text is followed by the `Ctrl+Z` (ASCII 26) character to send the message.

### 5. **Continuous Monitoring**

- The system enters a loop, continuously sampling the sensors and checking for threshold breaches.
- **Power Management**: To conserve energy, the microcontroller and sensors enter sleep modes when idle, and the GSM module uses low-power modes when not in use.

## Test Cases

### 1. **Temperature (°C) and ADC Input Range**

| Temp (°C) | ADC Input (Temp) | CO (ppm) | CO ADC Input | H2S (ppm) | H2S ADC Input | Flammable Gas (ppm) | Flammable Gas ADC Input |
|-----------|------------------|----------|--------------|-----------|---------------|---------------------|------------------------|
| 24.8      | ~77              | ~29      | 20           | ~19       | 124           | ~9.8                | 50                     |
| 39.9      | ~43              | ~39      | 30           | ~20       | 93            | ~14.6               | 80                     |
| 13.8      | ~93              | ~19      | 15           | ~7.3      | 40            | ~7.3                | 40                     |
| 49.9      | ~155             | ~97      | 100          | ~24.4     | 150           | ~24.4               | 150                    |
| 10.0      | ~31              | ~4.9     | 60           | -48       | 310           | ~10                 | 200                    |
| -99.9     | ~310             | ~999     | 1023         | -999      | 512           | ~999                | 1000                   |
| 24.8      | ~49              | ~39.9    | 512          | ~146      | 180           | ~24.8               | 180                    |

### 2. **Conditions**

- **Normal Conditions**: All sensors are within normal operational ranges.
- **High Temperature Only**: The temperature exceeds 38°C.
- **Low Temperature Only**: The temperature falls below 15°C.
- **High CO Only**: The carbon monoxide (CO) concentration exceeds 50 ppm.
- **High Temp + High CO**: Both temperature and CO exceed safe levels.
- **Low Temp + High CO**: Low temperature with high CO.
- **Extreme Conditions**: High temperature, high CO, high H2S, and high flammable gas.

## Installation

### Hardware Setup

1. **Connect the LPC2148 Microcontroller** to the temperature and gas sensors using the ADC pins (e.g., AD0.1, AD0.2, AD0.3, AD0.4).
2. **Wire the GSM Module (SIM800L)** to the UART pins of the LPC2148 for communication (TXD, RXD).
3. **Power Supply Setup**: Ensure proper voltage regulation to power the microcontroller (5V) and the GSM module (3.7V-4.2V).
4. **Test Circuit**: Verify the entire system in a simulation environment (e.g., **Proteus 8 Professional**) to ensure functionality.

### Software Setup

1. **Code Development**: Write and compile the embedded C code in **Keil μVision 4**.
2. **Upload to LPC2148**: Use a programmer to load the compiled code onto the LPC2148 microcontroller.
3. **Simulation**: Before deployment, simulate the system in **Proteus 8** to ensure the circuit and logic work as expected.

## Use Case

This smart helmet is ideal for workers in hazardous environments such as:
- **Mining**: Monitoring for dangerous gases like methane or carbon monoxide.
- **Manufacturing**: Detecting high temperatures and hazardous fumes.
- **Construction**: Alerting workers to sudden environmental changes that could indicate danger.

## Future Work

1. **IoT Integration**: Extend the system to integrate with cloud platforms for real-time monitoring and remote alerts.
2. **Wearable Integration**: Integrate with other wearable devices to track worker fatigue and physical strain.
3. **Predictive Analytics**: Use machine learning algorithms to predict potential safety incidents before they occur.

## Conclusion

The Smart Helmet for Industrial Safety is a significant advancement in industrial safety. It transitions from reactive to proactive safety management by leveraging real-time sensor data and wireless communication. By improving early hazard detection and enabling immediate intervention, this system has the potential to save lives and reduce industrial accidents.

## Contact

For more information, please contact ezhilkirthikm@gmail.com.
