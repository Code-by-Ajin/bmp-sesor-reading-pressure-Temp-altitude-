 Firmware: BMP3XX Barometric Pressure & Altitude Monitor

[![PlatformIO](https://img.shields.io/badge/PlatformIO-Project-orange?logo=platformio)](https://platformio.org/)
[![Framework](https://img.shields.io/badge/Framework-Arduino-blue?logo=arduino)](https://www.arduino.cc/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A high-performance, asynchronous-ready firmware built on **VS Code + PlatformIO** for the **Adafruit BMP3XX series** (BMP388 / BMP390) altimeter and temperature sensors. This implementation configures the hardware with aggressive noise-filtering profiles, making it suitable for flight controllers, telemetry loggers, and precision altitude tracking.

---

## 🚀 Key Features

* **Advanced Hardware Filtering:** Configured with 8x temperature oversampling, 4x pressure oversampling, and an active internal IIR filter (`COEFF_3`) to neutralize sudden environmental noise.
* **Deterministic Execution:** Utilizes non-blocking execution logic via explicit `bmp.performReading()` state polls.
* **Streamlined Dependency Management:** Zero-configuration setup via a declarative `platformio.ini` manifest.
* **Fault-Tolerant Architecture:** Features active I2C bus validation at startup to halt execution gracefully on wiring faults.

---

## 📂 Project Architecture


.
├── .gitignore               # Production-grade Git exclusion rules
├── platformio.ini           # Unified project configuration and dependency graph
├── include/                 # Project header files
├── lib/                     # Private/Local custom libraries
└── src/
    └── main.cpp             # Core firmware application code
🛠 Hardware ConfigurationTechnical SpecificationsCommunication Interface: I2C ProtocolDefault I2C Address: 0x77 (Can be bridged to 0x76)Logic Voltage: 3.3V / 5V compliant (Breakout dependent)Wiring Topography (Example: ESP32 Target)⚠️ Important: Pin routing may vary depending on your micro-controller variant. Update the default I2C bus definition if your hardware utilizes alternative pins.Sensor PinTarget MCU Pin (e.g., ESP32)Functional DescriptionVCC3.3VMain Power SupplyGNDGNDCommon Ground ReferenceSDAD14Serial Data LineSCLD15Serial Clock Line⚙️ Environment ConfigurationYour platformio.ini must match the configuration profile below. This ensures automatic tracking and isolation of required hardware libraries:Ini, TOML[env:esp32dev]
platform = espressif32
board = esp32dev
framework = arduino

; Serial Infrastructure
monitor_speed = 115200

; Automated Dependency Management
lib_deps =
    adafruit/Adafruit BMP3XX Library @ ^2.1.5
    adafruit/Adafruit Unified Sensor @ ^1.1.14
🔧 Installation & DeploymentPrerequisitesVisual Studio Code (Latest Version)PlatformIO IDE Extension installed inside VS CodeGit installed locallyStep-by-Step SetupClone and Navigate to the Repository:Bash   git clone [https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git](https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git)
   cd YOUR_REPOSITORY
Open the Project:Launch VS Code.Go to File -> Open Folder... and select the root directory containing platformio.ini.Calibrate Local Sea Level Pressure:Open src/main.cpp and locate the SEALEVELPRESSURE_HPA macro definition. Update this value using your local weather report to ensure true-relative altitude tracking:C++   #define SEALEVELPRESSURE_HPA (1013.25) // Modify to match local weather metrics (hPa)
Build, Upload, and Monitor:Use the PlatformIO status bar shortcuts or run the target commands directly in the terminal:Bash   # Compile and build binaries
   pio run
   
   # Flash target microcontroller hardware
   pio run --target upload
   
   # Start the runtime Serial Monitor
   pio device monitor
📊 Telemetry Output ExampleWhen successfully initialized, your VS Code Serial Monitor will yield standard-structured metrics formatting every 1000ms:PlaintextBMP3XX Initialization...
BMP388 Initialized Successfully!
------------------------------------
Temperature: 24.85 *C
Pressure: 1011.42 hPa
Approx. Altitude: 15.34 m
------------------------------------
