# VSD — CMOS Circuit Design on Cloud (with GUI/VNC)

Run **Sky130 CMOS design labs** on GitHub Codespaces — entirely in your browser, with **ngspice**, **noVNC GUI desktop**, and ready-to-use **SPICE simulation decks**.
No local setup required — everything runs inside your browser tab. CMOS Circuit Design and SPICE Simulations using Sky130


This repository documents my hands-on technical progress during CMOS Circiut Desgin using SPICE Simulations using SKY130 workshop organized by VLSI System Design(VSD).YHe focus of this workshop is device-level characterizationand robustness analysis using open-source EDA tools

**Workshop Flow:**
* Module 1: MOSFET Fundamentals & SPICE Setup
* Module 2: Velocity Saturation & CMOS Inverter Basics
* Module 3: Switching Threshold & Dynamic Behaviour
* Module 4: Noise‑Margin Robustness Analysis
* Module 5: Power‑Supply & Process‑Variation Evaluation


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
![](CMOS-8(30775901979921).jpg)

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
We know that the effective channel voltage (Veff) will vary w.r.t x, 
sky-2(386973605301613).jpg)
Example:
* At x = 0 → Vgs - V(x) = 1V
* At x = Vds → Vgs - V(x) = 0.95V

![](sky-3(386973625413597).jpg)

The induced charge equation is proportional to the effective channel voltage and depends on the position x

**The types of current:**

* Drift current
* Diffusion current
Because of the potential difference across the channel there exists a drift current

Here, drift current dominates because of the electric field.

![](sky-4(386973653148953).jpg)

To calculate drain current, we consider the top view of the transistor.

![](sky-5(386973679111348).jpg)

 ### L3: Drain current model for linear region of operation
Since there is a voltage variation along the channel which results in variation in carrier velocity.

Velocity depends on the following factors:
* Mobility (μ)
* Electric field (E)

![](sky-7(386973715675750).jpg)

By integrating the above equation where,
* limits of dV will be from 0 to Vds.
* limits of dx will be from 0 to L.

![](sky-8(386973733585404).jpg)

![](sky-9(386973753458695).jpg)

Technology parameters:

* Oxide Capacitance (Cox)
* Width and Length (W/L) ratio
* Mobility (μn)
* Threshold voltage (Vt)
These parametes are helpful in SPICE simulations to find out the characteristics.

 ![](sky-10(386973772798949).jpg)

 But, here we cannot say that it is in Linear region, since the Drain current is the quadratic function of Vds. We will calculate the Id with the given values.

 Now we can say that Even though equation is quadratic in Vds, the device behaves as **linear** when:

**(Vgs - Vt) ≥ Vds**

### L4: SPICE Conclusion for Resistive Operation
To analyze the impact of Vgs and Vds on the drain current,we will consider different values of Vgs and Vds as shown
* Sweep Vgs
* Sweep Vds
Condition for linear region:

(Vgs - Vt) > Vds

![](sky-11(386973790176363).jpg)

![](sky-12(386973828538815).jpg)

But calculating Id for different values of Vgs and sweeping Vds until (Vgs-Vt) at every value is complex
So,to calculate Id for different values:

We will use **SPICE simulation** here.

### L5: Pinch-off Region and Saturation
When Vds exceeds the value (Vgs-Vt) the region of operation is called "Saturation Region". We know the channel voltage is Vgs-Vds. Now, we will increase the Vds.

* When Vgs-Vds > Vt, there will be a conducting channel.

![](sky-17(386973910069199).jpg)
  
* When Vgs-Vds = Vt, At drain side,  Inversion has just happened as it is equal to Vt, so channel will start disappearing at drain side.This is the beginnig of the **Pinch-off**

![](sky-20(386973965146073).jpg)

![](sky-23(386974024666750).jpg)

![](sky-25(386974063189743).jpg)

* When Vgs-Vds < Vt, the channel has disappeared at drain side.

![](sky-24(386974047396548).jpg)

This region is called as "Saturation region".

### L6: Drain current model for saturation region of operation
At saturation region, the channel voltage is constant i.e 'Vgs-Vt', and the drain current will not depend on Vds.
To get drain current equation in saturation region we will replace Vds as Vgs-Vt.

image

According to the equation, the mosfet acts as perfect current source. But this is not true.
* we can see that increasing Vds also increases the depletion region at the Drain which reduces the effective channel length
* This causes slight increase in current (Resembles slight dependence of Vgs over Id )

![](x22.png)

![](x23.png)

This effect is called as ***Channel Length Modulation**

## Introduction to SPICE

### L1: Basic SPICE Setup

First, let us understand the SPICE simulation setup.

**SPICE Setup**

![](sky-27(386974101833700).jpg)

Some parameters are fixed and provided by the foundry. These are pre-defined and do not need to be calculated manually.

![](sky-28(386974118249374).jpg)

![](sky-29(386974133925966).jpg)

By providing:

* SPICE model parameters
* SPICE netlist

When we feed the SPICE model parameters and SPICE netlist into the SPICE software,
We can obtain device characteristics such as Id vs Vds for different Vgs values.

**SPICE Netlist** is used to feed the MOSFET device into SPICE engine in specific manner by defining its equivalent circuit in SPICE for simulation.

![](sky-32(386974184789352).jpg)


### L2: Circuit Description in SPICE Syntax
To write a SPICE netlist, follow these steps:

* Define nodes
* Assign names to nodes
* Write component connections

![](sky-33(386974202624791).jpg)

A MOSFET has 4 terminals:

* Drain
* Gate
* Source
* Substrate
It is written in the order: D G S B (DGSS format)
The SPICE Syntax of a MOSFET underly b/w 4 terminals
![](sky-34(386974220741196).jpg)
![](sky-35(386974238402619).jpg)
![](sky-36(386974257266820).jpg)
![](sky-37(386974278015499).jpg)
![](sky-38(386974294815899).jpg)
![](sky-39(386974311765303).jpg)
![](sky-40(386974335407732).jpg)
![](sky-41(386974359259653).jpg)

This represents a long-channel MOSFET.

Similarly, SPICE syntax for a resistor underly b/w 2 terminals is:

![](sky-42(386974370958253).jpg)
![](sky-43(386974384667844).jpg)
![](sky-44(386974398454968).jpg)
![](sky-45(386974420193147).jpg)

Similarly, SPICE syntax for a voltage source underly b/w 2 terminal is:

![](sky-46(386974444587193).jpg)
![](sky-47(386974462009678).jpg)
![](sky-48(386974480144889).jpg)
![](sky-49(386974505627060).jpg)
![](sky-50(386974523962873).jpg)
![](sky-51(386974542141209).jpg)
![](sky-52(386974557448142).jpg)

### L3: Define Technology Parameters
Each MOSFET requires a model file containing technology parameters.Now we will look for model of this particular NMOS.By using the technology parameters in model file it is easy to model the NMOS. The models for the name NMOS will be found in file which has the attribute of the similar name.

![](sky-53(386974572430679).jpg)

These parameters are defined inside model libraries.

![](sky-54(386974588670374).jpg)

We include this packaged file in .mod file and call this file in the SPICE netlist.

![](sky-55(386974609440467).jpg)

![](sky-56(386974630418326).jpg)

In SPICE, Lines starting with ** * ** represents the comments.

![](sky-57(386974649025313).jpg)

![](sky-58(386974673603024).jpg)

To analyze MOSFET behavior, we sweep Vgs and Vds.

![](sky-60(386974706706238).jpg)

![](sky-61(386974722335694).jpg)

### L4: First SPICE Simulation

* Open VirtualBox or Codespace
* Open terminal
* Clone the repository:
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git

![](1.jpg)

Open the sky130_fd_pr directory by using commands in the image shown we will see cells, models and tech files.

![](2.jpg)

Inside the cells files we will see nfet and pfet cells, these cells we will be using.

![](3.jpg)

Inside nfet we will see spice libraries at different corners, we will select one such typical corner.

![](4.jpg)

We will see all the model paramteres required for the process.

![](5.jpg)

![](6.jpg)

We have different W and L values which pre-described. For simulation we need to take any one value which is present inside the library.

![](7.jpg)

Now go inside models, We will see library files which are present for nfet and pfet. 

![](8.jpg)

In sky130.lib.spice, The corner files are present, include Typical, slow-fast and fast-fast corner files.

![](9.jpg)

![](10.jpg)

![](11.jpg)

Now let us go into the all.spice file

![](12.jpg)

![](13.jpg)

![](14.jpg)

![](15.jpg)

Now open design → day1:

![](16.jpg)

![](17.jpg)

**Input:**

From the above image,
* Vdd is sweeping from 0 to 1.8V with step size of 0.1V
* Vgs is sweeping from 0 to 1.8V with step size of 0.2V

Now Let us run SPICE simulations:

![](18.jpg)

 Give plot **-Vdd#branch** command 
 
![](19.jpg)

**Output:**
* Id vs Vds curves for different Vg

![](20.jpg)

![](21.jpg)

![](22.jpg)

![](23.jpg)



# Day2-Velocity saturation and basics of CMOS inverter VTC
## SPICE simulation for lower nodes and velocity saturation effect
### L1 SPICE simulation for lower nodes
### L2 Drain current vs gate voltage for long and short channel device
### L3 Velocity saturation at lower and higher electric fields
### L4 Velocity saturation drain current model
### L5 Labs Sky130 Id-Vgs
### L6 Labs Sky130 Vt
## CMOS voltage transfer characteristics (VTC)
### L1 MOSFET as a switch
### L2 Introduction to standard MOS voltage current parameters
### L3 PMOS/NMOS drain current vs drain voltage
### L4 Step1- Convert PMOS gate-source-voltage to Vin
### L5 Step2 & Step3- Convert PMOS and NMOS drain-source-voltage to Vout
### L6 Step4- Merge PMOS-NMOS load curves and plot VTC

# Day3-CMOS switching threshold and dynamic simulations
## Voltage transfer characteristics-SPICE simulations
### L1 SPICE deck creation for CMOS inverter
### L2 SPICE simulation for CMOS inverter
### L3 Labs Sky130 SPICE simulation for CMOS
## Static behaviour evaluation-CMOS inverter robustness-Switching Threshold
### L1 Switching Threshold, Vm
### L2 Analytical expression of Vm as a function of (W/L)n and (W/L)p
### L3 Analytical expression of (W/L)n and (W/L)p as a function of Vm
### L4 Static and Dynamic simulation of CMOS inverter
### L5 Static and Dynamic simulation of CMOS inverter with increased PMOS width
### L6 Applications of CMOS inverter in clock network and STA


# Day4-CMOS Noise Margin robustness evaluation

## Static behaviour evaluation-CMOS inverter robustness-Noise Margin

### L1 Introduction to Noise Margin

Let us discuss the CMOS robustness towards Noise Margin

**Noise Margin:** It is a measure of how much unwanted electrical noise a logic circuit can tolerate on its input without producing an incorrect output.It explains allowable noise range in CMOS inverter while transmitting logic 0 and 1.

* **Ideal Inverter**
  
We observe that the output instantly switchines from higher voltage to lower voltage (VDD to 0) at VDD/2 and the slope is infinite
  
![](p4.jpg)

* **Actual Inverter**
  
In practical, Due to presence of resistances and capacitances of a CMOS inverter there will be delay which gives a finite slope and transition become gradual.Also,unlike ideal the ouput Voltages can't exactly equal to VDD and 0

slope = -1

![](p5.jpg)

![](p7.jpg)

* VIL - Low Input Voltage
  - Any input voltage level between 0 and VIL  will be treated as logic 0
* VOL - Low output Voltage
  - Any input voltage level between 0 and VOL will be treated as logic 0
* VIH - High Input Voltage
  - Any input voltage level between VIH and VDD will be treated as logic 1
* VOH - High output Voltage
  - Any input voltage level between VOH and VDD will be treated as logic 1

### L2 Noise Margin voltage paramters
Let's see more practical case,

![](p10.jpg)

* When the 0<Vin<VIL --> output is VOH<Vout<Vdd ;
* When the input is VOL<Vin<Vdd --> output is 0<Vout<VOL
* At VOL<VOH<Vdd as VOH will become output high for the next inverter
* At 0<VOL<VIL as VOL will become output low for the next inverter.



Observation:

* Slope is approximately equal to -1.
  For example,

  Slope = change in output voltage /change in input voltage

      = (800mV -900mV)/(300mV-200mV)

      = -1

* Output voltage reduces with the increase in input voltage


### L3 Noise margin equation and summary
Plot the Voltages on a scale from 0 to VDD

![](p11.jpg)

Let us calculate the Noise Margin equation 

![](p12.jpg)

These are the tolerable levels of noise

* **High Noise Margin (NMH)** = VOH − VIH
* **Low Noise Margin (NML)** = VIL − VOL

In the Undefined region,the output logic level can swing between 'high' and 'low' which is a metastate.

![](p13.jpg)

Bumps in the tolerable region won't harm the output of a circuit.

But for the Bumps in between VIL to VIH , the output is undefined.

### L4 Noise margin variation with respect to PMOS width

Now let's observe the Noise Margin for different PMOS widths

For wide range of noises,we consider
- For PMOS : logic 1 on capacitor
- For NMOS : logic 0 on capacitor

* Find the points where slope = -1
* calculate NMH and NML

![](p14.jpg)

![](p15.jpg)

![](p16.jpg)

![](p17.jpg)

![](p18.jpg)

PMOS width is responsible for getting capacitance charged which creates low resistance path as a result of it is able to hold charges for longtime

![](p19.jpg)

Due to fabrication imperfections ,Wp is not exactly equal to the x.Wn ,but CMOS Noise margin won't affect as we observe that NMH increases to certain point w.r.t PMOS width i.e 0.3 to 0.42 and then it doen't change then.This shows the CMOS Robustness towards Noise Margin

The suitable ranges for Digital Design and Analog Design of CMOS VTC are

![](p20.jpg)

![](p21.jpg)


### L5 Sky130 Noise margin labs

Let us calculate Noise Margin through SPICE simulation

Let us go to the Day 4 file

![](p22.jpg)

We are taking 
* Wp/Wn = 2.77
* Tuning/sweeping the Vin from 0 to 1.8V with stepsize of 0.01V

![](p23.jpg)

Run the Simulation

![](p24.jpg)

![](p25.jpg)

We will take the point where the slope is -1 ; 

![](p27.jpg)

We get
* VOH = 1.70141
* VIH = 0.97479
* VOL = 0.107042
* VIL = 0.773109

** High Noise margin NMH = VOH - VIH = 1.70141 - 0.97479 = 0.726 **

** Low Noise margin NML = VIL - VOL = 0.773109 - 0.107042 = 0.66 **


# Day5-CMOS power supply and device variation robustness evaluation

## Static behaviour evaluation-CMOS inverter robustness-Power supply variation

### L1 Smart SPICE simulations for power supply variations

Power Supply Scaling is also tells about the robustness of an CMOS inverter.

When gate length scales reduced:

* Supply voltage (VDD) is reduced
* Power consumption reduces
* But circuit performance sholud not change

Let us observe this Power supply scaling by following simulations

![](images/y1.jpg)

Let us check whether CMOS inverter characteristics remain stable under different VDD values.

![](images/y2.jpg)

![](images/y3.jpg)

We can see that as VDD decreases,Output swing reduces and Transition becomes slower which reduced the gain

![](images/y5.jpg)

### L2 Advantages and disadvantages using low supply voltage

Let us observe the **Gain factor** for the above waveform

* Gain = Change in output voltage/change in input voltage

     = dVout/dVin

![](images/y8.jpg)

![](images/y9.jpg)

We get
* For High VDD → Higher gain
* For Low VDD → Reduced gain

![](images/y11.jpg)

Energy efficiency is improved

Power Consumpution has decreased with the value of VDD and the disadvantages of low supply voltage Due to low supply voltage, the charging and discharging of load capacitor becomes very slow, due to this the Both rise delay and fall delay will increase and lead to a performance impact.

![](images/y13.jpg)

![](images/y14.jpg)

![](images/y15.jpg)

![](images/y16.jpg)

![](images/y17.jpg)

![](images/y18.jpg)

![](images/y19.jpg)

![](images/y20.jpg)

The advantages and disadvantages are

![](images/y21.jpg)


### L3 Sky130 Supply variation Labs

Let us go to the Day 5 supply variation file.

![](images/y22.jpg)

The initial supply voltage is 1.8V and we are reducing it with the step of 0.2V, so there will be 6 iterations.

![](images/y23.jpg)

Run the simulation

![](images/y24.jpg)

![](images/y25.jpg)

Now let us calculate the gain

* Vdd=1.8V

![](images/y25.jpg)

|dy| =| 0.112676 - 1.69014| = 1.577464
|dx| = |0.976068 - 0.774359| = 0.201709

|Gain| = 7.82

If we do for Vdd=0.8V ,we get |Gain| = 9.38


## Static behaviour evaluation-CMOS inverter robustness-Device variation
### L1 Sources of variation - Etching process
Let us discuss about the Sources of Variation
* Etching Process
* Oxide Thickness

![](images/y27.jpg)

**Etching Process :** In a single inverter layout, we will see the length of gate, the width(common area between polysilicon and diffusion).

There will be a variation in gate length and gate width of CMOS due to the inacuraccies in the Etching Process

![](images/y28.jpg)

In the inverter chain, the variation can vary with different inverter like with respect to the position of inverter as shown in the diagrams.

![](images/y29.jpg)

![](images/y30.jpg)

![](images/y33.jpg)

* Edges - More Variation
* Center -less Variation

![](images/y35.jpg)

Hence,the change in w and L can change the drain current of CMOS inverter.


### L2 Sources of variation - Oxide thickness

**Oxide Thickness :** If we see the cross-sectional area of CMOS inverter, we can observe the oxide under polysilicon gate.During the fabrication oxide thickness may vary.

![](images/y37.jpg)

![](images/y38.jpg)

Observe the ideal thickness and actual thickness.

![](images/y40.jpg)

![](images/y41.jpg)

* **Cox=Eox/Tox**
* The change in Tox can change the drain current (from equation) which leads to change in characterisitcs of CMOS inverter.

![](images/y43.jpg)

### L3 Smart SPICE simulation for device variations

![](images/y44.jpg)

For device variations, let us simulate two extreme conditions.
* For strong PMOS and week NMOS : PMOS width is wider and it has least resistance, NMOS has high resistance
* For weak PMOS and strong PMOS : NMOS width is wider and it has least resistance, PMOS has high resistance

![](images/y45.jpg)

![](images/y46.jpg)

![](images/y47.jpg)

### L4 Conclusion

![](images/y48.jpg)

![](images/y49.jpg)

We can observe that

* Strong PMOS:
 - Higher pull-up strength
 - Vm shifts to right

* Strong NMOS:
  -Higher pull-down strength
  -Vm shifts to left
  
* In both the extreme cases there is a negelible variation, Therefore it behaves as a robust inverter in both the cases.

* It shows that CMOS inverter still maintains its functionality irrespective of power supply device variations

### L5 Sky130 device variations labs

Let us go to the Day 5 device variation file.

![](images/y51.jpg)

Considering width of WP > WN.It is strong PMOS and weak NMOS.

![](images/y52.jpg)

Run the simulation

![](images/y53.jpg)

![](images/y54.jpg)

![](images/y55.jpg)

![](images/y56.jpg)

**Observation :**
* Vm shifts towards right


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






