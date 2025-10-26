# Cam32esp - ESP32-CAM Module

ESP32 Camera Module for PBL5 project. This module uses ESP32-CAM for video streaming and image processing.

## 📋 Table of Contents

- [System Requirements](#system-requirements)
- [Installation](#installation)
  - [Installing Visual Studio 2022](#1-installing-visual-studio-2022)
  - [Installing Arduino IDE Extension](#2-installing-arduino-ide-extension-for-visual-studio)
  - [Configuring ESP32-CAM](#3-configuring-esp32-cam)
- [Clone and Run Project](#clone-and-run-project)
- [Hardware Configuration](#hardware-configuration)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)

---

## 🖥️ System Requirements

- **OS**: Windows 10/11 (64-bit)
- **Visual Studio**: 2022 (Community/Professional/Enterprise)
- **RAM**: Minimum 4GB (recommended 8GB+)
- **Hardware**: 
  - ESP32-CAM module
  - USB to TTL converter (FTDI, CP2102, CH340, etc.)
  - OV2640 Camera (usually included with ESP32-CAM)

---

## 📦 Installation

### 1. Installing Visual Studio 2022

If you don't have Visual Studio 2022:

1. Download Visual Studio 2022 Community (free):
   - Link: https://visualstudio.microsoft.com/vs/

2. During installation, select workloads:
   - ✅ **Desktop development with C++** (for Arduino extension)
   - ✅ **.NET desktop development** (if working with C#)

3. Install and restart your computer if needed

---

### 2. Installing Arduino IDE Extension for Visual Studio

#### Step 1: Open Visual Studio 2022

#### Step 2: Install Extension
1. Go to menu: **Extensions** → **Manage Extensions**
2. Search for: **"Arduino IDE for Visual Studio"**
3. Click **Download** extension:
   - **Arduino IDE for Visual Studio 2022** (by Visual Micro)
   
4. **Close Visual Studio** to install the extension
5. The installer will run automatically → Click **Modify** to install
6. Reopen Visual Studio after installation completes

#### Step 3: Activate Arduino Extension
1. After reopening VS, you'll see a new menu bar: **vMicro**
2. If not visible → Go to **Extensions** → **vMicro** → **Visual Micro Explorer**

---

### 3. Configuring ESP32-CAM

#### Step 1: Install ESP32 Board Package

1. In Visual Studio, click **vMicro** → **Board Manager**
2. Or go to: **Tools** → **Board** → **Board Manager**
3. Search for: **esp32**
4. Install: **esp32 by Espressif Systems** (latest version)
5. Wait for installation to complete (may take a few minutes)

#### Step 2: Select ESP32-CAM Board

1. Go to menu: **vMicro** → **Board**
2. Select: **ESP32 Arduino** → **AI Thinker ESP32-CAM**

   Or via toolbar:
   - Click "Select Board" dropdown
   - Find and select: **AI Thinker ESP32-CAM**

#### Step 3: Configure Port

1. Connect ESP32-CAM to computer via USB-TTL converter
2. Go to: **vMicro** → **Port**
3. Select the corresponding COM port (e.g., COM3, COM4, ...)
4. If no port is visible:
   - Install CH340/CP2102 driver: [Driver link](https://sparks.gogo.co.nz/ch340.html)
   - Check Device Manager → Ports (COM & LPT)

#### Step 4: Configure Upload Settings

In Visual Studio, configure as follows:

- **Board**: AI Thinker ESP32-CAM
- **Upload Speed**: 115200
- **Flash Frequency**: 80MHz
- **Flash Mode**: QIO
- **Partition Scheme**: Huge APP (3MB No OTA/1MB SPIFFS)
- **Core Debug Level**: None

To change: **vMicro** → **Configuration** → select the options above

---

## 🚀 Clone and Run Project

### Step 1: Clone Repository

Open **Command Prompt** or **Git Bash**:

```bash
# Clone to your machine
git clone https://github.com/PBL4-WEBCAM/Cam32esp.git

# Navigate to directory
cd Cam32esp
```

### Step 2: Open Project in Visual Studio

1. Open Visual Studio 2022
2. **File** → **Open** → **Folder**
3. Select the `Cam32esp` folder you just cloned
4. Visual Studio will automatically recognize the Arduino project

   **OR** if there's an `.ino` file:
   - **File** → **Open** → **File**
   - Select the `Cam32esp.ino` file

### Step 3: Configure WiFi

Open the main file (`.ino` or `main.cpp`) and edit WiFi credentials:

```cpp
const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";
```

### Step 4: Build Project

1. Click menu: **vMicro** → **Build** (or `Ctrl + Shift + B`)
2. View compilation results in the **Output** window
3. If successful, you'll see: `Compilation complete`

### Step 5: Upload to ESP32-CAM

⚠️ **IMPORTANT**: To enter upload mode:

1. **Connect GPIO 0 pin to GND** on ESP32-CAM
2. Press the **RESET** button on the board
3. Board enters upload mode (bootloader mode)

Then:

1. Click: **vMicro** → **Upload** (or `Ctrl + Shift + U`)
2. Wait for upload to complete (progress bar will show)
3. When done, you'll see: `Hard resetting via RTS pin...`
4. **Disconnect GPIO 0 from GND**
5. Press **RESET** to run the program

### Step 6: View Serial Monitor

1. **vMicro** → **Serial Monitor**
2. Select **Baud Rate**: 115200
3. Press **RESET** on ESP32-CAM
4. You should see:
   ```
   WiFi connected
   IP Address: 192.168.1.xxx
   Camera Ready! Use 'http://192.168.1.xxx' to connect
   ```

### Step 7: Access Camera Stream

1. Copy the IP address displayed in Serial Monitor
2. Open a web browser
3. Navigate to: `http://192.168.1.xxx`
4. View the video stream from camera! 🎥

---

## 🔧 Hardware Configuration

### Connecting ESP32-CAM to USB-TTL Converter

| ESP32-CAM | USB-TTL (FTDI) |
|-----------|----------------|
| 5V        | 5V (or VCC)    |
| GND       | GND            |
| U0R (RX)  | TX             |
| U0T (TX)  | RX             |
| GPIO 0    | GND (only when uploading) |

⚠️ **Note**:
- RX of ESP32 connects to TX of FTDI
- TX of ESP32 connects to RX of FTDI
- GPIO 0 → GND only when uploading, must disconnect after

### Connection Diagram

```
ESP32-CAM                    USB-TTL (FTDI)
┌─────────┐                 ┌──────────┐
│   5V    │────────────────→│   5V     │
│   GND   │────────────────→│   GND    │
│   U0R   │←────────────────│   TX     │
│   U0T   │────────────────→│   RX     │
│  GPIO0  │─────┐            
└─────────┘     │
                │
               GND (only when uploading)
```

---

## 📖 Usage

### API Endpoints (if available)

```
GET /                    - Camera stream homepage
GET /capture             - Capture photo
GET /stream              - Video stream
POST /control?var=value  - Control camera settings
```

### Usage Examples

```bash
# View stream
http://192.168.1.100/

# Capture photo
http://192.168.1.100/capture

# Change brightness
http://192.168.1.100/control?var=brightness&val=2
```

---

## 🐛 Troubleshooting

### 1. Compilation Failed

**Error**: `Arduino.h: No such file or directory`

**Fix**:
- Ensure Arduino extension is installed
- Select correct board: AI Thinker ESP32-CAM
- Reinstall ESP32 board package

---

### 2. Upload Failed

**Error**: `A fatal error occurred: Failed to connect to ESP32`

**Fix**:
- Check wire connections: RX-TX, TX-RX
- **Remember to connect GPIO 0 to GND** before uploading
- Press RESET button before uploading
- Try reducing Upload Speed to 115200
- Check CH340/CP2102 driver installation

---

### 3. COM Port Not Visible

**Fix**:
- Install driver: https://sparks.gogo.co.nz/ch340.html
- Try a different USB cable
- Check Device Manager → Ports (COM & LPT)
- Reconnect USB

---

### 4. Camera Failed to Initialize

**Error**: `Camera init failed with error 0x20001`

**Fix**:
- Check if camera is firmly plugged into slot
- Try providing stable 5V/2A power supply
- Check if OV2640 camera is damaged

---

### 5. WiFi Connection Failed

**Fix**:
- Check SSID and password in code
- Ensure WiFi is 2.4GHz (ESP32 doesn't support 5GHz)
- Try setting static IP instead of DHCP
- Check if router is blocking MAC address

---

### 6. Serial Monitor Shows Nothing

**Fix**:
- Select correct baud rate: 115200
- Select correct COM port
- Press RESET button on ESP32-CAM
- Check if TX-RX wires are correct
