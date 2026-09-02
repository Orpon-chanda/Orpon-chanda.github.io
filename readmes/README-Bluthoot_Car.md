# Bluthoot_Car — Bluetooth Controlled Car (Arduino)

> Simple 4-motor Bluetooth car driven via Serial commands (F/B/L/R/S). Arduino + HC-05 + motor driver.

![Language](https://img.shields.io/badge/language-C%2B%2B-blue) ![Board](https://img.shields.io/badge/board-Arduino%20Uno%2FNano-orange) ![Module](https://img.shields.io/badge/module-HC--05-blue) ![License](https://img.shields.io/badge/license-MIT-lightgrey)

**Author:** Orpon Chanda — Lab Assistant, Leading University, Sylhet  
**Repo:** `Orpon-chanda/Bluthoot_Car` • Created 2021-04-01 • `Bluthoot_Car.ino` (single sketch)

---

## Overview

A minimal Bluetooth car: phone sends single-char commands over HC-05 Serial, Arduino drives four motor pins. Ideal as a starter robotics lab demo.

**Commands:** `F` forward, `B` backward, `R` right, `L` left, `S` stop.

## Hardware

- Arduino Uno / Nano
- HC-05 Bluetooth module (TX→D0/RX→D1 or SoftwareSerial; here uses `Serial` at 9600 baud)
- L298N dual motor driver (or L293D)
- 4× DC geared motors / 2-wheel chassis with motor extension
- 7.4V Li-ion / 9V battery
- Jumper wires, chassis

Wiring (from sketch):

| Arduino Pin | Function |
|-------------|----------|
| D2 | Left motor forward (`lmf`) |
| D3 | Left motor backward (`lmb`) |
| D4 | Right motor forward (`rmf`) |
| D5 | Right motor backward (`rmb`) |
| 5V / GND | HC-05 VCC/GND |
| D0 (RX), D1 (TX) | HC-05 TX/RX (via voltage divider on RX) |

> For a more stable build, move HC-05 to `SoftwareSerial` on D10/D11 to free hardware Serial for debugging.

## Circuit

```
[Phone] --Bluetooth--> [HC-05] --Serial 9600--> [Arduino] --D2..D5--> [L298N] --> Motors
                                            5V/GND ------> HC-05, L298N logic
```

## Getting Started

```bash
git clone https://github.com/Orpon-chanda/Bluthoot_Car.git
cd Bluthoot_Car
# Open Bluthoot_Car.ino in Arduino IDE
# Board: Arduino Uno, Port: /dev/ttyUSB0, Baud: 9600
# Upload, pair HC-05 (default PIN 1234), connect with Bluetooth Terminal app
```

## Usage

1. Power the car, pair phone to `HC-05` (1234 / 0000).
2. Open **Bluetooth Terminal** / **Arduino Bluetooth Controller** app.
3. Send: `F` → forward, `B` → back, `L`/`R` → spin, `S` → stop. Each char triggers `forward()`, `backward()`, etc. in the sketch.

Demo sequence: `F` 2 sec → `S` → `R` 1 sec → `S`.

## Code Overview

```cpp
// Bluthoot_Car.ino — 40 lines
loop() {
  if (Serial.available()) command = Serial.read();
  if (command=='F') forward();   // lmf=1, rmf=1
  else if (command=='B') backward();
  else if (command=='R') right(); // differential
  else if (command=='L') left();
  else if (command=='S') stop1();
}
```

No speed control (digitalWrite only). Add `analogWrite` on EN pins for PWM speed.

## Improvements

- [ ] Add `SoftwareSerial` + PWM speed (`analogWrite` 0-255)
- [ ] Add obstacle avoidance (HC-SR04) fallback
- [ ] Add battery low indicator

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Car moves opposite | Swap `lmf`/`lmb` wires |
| No Serial response | Check baud 9600, HC-05 AT mode off, correct TX/RX crossover |
| Motors hum, no spin | Battery current low — use 2S Li-ion, not 9V PP3 |

## License

Add `MIT` license file to allow reuse.

## Contact

Orpon Chanda — Lab Assistant, Leading University, Sylhet — GitHub [@Orpon-chanda](https://github.com/Orpon-chanda)

---
*Generated README — original repo contained only the sketch without docs.*
