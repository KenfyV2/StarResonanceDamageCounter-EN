# Star Resonance Damage Counter

[![License: AGPL v3](https://img.shields.io/badge/License-AGPL%20v3-brightgreen.svg)](https://www.gnu.org/licenses/agpl-3.0.txt)
[![Node.js](https://img.shields.io/badge/Node.js-22%2B-green.svg)](https://nodejs.org/)
[![pnpm](https://img.shields.io/badge/pnpm-10.13.1-orange.svg)](https://pnpm.io/)

> **Note:** This is a fork of [dmlgzs/StarResonanceDamageCounter](https://github.com/dmlgzs/StarResonanceDamageCounter).
> All credit for the original work goes to dmlgzs and contributors.
> This fork provides English translations for user-facing text (README, UI, messages).
> Code comments remain in Chinese. Contributions to translate comments are welcome!
> English translation provided by [KenfyV2]

A real-time combat data statistics tool for the game *Star Resonance*. It uses network packet capture technology to analyze combat data in real-time, providing damage statistics, DPS calculations, and more.

The accuracy of this tool has been verified through multiple actual combat tests. Under stable network conditions, no data loss issues have been found so far.

This tool does not require modifications to the game client and does not violate the game's terms of service. It is designed to help players better understand combat data, reduce ineffective improvements, and enhance the gaming experience. Before using this tool, please ensure that you will not use the data results for behaviors that harm the game community environment, such as discrimination based on combat power.

[Introduction Video](https://www.bilibili.com/video/BV1T4hGzGEeX/)

## ✨ Features

- 🎯 **Real-time Damage Statistics** - Capture and track combat damage in real-time
- 📊 **DPS Calculation** - Provides instantaneous DPS and overall DPS calculations
- 🎲 **Detailed Classification** - Distinguishes between normal damage, critical damage, lucky damage, and more
- 🌐 **Web Interface** - Beautiful real-time data display with line charts
- 🌙 **Theme Switching** - Support for light/dark mode
- 🔄 **Auto Refresh** - Real-time data updates without manual refresh
- 📈 **Statistical Analysis** - Detailed statistics including critical rate, lucky rate, and more

## 🚀 Quick Start

### One-Click Usage

Visit the [GitHub Actions page](https://github.com/dmlgzs/StarResonanceDamageCounter/actions) to download the latest automatically packaged version.

Visit the [Release page](https://github.com/dmlgzs/StarResonanceDamageCounter/releases) to download release versions.

Visit [Quark Disk](https://pan.quark.cn/s/89c4795e0751) to download release versions.

### Manual Compilation

#### Prerequisites

- **Node.js** >= 22.15.0
- **pnpm** >= 10.13.1
- **WinPcap/Npcap** (network packet capture driver)
- **Visual Studio Build Tools** (build dependency)
  - Install via [Visual Studio Installer](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
  - Select "Desktop development with C++" workload
- **Python** 3.10 (build dependency)
  - Download and install from [Python Official Website](https://www.python.org/downloads/)
  - Ensure Python is added to system PATH

#### Installation Steps

1. **Clone Repository**

   ```bash
   git clone https://github.com/dmlgzs/StarResonanceDamageCounter.git
   cd StarResonanceDamageCounter
   ```

2. **Install Dependencies**

   ```bash
   corepack enable
   pnpm install
   ```

3. **Install WinPcap/Npcap**
   - Download and install [Npcap](https://nmap.org/npcap/) or [WinPcap](https://www.winpcap.org/) (Npcap recommended)
   - Ensure "WinPcap API-compatible mode" is selected during installation

4. **Run**

   ```bash
   node server.js
   ```

   After running, you will be prompted to:
   - Select the network device for packet capture (check network adapter info in Control Panel)
   - Select log level (`info`: basic information, `debug`: detailed logs)

   You can also specify via command-line arguments:

   ```bash
   node server.js <device_number> <log_level>
   ```

   Or use auto-detection mode (recommended):

   ```bash
   node server.js auto info
   ```

   Auto-detection mode will:
   - Intelligently identify physical network adapters, excluding virtual adapters (e.g., ZeroTier, VMware, etc.)
   - Analyze network traffic for 3 seconds and automatically select the most active adapter
   - Fall back to routing table method when no traffic is detected

   Manual specification example:

   ```bash
   node server.js 4 info
   ```

### Usage Instructions

1. **Select Network Device**
   - After starting the program, a list of available network devices will be displayed
   - Enter the number corresponding to the device shown in the program's output list (usually select the primary network adapter)
   - You can check the network adapter in Control Panel or System Settings

2. **Set Log Level**
   - Choose log level: `info` or `debug`
   - `info` level is recommended to reduce log output

3. **Start Game**
   - The program will automatically detect game server connections
   - When a game server is detected, server information will be output and data statistics will begin

4. **View Data**
   - Open browser and visit: `http://localhost:8989`
   - View combat data statistics in real-time

## 📱 Web Interface Features

### Data Display

- **Character ID** - Player character identifier
- **Total Damage/Healing** - Cumulative total damage/healing dealt
- **Damage Classification** - Detailed classification including pure critical, pure lucky, critical-lucky, etc.
- **Critical Rate/Lucky Rate** - Critical and lucky trigger probability in combat
- **Instantaneous DPS/HPS** - Current second's damage/healing output
- **Maximum Instantaneous** - Historical highest instantaneous output record
- **Total DPS/HPS** - Overall average output efficiency

### Operation Features

- **Clear Data** - Reset all statistics
- **Theme Switch** - Toggle between light/dark mode
- **Auto Refresh** - Automatically update data every 100ms

## 🛠️ Technical Architecture

### Core Dependencies

- **[cap](https://github.com/mscdex/cap)** - Network packet capture
- **[express](https://expressjs.com/)** - Web server framework
- **[protobufjs](https://github.com/protobufjs/protobuf.js)** - Protocol Buffers parsing
- **[winston](https://github.com/winstonjs/winston)** - Logging management

## 📡 API Endpoints

### GET /api/data

Get real-time combat data statistics

**Response Example:**

```json
{
  "code": 0,
  "user": {
    "114514": {
      "realtime_dps": 0,
      "realtime_dps_max": 3342,
      "total_dps": 451.970764813365,
      "total_damage": {
        "normal": 9411,
        "critical": 246,
        "lucky": 732,
        "crit_lucky": 0,
        "hpLessen": 8956,
        "total": 10389
      },
      "total_count": {
        "normal": 76,
        "critical": 5,
        "lucky": 1,
        "total": 82
      },
      "realtime_hps": 4017,
      "realtime_hps_max": 11810,
      "total_hps": 4497.79970662755,
      "total_healing": {
        "normal": 115924,
        "critical": 18992,
        "lucky": 0,
        "crit_lucky": 0,
        "hpLessen": 0,
        "total": 134916
      },
      "taken_damage": 65,
      "profession": "Healing"
    }
  },
  "enemy": {
    "15395": {
      "name": "Thunder Ogre",
      "hp": 18011262,
      "max_hp": 18011262
    }
  }
}
```

### GET /api/clear

Clear all statistics data

**Response Example:**

```json
{
  "code": 0,
  "msg": "Statistics have been cleared!"
}
```

### GET /api/enemies

Get enemy data

**Response Example:**

```json
{
  "code": 0,
  "enemy": {
    "15395": {
      "name": "Thunder Ogre",
      "hp": 18011262,
      "max_hp": 18011262
    }
  }
}
```

## Other APIs can be viewed in the [source code](server.js)

## 🔧 Troubleshooting

### Common Issues

1. **Cannot detect game server**
   - Check if the network device selection is correct
   - Confirm the game is running and connected to the server
   - Try going to a less crowded area on the same map

2. **Web interface cannot be accessed**
   - Check if port 8989 is occupied
   - Confirm firewall settings allow local connections

3. **Abnormal data statistics**
   - Check log output for error messages
   - Try restarting the program to recapture data

4. **cap module compilation error**
   - Ensure Visual Studio Build Tools and Python are installed
   - Confirm Node.js version meets requirements

5. **Program exits immediately after startup**
   - Ensure Npcap is installed
   - Confirm the correct numeric identifier was entered for network device selection

## 📄 License

[![](https://www.gnu.org/graphics/agplv3-with-text-162x68.png)](LICENSE)

This project is licensed under [GNU AFFERO GENERAL PUBLIC LICENSE version 3](LICENSE).

By using this project, you agree to comply with the terms of this license.

### Regarding Derivative Software

- If you modify the source code and republish it, you must prominently credit this project.
- If you reference the internal implementation (such as server identification, protocol parsing, data processing, etc.) and publish another project, you must prominently credit this project.

If you do not agree with this license and additional terms, please do not use this project or view the related code.

## 👥 Contributing

Issues and Pull Requests are welcome to improve the project!

### Contributors List

[![Contributors](https://contrib.rocks/image?repo=dmlgzs/StarResonanceDamageCounter)](https://github.com/dmlgzs/StarResonanceDamageCounter/graphs/contributors "Contributors")

## ⭐ Support

If this project helps you, please give it a Star ⭐

---

**Disclaimer**: This tool is for game data analysis and learning purposes only and must not be used for any behavior that violates the game's terms of service. Users are responsible for any related risks. The project developer is not responsible for any malicious combat power discrimination behavior by others using this tool. Please ensure compliance with the game community's relevant regulations and ethical standards before use.
