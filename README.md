# ESPHome Projects

> A collection of ESPHome configurations and projects for smart home automation

[![ESPHome](https://img.shields.io/badge/ESPHome-Compatible-blue.svg)](https://esphome.io/)
[![Home Assistant](https://img.shields.io/badge/Home%20Assistant-Integration-green.svg)](https://www.home-assistant.io/)

## About

This repository contains my ESPHome project configurations designed to bridge the gap between controlling home devices, having an AI assistant, and being able to control my home via dedicated devices. ESPHome provides the perfect foundation for creating custom smart home solutions that integrate seamlessly with Home Assistant while maintaining local control and privacy.

## Why ESPHome?

ESPHome is a powerful system for creating custom firmware for ESP8266/ESP32 devices that:

- **Bridges Multiple Control Methods**: Connect traditional home devices with modern voice assistants and dedicated control interfaces
- **Local Control**: Keeps your smart home functioning without cloud dependencies
- **Home Assistant Integration**: Native integration with Home Assistant for seamless automation
- **Voice Assistant Support**: Built-in support for wake word detection and voice control
- **Customizable**: Full control over every aspect of your device's behavior
- **Open Source**: Community-driven with extensive documentation and support

## Current Projects

### 📦 ESP32-P4 Smart 86-Box - Smart Box V1

A comprehensive ESPHome configuration for the ESP32-P4 Smart 86-Box and compatible devices, transforming them into powerful voice-controlled smart home control panels with a 720x720 high-resolution display.

**Compatible Devices:**
- ESP32-P4-86-Panel-ETH-2RO
- ESP32-P4-WIFI6-Touch-LCD-4B ([Purchase from Waveshare](https://www.waveshare.com/esp32-p4-wifi6-touch-lcd-4b.htm))

**Features:**
- Multi-wake word voice assistant (okay_nabu, hey_jarvis, alexa)
- High-resolution 720x720 MIPI-DSI touch display
- Weather dashboard with 3-day forecast
- Media player control with album art display
- Home control page with 4 customizable buttons
- Radio station and playlist presets
- Settings panel with brightness and volume control
- Swipe navigation between screens

**Documentation:**
- [Smart Box V1 Installation & Usage Guide →](./esp32-p4-86-panel/)
- [Home Assistant Configuration Guide →](./esp32-p4-86-panel/HA_CONFIGURATION.md)

### 🔮 Xiaozhi Ball V2 - OrbWare

A comprehensive ESPHome configuration transforming the Xiaozhi Ball V2 into a powerful voice-controlled smart home display.

**Features:**
- Multi-wake word voice assistant (okay_nabu, hey_jarvis, alexa)
- Interactive circular display with multiple screens
- Light control interface
- Media player control with now playing display
- Weather forecast display with today's and tomorrow's conditions
- Touch and swipe navigation

**Documentation:**
- [OrbWare Installation & Usage Guide →](./xiaozhi-ball-v2/orbware/)
- [Home Assistant Configuration Guide →](./xiaozhi-ball-v2/orbware/HA_CONFIGURATION.md)

## Getting Started

### Prerequisites

1. **Home Assistant** - Running instance on your network
2. **ESPHome** - Installed as a Home Assistant add-on or standalone
3. **Compatible Hardware** - See individual project requirements
4. **2.4GHz WiFi** - ESP devices require 2.4GHz networks

### Installation

Each project has its own detailed installation guide. Navigate to the project directory and follow the README instructions:

1. Clone this repository or download the project files
2. Configure entity IDs and settings in the YAML file
3. Set up your WiFi credentials in `secrets.yaml`
4. Flash the firmware using ESPHome
5. Configure required Home Assistant sensors (see project's HA_CONFIGURATION.md if available)

## Repository Structure

```
esphome-projects/
├── esp32-p4-86-panel/        # ESP32-P4 Smart 86-Box projects
│   ├── smart-box-v1.yaml     # Main ESPHome configuration
│   ├── HA_CONFIGURATION.md   # Home Assistant setup guide
│   ├── README.md             # Project documentation
│   └── resources/            # Shared resources
│       ├── playlist-radio-icons/  # Radio station logos
│       └── weather-icons/    # Weather condition icons
├── xiaozhi-ball-v2/          # Xiaozhi Ball V2 projects
│   ├── orbware/              # OrbWare V1 configuration
│   │   ├── orbware-v1.yaml   # Main ESPHome configuration
│   │   ├── HA_CONFIGURATION.md   # Home Assistant setup guide
│   │   ├── README.md         # Project documentation
│   │   └── images/           # UI screenshots
│   └── resources/            # Shared resources (sounds, icons)
│       ├── sounds/           # Startup sounds
│       └── weather-icons/    # Weather condition icons
└── README.md                 # This file
```

## Current Focus

I am currently working with two smart display devices:

### ESP32-P4 Smart 86-Box / ESP32-P4-WIFI6-Touch-LCD-4B
A wall-mounted control panel compatible with multiple ESP32-P4 based devices:
- **Supported devices**: ESP32-P4-86-Panel-ETH-2RO, ESP32-P4-WIFI6-Touch-LCD-4B
- High-resolution 720x720 MIPI-DSI display
- Voice assistant with on-device wake word detection
- Weather dashboard with multi-day forecasts
- Media player controls with album art
- Home automation control buttons

### Xiaozhi Ball V2
A compact smart display that combines:
- Voice assistant capabilities
- Touch-enabled circular display
- Home automation control
- Media playback interface
- Real-time information display

These devices exemplify how ESPHome can transform hardware into versatile smart home control hubs that work seamlessly with AI assistants while maintaining full local control.

## Future Projects

This repository will grow to include additional ESPHome configurations for:
- Other smart display devices
- Custom sensor integrations
- Home automation controllers
- Voice assistant devices

## Contributing

Contributions, suggestions, and improvements are welcome! Feel free to:
- Open issues for bugs or feature requests
- Submit pull requests with improvements
- Share your own ESPHome configurations
- Provide feedback on existing projects

## Resources

- [ESPHome Documentation](https://esphome.io/)
- [Home Assistant](https://www.home-assistant.io/)
- [ESPHome Discord Community](https://discord.gg/KhAMKrd)
- [Home Assistant Voice Assistant](https://www.home-assistant.io/voice_control/)

## License

This project is open source. See individual project directories for specific license information.

## Acknowledgments

- Thanks to the ESPHome team for creating an amazing platform
- Thanks to the Home Assistant community for continuous support
- Special thanks to [RealDeco/xiaozhi-esphome](https://github.com/RealDeco/xiaozhi-esphome) for inspiration and base configuration

---

⭐ If you find these projects useful, please consider starring this repository!
