# Arduino Currency Counter — IR + TCS3200 Color Sensor (Taka Counter)

> Automatic Taka note counter that detects 10 Taka (red) and 100 Taka (blue) via TCS3200 and counts with IR break-beam. LCD displays remaining balance.

![Language](https://img.shields.io/badge/language-C%2B%2B-blue) ![Board](https://img.shields.io/badge/board-Arduino%20Uno-orange) ![Sensors](https://img.shields.io/badge/sensors-TCS3200%20%2B%20IR-green) ![Display](https://img.shields.io/badge/display-16x2%20I2C%20LCD-lightgrey)

**Repo:** `Orpon-chanda/Arduino_Currency_Counter_using_IR_and_Color_Sensor` • 2021-05-13 • “Taka counter”

---

## Overview

Feed notes past an IR gate; the TCS3200 reads color frequency. Blue frequency `70–100` → 100 Taka, red `30–50` → 10 Taka. Each note passing the IR sensor decrements a `total` balance (starts 1000) and flashes the denomination on LCD.

## Hardware

- Arduino Uno/Nano
- TCS3200 color sensor (S0 D2, S1 D3, S2 D11, S3 D12, OUT D13)
- IR sensor (digital, A0) as note gate
- 16×2 I2C LCD `0x27` (SDA A4, SCL A5)
- Breadboard, 5V supply

From sketch:

```cpp
pinMode(2,OUTPUT); // S0
pinMode(3,OUTPUT); // S1
pinMode(11,OUTPUT);// S2
pinMode(12,OUTPUT);// S3
pinMode(13,INPUT); // OUT (pulseIn)
digitalWrite(2,HIGH); digitalWrite(3,LOW); // 20% scaling
```

## Getting Started

```bash
git clone https://github.com/Orpon-chanda/Arduino_Currency_Counter_using_IR_and_Color_Sensor.git
open Arduino_Currency_Counter_using_IR_and_Color_Sensor/Arduino_Currency_Counter_using_IR_and_Color_Sensor.ino
# Arduino IDE: Uno, install LiquidCrystal_I2C + Wire
# Upload, open Serial 9600
```

## Wiring

| Module | Arduino |
|--------|---------|
| TCS3200 VCC/GND | 5V/GND |
| S0/S1/S2/S3 | D2/D3/D11/D12 |
| OUT | D13 |
| IR OUT | A0 |
| LCD SDA/SCL | A4/A5 |

## Usage

1. Power on — LCD shows “Smart Wallet / Circuit Digest”, then `Total Bal:1000`.
2. Slide a note through: IR HIGH → color read → IR LOW → denomination latched → LCD shows `100 TAKA!!!` or `10 TAKA!!!` 1.5 s, `total` decremented.
3. Serial prints `red1` / `blue1` frequencies for tuning.

Tune thresholds in `loop()` for your lighting/notes:

```cpp
if (blue1 >=70 && blue1 <=100 ...) // 100 Taka
if (red1  >=30 && red1  <=50 ...)  // 10 Taka
```

Adjust, re-upload, test under consistent light (box the sensor).

## Code Overview

- `red()` / `blue()` / `green()` — set S2/S3, `pulseIn(OUT,LOW)` → frequency.
- Two-state edge latch (`a`, `b`) debounces IR gate.
- `total` starts 1000; guard `if (total>=10)` prevents negative.

## Improvements

- [ ] Add all denominations (5/20/50/500) via calibrated RGB table
- [ ] Add buzzer + EEPROM total
- [ ] Enclose sensor in dark tunnel for stable readings
- [ ] Add count-up mode and reset button

## Contact

Orpon Chanda — Lab Assistant, Leading University, Sylhet — [@Orpon-chanda](https://github.com/Orpon-chanda)

---
*Generated README — original was one line “Taka counter”. Thresholds quoted from sketch as of 2021.*
