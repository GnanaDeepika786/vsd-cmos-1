# CMOS Circuit Design and SPICE Simulations using Sky130
This repository documents my hands-on technical progress during CMOS Circiut Desgin using SPICE Simulations using SKY130 workshop organized by VLSI System Design(VSD).YHe focus of this workshop is device-level characterizationand robustness analysis using open-source EDA tools

**Workshop Flow:**
* Module 1: MOSFET Fundamentals & SPICE Setup
* Module 2: Velocity Saturation & CMOS Inverter Basics
* Module 3: Switching Threshold & Dynamic Behaviour
* Module 4: Noise‑Margin Robustness Analysis
* Module 5: Power‑Supply & Process‑Variation Evaluation

**Tools Used:**
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

 ![Delay Table](CMOS-5(30775710287972).jpg)
![](CMOS-6(30775811277372).jpg)
 
Therefore, SPICE plays a fundamental role in CMOS circuit design, as it allows us to characterize and
evaluate circuit performance accurately before implementation.


### L2: Introduction to Basic Element in Circuit Design – NMOS
An NMOS is a **4-terminal device** composed of a Gate (G), Source (S), Drain (D), and Body/Substrate (B) built over a P-substrate with $n^+$ diffusion regions(source and drain).

Above the substrate, there is a thin oxide layer, and on top of it a metal layer is deposited, which acts as the Gate terminal.
(CMOS-8(30775901979921).jpg)

#### Threshold Voltage:
Threshold voltage ($Vt$) is a very important parameter in MOSFET operation, all the characteristics of a device depends on this value.

Initially,

**At $V_{gs} = 0V$:**
*  Means source,drain and body terminals are grounded.
*  P-substrate and n+ doped regions act as reverse-biased PN junction diode and as there is no potential so there is a high resistance. **No channel formation** is there.The device remains **Off**

![](CMOS-10(30776008616005).jpg)
  
**Applying $+ve$ $V_{gs}>0$:**
* Now positive charge on the gate repels holes or positive charge in substrate and starts attracting negative charges (electrons) below the gate oxide layer in the substrate.
* This is beginning of the channel formation

![](CMOS-11(30776084500038).jpg)
![](CMOS-12(30776129719787).jpg)

### L3: Strong inversion and threshold voltage
Due to the accumulation of negative charges,there will be formation of Depletion Region, depleting of substrates majority carriers i.e positive carriers here .
  The further increase in the gate voltage $V_{gs}$ results
  * More positive carriers are repelled
  * Increase of depletion region width

![](CMOS-12(30776129719787).jpg)
 
  * **Inversion Layer:** At some point, the semiconductor surface completely converts into an **n-type material** from p-type,this process is called **Strong or Surface inversion**.
  * The gate voltage at which the strong inversion happens is called the **Threshold Voltage**

![](CMOS-13(30776190348334).jpg)

What will happen if we further increase Vgs>Vt?
* As there are no more negative charges in the substrate that will be attracted towards the positive Vgs, The negative charges from n+ region will get attracted opening a conducting channel for current to flow from drain to source.
* However,since there is no drain voltage,the current cannot flow from source to drain.

![](CMOS-14(30776279997966).jpg)

Now let us observe what happens when we change the **body (substrate) potential**.

The depletion region beteween the source and body increases due to the reverse bias at source terminal

![](CMOS-15(30776344977812).jpg)

### L4:Threshold voltage with positive substrate potential
If we increase the Vgs,depletion region wil increase in both the cases. But in the second case as the +ve Vsb pulls few charges from channel will be pulled towards the source.
* Results in slower inversion
* Increases the value of threshold potential due to +ve Vsb

![](CMOS-16(30776429513540).jpg)

![](CMOS-17(30776499522357).jpg)

![](CMOS-18(30776561651865).jpg)


The relation between threshold voltage and substrate bias is given by parameters such as Gamma (γ), which are obtained from the fabrication process.

![](CMOS-19(30776633170989).jpg)

* **Conclusion(Body Effect):** When an additional reverse bias voltage ($V_{sb}$) is applied between the source and the body substrate, the depletion layer width broadens near the source, which directly increases the device's threshold voltage ($V_{th}$).

##  NMOS resistive region and saturation region of operation

## L1: Resistive region of operation with small drain-source voltage
In previous lectures, we have studied about the cut-off region. Now, we will study **the Resistive or Linear** region by applying a small drain-to-source voltage (Vds).

As the gate voltage (Vgs) increases:

* The width of channel increases
* More charge carriers are available for Conduction Channel Formation

![](CMOS-21(30776790474539).jpg)

![](CMOS-22(30776846132191).jpg)

 The induced charge in the channel is proportional to:

**(Vgs - Vt)**

Now, let us Assume:
Vds = 0.05V
Vt = 0.45V
Vgs is slightly greater than Vt and small initially 

![](CMOS-23(30776913061288).jpg)

Since the source is grounded and drain is at some potential there will be formation of voltage gradient along the channel



The Effective Channel Width is slightly smaller than the actual channel width due to some fabrication factors

![](CMOS-24(30776987423290).jpg)

 Here
 * y axis → the width of transistor
 * x axis →  voltage across the channel.
On applying Vds, every point on x axis will vary w.r.t to Vgs-V(x), this will decide the current equation.

### L2: Drift current theory
















