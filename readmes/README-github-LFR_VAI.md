# github — LFR VAI CODE (Full Competition Firmware Bundle)

> Complete Line Follower + Object Grabber codebase for “LFR VAI” — line follow, sonar avoid, LCD, calibration, and grab-release.

![Language](https://img.shields.io/badge/language-C%2B%2B-blue) ![Board](https://img.shields.io/badge/board-Arduino%20Mega-orange) ![Modules](https://img.shields.io/badge/modules-LCD%2BSonar%2BGrabber-green)

**Repo:** `Orpon-chanda/github` • 2020-10-09 (oldest in profile) • 11 sketches, 1 zip

Files: `line_follow.ino`, `Check.ino`, `Motor.ino`, `Battery_Check.ino`, `Calibration.ino`, `IRO.ino`, `I2C`, `Lcd_Display`, `Debug`, `grab`, `switch`, `IRO.zip`

---

## Overview

This is the legacy full-stack LFR firmware predating `MindSpare1` and `PID_LFR_LU`. It includes everything needed for a competition run that involves line following *plus* grabbing an object (sonar-triggered `grab()`) and handling intersections.

Repo is named `github` (generic) but content is `LFR VAI CODE`.

## Features

- 6-sensor line follow with `check()` + `sum` logic
- `line_follow()` with `turn`/`cross` handling, 90° recovery, lost-line search (300 ms timeout)
- Sonar pre-check: `if (sonarf < 6cm && grabber==0) grab();`
- Left/right indicator LEDs (`rli`, `lli`, `mli`)
- Battery check, calibration, LCD status, debug serial

## Getting Started

```bash
git clone https://github.com/Orpon-chanda/github.git
cd github
# Open folder in Arduino IDE — all .ino files are tabs of one sketch
# Board: Mega 2560, libs: NewPing, LiquidCrystal_I2C
```

Rename locally to `LFR_VAI_CODE` for clarity.

## Notes

- This codebase is the predecessor to `MindSpare1.ino` — many patterns (turn==1/2, s[2]/s[3] polling, `motor(10*spl, -10*spr)`) reappear there.
- `IRO.zip` likely contains IR sensor test assets — unzip and inspect.
- Consider archiving this repo and pointing README to `PID_LFR_LU` as the current version.

## Contact

Orpon Chanda — Lab Assistant, Leading University, Sylhet

---
*Generated README — original was just “LFR VAI CODE”.*
