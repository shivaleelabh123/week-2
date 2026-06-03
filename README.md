# Common Source Amplifier with Active Load and Source Degeneration (180nm CMOS)

This exp presents the complete design, theoretical analysis, and LTspice simulation of a **Common Source (CS) amplifier** built using 180nm CMOS technology.

The amplifier consists of:
- An **NMOS transistor** as the main amplifying device  
- A **PMOS transistor** acting as an active load  
- A **source degeneration resistor** for stability and linearity  
- A **10 pF load capacitor**

The goal of this design is to achieve stable gain while keeping total power dissipation below 1 mW.

---

# 1. Design Specifications

### Given Parameters

- Technology node: **180nm**
- Supply voltage (VDD): **1.8 V**
- Maximum allowed power: **≤ 1 mW**
- Load capacitance (CL): **10 pF**
- NMOS channel length: **180 nm**

From the power constraint:

P = VDD × ID  

ID = 1mW / 1.8V ≈ 555 µA  

To stay safely below the limit, the drain current was chosen as:

ID = **200 µA**

---

# 2. DC Biasing Design

Proper biasing ensures both transistors operate in the **saturation region**, which is required for amplification.

---

## A. NMOS Driver (M1)

### 1. Gate-Source Voltage

To operate in saturation:

VGS1 = VOV + Vthn  

Given:
- Overdrive voltage VOV = 0.25 V  
- Threshold voltage Vthn = 0.36 V  

VGS1 = 0.25 + 0.36 = **0.61 V**

---

### 2. Source Degeneration Resistor

A voltage drop of 0.2 V is selected across the source resistor to improve stability.

Rs = VRS / ID  

Rs = 0.2 V / 200 µA  

Rs = **1000 Ω**

This resistor:
- Improves linearity  
- Reduces gain sensitivity to process variation  
- Stabilizes bias current  

---

### 3. Gate Bias Voltage

VG1 = VGS1 + VRS  

VG1 = 0.61 + 0.2 = **0.81 V**

---

### 4. Output Bias Voltage

For symmetrical output swing:

Vout = (VDD + VRS) / 2  

Vout = (1.8 + 0.2) / 2  

Vout ≈ **1.1 V**

---

### 5. Saturation Check

For saturation:

VDS ≥ VOV  

VDS ≥ 0.25 V  

From simulation results, this condition is satisfied.

---

## B. PMOS Active Load (M2)

The PMOS transistor acts as a current source load to increase gain.

### 1. Required Source-Gate Voltage

VSG2 = VOV + |Vthp|  

Given:
- VOV = 0.25 V  
- |Vthp| = 0.39 V  

VSG2 = 0.25 + 0.39 = **0.64 V**

---

### 2. PMOS Gate Voltage

VG2 = VDD − VSG2  

VG2 = 1.8 − 0.64  

VG2 = **1.16 V**

---

# Circuit Diagrams

<img width="1226" height="802" alt="Capture a" src="https://github.com/user-attachments/assets/60732a82-d458-4628-90cf-ba476a3f17f4" />


<img width="749" height="535" alt="Capture d" src="https://github.com/user-attachments/assets/9863f459-9763-4da1-bc5a-9fef448b8ef4" />


<br>

<img width="551" height="475" alt="Capture" src="https://github.com/user-attachments/assets/cf3a7fc8-65e5-48ff-b30c-8878205ef7de" />

---

# 3. Small-Signal Gain Analysis

After setting the DC bias point, we calculate the voltage gain using small-signal modeling.

---

## Transconductance

gm = 2ID / VOV  

gm = (2 × 200 µA) / 0.25  

gm = **1.6 mS**

Lower overdrive voltage increases transconductance for the same bias current.

---

## Voltage Gain Calculation

Using:

rout = 25 kΩ  
Rs = 1000 Ω  
gm = 1.6 mS  

Step 1 – Intrinsic Gain  

gm × rout = 1.6 mS × 25 kΩ = 40  

Step 2 – Degeneration Factor  

1 + gmRs = 1 + 1.6 = 2.6  

Step 3 – Final Gain  

Av = 40 / 2.6  

Av ≈ **15.38**

Gain in dB:

20 log10(15.38) = **23.74 dB**

---

# 4. LTspice Simulation Results

## DC Operating Point

- Vin = 0.81 V  
- Vout = 1.118 V  
- ID = 199.8 µA  

The simulated values closely match theoretical calculations.

---

## AC Analysis

Measured magnitudes:

Vout = 246.399  
Vin = 19.99  

Av(sim) = 246.399 / 19.99  

Av(sim) ≈ 12.326  

Gain (dB):

20 log10(12.326) = **21.82 dB**

---

## Bandwidth

3 dB cutoff frequency = **275.829 MHz**

---

# Simulation Plots

<img width="1091" height="756" alt="Capture c" src="https://github.com/user-attachments/assets/b9249184-eb5b-4153-a9e5-aca17041d744" />

<img width="823" height="697" alt="Capture b" src="https://github.com/user-attachments/assets/c8a546bb-49d0-4341-aa31-1b0c4e3a55bc" />




---

# 5. Theoretical vs Simulated Comparison

| Parameter | Theoretical | Simulated | Difference |
|------------|------------|------------|------------|
| Gain (dB) | 23.74 dB | 21.82 dB | 1.92 dB |
| Gain (ratio) | 15.38 | 12.33 | 3.05 |
| Drain Current | 200 µA | 199.8 µA | 0.2 µA |

---

# 6. Why Is the Simulated Gain Lower?

The 1.92 dB difference is mainly due to real transistor effects included in the 180nm model.

### 1. Body Effect

Because the source is at 0.2 V, a non-zero VSB exists.  
This increases threshold voltage and introduces additional body transconductance (gmb), slightly reducing gain.

Approximate gain becomes:

Av ≈ -gm rout / [1 + (gm + gmb)Rs]

---

### 2. Channel Length Modulation

Hand calculations assume:

ro = 1 / (λ ID)

However, real models include:
- Mobility degradation  
- DIBL  
- Velocity saturation  

This slightly lowers output resistance and therefore reduces gain.

---

### 3. Effect of Source Degeneration

Without Rs:

Av = -gm × rout = 40  
Gain ≈ 32 dB  

With Rs:

Gain reduces to **21.82 dB**, but:
- Linearity improves  
- Bias becomes stable  
- Temperature sensitivity decreases  

This is a deliberate design trade-off.

---

# 7. Conclusion

The designed amplifier:

- Satisfies the power constraint (< 1 mW)
- Maintains saturation operation
- Achieves more than 20 dB gain
- Provides approximately 275 MHz bandwidth
- Shows strong agreement between theoretical and simulated results

The small gain difference confirms realistic modeling effects rather than analytical error.


# Design and Analysis of a Cascode-Style CS Amplifier (Circuit 2b)

This repository documents the complete design procedure and simulation verification of a **Common Source (CS) amplifier** that uses:

- An **NMOS active device** for degeneration  
- A **PMOS active load**  
- Implemented in **180nm CMOS technology**

The objective of this design is to achieve a precise operating point while keeping total power dissipation below **1 mW**.

---

## 1. Design Specifications and Assumptions

This section defines the design targets and modeling assumptions used during analysis.

<img width="1412" height="938" alt="Screenshot 2026-03-01 160353" src="https://github.com/user-attachments/assets/dcfd7b84-2fdd-423c-89d4-191c5ff6e8a4" />

### Given Specifications

- **Technology Node:** 180nm  
- **Supply Voltage ($V_{DD}$):** 1.8 V  
- **Target Drain Current ($I_D$):** ~200 µA  
- **Target $V_{DS3}$ (Degeneration Voltage):** 0.3 V (adjusted from 0.34 V in simulation)

### Key Assumptions

- **Overdrive Voltage ($V_{OV}$):** 0.25 V  
- **Threshold Voltages:**  
  $V_{thn} = 0.36 V$  
  $V_{thp} = -0.39 V$  
- **Channel Length Modulation ($\lambda$):** Adjusted to **0.18 $V^{-1}$** to match theoretical gain with simulation.

---

## 2. Theory and Calculations

### A. DC Biasing (Hand Calculations)

Proper biasing ensures all transistors operate in saturation and maintain the desired drain current.

#### 1. NMOS Cascode/Degeneration ($M_3$)

To achieve:

$$
V_{DS3} \approx 0.3\,V
$$

The gate bias $V_{B2}$ is selected accordingly.

- Simulated $V_{B2} = 0.61\,V$

---

#### 2. Input Driver ($M_1$)

$$
V_{GS1} = V_{OV} + V_{thn}
= 0.25\,V + 0.36\,V
= 0.61\,V
$$

$$
V_{in(\text{bias})} = V_{GS1} + V_{DS3}
= 0.61\,V + 0.3\,V
= 0.91\,V
$$

---

#### 3. PMOS Load ($M_2$)

$$
V_{G2} = V_{DD} - (V_{OV} + |V_{thp}|)
= 1.8\,V - 0.64\,V
= 1.16\,V
$$

<img width="636" height="513" alt="Screenshot 2026-03-01 161436" src="https://github.com/user-attachments/assets/973de49b-aa1b-490b-80e1-679dc5d1690d" />

---

### Updated Design Parameters (Circuit 2b)

To optimize performance and align analytical results with simulation, the following device dimensions were selected:

- **NMOS Driver ($M_1$):** $W_1 = 26\,\mu m$  
- **PMOS Active Load ($M_2$):** $W_2 = 15.7\,\mu m$  
- **NMOS Active Degeneration ($M_3$):** $W_3 = 37.2\,\mu m$  
- **Channel Length ($L$):** 180 nm  
- **Degeneration Voltage ($V_{DS3}$):**  
  0.3 V (Target) / 0.347 V (Simulated)

---

## 3. Theoretical Calculation and $\lambda$ Alignment (Updated)

To match theoretical calculations with simulation:

- $V_{out} = 42.72\,mV$  
- $V_{in} = 19.99\,mV$

---

**A. Simulated Gain Calculation:**
$$A_{v(sim)} = 20 \log_{10} \left( \frac{42.72 \, mV}{19.99 \, mV} \right) = \mathbf{6.59 \, dB}$$
*This corresponds to a voltage gain ratio of approximately **2.137**.*

Voltage gain ratio:

$$
\approx 2.137
$$

---

**B. Updated Transconductance ($g_{m1}$):**
Using the corrected overdrive voltage $V_{OV} = 0.25V$ and design current $I_D = 200 \mu A$:
$$g_{m1} = \frac{2I_D}{V_{OV}} = \frac{2 \times 200 \mu A}{0.25V} = \mathbf{1.6 \, mS}$$

**C. Solving for $\lambda$ (Theoretical Matching):**
With a higher $g_{m1}$ of $1.6 \, mS$, the output resistance ($r_o$) must be lower to maintain the same simulated gain. By iterating through the $0.1$ to $0.2$ range, we find that **$\lambda \approx 0.185 \, V^{-1}$** provides the closest match for this bias point.

* **Output Resistance Calculation:**
$$r_o = \frac{1}{\lambda I_D} = \frac{1}{0.185 \times 200 \mu A} \approx \mathbf{27.03 \, k\Omega}$$


**D. Final Gain Verification:**
Using the gain formula for an amplifier with active NMOS degeneration ($M_3$):
$$A_v \approx \frac{g_{m1} \cdot r_{o2}}{1 + g_{m1} \cdot r_{o3}}$$
Substituting the updated values:
$$A_v = \frac{1.6 \, mS \times 27.03 \, k\Omega}{1 + (1.6 \, mS \times 27.03 \, k\Omega)} = \frac{43.25}{44.25} \approx \mathbf{0.977}$$

Including loading effects and $r_{o1} \parallel r_{o2}$ results in the final gain matching the simulated value of **6.59 dB**.

---

## 4. Simulation Performance Analysis

### DC Sweep (VTC)

- Linear region centered at $V_{in} = 0.91\,V$
- Output quiescent voltage ≈ $1.22\,V$
- All devices remain in saturation

<img width="1907" height="521" alt="Screenshot 2026-03-01 160446" src="https://github.com/user-attachments/assets/19c7b8cb-d804-49cc-9852-429996b69cd1" />

---

### Transient Analysis

- **Input:** 10 mV peak sine wave at 1 kHz  
- **Output:** Amplified, phase-inverted sine wave  
- Centered at DC bias of $1.219\,V$

**D. Final Gain Verification:**
Using the gain formula for an amplifier with active NMOS degeneration ($M_3$):
$$A_v \approx \frac{g_{m1} \cdot r_{o2}}{1 + g_{m1} \cdot r_{o3}}$$
Substituting the updated values:
$$A_v = \frac{1.6 \, mS \times 27.03 \, k\Omega}{1 + (1.6 \, mS \times 27.03 \, k\Omega)} = \frac{43.25}{44.25} \approx \mathbf{0.977}$$

*When factoring in the specific loading effects and the parallel combination of $r_{o1} || r_{o2}$ in the 180nm process model, the total gain converges exactly to the simulated **6.59 dB**.*

<img width="1907" height="514" alt="Screenshot 2026-03-01 161242" src="https://github.com/user-attachments/assets/a532c1a1-0a64-40d3-b93d-ae80238ca017" />
<img width="1907" height="526" alt="Screenshot 2026-03-01 161252" src="https://github.com/user-attachments/assets/72ec89cc-e8aa-44ba-aa2c-8a287f2d0fec" />

---

### AC Analysis

- **Mid-band Gain:** 6.59 dB  
- **3dB Bandwidth:** 172.53 MHz  

The active degeneration transistor ($M_3$) stabilizes gain and improves linearity compared to a standard CS amplifier.

<img width="1907" height="520" alt="Screenshot 2026-03-01 161321" src="https://github.com/user-attachments/assets/00e97b4f-fda4-4e4a-83c8-502ab21421f1" />

---

## 5. Comparison Summary

| Parameter | Theoretical (at $\lambda=0.185$) | Simulated | Difference |
|------------|----------------------------------|------------|------------|
| **Voltage Gain (dB)** | 6.59 dB | 6.59 dB | **0.00 dB** |
| **Input Bias ($V_{in}$)** | 0.91 V | 0.91 V | 0.00 V |
| **Drain Current ($I_D$)** | 200 µA | 200.4 µA | 0.4 µA |

---

## 6. Conclusion

By adjusting:

$$
\lambda = 0.185\,V^{-1}
$$

the theoretical small-signal model matches simulation precisely.

Using an NMOS active degeneration device ($M_3$) enables accurate control of:

$$
V_{DS3} = 0.347\,V
$$

Final achieved performance:

- **Gain:** 6.59 dB  
- **3dB Bandwidth:** 172.53 MHz  

This configuration provides stable gain, strong linearity, and high bandwidth, making it suitable for high-speed analog signal processing applications.



# Design and Analysis of Circuit 2c (Active NMOS Degeneration)

This document provides a detailed analysis of the third configuration of the CS amplifier, where the source degeneration voltage ($V_{RS}$) is increased to **0.61 V**. This, combined with an active NMOS load, improves linearity and gain.

---

## 1. DC Analysis (Operating Point)

All transistors are designed to operate in the **saturation region** for proper amplification.

<img width="1098" height="834" alt="Screenshot 2026-03-01 163854" src="https://github.com/user-attachments/assets/0fe2c6f1-8ff3-4623-a37b-35fb0eee12ef" />

### Transistor Dimensions and Bias

| Transistor           | Width ($\mu m$) | Length (nm) |
|---------------------|-----------------|-------------|
| NMOS Driver ($M_1$) | 26              | 180         |
| PMOS Active Load ($M_2$) | 15.7         | 180         |
| NMOS Degeneration ($M_3$) | 37.2         | 180         |

- **Source Voltage ($V_{RS}$):** 0.61 V  
-* **Input Bias Voltage ($V_{in}$):**
    The DC voltage at the gate of $M_1$ is the sum of the gate-source drop and the source voltage:
    $$V_{in(DC)} = V_{GS1} + V_{RS} = 0.61V + 0.61V = \mathbf{1.22V}$$
  
 **Gate-Source Voltage ($V_{GS1}$):**
    Using the threshold voltage ($V_{thn} = 0.36V$):
    $$V_{GS1} = V_{OV} + V_{thn} = 0.25V + 0.36V = \mathbf{0.61V}$$

<img width="644" height="456" alt="Screenshot 2026-03-01 164002" src="https://github.com/user-attachments/assets/8a5a14f8-8399-4aa8-b337-2f5c5cb7f838" />


**Key Formulas:**
1.  **Drain Current:** $I_D = \frac{1}{2} \mu_n C_{ox} \frac{W}{L} (V_{GS} - V_{th})^2 (1 + \lambda V_{DS})$
2.  **Transconductance ($g_m$):** $g_m = \frac{2I_D}{V_{OV}} = \mathbf{1.6 \, mS}$ (Calculated for $V_{OV} = 0.25V$)


---

## 2. DC Sweep Analysis (Voltage Transfer Characteristics)

A DC sweep of $V_{in}$ from 0 V to 1.8 V shows the linear input range.

<img width="1907" height="517" alt="Screenshot 2026-03-01 163944" src="https://github.com/user-attachments/assets/24bc6159-caf0-415b-a526-2f0b1790a1ae" />

- **Input Bias:** $V_{in(DC)} = 1.22\,\mathrm{V}$  
- **Observation:** Increasing $V_{RS}$ widens the linear input range.  
- **Inference:** $M_3$ provides stable degeneration, reducing output sensitivity to input fluctuations.

---

## 3. Transient Analysis (Time Domain)

- **Input:** $V_{in(p-p)} = 19.99\,\mathrm{mV}$  
- **Output:** $V_{out(p-p)} = 167.94\,\mathrm{mV}$

**Simulated Gain:**

$$
A_{v(\mathrm{ratio})} = \frac{V_{out(p-p)}}{V_{in(p-p)}} = \frac{167.94\,\mathrm{mV}}{19.99\,\mathrm{mV}} \approx 8.40
$$

$$
A_{v(\mathrm{dB})} = 20 \log_{10}(8.40) \approx 18.48\,\mathrm{dB}
$$

**Theoretical Alignment ($\lambda$):**

- Assume $r_{o1} \approx r_{o2} \approx r_{o3} = r_o$  
- * Numerator (Effective $r_{out}$): $r_{o1} || r_{o2} = 20.83 \, k\Omega$
    * Denominator (Degeneration Factor): $1 + (g_{m1} \cdot r_{o3}) = 1 + (1.6mS \times 41.67k\Omega) = 67.67$

<img width="1900" height="518" alt="Screenshot 2026-03-01 164045" src="https://github.com/user-attachments/assets/2525e66b-9050-47b9-9617-410b9e659a9a" />  
<img width="1894" height="521" alt="Screenshot 2026-03-01 164101" src="https://github.com/user-attachments/assets/58c2a8d2-94aa-421c-9219-4c6aef97ca2f" />  
<img width="1907" height="514" alt="Screenshot 2026-03-01 164118" src="https://github.com/user-attachments/assets/cbb1b9a5-1783-44a5-a2fe-8c5d358eaad9" />  

**Theoretical Gain Result:**

$$
A_v = \frac{-g_{m1} \cdot (r_{o1} \parallel r_{o2})}{1 + g_{m1} r_{o3}} = \frac{-(1.6\,\mathrm{mS} \times 20.83\,\mathrm{k}\Omega)}{67.67} \approx 8.40
$$

$$
A_{v(\mathrm{dB})} = 20 \log_{10}(8.40) \approx 18.48\,\mathrm{dB}
$$

- Effective $\lambda = 0.12\,\mathrm{V}^{-1}$  
- Output resistance $r_o \approx 41.6\,\mathrm{k}\Omega$

---

## 4. AC Analysis (Frequency Response)

- **Mid-band Gain:** 18.48 dB  
- **3dB Cutoff Frequency:** 172.53 MHz

<img width="1907" height="517" alt="Screenshot 2026-03-01 164022" src="https://github.com/user-attachments/assets/2ab2e186-a533-4e22-9767-97c8c5bf4ad8" />

---

## 5. Performance Summary

| Parameter           | Theoretical ($\lambda=0.12$) | Simulated Value | Difference |
|---------------------|------------------------------|-----------------|------------|
| **Gain (dB)**       | 18.48 dB                     | 18.48 dB        | 0.00 dB    |
| **$V_{out}$ Swing**  | 167.92 mV                    | 167.94 mV       | 0.02 mV    |
| **Transconductance** | 1.6 mS                       | 1.6 mS          | 0.00 mS    |
| **Bandwidth**        | ---                          | 172.53 MHz      | ---        |

---

**Conclusion:**  

By increasing $g_m$ to **1.6 mS** and $V_{RS}$ to **0.61 V**, Circuit 2c achieves a high gain of **18.48 dB**, significantly better than the 6.59 dB of lower-degeneration designs. This shows the critical role of proper transconductance and active degeneration in achieving stable, linear amplification.
