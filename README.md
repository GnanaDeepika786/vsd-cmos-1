# CMOS Circuit Design and SPICE Simulations using Sky130
This repository documents my hands-on technical progress during CMOS Circiut Desgin using SPICE Simulations using SKY130 workshop organized by VLSI System Design(VSD).YHe focus of this workshop is device-level characterizationand robustness analysis using open-source EDA tools

**Workshop Flow:**
* Module 1: MOSFET Fundamentals & SPICE Setup
* Module 2: Velocity Saturation & CMOS Inverter Basics
* Module 3: Switching Threshold & Dynamic Behaviour
* Module 4: Noise‑Margin Robustness Analysis
* Module 5: Power‑Supply & Process‑Variation Evaluation

**Tools:**
* NGSpice + SKY130 PDK – Open‑source SPICE engine paired with accurate 130 nm device models for all simulations.
* NGSpice Built‑in Plotter – Immediate waveform visualisation without external graphing packages.
* VS Code / Vim Netlist Editor – Syntax‑highlighted templates that speed up deck creation and revision.
* Bash & Make Scripts – Automate multi‑corner sweeps and parameter studies in a single command.
* Git & GitHub – Version‑control every deck, share lab results, and reproduce plots with full traceability.

**Lab Exercises:**
* MOSFET DC Characterisation – Sweep Id‑Vds/Vgs to extract Vt, µ, and channel‑length modulation.
* CMOS Inverter VTC Sizing – Adjust (W/L) ratios, overlay switching‑threshold curves, and balance delays.
* Transient Delay Profiling – Apply PULSE stimuli, measure tpHL/tpLH, and correlate with load capacitance.
* Noise‑Margin Exploration – Compute NMH/NML, inject glitches, and map safe versus undefined logic regions.
* Supply‑&‑Corner Sweep – Step VDD from 1.8 V to 0.6 V and run TT/SS/FF corners to derive guard‑band guidelines.

**Projects Covered in the Workshop:**
* Design & Characterisation of a CMOS Inverter – Size PMOS/NMOS pairs, generate VTC plots, and optimise switching‑threshold and rise/fall delays.
* Noise‑Margin & Delay Analysis of an Inverter Chain – Evaluate NMH/NML and timing through TT/SS/FF corners and sub‑1 V supply sweeps.
* PVT‑Aware Low‑Power Standard Cell – Build a process‑tolerant library cell, script parametric sweeps, and document guard‑band guidelines for tape‑out.

# **Day 1 - Basics of NMOS Drain current (Id) vs Drain-to-source Voltage (Vds)**

## Introduction to Circuit Design and SPICE simulations

### L1 Why do we need SPICE simulations?

 A CMOS circuit design has both PMOS and NMOS transistors are connected together in a specific different fashion to form logic gates such as NAND, NOR, AND, OR, etc. These basic gates are the building blocks of all digital circuits.A standard CMOS inverter is built using a PMOS connected to $V_{DD}$ and an NMOS connected to $V_{SS}$ driving a load capacitance ($C_L$).
 
 ![Inverter Circuit](CMOS-1(30775193327552).jpg)
 
 The above inverter circuit has certain electrical characteristics. To understand its behavior, we perform SPICE simulations which help us analyze important parameters such as delay, switching behavior, and performance. Based on these results, we can determine the proper W/L (Width/Length) ratio of the transistors.

### VTC Circuit and Waveform Plot
![CMOS Inverter VTC Characteristics](CMOS-2(30775566694972).jpg)

* **Voltage Transfer Characteristics (VTC):** By plotting $V_{out}$ against $V_{in}$, we observe how the circuit switches b/w pmos and nmos. 
* **Operating Regions:**
  - When $V_{in} = 0V$, PMOS is in **Linear** and NMOS is **Off** ($V_{out} = V_{DD}=2$).
  - As $V_{in}$ reaches the center threshold ($V_{in} = 1V$), both transistors enter the **Saturation** region simultaneously.
  - When $V_{in} = 2V$, PMOS is **Off** and NMOS operates in the **Linear** region ($V_{out} = 0V$).


### Why do we need SPICE?

 Clock Tree Synthesis (CTS), crosstalk, and timing analysis are completely dependent on 
SPICE (Simulation Program with Integrated Circuit Emphasis).Without SPICE, it would not be possible to calculate delays, and without delay information, 
physical design and timing verification would not be meaningful.

![Clock Network](CMOS-4(30775650902025).jpg)

 Suppose we perform CTS on a circuit using buffers that are connected with different capacitive loads at their outputs.
By performing SPICE simulation ,we get a **Delay Table** for these buffer cells.

* **Buffer Trees:** Buffering is used across multiple levels (Level 1, Level 2) to maintain a clean clock signal and balance capacitive loads.
* **Delay Tables (Look-up Tables):** In Static Timing Analysis (STA), circuit delays are calculated dynamically based on two key parameters:
  1. **Input Slew:** The transition speed of the incoming signal.
  2. **Output Load:** The total capacitance ($fF$) the node needs to drive.

By mapping the input slew (e.g., $40ps$, $60ps$) against output load (e.g., $30fF$, $50fF$) in the lookup matrices, we can determine the precise delay value at their intersection point.
This delay tables for the level 1 and level 2 buffers is a direct result of transistor-level circuit design and SPICE simulation.

 ![Delay Table](CMOS-5(30775710287972).jpg).
 
Therefore, SPICE plays a fundamental role in CMOS circuit design, as it allows us to characterize and
evaluate circuit performance accurately before implementation.


### L2: Introduction to Basic Element in Circuit Design – NMOS
An NMOS is a **4-terminal device** composed of a Gate (G), Source (S), Drain (D), and Body/Substrate (B) built over a P-substrate with $n^+$ diffusion regions(source and drain).

Above the substrate, there is a thin oxide layer, and on top of it a metal layer is deposited, which acts as the Gate terminal.


* **Channel Formation & Threshold Voltage ($V_{th}$):**
  - **At $V_{gs} = 0V$:** The source-substrate and drain-substrate form reverse-biased p-n junctions. Channel resistance is extremely high, and the device remains **Off**.
  - **Applying $+ve$ $V_{gs}$:** Positive charge on the gate repels holes and starts accumulating negative charges (electrons) underneath the gate oxide layer.
  - **Inversion Layer:** Once $V_{gs}$ exceeds the threshold voltage ($V_{th}$), the semiconductor surface completely inverts into an **n-type material**, opening a conducting channel for current to flow from drain to source.
* **Body Effect:** When an additional reverse bias voltage ($V_{sb}$) is applied between the source and the body substrate, the depletion layer width broadens near the source, which directly increases the device's threshold voltage ($V_{th}$).

### NMOS Structure and Channel Inversion
![NMOS Cross Section Architecture](path_to_your_nmos_structure_image.png)
