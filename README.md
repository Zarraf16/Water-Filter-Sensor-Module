# Smart Water Filtration — Sensor Module

**4 sensor channels • Modular Arduino C++ • Color TFT display**

A Schoolcraft Engineering Club collaborative project integrating pump-driven filtration, water-quality sensing, and a display. This fork preserves the original sensor-module code and documents the broader physical prototype.

[Read the engineering report](Smart_Water_Filtration_Engineering_Report.pdf)

## Project at a glance

| Area | Implementation |
|---|---|
| Measurement channels | pH, TDS, temperature, and optical clarity |
| Software structure | 12 source files; four sensor classes, a shared interface, and a display class |
| Display | SPI TFT configured for ST7796S through LCDWIKI |
| Physical prototype | Pump, filter assembly, tubing, fittings, power conversion, and sensor electronics |

## Team collaboration

Schematic design, coding, component selection, assembly, integration, and troubleshooting were shared across **Schoolcraft Engineering Club** members. Responsibilities overlapped throughout development. This portfolio fork presents the team's work and does not claim exclusive ownership or individual authorship of the firmware.

The original sensor-module repository is [Ce3Pi0/Water-Filter-Sensor-Module](https://github.com/Ce3Pi0/Water-Filter-Sensor-Module). Its commit history is preserved. Project report prepared by **Zarraf Alam Fahmid**.

## What the code does

The main loop reads temperature, estimates TDS using a 30-sample median and temperature compensation, reads optical clarity and pH, updates four display rows, and waits one second.

| Module | Role |
|---|---|
| [Water-Filter-Sensor-Module.ino](Water-Filter-Sensor-Module.ino) | Initialization, acquisition, and display updates |
| [Sensor.h](Sensor.h) | Shared sensor initialization interface |
| PhSensor | Analog pH acquisition and DFRobot conversion |
| TemperatureSensor | OneWire / DallasTemperature interface |
| TDSSensor | Sampling, median filtering, and compensation |
| TurbiditySensor | Sensor-voltage normalization |
| SensorModuleLCDScreen | TFT initialization, formatting, and range checks |

**Firmware scope:** this source contains the sensing and display module. Pump automation, flow/level control, and communication between two controllers are not implemented in this snapshot.

## Hardware connections in the sketch

| Signal | Pin |
|---|---|
| pH | A0 |
| TDS | A1 |
| Turbidity | A2 |
| Temperature data | D3 |
| TFT CS / reset / register select | D10 / D8 / D9 |
| TFT SDI / SDO / clock | D11 / D12 / D13 |
| TFT backlight/LED | D5 |

The project drawings include earlier display and control concepts. Confirm the actual module voltages and pinout against the chosen hardware revision.

## Build preparation

1. Open `Water-Filter-Sensor-Module.ino` in Arduino IDE, keeping all `.h` and `.cpp` files together in the sketch folder.
2. Install **LCDWIKI_GUI**, **LCDWIKI_SPI**, **DFRobot_PH**, **OneWire**, and **DallasTemperature**.
3. Select the board and port matching the prototype.
4. Review the archived-code issues below, confirm wiring, and calibrate the sensors before interpreting measurements.

Dependency versions are not pinned. This documentation update does not claim a successful compilation or a new hardware test.

## Engineering challenges and outcome

The team compared filter options, tested different pumps and motors, and resolved incompatible fittings and tubing sizes. Packaging the plumbing and electronics and combining the sensor and display code required repeated adjustment.

The assembled prototype operated and the team reported that it filtered water, but flow was slower than expected. No validated flow-rate or removal-efficiency measurements are reported.

**Reading the display:** the clarity percentage normalizes sensor voltage against a 4.21 V reference. It is not turbidity in NTU, a contaminant-removal percentage, or evidence that water is safe to drink.

## Archived-code review

The original firmware is retained unchanged. The report identifies items for a future revision:

- Keep the OneWire instance alive beyond `TemperatureSensor::begin()`.
- Reconcile TDS sampling with the one-second main-loop delay and handle buffer warm-up.
- Correct the missing return in `readTdsValue()` and the display's pointer-based length check.
- Make sensor initialization and measurement-validity handling consistent.

See the [full report](Smart_Water_Filtration_Engineering_Report.pdf) for photographs, circuit records, component information, limitations, and all 12 source listings.
