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

### 🔮 Xiaozhi Ball V2 - OrbWare

A comprehensive ESPHome configuration transforming the Xiaozhi Ball V2 into a powerful voice-controlled smart home display.

**Features:**
- Multi-wake word voice assistant (okay_nabu, hey_jarvis, alexa)
- Interactive circular display with multiple screens
- Light control interface
- Media player control with now playing display
- Weather forecast display
- Touch and swipe navigation

[View OrbWare Documentation →](./xiaozhi-ball-v2/orbware/)

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

## Repository Structure

```
esphome-projects/
├── xiaozhi-ball-v2/          # Xiaozhi Ball V2 projects
│   ├── orbware/              # OrbWare V1 configuration
│   └── resources/            # Shared resources (sounds, icons)
└── README.md                 # This file
```

## Current Focus

I am currently working with the **Xiaozhi Ball V2**, a smart display device that combines:
- Voice assistant capabilities
- Touch-enabled circular display
- Home automation control
- Media playback interface
- Real-time information display

This device exemplifies how ESPHome can transform hardware into a versatile smart home control hub that works seamlessly with AI assistants while maintaining full local control.

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
