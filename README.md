# week-2

# Experiment 2  
Design of Common Source (CS) Amplifier using 180nm Technology with PMOS Active Load in LTspice  

---

## 1. Aim  
To design and simulate a Common Source amplifier using NMOS with PMOS active load in 180nm technology and verify the gain using LTspice.

---

## 2. Given Specifications  

VDD = 1.5V  
Power ≤ 0.5mW  
CL = 1pF  
Technology = 180nm  
Desired VOUT ≈ VDD/2  

---

## 3. Design Calculations  

Drain current:  

ID = P / VDD  
ID = 0.5mW / 1.5  
ID = 0.334 mA  

Overdrive voltage:  

VOV = 0.25V  

VGS = VOV + VTH  
VGS = 0.25 + 0.366  
VGS = 0.61V  

Source resistor:  

RS = VRS / ID  
RS = 0.2 / 0.334m  
RS ≈ 598Ω  

Gate voltage:  

VG = VGS + VRS  
VG = 0.61 + 0.2  
VG = 0.81V  

Output voltage:  

VOUT ≈ VDD/2 + VRS  
VOUT ≈ 0.95V  

Width calculation (after tuning):  

NMOS → W = 48.32µm  
PMOS → W = 62.86µm  

---

## 4. Circuit Diagram  

![Image description](your_image1.png)  
![Image description](your_image2.png)  

---

## 5. Simulation Steps  

### Step 1: Create New Schematic  
Open LTspice  
Click File → New Schematic  

Place components:  
NMOS  
PMOS  
Resistor (RS)  
Voltage source (VDD)  
Input voltage (Vin)  
Capacitor (CL)  
Ground  

---

### Step 2: Include Technology Library  
Press S and write:  

.include tsmc018.lib  

---

### Step 3: Set MOS Parameters  

NMOS:  
W = 48.32u  
L = 180n  

PMOS:  
W = 62.86u  
L = 180n  

---

### Step 4: Operating Point Analysis  

Add:  
.op  

Click Run  

Observe:  
Drain current (ID)  
VGS  
VDS  

---

### Step 5: DC Analysis  

Add:  
.dc Vin 0 1.5 0.01  

Observe transfer characteristics  

---

### Step 6: Transient Analysis  

Set input:  
SINE(0.8 10m 1k)  

Add:  
.tran 5m  

Observe:  
Input waveform  
Output waveform  
Phase shift (180°)  

Calculate:  

Av = Vout / Vin  

---

### Step 7: AC Analysis (Without Capacitor)  

Add:  
.ac dec 10 0.1 100G  

Observe:  
Gain (dB)  
Bandwidth  

---

### Step 8: AC Analysis (With Capacitor)  

Add capacitor  

Repeat:  
.ac dec 10 0.1 100G  

Compare gain and bandwidth  

---

### (a) Operating Point Analysis  

![Image description](your_op1.png)  
![Image description](your_op2.png)  

---

### (b) DC Analysis  

![Image description](your_dc.png)  

---

### (c) Transient Analysis  

INPUT:  
![Image description](your_input.png)  

OUTPUT:  
![Image description](your_output.png)  

---

### (d) AC Analysis  

WITHOUT CAPACITOR:  
![Image description](your_ac1.png)  

WITH CAPACITOR:  
![Image description](your_ac2.png)  

---

## 6. Results  

Voltage Gain:  

Av = Vout / Vin  
Av ≈ 12.21  

Gain(dB) = 20 log(Av)  
= 21.75 dB  

---

## 7. Comparison  

| Parameter | Theoretical Value | Simulated Value |
|------------|------------------|-----------------|
| ID | 0.334 mA | 0.335 mA |
| VOUT | 0.95 V | 0.94 V |
| Gain | 15.38 | 12.21 |
| Gain(dB) | 23.74 dB | 21.75 dB |

---

## 8. Validation  

Power consumption is within limit (0.5 mW)  
VOUT ≈ VDD/2 → correct bias  
Gain achieved  
MOSFET operates in saturation  

---

## 9. Conclusion  

The Common Source amplifier using PMOS active load is successfully designed and simulated.  
The required gain and bias conditions are achieved.  

---

# 🔹 B. 3-Transistor CS Amplifier  

---

## 1. Aim  

To design and simulate a 3-transistor CS amplifier using NMOS current source and PMOS active load.

---

## 2. Given Specifications  

VDD = 1.5V  
Power ≤ 0.5mW  
Technology = 180nm  

---

## 3. Design Calculations  

ID = 0.334 mA  

VGS = 0.616V  

VOUT ≈ 0.75V  

Widths:  

NMOS = 25.77µm  
PMOS = 59.9µm  

---

## 4. Circuit Diagram  

![Image description](your_circuit2.png)  

---

## 5. Simulation Steps  

(Same steps as above)

---

### (a) Operating Point  

![Image description](your_op3.png)  

---

### (b) Transient Analysis  

![Image description](your_tran2.png)  

---

### (c) AC Analysis  

![Image description](your_ac3.png)  

---

## 6. Results  

Av = 2.96  

Gain(dB) = 9.43 dB  

---

## 7. Comparison  

| Parameter | Theoretical | Simulated |
|----------|------------|-----------|
| Gain | 6.95 dB | 9.43 dB |

---

## 8. Validation  

Bias achieved  
Saturation satisfied  
Gain verified  

---

## 9. Conclusion  

The 3-transistor CS amplifier provides better bias stability but lower gain compared to basic CS amplifier.  

---
