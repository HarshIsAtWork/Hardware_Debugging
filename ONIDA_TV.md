# ONIDA 24-inch LCD TV Repair Log

**Status:** 🟡 Partially Repaired
**Time Invested:** ~10 Hours
**Objective:** Repair a scrap TV while practicing hardware debugging techniques.

---

## Background

> [!NOTE]
> I wanted to improve my hardware debugging skills by repairing discarded electronics instead of simply salvaging components.

I asked a few neighbors if they had any broken electronics lying around. One of them handed me an **ONIDA 24-inch LCD TV** and said:

> *"If you can fix it, great. If not, it's yours anyway."*

Challenge accepted.

---

# Initial Symptoms

After connecting the TV to mains power:

* 🔴🔵 Front LED continuously blinked between **Red** and **Blue**
* ❌ No display
* ❌ No sound
* ⚠️ High-frequency chirping noise from the SMPS board

> [!WARNING]
> The chirping initially sounded like electrical arcing, but was later identified as the SMPS repeatedly attempting to start (hiccup mode).

---

# Safety Precautions

> [!CAUTION]
> The primary capacitor inside an SMPS can retain dangerous voltages even after the TV is unplugged. ALways Safely Discharge the Main Capacitor!!

Before any testing I:

* Unplugged the TV
* Discharged the primary capacitor using a load a 2w resistor 
* Used a multimeter to verify safe voltage before handling the board

---

# SMPS Diagnosis

## Bridge Rectifier

### Method

Measured every internal diode using the multimeter in **Diode Mode**.

**Expected**

* Forward voltage: ~0.5–0.7V
* Reverse: OL

**Result**

All four diodes measured correctly.

✅ **Verdict:** Good

---

## Fuse

### Method

Checked continuity across the fuse.

**Result**

Continuous beep.

✅ **Verdict:** Good

---

## Main Filter Capacitor

### Method

* Visually inspected for bulging or leakage.
* Measured capacitance using the multimeter.

**Observed**

* Rated: **68µF 450V**
* Measured: **61µF**

Within normal tolerance.

✅ **Verdict:** Good

---

## Primary MOSFET

### Method

Identified the MOSFET using its datasheet and tested it in **Diode Mode**.

Checked:

* Body diode
* Drain-Source short

**Observed**

Body diode behaved normally and no short was detected.

✅ **Verdict:** Likely Good (tested in-circuit)

---

## Optocoupler

> [!IMPORTANT]
> This was the **first faulty component** I successfully diagnosed.

### Method

Desoldered the optocoupler and tested both sides.

**LED Side (Pins 1-2)**

Expected:

* Forward ≈1V
* Reverse = OL

Measured:

* 0.8V one way
* 1.0V reverse

The reverse reading indicated leakage.

**Phototransistor Side (Pins 3-4)**

Expected:

* OL in both directions

Measured:

Unexpected conduction.

To verify my testing method, I repeated the same measurements on a known-good spare optocoupler.

The spare behaved exactly as expected.

❌ **Verdict:** Faulty

---

# Repair Attempt

I replaced the faulty optocoupler with the known-good spare.

After soldering it back:

* Still blinking Red/Blue
* Chirping unchanged
* TV still wouldn't boot

> [!NOTE]
> The optocoupler was genuinely faulty, but it wasn't the only fault on the power supply.

---

# Changing Strategy

Instead of continuing to blindly troubleshoot the SMPS without a schematic, I decided to isolate it completely.

> [!REALISATION]
> If the main board runs on 12v, I could just bypass the main SMPS board Entirely.

---

# Powering the Mainboard Directly

The mainboard exposed clearly labelled:

* +12V
* GND
* UART (RX/TX)

Using a **12V 2A bench power supply**, I bypassed the faulty SMPS and powered the mainboard directly.

### Observations

* ✅ Processor became warm
* ✅ Logic rails measured correctly (5V & 12V)
* ✅ Front LED changed from **Red → Solid Blue**
* ✅ LCD panel became active

This proved the mainboard was functional.

---

# LCD Panel Investigation

With external power applied, the LCD displayed a transparent grey image with **no backlight**.

> [!IMPORTANT]
> The LCD glass itself was working.

The issue had moved from **"dead TV"** to **"working panel with no illumination."**

---

# Backlight Investigation

Assuming the LEDs had failed, I completely disassembled the LCD panel and replaced the entire LED backlight strip.

### Result

❌ No improvement.

The replacement LEDs also failed to illuminate.

This suggested the problem was likely the **backlight driver circuitry** or the **BL_EN/PWM control signal**, not the LEDs themselves.

---

# Current Status

| Component        | Status                    |
| ---------------- | ------------------------- |
| SMPS             | ❌ Faulty                  |
| Fuse             | ✅ Good                    |
| Bridge Rectifier | ✅ Good                    |
| Main Capacitor   | ✅ Good                    |
| Primary MOSFET   | ✅ Likely Good             |
| Optocoupler      | ✅ Replaced                |
| Mainboard        | ✅ Working on external 12V |
| LCD Panel        | ✅ Working                 |
| LED Backlight    | ✅ Replaced                |
| Backlight Driver | 🔍 Suspected Fault        |

---

# Future Work

* [ ] Capture UART boot logs
* [ ] Trace the BL_EN signal
* [ ] Check PWM brightness control
* [ ] Investigate the backlight driver
* [ ] Continue tracing the original SMPS fault

---

# What I Learned

This project taught me far more than simply replacing components.

* Reading datasheets to identify unknown parts
* Diagnosing SMPS circuits using only a multimeter
* Testing bridge rectifiers, MOSFETs and optocouplers
* Bench powering electronics safely
* Isolating faults by dividing a system into subsystems
* Making decisions based on measurements instead of assumptions

> [!TIP]
> Although the original TV is still not fully repaired, I successfully proved that the LCD panel and mainboard are functional, isolated the SMPS as the primary fault, and gained valuable experience in systematic hardware debugging.
