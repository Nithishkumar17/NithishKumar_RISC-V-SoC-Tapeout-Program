# Week 4 — CMOS Circuit Design & SPICE Simulation  
**Day 1**

---

## Overview
Day 1 focuses on transistor-level CMOS behavior, how MOSFET W/L ratio affects delay, and how SPICE validates timing models used by STA (OpenSTA).

---

## CMOS Basics
- **PMOS**: N-type substrate, P-doping (minority P, majority N)  
- **NMOS**: P-type substrate, N-doping (minority N, majority P)  
- **W/L ratio** controls drain current → affects propagation delay.  
- Delay tables (input slew vs output load) are generated using **SPICE**, then used by **STA**.

---

## Semiconductor & PN Junction Revision
- Semiconductors become **extrinsic** when doped:
  - **P-type:** trivalent dopants (B, Al, Ga)
  - **N-type:** pentavalent dopants (P, As, Sb)
- PN junction forms **depletion region** and **built-in potential**:
  - Silicon ≈ **0.7 V**
  - Germanium ≈ **0.3 V**

### Biasing:
- **Forward bias:** reduces depletion → current flows.
- **Reverse bias:** widens depletion → blocks current (small leakage only).

---

## MOSFET Basics (NMOS Example)
- 4 Terminals: **Gate (G), Source (S), Drain (D), Body (B)**
- P-type substrate, SiO₂ gate oxide, N⁺ source/drain
- Behavior depends on **VGS** and **VDS**
- Levels of operation based on inversion/channel formation

---

## NMOS Operation Modes

### 1. Cut-Off Region (Case 0)
- \( V_{GS} < V_{th} \)
- No inversion channel
- Very high \( R_{DS} \), no conduction.

---

### 2. Threshold Formation (Case 1)
- \( V_{GS} \) increases
- Gate attracts electrons → inversion layer forms
- At \( V_{GS} = V_{th} \), channel is created
- **Body effect**: Source-body reverse bias increases \( V_{th} \)

---

### 3. Linear (Triode) Region (Case 2)
- \( V_{GS} > V_{th} \)
- \( V_{DS} < V_{GS} - V_{th} \)
- Channel exists from source to drain
- Small \( V_{DS} \): behaves like a resistor

#### **Full Drain Current Equation (Linear Region):**
\[
I_D = \mu_n C_{ox} \frac{W}{L} \left[(V_{GS}-V_{th})V_{DS} - \frac{V_{DS}^2}{2}\right]
\]
Or using \( k_n = \mu_n C_{ox} \frac{W}{L} \):
\[
I_D = k_n \left[(V_{GS}-V_{th})V_{DS} - \frac{V_{DS}^2}{2}\right]
\]

For very small \( V_{DS} \), the quadratic term is negligible:
\[
I_D \approx k_n (V_{GS} - V_{th})V_{DS}
\]

---

### 4. Saturation Region
- Pinch-off occurs when:
\[
V_{DS} \ge V_{GS} - V_{th}
\]
- Channel disappears near drain
- Current becomes almost independent of \( V_{DS} \)

#### **Full Drain Current Equation (Saturation Region):**
Without channel length modulation:
\[
I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{th})^2
\]
Or:
\[
I_D = \frac{1}{2} k_n (V_{GS} - V_{th})^2
\]

Including channel-length modulation (\( \lambda \)):
\[
I_D = \frac{1}{2} k_n (V_{GS} - V_{th})^2 (1 + \lambda V_{DS})
\]

This shows \( I_D \) slightly depends on \( V_{DS} \) in saturation due to shortened effective channel length.

---

## SPICE vs STA
- **STA (OpenSTA)** uses delay tables based on W/L, input slew, output load.
- **SPICE** generates these tables using transistor models.
- SPICE verifies if STA timing assumptions are correct.

---
