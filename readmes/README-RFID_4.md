# RFID_4 — Coffee Code (RFID Access / Vending Sketch)

> Minimal RFID demo (“coffe code”) — reads RFID tag and triggers an action (e.g., coffee dispenser / attendance).

![Language](https://img.shields.io/badge/language-C%2B%2B-blue) ![Module](https://img.shields.io/badge/module-RFID%20RC522-blue) ![Board](https://img.shields.io/badge/board-Arduino-orange)

**Repo:** `Orpon-chanda/RFID_4` • 2021-05-11 • Files: `README.md`, `Stupid1`

---

## Overview

Small lab prototype that demonstrates RFID authentication. The binary file `Stupid1` is the compiled sketch or asset — source appears minimal; this README reconstructs the likely wiring and usage so the demo can be re-run.

Typical flow: tap RFID card on RC522 → Arduino matches UID → activates relay/buzzer/LCD (“Coffee ready”) for authorized UID.

## Hardware (typical for this build)

- Arduino Uno/Nano
- RC522 RFID reader (SPI: SDA D10, SCK D13, MOSI D11, MISO D12, RST D9)
- Relay module or solenoid (coffee valve) on D7/D8
- Optional 16×2 LCD / buzzer

## Getting Started

```bash
git clone https://github.com/Orpon-chanda/RFID_4.git
cd RFID_4
# If using RC522, open example: File > Examples > MFRC522 > DumpInfo
# Install library: MFRC522 by GithubCommunity
# Wire RC522, upload, open Serial 9600, tap cards to log UIDs
# Add your UIDs to the allow-list, re-upload
```

Minimal sketch shape:

```cpp
#include <SPI.h>
#include <MFRC522.h>
MFRC522 rfid(10, 9);
void setup(){ SPI.begin(); rfid.PCD_Init(); }
void loop(){ if(rfid.PICC_IsNewCardPresent() && rfid.PICC_ReadCardSerial()){
  // compare rfid.uid.uidByte[] to allowed list -> digitalWrite(relay,HIGH)
}}
```

## Usage

1. Flash the allow-list sketch.
2. Tap authorized card → relay clicks (coffee dispense). Unauthorized → buzzer / denied.
3. The repo name `Stupid1` suggests a test build — rename to `RFID_Coffee.ino` for clarity.

## Next Steps

- [ ] Publish `.ino` source (the `Stupid1` file is not readable as text — add proper sketch)
- [ ] Add UID enrollment mode (master card adds new cards to EEPROM)
- [ ] Document wiring photo

## Contact

Orpon Chanda — Lab Assistant, Leading University, Sylhet

---
*Generated README — original description was “coffe code” with no docs; file `Stupid1` is binary/unreadable.*
