# Fully-Differential 2-Stage Miller Compensated OTA in gpdk180

Cadence Virtuoso implementation of a Fully-Differential Two-Stage Miller Compensated OTA featuring dual CMFB loops, achieving >60dB DC gain and a 5.1MHz closed-loop bandwidth.

## Project Overview
This repository contains the Cadence Virtuoso implementation and detailed design documentation for a Fully-Differential Two-Stage Operational Transconductance Amplifier (OTA) utilizing Miller Compensation. Developed as part of the EE610 Analog IC Design coursework at the **Indian Institute of Technology (IIT) Kanpur**, the opamp is engineered in the gpdk180 (180nm) process. It is configured and simulated as an inverting amplifier with a closed-loop gain of -2 to verify stability and transient response under specific loading conditions.

## Design Specifications
The circuit was strictly designed to meet the following parameters:
* **Closed-loop gain**: -2 (Configured using $R_1 = 100k\Omega$, $R_2 = 200k\Omega$).
* **Load configuration**: $R_L = 10k\Omega$ in parallel with $C_L = 2pF$.
* **DC loop gain**: $\geq 60dB$.
* **Closed-loop 3-dB bandwidth**: $\geq 1MHz$ with absolutely no magnitude peaking allowed.
* **CMFB loops**: Phase margin $\geq 60^\circ$ and Unity Gain Bandwidth (UGB) $\geq 1/4$ of the main differential UGB.
* **Second stage output common-mode**: Strictly enforced to $V_{DD}/2$.

---

## System Architecture & Circuit Design

The system architecture is broken down into the core high-gain amplifier stages, the biasing network, and two independent Common-Mode Feedback (CMFB) loops to ensure differential stability.

### 1. Core Amplifier Stages
* **First Stage (High Gain)**: Designed using an nMOS differential pair (NM10, NM11) with pMOS active loads (PM9, PM10). The differential pair is sized with $W/L = 10\mu m/1\mu m$ ($m=2$) to achieve a high transconductance ($g_m \approx 65.14 \mu S$) at a low bias current of $5 \mu A$, providing an initial DC gain of approximately 50 dB.
* **Second Stage (High Swing & Drive)**: Utilizes a pMOS differential pair (PM19, PM20) sized heavily at $W/L = 145\mu m/1\mu m$ ($m=2$) to drive the resistive/capacitive load. The nMOS active loads (NM20, NM21) complete the stage, drawing a total bias current of $776.35 \mu A$ to provide a transconductance of $2.8 mS$ and at least 20 dB of additional gain.

### 2. Common-Mode Feedback (CMFB) Network
Because fully differential amplifiers cannot establish their own output common-mode voltage, two continuous-time CMFB loops are implemented:
* **CMFB1 (First Stage)**: An nMOS-input 5-transistor OTA topology that senses the first-stage output common-mode and adjusts the tail current source of the first differential pair.
* **CMFB2 (Second Stage)**: A pMOS-input 5-transistor OTA topology that strictly enforces the $V_{DD}/2$ common-mode requirement at the final output nodes by controlling the active load currents of the second stage.

### 3. Biasing and Miller Compensation
* **Biasing**: A robust current mirror network derives all necessary gate bias voltages ($V_{b1}, V_{b2}, V_{b3}$) from a single ideal $1\mu A$ reference current source, ensuring all transistors remain deeply in the saturation region.
* **Compensation**: To ensure closed-loop stability across all three feedback loops (Main differential, CMFB1, and CMFB2), RC Miller compensation is applied. Series nulling resistors ($R_c$) are placed in series with the compensation capacitors ($C_c$) to push the Right-Half-Plane (RHP) zero to higher frequencies, improving the overall phase margin.

---

## Component Sizing & Design Parameters
The following table summarizes the key device sizings and compensation values calculated and tuned during the design phase:

| Parameter | First Stage | Second Stage | Main Diff. Loop | CMFB1 | CMFB2 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Input Pair W/L** | $10\mu m / 1\mu m$ | $145\mu m / 1\mu m$ | - | - | - |
| **Transconductance ($g_m$)** | $65.14 \mu S$ | $2.8 mS$ | - | - | - |
| **Drain Current ($I_D$)** | $5 \mu A$ | $776.35 \mu A$ | - | - | - |
| **Compensation Cap ($C_c$)** | - | - | $800 fF$ | $1.5 pF$ | $500 fF$ |
| **Nulling Resistor ($R_c$)** | - | - | $600 \Omega$ | $30 k\Omega$ | $600 \Omega$ |

---

## Simulation Results (Virtuoso ADE L)
The design was rigorously simulated and verified using Cadence Virtuoso, meeting or exceeding all required specifications under the loaded condition ($R_L=10k\Omega || C_L=2pF$).

* **DC Operating Point**: The first stage achieved a simulated $g_m$ of $65.95 \mu S$ at $5.07 \mu A$, while the second stage achieved $2.77 mS$ at $727 \mu A$.
* **Differential Loop Gain**: Achieved a Unity Gain Frequency (UGF) of 3.42 MHz with a highly stable Phase Margin of $68.42^\circ$.
* **Closed-Loop Frequency Response**: Demonstrated a -3dB Bandwidth of 5.11 MHz with absolutely no magnitude peaking observed.
* **CMFB1 Performance**: Reached a UGF of 7.13 MHz and a Phase Margin of $67.97^\circ$.
* **CMFB2 Performance**: Reached a UGF of 11.86 MHz and a Phase Margin of $64.14^\circ$.
* **Transient Response**: A 1mV differential step input yielded a clean amplified output without ringing. A 1mV common-mode step produced a negligible output deviation, confirming an excellent Common-Mode Rejection Ratio (CMRR) where $A_{CM} \approx 0$.

---

## Result Imagery
*(To display the images below, upload the screenshots from your report into an `images` folder within this repository).*

* **Complete Opamp Schematic:**
  `![Opamp Schematic](images/opamp_schematic.png)`
* **CMFB OTA Circuits:**
  `![CMFB Circuits](images/cmfb_circuits.png)`
* **Differential Loop Response:**
  `![Differential Loop](images/diff_loop.png)`
* **Closed-Loop Response:**
  `![Closed Loop Response](images/closed_loop.png)`
* **CMFB Loop Gains:**
  `![CMFB Loops](images/cmfb_loops.png)`
* **Step Responses:**
  `![Step Responses](images/step_responses.png)`
