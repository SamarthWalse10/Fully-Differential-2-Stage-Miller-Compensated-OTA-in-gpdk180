# Fully-Differential 2-Stage Miller Compensated OTA in gpdk180

Cadence Virtuoso implementation of a Fully-Differential Two-Stage Miller Compensated OTA featuring dual CMFB loops, achieving >60dB DC gain and a 5.1MHz closed-loop bandwidth.

## Project Overview
This repository contains the Cadence Virtuoso implementation and design 
documentation for a Fully-Differential Two-Stage Operational Transconductance 
Amplifier (OTA) with Miller Compensation. Developed as part of 
the EE610 Analog IC Design coursework, the opamp is configured as an inverting 
amplifier with a closed-loop gain of -2. 

## Design Specifications
* **Closed-loop gain**: -2 (using $R_1 = 100k\Omega$, $R_2 = 200k\Omega$).
* **Load configuration**: $R_L = 10k\Omega$ parallel with $C_L = 2pF$.
* **DC loop gain**: $\geq 60dB$.
* **Closed-loop 3-dB bandwidth**: $\geq 1MHz$ with no magnitude peaking.
* **CMFB loops**: Phase margin $\geq 60^\circ$ and Unity Gain Bandwidth (UGB) $\geq 1/4$ of the differential UGB.
* **Second stage output common-mode**: Set strictly to $V_{DD}/2$.

---

## System Architecture

### 1. Core Amplifier Stages
* **First Stage**: An nMOS differential pair (NM10, NM11) utilizing pMOS active 
loads (PM9, PM10) to achieve an initial gain of approximately 50 dB.
* **Second Stage**: A pMOS differential pair (PM19, PM20) with nMOS active 
loads (NM20, NM21) designed to provide at least 20 dB of additional gain.

### 2. Common-Mode Feedback (CMFB)
* **CMFB1**: An nMOS-input 5-transistor OTA controls the common-mode voltage 
of the first stage.
* **CMFB2**: A pMOS-input 5-transistor OTA manages the second stage, enforcing 
the $V_{DD}/2$ output common-mode requirement.

### 3. Biasing and Compensation
* **Biasing**: Current mirrors derive all necessary bias currents and voltages 
from a single $1\mu A$ reference source to maintain saturation across all 
devices.
* **Compensation**: Miller compensation with series nulling resistors is 
implemented across the differential stage and both CMFB loops to ensure 
stability.

---

## Component Sizing & Design Parameters
The following table summarizes the key transistor sizing and compensation 
values achieved during the design phase:

| Parameter | First Stage | Second Stage | Main Diff. Loop | CMFB1 | CMFB2 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Transconductance ($g_m$)** | $65.14 \mu S$ | $2.8 mS$ | - | - | - |
| **Drain Current ($I_D$)** | $5 \mu A$ | $776.35 \mu A$ | - | - | - |
| **Compensation Cap ($C_c$)** | - | - | $800 fF$ | $1.5 pF$ | $500 fF$ |
| **Nulling Resistor ($R_c$)** | - | - | $600 \Omega$ | $30 k\Omega$ | $600 \Omega$ |

---

## Simulation Results
The design was simulated using Cadence Virtuoso, meeting or exceeding all 
required specifications.

* **DC Operating Point**: The first stage achieved a simulated $g_m$ of 
$65.95 \mu S$ at $5.07 \mu A$, while the second stage hit $2.77 mS$ at 
$727 \mu A$.
* **Differential Loop Gain**: Achieved a Unity Gain Frequency (UGF) of 3.42 MHz 
with a Phase Margin of $68.42^\circ$.
* **Closed-Loop Frequency Response**: Demonstrated a -3dB Bandwidth of 5.11 MHz 
with absolutely no peaking observed.
* **CMFB1 Performance**: Reached a UGF of 7.13 MHz and a Phase Margin of 
$67.97^\circ$.
* **CMFB2 Performance**: Reached a UGF of 11.86 MHz and a Phase Margin of 
$64.14^\circ$.
* **Transient Response**: A 1mV differential step input yielded a clean 
amplified output, while a 1mV common-mode step produced negligible output 
change, confirming excellent common-mode rejection ($A_{CM} \approx 0$).

---

## Result Imagery
*(Create an `images` folder in your repository, upload the screenshots from your report, and update the filenames below to display them on your GitHub page).*

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
