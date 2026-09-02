# MindSpare1 — LFR for MindSpare-1 Competition (6-Sensor + Sonar + Color)

> Full competition LFR firmware with 6 IR sensors, 3× ultrasonic sonars, TCS3200 color sensor, LCD, EEPROM and wall-follow logic.

![Language](https://img.shields.io/badge/language-C%2B%2B-blue) ![Board](https://img.shields.io/badge/board-Arduino%20Mega-orange) ![Sensors](https://img.shields.io/badge/sensors-IR%20%2B%20Sonar%20%2B%20Color-green) ![Comp](https://img.shields.io/badge/competition-MindSpare--1-red)

**Repo:** `Orpon-chanda/MindSpare1` • 2021-05-11 • 12 sketches (modular `.ino` tabs)

---

## Overview

MindSpare1 competed as a line follower with obstacle and color handling. The repo splits logic across Arduino IDE tabs (which build as one sketch): `MindSpare1.ino` (main) + `Line_Follow`, `Calibration`, `Motor`, `Color`, `Sonar`, `Wall`, `Button`, `Break_`, `Check`, `Debug`, `Lcd_Display`.

## Features

- **6× IR line sensors** with EEPROM calibration (`maximum[]`, `minimum[]`, `tht=120`)
- **3× HC-SR04 sonars** (front/left/right via `NewPing`) for obstacle & wall detection
- **TCS3200 color sensor** (S0–S3 on D26/D24/D30/D28, OUT D32) for color tasks
- **16×2 I2C LCD** (`0x27`) for menu/debug
- **State machine:** `turn`, `cross`, `k90`, `k30` for 90°/30° turns, T-sections
- Motor control via `spl`/`spr` speed factors, `lmf/lmb/rmf/rmb` on D2-D5

## Project Structure

```
MindSpare1/
├── MindSpare1.ino   # setup, loop, globals, pin map
├── Line_Follow.ino  # core PID-ish follow logic
├── Calibration.ino  # auto-calibrate IR thresholds
├── Color.ino        # rr/gg/bb reading via TCS3200
├── Sonar.ino        # NewPing front/left/right
├── Wall.ino         # wall-follow fallback
├── Motor.ino        # motor(l,r)
├── Button.ino       # push-button menu (D10-D12)
├── Break_.ino       # braking logic
├── Check.ino        # sensor check()
├── Debug.ino        # Serial debug
└── Lcd_Display.ino  # LCD helpers
```

## Hardware

| Part | Pin |
|------|-----|
| IR array (6) | A0–A5 (via `check()`) |
| Button / Button1 / Button2 | D10, D11, D12 |
| Motors L/R | D4/D5, D2/D3 |
| Sonar trig/echo F | D47/D49 |
| Sonar L/R (shared trig/echo) | D23/D25, D53/D51 via NewPing |
| TCS3200 S0–S3, OUT | D26,24,30,28,32 |
| LCD I2C | SDA/SCL (A4/A5) |
| Buzzer | D52 |

## Getting Started

```bash
git clone https://github.com/Orpon-chanda/MindSpare1.git
cd MindSpare1
# Open MindSpare1.ino in Arduino IDE (all tabs load together)
# Board: Arduino Mega 2560 (memory fits Mega better than Uno)
# Install libs: NewPing, LiquidCrystal_I2C, Wire, EEPROM
# Upload
```

Calibrate: hold robot over white then black, run `Calibration.ino` path (fills `maximum`/`minimum`), stored in EEPROM.

## Usage

1. Power on, select mode via buttons (menu variable).
2. Place on line, press main button to start line follow.
3. Observe LCD and serial (9600) for `s[]`, `sum`, `sonarf`.

## Notes

- Global variables (`turn`, `cross`, `k90`) drive intersection behavior — tune delays (10–300 ms) for your track.
- Color thresholds in `Color.ino` are hard-coded — recalibrate per lighting.

## Roadmap

- [ ] Convert to PlatformIO + modular headers (like PID_LFR_LU)
- [ ] PID instead of bang-bang speed
- [ ] Document wiring diagram

## Contact

Orpon Chanda — Lab Assistant, Leading University, Sylhet

---
*Generated README — original description: “LFR code for MindSpare1 compesition”.*
