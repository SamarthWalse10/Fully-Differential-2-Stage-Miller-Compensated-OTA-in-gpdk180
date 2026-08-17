# Fully-Differential 2-Stage Miller Compensated OTA in gpdk180

Cadence Virtuoso implementation of a Fully-Differential Two-Stage Miller Compensated OTA featuring dual CMFB loops, achieving >60dB DC gain and a 5.1MHz closed-loop bandwidth.

## Project Overview
This repository contains the Cadence Virtuoso implementation and detailed design documentation for a Fully-Differential Two-Stage Operational Transconductance Amplifier (OTA) utilizing Miller Compensation. Developed as part of the EE610 Analog IC Design coursework at IIT Kanpur, the opamp is engineered in the gpdk180 pdk.

The design is configured and simulated as an inverting amplifier with a closed-loop gain of -2 to verify stability, common-mode rejection, and transient response under specific loading conditions.

## Design Specifications
The circuit was designed to meet the following parameters:
* **Closed-loop gain**: -2 (Configured using $R_1 = 100k\Omega$, $R_2 = 200k\Omega$).
* **Load configuration**: $R_L = 10k\Omega$ in parallel with $C_L = 2pF$.
* **DC loop gain**: $\geq 60dB$.
* **Closed-loop 3-dB bandwidth**: $\geq 1MHz$ with absolutely no magnitude peaking allowed.
* **CMFB loops**: Phase margin $\geq 60^\circ$ and Unity Gain Bandwidth (UGB) $\geq 1/4$ of the main differential UGB.
* **Second stage output common-mode**: Strictly enforced to $V_{DD}/2$.

---

## System Architecture

The system architecture is broken down into the core high-gain amplifier stages, the biasing network, and two independent Common-Mode Feedback (CMFB) loops.

### 1. Core Amplifier Stages
* **First Stage (High Gain)**: Designed using a high-gain nMOS differential pair with pMOS active loads. 
* **Second Stage (High Swing)**: Utilizes heavily sized pMOS common-source amplifiers with nMOS active loads to provide additional gain and cleanly drive the resistive/capacitive load.

### 2. Common-Mode Feedback (CMFB) Network
Because fully differential amplifiers cannot naturally establish their own output common-mode voltage, two CMFB loops are implemented:
* **CMFB1 (First Stage)**: An nMOS-input 5-transistor OTA topology that senses and stabilizes the first-stage output common-mode.
* **CMFB2 (Second Stage)**: A pMOS-input 5-transistor OTA topology that strictly enforces the $V_{DD}/2$ common-mode requirement at the final output nodes.

### 3. Biasing and Miller Compensation
* **Biasing**: A robust current mirror network derives all necessary gate bias voltages from a single $1\mu A$ reference current source.
* **Compensation**: To ensure closed-loop stability across all three feedback loops, RC Miller compensation is applied. Series nulling resistors are used to push the Right-Half-Plane (RHP) zero to higher frequencies, improving overall stability.

---

## 📘 Detailed Mathematical Calculations & Sizing
> For in-depth theoretical calculations, transistor sizing ($W/L$ ratios, multipliers), transconductance ($g_m$) derivations, and the complete mathematical design methodology, **please refer to the attached Project Report PDF (`EE610_Project_2_Report_251040092.pdf`) included in this repository.**

---

## Simulation Results (Virtuoso ADE L)
The design was simulated and verified using Cadence Virtuoso, meeting or exceeding all required specifications under the loaded condition ($R_L=10k\Omega || C_L=2pF$).

* **Differential Loop Gain**: Achieved a Unity Gain Frequency (UGF) of 3.42 MHz with a highly stable Phase Margin of $68.42^\circ$.
* **Closed-Loop Frequency Response**: Demonstrated a -3dB Bandwidth of 5.11 MHz with absolutely no magnitude peaking observed.
* **CMFB Stability**: Both the First Stage and Second Stage CMFB loops achieved a Phase Margin of $>64^\circ$.
* **Transient Response**: A 1mV differential step input yielded a clean amplified output without ringing. A 1mV common-mode step produced a negligible output deviation, confirming an excellent Common-Mode Rejection Ratio (CMRR).

---

## Result Imagery

* **Complete Opamp Schematic:**
<img width="3402" height="988" alt="full_schematic" src="https://github.com/user-attachments/assets/31f02581-f2b6-4e7a-80b2-cb6348fdab20" />
&nbsp;

* **CMFB1 OTA Circuit:**
<img width="1037" height="645" alt="cmfb1" src="https://github.com/user-attachments/assets/03712262-d5c6-46f1-a770-aca72ba82640" />
&nbsp;

* **CMFB2 OTA Circuit:**
<img width="988" height="648" alt="cmfb2" src="https://github.com/user-attachments/assets/c987445c-503b-4f97-a222-63762d412a12" />
&nbsp;

* **Differential Loop Response:**
<img width="1910" height="654" alt="differential_loop_gain_phase" src="https://github.com/user-attachments/assets/c18fa04a-b1a5-46a2-92f1-5cc1fc69ae59" />
&nbsp;

* **Closed-Loop Response:**
<img width="1910" height="654" alt="closed_loop_gain_phase" src="https://github.com/user-attachments/assets/8210b9e1-845f-41a5-bbc1-1cb4f24d8596" />
&nbsp;

* **CMFB1 Loop Gain:**
<img width="1910" height="654" alt="cmfb1_nOTA_loop_gain_phase" src="https://github.com/user-attachments/assets/7e8604a9-d953-4bba-9d4b-6dc9bfe76918" />
&nbsp;

* **CMFB2 Loop Gain:**
<img width="1910" height="654" alt="cmfb2_pOTA_loop_gain_phase" src="https://github.com/user-attachments/assets/929f0aa8-aee4-40c7-9744-5ba4ab900115" />
&nbsp;

* **Differential Step Response:**
<img width="1910" height="654" alt="differential_step_response" src="https://github.com/user-attachments/assets/2168f46a-7f3d-48eb-9729-bdccdf38f885" />
&nbsp;

* **Common-Mode Step Response:**
<img width="1910" height="654" alt="cm_step_response" src="https://github.com/user-attachments/assets/86f44df1-5454-4da4-9b7d-88c347530353" />
