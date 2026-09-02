# PWM — AC Dimmer using Arduino (Phase-Angle Control)

> Single-channel AC dimmer / fan regulator using zero-cross detection and TRIAC phase control. Potentiometer sets firing delay.

![Language](https://img.shields.io/badge/language-C%2B%2B-blue) ![Board](https://img.shields.io/badge/board-Arduino-orange) ![Control](https://img.shields.io/badge/control-TRIAC%20%2B%20ZVC-orange) ![Status](https://img.shields.io/badge/status-demo-yellow)

**Author:** Orpon Chanda (Robot Man OC/SPI) — Lab Assistant, Leading University, Sylhet  
**Repo:** `Orpon-chanda/PWM` • Created 2021-05-07 • Sketch: `AC_Dimmer_Using_Arduino.ino` + `dimar mu.jpg`

---

## Overview

This project dims an AC lamp / controls AC fan speed by detecting the AC zero-cross and firing a TRIAC after a variable delay `x` set by a potentiometer on `A0`. It is a classic lab demo for power electronics and Arduino interrupts.

> **Safety warning:** This circuit switches mains AC (220 V). Build only under supervision, use proper isolation (optocoupler MOC3021 / 4N25), fuse, and insulated enclosure. Mains can kill.

## How It Works

1. **Zero-cross detector** on digital pin `D2` (interrupt 0) goes FALLING at each AC zero crossing.
2. `attachInterrupt(0, acon, FALLING)` triggers `acon()` ISR.
3. `acon()` waits `delayMicroseconds(x)` (1–8500 µs from pot), then pulses TRIAC gate on `D10` for 50 µs → TRIAC conducts for the remainder of the half-cycle → lamp brightness changes.

```
Pot A0 (0-1023) --map--> x (1-8500 us) --delay--> TRIAC pulse (D10) --> AC Load
ZVC (D2) --interrupt FALLING--> acon()
```

## Hardware

- Arduino Uno/Nano
- Zero-cross detector module (H11AA1 / 4N25 + bridge) → D2
- Opto-isolated TRIAC driver MOC3021 + BT136 TRIAC → D10
- 10k potentiometer → A0, 5V, GND
- AC lamp / fan as load (with snubber for inductive loads)
- **Isolation, fuse, proper PCB / veroboard**

Pins in sketch:

| Pin | Role |
|-----|------|
| D2 | ZVC input (`INPUT_PULLUP`) |
| A0 | Potentiometer |
| D10 | TRIAC gate pulse |

## Getting Started

```bash
git clone https://github.com/Orpon-chanda/PWM.git
cd PWM
# Open AC_Dimmer_Using_Arduino.ino in Arduino IDE
# Upload to Uno at 9600 baud
# Power ZVC + TRIAC stage from isolated supply, load via TRIAC
```

## Usage

1. Wire ZVC output to D2, TRIAC driver to D10, pot to A0.
2. Upload sketch, open Serial Monitor (9600) — prints `digitalRead(ZVC)`.
3. Rotate potentiometer: `x = map(y,0,1024,1,8500)` → lamp smoothly dims. Low `x` = bright, high `x` = dim.

## Code Notes

```cpp
#define triacPulse 10
#define ZVC 2
int x = map(analogRead(A0),0,1024,1,8500);
attachInterrupt(0, acon, FALLING);
void acon(){ delayMicroseconds(x); digitalWrite(triacPulse,HIGH); delayMicroseconds(50); digitalWrite(triacPulse,LOW); }
```

> `attachInterrupt` inside `loop()` and `delayMicroseconds` inside an ISR are not ideal — ISR should be minimal. A more robust version uses a flag + timer or `Timer1`.

## Suggested Improvements

- Move `attachInterrupt` to `setup()`; use `volatile` for `x`; avoid `delayMicroseconds` in ISR — use `micros()` compare in loop.
- Add second channel for dual-lamp dimmer.
- Add serial command for brightness 0-100% and smooth fade.

## Safety Checklist

- [ ] Optocoupler between Arduino and TRIAC (never direct gate drive)
- [ ] Fuse on AC live, proper earth, insulated case
- [ ] Snubber (RC) for fan/inductive load
- [ ] Never touch circuit while mains connected

## License

No license — add MIT if you want reuse.

## Contact

Orpon Chanda — Lab Assistant, Leading University, Sylhet — [@Orpon-chanda](https://github.com/Orpon-chanda)

---
*Generated README — original description was “it is MU code for pwm signal”. Image `dimar mu.jpg` documents the build.*
