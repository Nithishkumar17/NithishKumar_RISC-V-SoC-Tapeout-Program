# Week 4 — CMOS Circuit Design & SPICE Simulation  

# Day 1

---

## Overview
Day 1 focuses on transistor-level CMOS behavior, how MOSFET W/L ratio affects delay, and how SPICE validates timing models used by STA (OpenSTA).

---

## CMOS Basics
- **PMOS**: N-type substrate, P-type doping (minority P, majority N)  
- **NMOS**: P-type substrate, N-type doping (minority N, majority P)  
- **W/L ratio** controls drain current → affects propagation delay.  
- Delay tables (input slew vs output load) are generated using **SPICE**, then used by **STA**.

---

## Semiconductor & PN Junction Revision
- Extrinsic semiconductors are formed by doping:
  - P-type: trivalent dopants (B, Al, Ga)
  - N-type: pentavalent dopants (P, As, Sb)
- PN junction creates depletion region and built-in potential:
  - Silicon ≈ 0.7 V
  - Germanium ≈ 0.3 V

### Biasing:
- Forward bias → depletion shrinks → current flows  
- Reverse bias → depletion widens → current blocked (only small leakage)

---

## MOSFET Basics (NMOS Example)
- Terminals: Gate, Source, Drain, Body  
- P-type substrate, SiO2 oxide, N+ source/drain regions  
- Operation depends on VGS and VDS.

---

## NMOS Operation Modes

### 1. Cut-Off Region (Case 0)
- VGS < Vth
- No inversion layer
- Very high resistance, no current.

---

### 2. Threshold Formation (Case 1)
- VGS increases
- Gate attracts electrons → inversion layer forms
- At VGS = Vth → channel created
- Body effect increases Vth when source-body reverse bias exists.

---

### 3. Linear (Triode) Region (Case 2)
- VGS > Vth
- VDS < (VGS - Vth)
- Channel exists from source to drain
- MOSFET behaves like a variable resistor

**Full Drain Current Equation (Linear Region):**

ID = μn * Cox * (W/L) * [ (VGS - Vth) * VDS - (VDS² / 2) ]

OR using kn = μn * Cox * (W/L):

ID = kn * [ (VGS - Vth) * VDS - (VDS² / 2) ]

**For very small VDS:**

ID ≈ kn * (VGS - Vth) * VDS

---

### 4. Saturation Region
- Pinch-off when VDS ≥ (VGS - Vth)
- Channel disappears near drain side
- Current becomes almost independent of VDS

**Full Drain Current Equation (Saturation Region, no channel-length modulation):**

ID = (1/2) * μn * Cox * (W/L) * (VGS - Vth)²

OR

ID = (1/2) * kn * (VGS - Vth)²

**With channel-length modulation (λ):**

ID = (1/2) * kn * (VGS - Vth)² * (1 + λ * VDS)

This shows ID slightly depends on VDS due to effective channel shortening.

---

## SPICE vs STA
- STA uses delay tables generated from SPICE data.
- SPICE simulates transistor-level models to extract delay, slew, and current.
- SPICE verifies STA assumptions.

![drain_current](Lab_Images/Id_Current.jpg)

---

# Week 4 – CMOS Circuit Design & SPICE Simulation  
**Lab – Day 1**

---

## 1) SPICE Netlist
This image shows the SPICE netlist used for the simulation.

![SPICE Netlist](Lab_Images/spice_netlist.jpg)

---

## 2) SPICE Code
The SPICE code for the circuit and model definitions.

![SPICE Code](Lab_Images/spice_code.jpg)

---

## 3) SPICE Simulation
The simulation setup and waveform analysis view in SPICE.
```bash
ngspice day1_nfet_idvds_L2_W5.spice 
```
```ngspice 
plot -#vddbranch
```

![SPICE Simulation](Lab_Images/spice_simulation.jpg)

---

## 4) SPICE Output
Output results after running the SPICE simulation.

![SPICE Output](Lab_Images/spice_output.jpg)

---

## 5) VDS and ID Value of One Instance
Shows the drain-source voltage (VDS) and drain current (ID) for one MOSFET instance.

![VDS and ID](Lab_Images/vds_id_value.jpg)




# Day 2

---

## Overview
Day 2 focuses on short-channel NMOS effects, CMOS voltage transfer characteristics (VTC), and the impact of channel length on drain current.  

---

## Short Channel NMOS Effects
- Two NMOS transistors with **same W/L ratio** but different **absolute W and L** show different Id vs VGS curves.
- For **short channel devices (L = 250 nm)**:
  - ID vs VGS shows **linear dependence** even after pinch-off (VDS > VGS - Vth)
  - This is due to **velocity saturation effect**:  
    - At high electric fields, carrier velocity saturates due to scattering.
    - Velocity is proportional to induced charges; at short channel, velocity saturates at **Vsat**.

### NMOS Operating Regions (short channel)
1. Cut-off
2. Linear (triode)
3. Saturation
4. Velocity Saturation

### Full Drain Current Equation (Short Channel, unified model)
ID = (1/2) * kn * (VGS-vmin) –( Vmin² /2)* (1 + λ * VDS)


Where:  
 vmin = min(vgs(saturation mode),vds(linear mode),vsat(velocity saturation mode))
 
**Observation:**  
- Shorter channel length reduces drain current, longer length increases it.
- Although W/L ratio is same, absolute length affects ID directly.

---

## CMOS Voltage Transfer Characteristics (VTC)

### NMOS
- Gate = VDD, Body = GND  
- Can pass **0** from source easily (VGS > Vth)  
- If source = VDD, NMOS passes up to **VDS - Vth**  
- NMOS acts as **strong 0, weak 1** pull-down transistor

### PMOS
- Gate = GND, Body = VDD  
- Can pass **VDD** from source easily (VSG > Vth)  
- If source = 0, PMOS passes up to **Vth**  
- PMOS acts as **strong 1, weak 0** pull-up transistor

### CMOS
- If VIN = VDD → NMOS on → load capacitor discharged  
- If VIN = 0 → PMOS on → load capacitor charged  

---

### Nomenclature
| Symbol | Description |
|--------|-------------|
| GS     | Gate to Source |
| DS     | Drain to Source |
| VGSN   | VIN (NMOS) |
| VDSN   | VOUT (NMOS) |
| VGSP   | VIN - VDD (PMOS) |
| VDSP   | VOUT - VDD (PMOS) |
| IDSN   | NMOS Drain Current |
| IDSP   | PMOS Drain Current |
| IDSN = -IDSP | Current relationship |

- VDSN vs IDSN sweep converted to VOUT vs VIN to visualize **CMOS VTC**  
![vtf1](Lab_Images/vtf1.jpg)
![vtf2](Lab_Images/vtf2.jpg)
- Intersection points in NMOS and PMOS POV combined to plot **CMOS VTF**.
![vtf3](Lab_Images/vtf3.jpg)

---

## Lab – Day 2 Images

### 1) Short Channel SPICE Code
![Short Channel Code](Lab_Images/shortchannel_code.jpg)

### 2) Short Channel SPICE Output
![Short Channel Output](Lab_Images/shortchannel_output.jpg)  
**Observation:** The graph shows **linear behavior for higher VDS values due to velocity saturation**.

### 3) Short Channel VGS Code
![Short Channel VGS Code](Lab_Images/shortchannel_vgs_code.jpg)

### 4) Short Channel VGS Output
![Short Channel VGS Output](Lab_Images/shortchannel_vgs_output.jpg)  
**Observation:** Threshold voltage (Vth) found to be **0.74 V** from this graph.





