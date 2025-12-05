# Class Signal

<div align="center">

![Status](https://img.shields.io/badge/status-proof%20of%20concept-yellow)
![License](https://img.shields.io/badge/license-TBD-blue)
![Platform](https://img.shields.io/badge/platform-Raspberry%20Pi%204-red)
![Hardware](https://img.shields.io/badge/hardware-ESP32-green)
![Python](https://img.shields.io/badge/python-3.9%2B-blue)

**Digital classroom engagement system using ESP32 devices for real-time student participation tracking**

[Features](#features) • [Installation](#installation) • [Usage](#usage) • [Protocol](#communication-protocol) • [Contributing](#contributing)

</div>

---

## Overview

Class Signal is a self-contained classroom management system that enables students to signal participation through dedicated ESP32 hardware devices. Teachers receive real-time notifications via a web dashboard and can provide instant feedback. The entire system operates on a local network without requiring internet connectivity.

## Features

-  **Student Authentication** - Personal 4-digit codes with OLED feedback
-  **Instant Participation Signaling** - One-button press to join the discussion queue
-  **Real-time Teacher Dashboard** - Live updates via WebSocket
-  **Feedback System** - Direct assessment of student contributions
-  **Analytics & Reports** - Engagement statistics and CSV/YAML exports
-  **Local Network** - Complete data sovereignty, no internet required
-  **Low Latency** - Sub-second response times

## System Architecture

```
┌─────────────┐         WiFi          ┌──────────────────┐
│   ESP32     │◄─────────────────────►│  Raspberry Pi 4  │
│  (Student)  │      TCP/JSON         │   (AP + Server)  │
└─────────────┘                       └────────┬─────────┘
                                               │
┌─────────────┐                                |
│   ESP32     │◄───────────────────────────────|
│  (Student)  │         WiFi                   |
└─────────────┘                       ┌──────────────────┐
                                      │  Web Dashboard   │
                                      │    (Teacher)     │
                                      └──────────────────┘
```

**Hardware:**
- ESP32 Xiao with OLED display and buttons
- Raspberry Pi 4 as WiFi access point and server

**Software:**
- Backend: Python (Flask/FastAPI) with WebSocket support
- Frontend: React/Svelte + Tailwind CSS
- Storage: Local YAML files



### ESP32 Firmware

1. Open `esp32/class_signal.ino` in Arduino IDE
2. Install required libraries:
   - Adafruit SSD1306
   - ArduinoJson
   - WiFi
3. Configure WiFi credentials in the code
4. Upload to ESP32 Xiao

## Usage

### For Students

1. Power on ESP32 device
2. Enter 4-digit personal code using buttons
3. Wait for "Willkommen [Name]" on display
4. Press button to signal participation
5. Display confirms when teacher acknowledges

### For Teachers

1. Open web dashboard at `http://192.168.4.1:5000`
2. Monitor participation queue in real-time
3. Click on student names to acknowledge
4. Provide feedback after student contributions
5. View statistics and export reports

## Communication Protocol

All messages use JSON format over TCP sockets.

### Login
```json
// ESP → Server
{"type":"login","device_id":"ESP01","user_code":"1234","timestamp":"2025-11-08T09:18:01Z"}

// Server → ESP
{"type":"login_response","status":"ok","user_id":1234,"user_name":"Leonardo","timestamp":"2025-11-08T09:18:02Z"}
```

### Participation Signal
```json
// ESP → Server
{"type":"raise_hand","device_id":"ESP01","user_id":1234,"timestamp":"2025-11-08T09:20:00Z"}

// Server → ESP
{"type":"raise_hand_ack","status":"ok","timestamp":"2025-11-08T09:20:00Z"}
```

## Data Structure

### Students Database (`schueler.yaml`)
```yaml
schueler:
  - id: 1234
    name: "Leonardo"
    klasse: "8A"
  - id: 4321
    name: "Anna"
    klasse: "8A"
```

### Participation Records (`meldungen.yaml`)
```yaml
meldungen:
  - user_id: 1234
    timestamp: "2025-11-08T09:20:00Z"
    device_id: "ESP01"
    status: "acknowledged"
    feedback: "excellent contribution"
```

## Configuration

Edit `config.yaml` to customize:
- Network settings (SSID, password)
- Server port and address
- Feedback options
- Display preferences

## Roadmap

- [ ] Basic WiFi and TCP communication
- [ ] Student authentication system
- [ ] Web dashboard with live updates
- [ ] Feedback and rating system
- [ ] Statistical analysis and reports
- [ ] Multi-classroom support
- [ ] Mobile app for teachers

## Technical Requirements

**Hardware:**
- Raspberry Pi 4 (2GB+ RAM)
- ESP32 Xiao boards
- 0.96" OLED displays (I2C)
- Push buttons

**Software:**
- Raspberry Pi OS Bookworm
- Python 3.9+
- Modern web browser with WebSocket support

## Security

- WPA2/WPA3 encrypted local network
- No internet connectivity required
- All data stored locally on Raspberry Pi
- Personal authentication codes for students

## Contributing

Contributions welcome! Please:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License
we will see :)
## Author

**Leonardo**

For questions or feedback, please open an issue on GitHub
