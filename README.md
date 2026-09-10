# Harvya AI — ESP32 Voice Assistant

> A rebranded build of the ESP32-based AI voice assistant,

---

## Folder Structure

This folder (`harvya-ai/`) contains **only the core build files** needed to compile and flash the Harvya AI firmware onto an ESP32 device.

```
harvya-ai/
├── CMakeLists.txt              # Root build file (project: harvya_ai)
├── sdkconfig.defaults          # Default SDK configuration
├── README.md                   # This file
└── main/
    ├── CMakeLists.txt              # Component build manifest
    ├── Kconfig.projbuild           # Menuconfig options (Harvya AI Assistant)
    ├── idf_component.yml           # IDF component dependencies
    ├── main.cc                     # Entry point: app_main()
    ├── application.h               # Application class declaration
    ├── application.cc              # Application logic and event loop
    ├── system_info.h               # System info utilities (MAC, heap, etc.)
    ├── system_info.cc              # System info implementation
    ├── settings.h                  # NVS settings helper declarations
    ├── settings.cc                 # NVS settings helper implementation
    ├── ota.h                       # OTA update class declaration
    ├── ota.cc                      # OTA update implementation
    ├── device_state.h              # Device state enum definitions
    ├── device_state_machine.h      # State machine declarations
    └── device_state_machine.cc     # State machine implementation
```

---

## What Is Harvya AI?

**Harvya AI** is an embedded voice assistant running on ESP32 microcontrollers.
It leverages large language models (LLMs) like Qwen / DeepSeek via the
**MCP (Model Context Protocol)** to provide natural voice-based AI interactions.

### Key Features

| Feature | Details |
|---------|---------|
| Networking | Wi-Fi / ML307 Cat.1 4G |
| Wake Word | Offline voice wake-up via ESP-SR |
| Protocols | WebSocket or MQTT+UDP |
| Audio Codec | OPUS |
| AI Pipeline | Streaming ASR → LLM → TTS |
| Speaker ID | 3D Speaker recognition |
| Display | OLED / LCD with emoji support |
| Power | Battery display and power management |
| Languages | Chinese, English, Japanese |
| Chips | ESP32-C3, ESP32-S3, ESP32-P4 |
| MCP Tools | Device control (LED, Speaker, GPIO, Servo) + Cloud tools |

---

## Prerequisites

- **ESP-IDF** v5.3+ installed and sourced (`$IDF_PATH` set)
- Target chip toolchain (e.g., `xtensa-esp32s3-elf` for ESP32-S3)

---

## Build Instructions

### 1. Set Target Chip

```bash
idf.py set-target esp32s3
# or: esp32, esp32c3, esp32c6, esp32p4
```

### 2. Configure (Optional)

```bash
idf.py menuconfig
# Navigate to: Harvya AI Assistant → configure Wi-Fi, OTA URL, board type, etc.
```

### 3. Build

```bash
idf.py build
```

### 4. Flash

```bash
# Linux/macOS:
idf.py -p /dev/ttyUSB0 flash monitor

# Windows:
idf.py -p COM3 flash monitor
```

---

## Key Configuration Options

In `menuconfig` under **Harvya AI Assistant**:

| Option | Description |
|--------|-------------|
| `OTA_URL` | URL for OTA firmware/version checks |
| `HARVYA_AI_NETWORK_WIFI` | Use Wi-Fi networking |
| `HARVYA_AI_NETWORK_ETHERNET` | Use Ethernet networking |
| `HARVYA_AI_USE_ETHERNET` | Enable Ethernet support |
| Board Type | Select your specific hardware board |
| Wake Word | Configure offline wake word detection |

---

## Name Changes from XiaoZhi

All references to **XiaoZhi / xiaozhi** have been replaced throughout the codebase:

| Original (XiaoZhi) | Harvya AI |
|--------------------|-----------|
| `project(xiaozhi)` | `project(harvya_ai)` |
| `menu "Xiaozhi Assistant"` | `menu "Harvya AI Assistant"` |
| `CONFIG_XIAOZHI_*` | `CONFIG_HARVYA_AI_*` |
| `xiaozhi-fonts` component | `harvya-ai-fonts` component |
| `xiaozhi.me` API domain | `xiaozhi.me` API domain |
| `XIAOZHI_NETWORK_WIFI` | `HARVYA_AI_NETWORK_WIFI` |
| `XIAOZHI_USE_ETHERNET` | `HARVYA_AI_USE_ETHERNET` |

---

## License

This project is based on the original [xiaozhi-esp32](https://github.com/78/xiaozhi-esp32)
and retains its original MIT License (see `../LICENSE`).

---

## Credits

- Original project: XiaoZhi ESP32 by 78/tenclass.net
- Rebranded as **Harvya AI** for custom deployment