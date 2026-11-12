# 🌿☀️ Solar Panel + Algae Bioreactor Hybrid System

> **Concept / prototype repository.** This project explores co-locating a photovoltaic panel and an algae photobioreactor. The repository contains an ESP32 monitoring/control sketch, wiring and cultivation notes, a planning BOM, research notes, and a static HTML concept page. It does not contain verified measurements of panel-efficiency gain, biomass yield, or CO₂ capture.

---

![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-Prototype%20%2F%20Research-yellow)
![Energy](https://img.shields.io/badge/energy-Hybrid%20Renewable-brightgreen)
![CO2](https://img.shields.io/badge/CO₂-Scrubbing-blue)

---

## 📖 Table of Contents

- [Overview](#overview)
- [How It Works](#how-it-works)
- [System Components](#system-components)
- [Circuit Diagram](#circuit-diagram)
- [Bill of Materials](#bill-of-materials)
- [Setup Instructions](#setup-instructions)
- [Pros & Cons](#pros--cons)
- [Real-World Research & Results](#real-world-research--results)
- [Efficiency Data](#efficiency-data)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

The **Solar-Algae Hybrid System** integrates two complementary renewable technologies:

1. **Photovoltaic (PV) Solar Panels** — convert sunlight into electricity directly.
2. **Algae Photobioreactor (PBR)** — cultivates microalgae (e.g., *Chlorella vulgaris* or *Spirulina*) that:
   - Fix CO₂ from ambient air (air purification)
   - Produce biomass for biodiesel / biogas
   - Generate heat that can be converted via thermoelectric modules

The concept is inspired by building-integrated photovoltaics (BIPV) research and **agrivoltaic** farming. The algae panels are mounted **behind or beneath** semi-transparent solar cells, using the infrared/thermal portion of sunlight that PV cells normally waste as heat.

---
## All the graphs of algie growth and etc
<img width="691" height="470" alt="image" src="https://github.com/user-attachments/assets/ce71a535-8591-4bfa-a2bc-2b29e7946173" />
<img width="686" height="470" alt="image" src="https://github.com/user-attachments/assets/447bd67c-b6b4-4f5b-a256-a6e552316393" />
<img width="686" height="470" alt="image" src="https://github.com/user-attachments/assets/6982d684-fd60-4e29-a8a1-dc4adccabaf9" />

## How It Works

```
Sunlight
   │
   ├──► [Transparent / Semi-transparent PV Layer]
   │         │
   │         ▼
   │    Electricity ──► Battery / Grid
   │
   └──► [Algae Bioreactor Layer] (uses IR + diffused light)
             │
             ├──► CO₂ absorbed from air pumped through system
             ├──► O₂ released back to atmosphere
             ├──► Biomass harvested → Biodiesel / Biogas / Feed
             └──► Waste heat → Thermoelectric Generator (optional)



### Energy Flow Diagram

```
┌─────────────────────────────────────────────────────────┐
│                     SOLAR SPECTRUM                       │
│  UV (5%)    Visible (43%)    Infrared (52%)             │
└──────┬──────────────┬───────────────┬────────────────────┘
       │              │               │
       ▼              ▼               ▼
  [Absorbed     [PV Cell converts] [Algae uses +
  by glass]      → ~20% to elec]   Thermoelectric]
                      │               │
                      ▼               ▼
                 AC/DC Power     Biomass + Heat Energy
```
<img width="691" height="470" alt="image" src="https://github.com/user-attachments/assets/c6af9ec3-01bb-4f66-a124-08ff41cd9eb6" />

---

## System Components

### Hardware

| Component | Description | Approx. Cost |
|-----------|-------------|-------------|
| Semi-transparent solar panel | 100W, ~10% transparency | $80–$150 |
| Flat-plate photobioreactor | Acrylic/glass panel, 10–50L | $30–$200 (DIY) |
| Microalgae culture | *Chlorella* or *Spirulina* starter | $10–$30 |
| Air pump + diffuser | For CO₂-rich air injection | $15–$40 |
| Water circulation pump | 12V DC, 3–5W | $10–$20 |
| MPPT charge controller | 12V/24V 20A | $25–$60 |
| 12V LiFePO4 battery | 50–100Ah | $80–$200 |
| Thermoelectric module (optional) | TEG TEC1-12706 | $5–$15 |
| Arduino/ESP32 controller | Monitoring + automation | $5–$20 |
| Temperature sensors (DS18B20) | Water & panel temp | $2–$5 each |
| pH sensor | Algae culture health | $10–$30 |
| CO₂ sensor (MH-Z19B) | Air quality monitoring | $15–$25 |
| Light sensor (BH1750) | Lux measurement | $2–$5 |
| Relay module (4-channel) | Pump automation | $5–$10 |

**Total Estimated Cost (DIY Prototype):** $300–$800

---

## Circuit Diagram

See [`/circuit/circuit_diagram.md`](circuit/circuit_diagram.md) for full ASCII + description diagram.

### Main Circuit Overview

```
                    ┌──────────────┐
                    │  SOLAR PANEL │
                    │   (100W PV)  │
                    └──────┬───────┘
                           │ DC Out
                    ┌──────▼───────┐
                    │     MPPT     │
                    │   CHARGE     │
                    │  CONTROLLER  │
                    └──┬───────┬───┘
                       │       │
              ┌────────▼─┐   ┌─▼────────────┐
              │ 12V LiFe │   │  LOAD OUTPUT │
              │ BATTERY  │   │  (Inverter / │
              └──────────┘   │  DC Loads)   │
                             └──────────────┘

┌─────────────────────────────────────────────────────────┐
│                ALGAE BIOREACTOR SUBSYSTEM               │
│                                                         │
│  12V Battery ──► Relay Module ──► Air Pump             │
│                             ├──► Water Circulation Pump │
│                             └──► LED Grow Lights (opt.) │
│                                                         │
│  Sensors ──► Arduino/ESP32:                            │
│    - DS18B20 (Temperature)                              │
│    - pH Sensor (Algae health)                           │
│    - BH1750 (Light intensity)                           │
│    - MH-Z19B (CO₂ level)                               │
│                                                         │
│  ESP32 ──► WiFi ──► Dashboard (MQTT / Web)             │
└─────────────────────────────────────────────────────────┘
```

Full Fritzing diagram: [`/circuit/fritzing_diagram.fzz`](circuit/) *(upload your own)*
KiCad schematic: [`/circuit/schematic.kicad_sch`](circuit/) *(see template)*

---

## Bill of Materials

See [`BOM.csv`](BOM.csv) for complete spreadsheet with suppliers and part numbers.

---

## Setup Instructions

This is a prototype/design repository, not a validated construction package. Confirm component datasheets, electrical ratings, sensor calibration, and biological requirements before building. The BOM and diagrams are planning material, not proof of an as-built system.

### Step 1: Review the hardware documents

- [`BOM.csv`](BOM.csv) — proposed parts and rough cost estimates; prices and compatibility have not been verified.
- [`circuit_diagram.md`](circuit_diagram.md) — conceptual system-level diagram.
- [`wiring_guide.md`](wiring_guide.md) — sensor and relay pin mapping used by the current firmware.
- [`troubleshooting.md`](troubleshooting.md) — common prototype issues.
- [`algae_cultivation.md`](algae_cultivation.md) — cultivation notes; validate species-specific guidance before use.

The pH sensor connection in the wiring guide requires the specified voltage divider before ESP32 GPIO 34. The MH-Z19B requires a 5 V supply; follow the guide's UART level-protection notes. The relays are active-low in the current sketch. Do not connect pumps directly to ESP32 GPIO pins.

### Step 2: Prepare a local firmware project

The firmware source is the root-level `main.cpp`. This repository currently has no `platformio.ini`, `src/` directory, or automated build configuration, so `pio run` from the repository root will **not** work as-is.

1. Install [VS Code](https://code.visualstudio.com/) and the [PlatformIO IDE extension](https://platformio.org/install/ide?install=vscode).
2. Create a local PlatformIO project for an ESP32 Dev Module (commonly `esp32dev`) using the Arduino framework.
3. Copy this repository's `main.cpp` into the local project's `src/main.cpp`. Keep the repository file in place; this is a local build scaffold.
4. Add compatible versions of these libraries to the local project: `PubSubClient`, `OneWire`, `DallasTemperature`, `BH1750`, `Adafruit INA219`, `MHZ19`, and `ArduinoJson`. `WiFi` and `Wire` come from the ESP32 Arduino core.
5. In the local copy of `main.cpp`, configure `WIFI_SSID`, `WIFI_PASSWORD`, `MQTT_SERVER`, and `MQTT_PORT`. The current defaults include a placeholder Wi-Fi configuration and a broker address of `192.168.1.100:1883`; use values for your own network.
6. From the **local PlatformIO project directory**, build, upload, and open the serial monitor:

```bash
pio run
pio run --target upload
pio device monitor --baud 115200
```

These are the standard PlatformIO commands for the local scaffold; firmware compilation and hardware operation were not verified during this documentation pass.

### Step 3: Configure and check MQTT

| Setting | Current firmware default | Purpose |
|---|---|---|
| `MQTT_SERVER` | `192.168.1.100` | Broker IPv4 address |
| `MQTT_PORT` | `1883` | MQTT port |
| `MQTT_TOPIC` | `solar_algae/sensors` | JSON telemetry published about every 10 seconds |
| `MQTT_CMD` | `solar_algae/control` | Plain-text command subscription |

The telemetry JSON includes temperature, pH, lux, CO₂ ppm, voltage, current, power, air-pump state, grow-LED state, and uptime. The command payloads recognized by the sketch are `air_pump_on`, `air_pump_off`, `water_pump_on`, `water_pump_off`, `led_on`, and `led_off`. The air-pump schedule in the main loop can override a manual air-pump command, so verify control behaviour before connecting a real load.

The sketch uses ordinary MQTT on port 1883 without authentication or TLS settings. Keep the broker and device on a trusted private network; do not expose this setup directly to the public internet.

### Step 4: Prepare the biological prototype

The steps below summarize the existing project notes, not a validated laboratory protocol. Use suitable PPE and species-specific guidance, and verify the growth medium, cleaning procedure, temperature, pH, light, and aeration requirements before inoculating a culture.

1. Build and leak-check the photobioreactor before adding culture.
2. Mounting and shading should be planned around the panel, enclosure, and culture's light needs; the proposed geometry has not been validated.
3. Check sensors and pumps individually before enabling automated control.
4. Use [`algae_cultivation.md`](algae_cultivation.md) for the project's current cultivation notes and [`troubleshooting.md`](troubleshooting.md) for a starting checklist.
5. Record measured panel output, temperature, pH, light, and culture observations before drawing performance conclusions.

---

## Pros & Cons

### ✅ Advantages

| Benefit | Details |
|---------|---------|
| **Dual energy output** | Electricity from PV + biomass from algae from the same footprint |
| **CO₂ sequestration** | Microalgae fix ~1.8g CO₂ per 1g of biomass produced |
| **Air purification** | Pumping ambient air through algae culture filters pollutants |
| **Thermal management** | Algae water layer cools the PV panel, improving electrical efficiency by 5–15% |
| **Water use** | Closed-loop system; minimal evaporation vs. open pond algae |
| **Scalability** | Works at home scale (1m²) to commercial building facades |
| **No land competition** | Can be roof-mounted or building-integrated |
| **O₂ production** | Algae release clean oxygen as a byproduct |

### ❌ Disadvantages

| Limitation | Details |
|-----------|---------|
| **Complexity** | More components = more maintenance than standalone PV |
| **Initial cost** | Higher upfront cost than single-technology systems |
| **Algae culture management** | Requires regular monitoring, pH control, and harvesting |
| **Contamination risk** | Open/semi-open systems can be contaminated by competing organisms |
| **Light shading** | Algae layer can reduce PV output by 10–30% if culture is too dense |
| **Biomass processing** | Requires downstream processing (centrifuge, drying) to use algae fuel |
| **Seasonal variation** | Algae growth slows in cold temperatures |
| **Regulatory** | Algae-to-fuel processing may require permits in some regions |

---

## Research context and performance claims

This repository does not report controlled measurements from a completed hybrid prototype. The numerical comparisons and named studies previously described in this README and in [`research_references.md`](research_references.md) have not been independently validated during this maintenance pass. Check each primary publication and its methods before using any cited value in a report, pitch, or scientific claim. Do not present the figures as results achieved by this project.

The concept has plausible engineering questions to investigate—light transmission to the culture, thermal coupling, panel output, culture productivity, pumping energy, and maintenance cost—but these require measured data from a defined test setup.

## File Structure

```text
.
├── README.md                 # Project overview, setup, limitations
├── BOM.csv                  # Draft bill of materials and cost estimates
├── circuit_diagram.md        # Conceptual system diagram and pin map
├── wiring_guide.md           # Firmware sensor/relay wiring notes
├── main.cpp                  # ESP32 Arduino-framework firmware source
├── index.html                # Static HTML concept page; not connected to MQTT by the firmware
├── algae_cultivation.md      # Draft algae cultivation notes
├── research_references.md    # Research notes; citations need independent verification
├── troubleshooting.md        # Prototype troubleshooting notes
├── CONTRIBUTING.md
└── LICENSE
```

## Firmware configuration and known behaviour

- Sensor reads and telemetry publication are scheduled at approximately 10-second intervals.
- The firmware uses GPIO 4 for the DS18B20 data line, GPIO 34 for pH ADC input, GPIO 16/17 for MH-Z19B UART, GPIO 21/22 for the shared I²C bus, and GPIO 25/26/27 for the air-pump, water-pump, and grow-light relays.
- The relay outputs are active-low. The sketch turns the water-pump relay on during startup, cycles the air pump for 30 minutes on / 30 minutes off, and switches the grow-light relay on when reported light is below 1,000 lux.
- The pH calculation uses a default linear formula; calibrate it with buffer solutions before treating pH values as accurate. Missing/disconnected sensors may yield invalid or misleading readings.
- `index.html` is a static page. No live MQTT connection from that page to the firmware was found in the files inspected.

## Known limitations and verification status

- No `platformio.ini`, dependency lockfile, automated test suite, or CI build is present in the repository.
- The firmware, sensor calibration, relay timing, MQTT commands, and physical wiring were not compiled or tested on hardware during this pass.
- The wiring diagram, BOM, cultivation guidance, and research notes should be treated as drafts. Confirm actual part ratings and safe power distribution before connecting pumps or relays.
- No measured panel-efficiency improvement, net energy gain, biomass yield, or CO₂ capture result is established by this repository.
- MQTT uses an unauthenticated, unencrypted configuration in the current sketch. Use a trusted private network and do not expose the broker/device publicly.


## Contributing

Pull requests are welcome! See [`CONTRIBUTING.md`](CONTRIBUTING.md) for guidelines.

Areas where help is needed:
- Real-world test data submissions
- Improved firmware (MQTT, Home Assistant integration)
- 3D printable bracket designs
- Alternative algae species testing

---

## License

MIT License — see [`LICENSE`](LICENSE) for details.

---

## Acknowledgements

- AlgaePARC, Wageningen University
- Fraunhofer ISE, Germany
- IIT Bombay Department of Energy Science
- Open-source Arduino/ESP32 community

---
## Performance measurement status

No validated efficiency gain is established by this repository. Any percentage or energy-yield estimate should be treated as a hypothesis until a documented experiment compares panel output, culture growth, auxiliary pump/LED energy, and operating conditions against a suitable baseline.

*Built with a focus on exploring a cleaner, greener energy concept.*


## Skills & technologies

`Arduino/C++` · `HTML/CSS`


## Maintenance & Audit

- **Audit date:** 11 November 2026
- **Documentation status:** README reviewed and audit record added on this date.
- **Scope and evidence:** This is a documentation-level maintenance pass. It does not establish that source code compiles, dependencies are current, security has been tested, or hardware/runtime behavior has been verified.
- **Code and tests:** No new functional code changes or test results are claimed by this README update. Run project-specific build, test, and hardware/runtime checks before relying on the project.
- **Maintenance baseline:** Documentation status recorded through 11 November 2026; revisit setup instructions, dependencies, known limitations, and tests when the project changes.
