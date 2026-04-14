WEEK 4:
# 🔵 EXPERIMENT 04: MOS DIFFERENTIAL AMPLIFIER 
---

## Aim

To design and simulate MOS Differential Amplifiers using:
- Resistive Load  
- Improved Bias  
- CMOS Active Load  

and analyze their performance using DC, Transient, and AC analysis in LTspice.

---

## Given Specifications

- VDD = +0.9 V  
- VSS = −0.9 V  
- Total Supply = 1.8 V  
- Power ≤ 2 mW  
- VINCM = 0 V  
- VOUTCM ≈ 0 V  
- VP ≈ −0.7 V  
- Channel Length (L) = 540 nm  
- VTHn ≈ 0.366 V  
- |VTHp| ≈ 0.39 V  

---

## Theory

A differential amplifier amplifies the **difference between two input signals**.

**Why it works :**
- Tail current remains constant  
- Current shifts between two transistors  
- Load converts current into voltage  
- Output is proportional to input difference  

---

# ⚫ CIRCUIT 1: RESISTIVE LOAD DIFFERENTIAL AMPLIFIER

---

## Design Calculations

### 1. Tail Current (I TAIL)
P = V × I  
I_TOTAL = 2mW / 1.8V ≈ 1.11mA  
Assume I TAIL ≈ 1mA  

### 2. Branch Current
ID1 = ID2 = ITAIL / 2 = 0.5mA  

### 3. Load Resistance (RD)
RD = VDD / ID = 0.9 / 0.5mA = 1.8kΩ  

### 4. Transconductance (gm)
gm = 2ID / VOV  
Assume VOV ≈ 0.2V  
gm = (2 × 0.5mA) / 0.2 = 5mS  

### 5. Gain
Av = gm × RD = 5mS × 1.8k = 9 V/V  

### 6. Width
W ≈ 30–32 µm  

---

## Circuit Diagram
NMOS differential pair with resistive loads.

---

## Simulation Steps

1. Open LTspice  
2. New Schematic  
3. Place NMOS (M1, M2), resistors, current source, voltage sources  
4. Add library:
.include tsmc018.lib  
5. Set MOS: L = 540nm, W ≈ 30µm  
6. Add commands:  
.op  
.dc Vin -0.5 0.5 0.01  
.tran 0 5ms  
.ac dec 100 1 1G  
7. Run simulation  

---

## Analyses

### a) Operating Point Analysis
ID1 = ID2 ≈ 0.5mA  
VOUT ≈ 0  

### b) DC Analysis
Linear near 0, saturation at extremes  

### c) Transient Analysis
Small vin → linear output  
Large vin → distortion  

### d) AC Analysis
Gain ≈ 5–6 V/V  
Gain ≈ 14–15 dB  
Bandwidth: Few MHz  

**Reason:** Low gain due to RD  

---

## Results
Av = Vout / Vin  
Av(dB) = 20 log(Av)  

---

## Conclusion
Low gain due to resistive load.

---

# ⚫ CIRCUIT 2: DIFFERENTIAL AMPLIFIER WITH IMPROVED BIAS

---

## Design Calculations

ITAIL = 1mA  
ID = 0.5mA  
RD ≈ 1.8kΩ  

Better bias improves stability.

---

## Circuit Diagram
Same as Circuit 1 with improved bias.

---

## Simulation Steps
Same as Circuit 1.

---

## Analyses

### a) Operating Point
Stable bias and better current matching  

### b) DC Analysis
Improved linear region  

### c) Transient Analysis
Less distortion  

### d) AC Analysis
Gain ≈ 1.8 V/V  
Gain ≈ 5 dB  

**Reason:** Non-ideal bias reduces gain  

---

## Results
Av = Vout / Vin  
Av(dB) = 20 log(Av)  

---

## Conclusion
Better stability but reduced gain.

---

# ⚫ CIRCUIT 3: CMOS DIFFERENTIAL AMPLIFIER (ACTIVE LOAD)

---

## Design Calculations

ITAIL ≈ 1mA  
ID = 0.5mA  
gm ≈ 5mS  

Av = gm × ro ≈ 40 V/V  

### Widths
M1, M2 = 32 µm  
M3 = 65 µm  
M4, M5 = 58 µm  

---

## Circuit Diagram
NMOS pair with PMOS active load.

---

## Simulation Steps
Same as Circuit 1.

---

## Analyses

### a) Operating Point
ID ≈ 0.5mA  
VOUT ≈ 0  

### b) DC Analysis
Strong linear region  

### c) Transient Analysis
Clean output, clipping at high input  

### d) AC Analysis
Gain ≈ 41 V/V  
Gain ≈ 32 dB  
Bandwidth ≈ 400+ MHz  

**Reason:** High output resistance (ro)  

---

## Results
Av = Vout / Vin  
Av(dB) = 20 log(Av)  

---

## Conclusion
Highest gain due to active load.

---

# 🔵 COMPARISON

| Parameter | Circuit 1 | Circuit 2 | Circuit 3 |
|----------|----------|----------|----------|
| Gain | ~5.5 | ~1.8 | ~41 |
| Bandwidth | Low | Medium | Very High |
| Load | Resistor | Improved | Active |
| Performance | Basic | Stable | Best |

---

## Explanation

Circuit 1 → Limited by RD  
Circuit 2 → Better bias but reduced gain  
Circuit 3 → High gain due to large ro  

---

# 🔵 VALIDATION

✔ Power condition satisfied  
✔ VOUT ≈ 0  
✔ Saturation achieved  
✔ Gain achieved  

---

# 🔵 FINAL CONCLUSION

All three differential amplifiers are successfully designed and analyzed.  
Gain increases from Circuit 1 to Circuit 3 due to increase in output resistance.

---
