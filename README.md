# ATmega328P Datalogger PCB

A custom-designed data logging PCB built around the ATmega328P microcontroller, featuring real-time clock functionality and expanded non-volatile storage. Designed in KiCad 9.0 as a hands-on PCB design project, covering schematic capture, layout, routing, and design rule verification across two board variants.

## Overview

This project is a compact datalogger board with:
- **MCU:** ATmega328P
- **RTC:** DS3231 (high-precision real-time clock)
- **Storage:** 512KB EEPROM
- **Connectors:** ICSP header, GPIO breakout, serial header, power connectors
- **Crystal oscillators:** 16MHz (MCU clock) + 32.768kHz (RTC)
- **Indicators:** Onboard LEDs for status feedback

The board was developed in two layout variants — a 2-layer version and a 4-layer version — to explore how additional copper layers affect power/ground plane integrity and routing density.

## Repository Structure

This repo uses a branch-per-layout strategy:

- **`main`** — canonical schematic, project files, and documentation. Does not contain a finalized PCB layout (see branches below for actual board files).
- **`2-layer`** — 2-layer PCB layout and routing.
- **`4-layer`** — 4-layer PCB layout and routing.

Schematic changes are made on `main` and pulled into each layout branch via `Update from main`. PCB layout files are kept separate per branch to avoid merge conflicts between the two distinct board designs.

## Renders

**4-Layer Board:**
![4-layer render](docs/4%20layer%20render%20.png)

**2-Layer Board:**
![2-layer render](docs/2%20layer%20render%20datalogger.png)

*(Renders exported from KiCad's 3D Viewer, available on their respective branches.)*

## Design Verification

### Schematic (ERC)
✅ **0 errors, 0 warnings** — verified clean on `main`.

### 4-Layer PCB (DRC)
✅ **0 errors** — fully clean. Dedicated internal power/ground plane layers provided sufficient copper for proper thermal relief connections across all pads.

### 2-Layer PCB (DRC)
⚠️ **4 known errors, 32 warnings (unresolved by design choice)**

- **4 thermal relief / isolated island errors** on GND and VCC zones (affecting J1, J2, R2 pads). Root cause: tight routing clearance between a VCC trace and nearby GND/VCC pads pinched off zone copper pour at those specific points. Attempted fixes (zone re-fill, hatch-to-solid fill type change, minor rerouting) did not fully resolve the geometry constraint.
- **32 warnings** — primarily silkscreen text height falling under the board's configured minimum (0.7mm actual vs 0.8mm constraint) and one silkscreen overlap. Cosmetic only.

**Why these remain unresolved:** This board is not intended for fabrication — it served as the 2-layer baseline for comparison against the 4-layer design. The 4-layer board's clean DRC result directly demonstrates how added plane layers resolve the exact grounding constraint the 2-layer version hit. This is intentionally documented here as a real finding from the design process rather than hidden.

## Tools Used

- KiCad 9.0 (schematic capture, PCB layout, 3D visualization)
- Git / GitHub Desktop (version control)

## Getting Started

To explore a specific board layout:
```bash
git clone https://github.com/bhatnagarpakhi-eng/atmega328p-datalogger.git
cd atmega328p-datalogger
git checkout 4-layer   # or 2-layer
```
Open `mcu datalogger.kicad_pro` in KiCad 9.0 or later.

## Author

Pakhi Bhatnagar
[GitHub](https://github.com/bhatnagarpakhi-eng) · [LinkedIn](https://linkedin.com/in/pakhi-bhatnagar-a49428306)