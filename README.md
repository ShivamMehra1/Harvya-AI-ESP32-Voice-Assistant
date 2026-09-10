# Harvya AI — ESP32 Voice Assistant

> A rebranded build of the ESP32-based AI voice assistant,

---

## Folder Structure

This folder (`harvya-ai/`) is a **fully self-contained** build of the Harvya AI firmware.
It includes all source code, board support, libraries, and partition tables needed to compile and flash onto an ESP32 device.

```
harvya-ai/
├── CMakeLists.txt                  # Root build file (project: harvya_ai)
├── sdkconfig                       # Active SDK configuration
├── sdkconfig.defaults              # Default SDK configuration
├── sdkconfig.defaults.esp32        # ESP32-specific defaults
├── sdkconfig.defaults.esp32s3      # ESP32-S3-specific defaults
├── sdkconfig.defaults.esp32c3      # ESP32-C3-specific defaults
├── sdkconfig.defaults.esp32c6      # ESP32-C6-specific defaults
├── sdkconfig.defaults.esp32p4      # ESP32-P4-specific defaults
├── sdkconfig.defaults.esp32c5      # ESP32-C5-specific defaults
├── dependencies.lock               # Locked dependency versions
├── .clang-format                   # Code style config
├── LICENSE                         # MIT License
├── README.md                       # This file
│
├── main/                           # Core application source
│   ├── CMakeLists.txt              # Component build manifest
│   ├── Kconfig.projbuild           # Menuconfig options (Harvya AI Assistant)
│   ├── idf_component.yml           # IDF component dependency list
│   ├── main.cc                     # Entry point: app_main()
│   ├── application.h / .cc         # Main application class and event loop
│   ├── system_info.h / .cc         # System utilities (MAC, heap, flash)
│   ├── settings.h / .cc            # NVS persistent settings
│   ├── ota.h / .cc                 # OTA firmware update
│   ├── mcp_server.h / .cc          # MCP AI tool protocol server
│   ├── assets.h / .cc              # Asset management (fonts, sounds)
│   ├── device_state.h              # Device state enum
│   ├── device_state_machine.h/.cc  # State machine
│   │
│   ├── audio/                      # Audio subsystem
│   │   ├── audio_codec.h/.cc       # Codec abstraction (ES8311, ES8388, etc.)
│   │   ├── audio_service.h/.cc     # Audio pipeline (ASR, TTS, wake word)
│   │   ├── codecs/                 # Hardware codec drivers
│   │   ├── demuxer/                # OGG/OPUS demuxer
│   │   ├── processors/             # Audio effects/AEC
│   │   └── wake_words/             # Offline wake word models
│   │
│   ├── protocols/                  # Network communication
│   │   ├── protocol.h/.cc          # Base protocol class
│   │   ├── mqtt_protocol.h/.cc     # MQTT+UDP protocol
│   │   └── websocket_protocol.h/.cc# WebSocket protocol
│   │
│   ├── display/                    # Display drivers
│   │   ├── display.h/.cc           # Display abstraction
│   │   ├── lcd_display.h/.cc       # LCD driver
│   │   ├── oled_display.h/.cc      # OLED driver
│   │   ├── emote_display.h/.cc     # Emoji/emote display
│   │   └── lvgl_display/           # LVGL-based display (fonts, GIF, images)
│   │
│   ├── led/                        # LED drivers
│   │   ├── single_led.h/.cc        # Single LED
│   │   ├── circular_strip.h/.cc    # Circular LED strip
│   │   └── gpio_led.h/.cc          # GPIO-controlled LED
│   │
│   ├── boards/                     # 100+ hardware board definitions
│   │   ├── common/                 # Shared board base classes
│   │   └── <board-name>/           # Per-board pin maps and init
│   │
│   └── assets/                     # Localization and language assets
│       ├── lang_config.h           # Language configuration
│       └── locales/                # Per-language string tables
│
├── managed_components/                         # All 70 components used in build (ESP32-S3 target)
│   │
│   │   # --- Custom / Third-party ---
│   ├── 78__esp-ml307/                          # ML307 Cat.1 4G modem driver
│   ├── 78__esp-wifi-connect/                   # Wi-Fi provisioning helper
│   ├── 78__esp_lcd_nv3023/                     # NV3023 LCD panel driver
│   ├── 78__uart-eth-modem/                     # UART ethernet modem
│   ├── 78__uart-uhci/                          # UART UHCI driver
│   ├── 78__xiaozhi-fonts/                      # Custom fonts (xiaozhi-fonts)
│   ├── esphome__esp-hub75/                     # HUB75 LED matrix panel
│   ├── laride__heatshrink/                     # Heatshrink data compression
│   ├── m5stack__m5ioe1/                        # M5Stack IO expander
│   ├── m5stack__m5pm1/                         # M5Stack power management
│   ├── tny-robotics__sh1106-esp-idf/           # SH1106 OLED driver
│   ├── txp666__otto-emoji-gif-component/       # Otto robot emoji GIF
│   ├── waveshare__custom_io_expander_ch32v003/ # Waveshare IO expander (CH32V003)
│   ├── waveshare__esp_lcd_sh8601/              # Waveshare SH8601 LCD driver
│   ├── waveshare__esp_lcd_touch_cst9217/       # Waveshare CST9217 touch driver
│   ├── wvirgil123__sscma_client/              # SenseCAP SSCMA client
│   │
│   │   # --- Espressif2022 ---
│   ├── espressif2022__esp_emote_assets/        # Emote animation assets
│   ├── espressif2022__esp_emote_expression/    # Emote expression engine
│   ├── espressif2022__esp_emote_gfx/           # Emote graphics renderer
│   ├── espressif2022__image_player/            # Image/GIF player
│   │
│   │   # --- Espressif Audio ---
│   ├── espressif__esp_audio_codec/             # Audio codec abstraction layer
│   ├── espressif__esp_audio_effects/           # Audio effects (AEC, AGC, etc.)
│   ├── espressif__esp_codec_dev/               # Codec device drivers (ES8311, ES8388…)
│   ├── espressif__esp-sr/                      # Speech recognition (wake word, ASR)
│   ├── espressif__dl_fft/                      # Deep learning FFT
│   ├── espressif__esp-dsp/                     # DSP algorithms library
│   │
│   │   # --- Espressif Display / LCD ---
│   ├── espressif__esp_lcd_axs15231b/           # AXS15231B LCD driver
│   ├── espressif__esp_lcd_co5300/              # CO5300 LCD driver
│   ├── espressif__esp_lcd_gc9a01/              # GC9A01 round LCD driver
│   ├── espressif__esp_lcd_ili9341/             # ILI9341 LCD driver
│   ├── espressif__esp_lcd_panel_io_additions/  # LCD panel IO helpers
│   ├── espressif__esp_lcd_spd2010/             # SPD2010 LCD driver
│   ├── espressif__esp_lcd_st7701/              # ST7701 LCD driver
│   ├── espressif__esp_lcd_st77916/             # ST77916 LCD driver
│   ├── espressif__esp_lcd_st7796/              # ST7796 LCD driver
│   ├── espressif__esp_lvgl_port/               # LVGL ESP-IDF port layer
│   ├── espressif__freetype/                    # FreeType font rendering
│   ├── lvgl__lvgl/                             # LVGL graphics library v9.x
│   │
│   │   # --- Espressif Touch ---
│   ├── espressif__esp_lcd_touch/               # Touch controller base
│   ├── espressif__esp_lcd_touch_cst816s/       # CST816S touch driver
│   ├── espressif__esp_lcd_touch_ft5x06/        # FT5x06 touch driver
│   ├── espressif__esp_lcd_touch_gt1151/        # GT1151 touch driver
│   ├── espressif__esp_lcd_touch_gt911/         # GT911 touch driver
│   ├── espressif__esp_lcd_touch_st7123/        # ST7123 touch driver
│   ├── espressif__touch_button_sensor/         # Capacitive touch button
│   ├── espressif__touch_sensor_fsm/            # Touch sensor FSM
│   ├── espressif__touch_sensor_lowlevel/       # Touch sensor low-level
│   ├── espressif__touch_slider_sensor/         # Touch slider sensor
│   │
│   │   # --- Espressif Camera / Image ---
│   ├── espressif__esp32-camera/                # ESP32 camera driver
│   ├── espressif__esp_cam_sensor/              # Camera sensor abstraction
│   ├── espressif__esp_image_effects/           # Image processing effects
│   ├── espressif__esp_jpeg/                    # JPEG encode/decode
│   ├── espressif__esp_new_jpeg/                # New JPEG hardware support
│   ├── espressif__esp_sccb_intf/               # SCCB (I2C-like camera bus)
│   ├── espressif__esp_video/                   # Video pipeline (ESP32-P4/S3)
│   ├── espressif__esp_mmap_assets/             # Memory-mapped asset loader
│   │
│   │   # --- Espressif IO / Peripheral ---
│   ├── espressif__adc_battery_estimation/      # ADC battery level estimation
│   ├── espressif__bmi270_sensor/               # BMI270 IMU sensor
│   ├── espressif__button/                      # Button debounce library
│   ├── espressif__cmake_utilities/             # CMake build utilities
│   ├── espressif__esp_io_expander/             # IO expander base class
│   ├── espressif__esp_io_expander_tca9554/     # TCA9554 IO expander
│   ├── espressif__esp_io_expander_tca95xx_16bit/ # TCA95xx 16-bit IO expander
│   ├── espressif__i2c_bus/                     # I2C bus helper
│   ├── espressif__iot_eth/                     # Ethernet IOT helper
│   ├── espressif__iot_usbh_cdc/               # USB host CDC
│   ├── espressif__iot_usbh_rndis/             # USB host RNDIS (ethernet over USB)
│   ├── espressif__knob/                        # Rotary knob/encoder
│   ├── espressif__led_strip/                   # Addressable LED strip (WS2812)
│   └── espressif__usb_host_uvc/               # USB host UVC (camera)
│
└── partitions/                                 # Flash partition tables
    ├── v1/                                     # v1 partition layout
    └── v2/                                     # v2 partition layout (current)
```

**Total: ~11,230 files across ~1,964 folders | Build target: ESP32-S3**

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

- **ESP-IDF** v5.5.2+ installed and sourced (`$IDF_PATH` set)
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

