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
 
 ![Inverter Circuit](images/CMOS-1(30775193327552).jpg)
 
 The above inverter circuit has certain electrical characteristics. To understand its behavior, we perform SPICE simulations which help us analyze important parameters such as delay, switching behavior, and performance. Based on these results, we can determine the proper W/L (Width/Length) ratio of the transistors.

### VTC Circuit and Waveform Plot
![CMOS Inverter VTC Characteristics](images/CMOS-2(30775566694972).jpg)

* **Voltage Transfer Characteristics (VTC):** By plotting $V_{out}$ against $V_{in}$, we observe how the circuit switches b/w pmos and nmos. 
* **Operating Regions:**
  - When $V_{in} = 0V$, PMOS is in **Linear** and NMOS is **Off** ($V_{out} = V_{DD}=2$).
  - As $V_{in}$ reaches the center threshold ($V_{in} = 1V$), both transistors enter the **Saturation** region simultaneously.
  - When $V_{in} = 2V$, PMOS is **Off** and NMOS operates in the **Linear** region ($V_{out} = 0V$).


### Why do we need SPICE?

 Clock Tree Synthesis (CTS), crosstalk, and timing analysis are completely dependent on 
SPICE (Simulation Program with Integrated Circuit Emphasis).Without SPICE, it would not be possible to calculate delays, and without delay information, 
physical design and timing verification would not be meaningful.

![Clock Network](images/CMOS-4(30775650902025).jpg)

 Suppose we perform CTS on a circuit using buffers that are connected with different capacitive loads at their outputs.
By performing SPICE simulation ,we get a **Delay Table** for these buffer cells.

* **Buffer Trees:** Buffering is used across multiple levels (Level 1, Level 2) to maintain a clean clock signal and balance capacitive loads.
* **Delay Tables (Look-up Tables):** In Static Timing Analysis (STA), circuit delays are calculated dynamically based on two key parameters:
  1. **Input Slew:** The transition speed of the incoming signal.
  2. **Output Load:** The total capacitance ($fF$) the node needs to drive.

By mapping the input slew (e.g., $40ps$, $60ps$) against output load (e.g., $30fF$, $50fF$) in the lookup matrices, we can determine the precise delay value at their intersection point.
This delay tables for the level 1 and level 2 buffers is a direct result of transistor-level circuit design and SPICE simulation.

 ![Delay Table](images/CMOS-5(30775710287972).jpg)
![](images/CMOS-6(30775811277372).jpg)
 
Therefore, SPICE plays a fundamental role in CMOS circuit design, as it allows us to characterize and
evaluate circuit performance accurately before implementation.


### L2 Introduction to Basic Element in Circuit Design – NMOS
An NMOS is a **4-terminal device** composed of a Gate (G), Source (S), Drain (D), and Body/Substrate (B) built over a P-substrate with $n^+$ diffusion regions(source and drain).

Above the substrate, there is a thin oxide layer, and on top of it a metal layer is deposited, which acts as the Gate terminal.
![](images/CMOS-8(30775901979921).jpg)

#### Threshold Voltage:
Threshold voltage ($Vt$) is a very important parameter in MOSFET operation, all the characteristics of a device depends on this value.

Initially,

**At $V_{gs} = 0V$:**
*  Means source,drain and body terminals are grounded.
*  P-substrate and n+ doped regions act as reverse-biased PN junction diode and as there is no potential so there is a high resistance. **No channel formation** is there.The device remains **Off**

![](images/CMOS-10(30776008616005).jpg)
  
**Applying $+ve$ $V_{gs}>0$:**
* Now positive charge on the gate repels holes or positive charge in substrate and starts attracting negative charges (electrons) below the gate oxide layer in the substrate.
* This is beginning of the channel formation

![](images/CMOS-11(30776084500038).jpg)
![](images/CMOS-12(30776129719787).jpg)

### L3 Strong inversion and threshold voltage
Due to the accumulation of negative charges,there will be formation of Depletion Region, depleting of substrates majority carriers i.e positive carriers here .
  The further increase in the gate voltage $V_{gs}$ results
  * More positive carriers are repelled
  * Increase of depletion region width

![](images/CMOS-12(30776129719787).jpg)
 
  * **Inversion Layer:** At some point, the semiconductor surface completely converts into an **n-type material** from p-type,this process is called **Strong or Surface inversion**.
  * The gate voltage at which the strong inversion happens is called the **Threshold Voltage**

![](images/CMOS-13(30776190348334).jpg)

What will happen if we further increase Vgs>Vt?
* As there are no more negative charges in the substrate that will be attracted towards the positive Vgs, The negative charges from n+ region will get attracted opening a conducting channel for current to flow from drain to source.
* However,since there is no drain voltage,the current cannot flow from source to drain.

![](images/CMOS-14(30776279997966).jpg)

Now let us observe what happens when we change the **body (substrate) potential**.

The depletion region beteween the source and body increases due to the reverse bias at source terminal

![](images/CMOS-15(30776344977812).jpg)

### L4 Threshold voltage with positive substrate potential
If we increase the Vgs,depletion region wil increase in both the cases. But in the second case as the +ve Vsb pulls few charges from channel will be pulled towards the source.
* Results in slower inversion
* Increases the value of threshold potential due to +ve Vsb

![](images/CMOS-16(30776429513540).jpg)

![](images/CMOS-17(30776499522357).jpg)

![](images/CMOS-18(30776561651865).jpg)


The relation between threshold voltage and substrate bias is given by parameters such as Gamma (γ), which are obtained from the fabrication process.

![](images/CMOS-19(30776633170989).jpg)

* **Conclusion(Body Effect):** When an additional reverse bias voltage ($V_{sb}$) is applied between the source and the body substrate, the depletion layer width broadens near the source, which directly increases the device's threshold voltage ($V_{th}$).

##  NMOS resistive region and saturation region of operation

## L1 Resistive region of operation with small drain-source voltage
In previous lectures, we have studied about the cut-off region. Now, we will study **the Resistive or Linear** region by applying a small drain-to-source voltage (Vds).

As the gate voltage (Vgs) increases:

* The width of channel increases
* More charge carriers are available for Conduction Channel Formation

![](images/CMOS-21(30776790474539).jpg)

![](images/CMOS-22(30776846132191).jpg)

 The induced charge in the channel is proportional to:

**(Vgs - Vt)**

Now, let us Assume:

Vds = 0.05V

Vt = 0.45V

Vgs is slightly greater than Vt and small initially 

![](images/CMOS-23(30776913061288).jpg)

Since the source is grounded and drain is at some potential there will be formation of voltage gradient along the channel

The Effective Channel Width is slightly smaller than the actual channel width due to some fabrication factors

![](images/CMOS-24(30776987423290).jpg)

 Here
 * y axis → the width of transistor
 * x axis →  voltage across the channel.
On applying Vds, every point on x axis will vary w.r.t to Vgs-V(x), this will decide the current equation.

### L2 Drift current theory
We know that the effective channel voltage (Veff) will vary w.r.t x, 
sky-2(386973605301613).jpg)
Example:
* At x = 0 → Vgs - V(x) = 1V
* At x = Vds → Vgs - V(x) = 0.95V

![](images/sky-3(386973625413597).jpg)

The induced charge equation is proportional to the effective channel voltage and depends on the position x

**The types of current:**

* Drift current
* Diffusion current
Because of the potential difference across the channel there exists a drift current

Here, drift current dominates because of the electric field.

![](images/sky-4(386973653148953).jpg)

To calculate drain current, we consider the top view of the transistor.

![](images/sky-5(386973679111348).jpg)

 ### L3 Drain current model for linear region of operation
Since there is a voltage variation along the channel which results in variation in carrier velocity.

Velocity depends on the following factors:
* Mobility (μ)
* Electric field (E)

![](images/sky-7(386973715675750).jpg)

By integrating the above equation where,
* limits of dV will be from 0 to Vds.
* limits of dx will be from 0 to L.

![](images/sky-8(386973733585404).jpg)

![](images/sky-9(386973753458695).jpg)

Technology parameters:

* Oxide Capacitance (Cox)
* Width and Length (W/L) ratio
* Mobility (μn)
* Threshold voltage (Vt)
These parametes are helpful in SPICE simulations to find out the characteristics.

 ![](images/sky-10(386973772798949).jpg)

 But, here we cannot say that it is in Linear region, since the Drain current is the quadratic function of Vds. We will calculate the Id with the given values.

 Now we can say that Even though equation is quadratic in Vds, the device behaves as **linear** when:

**(Vgs - Vt) ≥ Vds**

### L4 SPICE Conclusion for Resistive Operation
To analyze the impact of Vgs and Vds on the drain current,we will consider different values of Vgs and Vds as shown
* Sweep Vgs
* Sweep Vds
Condition for linear region:

(Vgs - Vt) > Vds

![](images/sky-11(386973790176363).jpg)

![](images/sky-12(386973828538815).jpg)

But calculating Id for different values of Vgs and sweeping Vds until (Vgs-Vt) at every value is complex
So,to calculate Id for different values:

We will use **SPICE simulation** here.

### L5 Pinch-off Region and Saturation
When Vds exceeds the value (Vgs-Vt) the region of operation is called "Saturation Region". We know the channel voltage is Vgs-Vds. Now, we will increase the Vds.

* When Vgs-Vds > Vt, there will be a conducting channel.

![](images/sky-17(386973910069199).jpg)
  
* When Vgs-Vds = Vt, At drain side,  Inversion has just happened as it is equal to Vt, so channel will start disappearing at drain side.This is the beginnig of the **Pinch-off**

![](images/sky-20(386973965146073).jpg)

![](images/sky-23(386974024666750).jpg)

![](images/sky-25(386974063189743).jpg)

* When Vgs-Vds < Vt, the channel has disappeared at drain side.

![](images/sky-24(386974047396548).jpg)

This region is called as "Saturation region".

### L6 Drain current model for saturation region of operation
At saturation region, the channel voltage is constant i.e 'Vgs-Vt', and the drain current will not depend on Vds.
To get drain current equation in saturation region we will replace Vds as Vgs-Vt.

image

According to the equation, the mosfet acts as perfect current source. But this is not true.
* we can see that increasing Vds also increases the depletion region at the Drain which reduces the effective channel length
* This causes slight increase in current (Resembles slight dependence of Vgs over Id )

![](images/x22.png)

![](images/x23.png)

This effect is called as ***Channel Length Modulation**

## Introduction to SPICE

### L1 Basic SPICE Setup

First, let us understand the SPICE simulation setup.

**SPICE Setup**

![](images/sky-27(386974101833700).jpg)

Some parameters are fixed and provided by the foundry. These are pre-defined and do not need to be calculated manually.

![](images/sky-28(386974118249374).jpg)

![](images/sky-29(386974133925966).jpg)

By providing:

* SPICE model parameters
* SPICE netlist

When we feed the SPICE model parameters and SPICE netlist into the SPICE software,
We can obtain device characteristics such as Id vs Vds for different Vgs values.

**SPICE Netlist** is used to feed the MOSFET device into SPICE engine in specific manner by defining its equivalent circuit in SPICE for simulation.

![](images/sky-32(386974184789352).jpg)


### L2 Circuit Description in SPICE Syntax
To write a SPICE netlist, follow these steps:

* Define nodes
* Assign names to nodes
* Write component connections

![](images/sky-33(386974202624791).jpg)

A MOSFET has 4 terminals:

* Drain
* Gate
* Source
* Substrate
It is written in the order: D G S B (DGSS format)
The SPICE Syntax of a MOSFET underly b/w 4 terminals
![](images/sky-34(386974220741196).jpg)
![](images/sky-35(386974238402619).jpg)
![](images/sky-36(386974257266820).jpg)
![](images/sky-37(386974278015499).jpg)
![](images/sky-38(386974294815899).jpg)
![](images/sky-39(386974311765303).jpg)
![](images/sky-40(386974335407732).jpg)
![](images/sky-41(386974359259653).jpg)

This represents a long-channel MOSFET.

Similarly, SPICE syntax for a resistor underly b/w 2 terminals is:

![](images/sky-42(386974370958253).jpg)
![](images/sky-43(386974384667844).jpg)
![](images/sky-44(386974398454968).jpg)
![](images/sky-45(386974420193147).jpg)

Similarly, SPICE syntax for a voltage source underly b/w 2 terminal is:

![](images/sky-46(386974444587193).jpg)
![](images/sky-47(386974462009678).jpg)
![](images/sky-48(386974480144889).jpg)
![](images/sky-49(386974505627060).jpg)
![](images/sky-50(386974523962873).jpg)
![](images/sky-51(386974542141209).jpg)
![](images/sky-52(386974557448142).jpg)

### L3 Define Technology Parameters
Each MOSFET requires a model file containing technology parameters.Now we will look for model of this particular NMOS.By using the technology parameters in model file it is easy to model the NMOS. The models for the name NMOS will be found in file which has the attribute of the similar name.

![](images/sky-53(386974572430679).jpg)

These parameters are defined inside model libraries.

![](images/sky-54(386974588670374).jpg)

We include this packaged file in .mod file and call this file in the SPICE netlist.

![](images/sky-55(386974609440467).jpg)

![](images/sky-56(386974630418326).jpg)

In SPICE, Lines starting with ** * ** represents the comments.

![](images/sky-57(386974649025313).jpg)

![](images/sky-58(386974673603024).jpg)

To analyze MOSFET behavior, we sweep Vgs and Vds.

![](images/sky-60(386974706706238).jpg)

![](images/sky-61(386974722335694).jpg)

### L4 First SPICE Simulation

* Open VirtualBox or Codespace
* Open terminal
* Clone the repository:
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git

![](images/1.jpg)

Open the sky130_fd_pr directory by using commands in the image shown we will see cells, models and tech files.

![](images/2.jpg)

Inside the cells files we will see nfet and pfet cells, these cells we will be using.

![](images/3.jpg)

Inside nfet we will see spice libraries at different corners, we will select one such typical corner.

![](images/4.jpg)

We will see all the model paramteres required for the process.

![](images/5.jpg)

![](images/6.jpg)

We have different W and L values which pre-described. For simulation we need to take any one value which is present inside the library.

![](images/7.jpg)

Now go inside models, We will see library files which are present for nfet and pfet. 

![](images/8.jpg)

In sky130.lib.spice, The corner files are present, include Typical, slow-fast and fast-fast corner files.

![](images/9.jpg)

![](images/10.jpg)

![](images/11.jpg)

Now let us go into the all.spice file

![](images/12.jpg)

![](images/13.jpg)

![](images/14.jpg)

![](images/15.jpg)

Now open design → day1:

![](images/16.jpg)

![](images/17.jpg)

**Input:**

From the above image,
* Vdd is sweeping from 0 to 1.8V with step size of 0.1V
* Vgs is sweeping from 0 to 1.8V with step size of 0.2V

Now Let us run SPICE simulations:

![](images/18.jpg)

 Give plot **-Vdd#branch** command 
 
![](images/19.jpg)

**Output:**
* Id vs Vds curves for different Vg

![](images/20.jpg)

![](images/21.jpg)

![](images/22.jpg)

![](images/23.jpg)

# Day2-Velocity saturation and basics of CMOS inverter VTC

## SPICE simulation for lower nodes and velocity saturation effect

### L1 SPICE simulation for lower nodes

We have observed the curve for Id vs Vds for different values of Vgs.

![](images/v2.jpg)

In the above graph:

* Left side of curve (Vds = Vgs − Vt) → Linear region
* Right side → Saturation region
* Bottom → Cut-off region (Device OFF)
  
This behavior is for long channel devices.

At certain transition point of Vds the region is not completely saturated nor completely linear

This is how the Id equation maps the graphical curves

Now, we take different values of W and L while keeping W/L constant. Ideally, we expect Id should remain the same, but practically, it does not obeys.

Below is the SPICE deck where only W and L are changed and everything remians same

![](images/v3.jpg)

![](images/v4.jpg)

![](images/v10.jpg)


### L2 Drain current vs gate voltage for long and short channel device

Plot Id at different values of Vgs and Vds

![](images/v12.jpg)

Let us compare the two simulations.

![](images/v13.jpg)


**Observations :**

* For long channel devices :

   At Vds = 2.5V , Id has quadratic dependency on Vgs 


* For short channel devices :
 
   For lower values of Vds,Id shows quadratic behaviour w.r.t Vgs .After certain point Vds=2.5V
  
   Id has shown linear behavior due to velocity saturation.


For Long Channel Device (L = 1.2µm): 

Now we plot Id vs Vgs graph seeping Vds to 2.5V with a step size of 2.5 i.e Vds=2.5 

This syntax means the left-side parameter is swept/tuned for every value of the right-side parameter.

![](images/v14.jpg)


For short channel device (L = 0.25µm):

![](images/v24.jpg)

Any Device L<=0.25 is a short channel device


### L3 Velocity saturation at lower and higher electric fields

For Short Channel Devices,we will see more of a linear behaviour as the Vgs increases (Id vs Vgs). This is due to velocity saturation effect.

![](images/v24.jpg)

* Higher values of Vgs - Linear Function of Id
* Lower values of Vgs - Quadratic Function of Id

Velocity Saturation is one of the Short Channel Effect which becomes one of region for operating the lower nodes 

![](images/v25.jpg)

![](images/v26.jpg)

For lower nodes,

We have have 4 regions of operations: 
* Cut Off
* Linear
* Saturation
* Velocity Saturation
  
**Velocity Saturation :**

 We know that velocity and electric field are related to each other 
 * v=uE
where,
* v is velocity
* E is electric field
* u is mobility.

Velocity increases linearly with electric field over certain electric field value(upper limit) after which it gets saturated. This is due to scattering at higher fields and mobility decreases.
* Low values of Electricfield : Velocity is linear
* High values of Electricfield : Velocity is saturated

From the Device physicis point of view

![](images/v27.jpg)

![](images/v30.jpg)

![](images/v31.jpg)

We will now Re-derive Id

![](images/v31.jpg)

![](images/v32.jpg)

The velocity saturation happens at higher values of Vgs

![](images/v34.jpg)

### L4 Velocity saturation drain current model
Here we are considering large values of Vgs

![](images/v35.jpg)

* Let Vgs − Vt = Vgt

For small values of Vds, we neglect (1+λVds) term

![](images/v36.jpg)

* Another technology parameter is **Vdsat** it defines at what value of voltage the device enter into velocity saturation
  simply ,where velocity saturation begins.

#### Cut-off Region Equation
* Device is OFF i.e Id=0

#### Saturation Region Equation
* When Vgs-Vt i.e Vgt is minimum implies Vds is maximum

![](images/v38.jpg)

![](images/v39.jpg)

#### Resistive Region Equation
* When Vds is minimum i.e the lower values derive drain current Id 

![](images/v41.jpg)

#### Velocity Saturation Region Equation

* When Vd sat is minimum

![](images/v44.jpg)

![](images/v45.jpg)


In the above equation, it seems when W is constant and L is lowered then Id should increase, But it is not so practically.

![](images/v46.jpg)

**Observation :**

The saturation current for lower nodes is low instead of being high. This is because Velocity saturation tends to saturate the device early.

Hence,Velocity saturation limits peak current So, peak current of lower nodes is smaller than the higher nodes

### L5 Labs Sky130 Id-Vgs

 Let us do SPICE Simulation for lower nodes

**Id-Vds**

Let us simulate drain characteristics
 
 Now go to the Day 2 Vds file.

![](images/v49.jpg)

 We are taking L=0.15u and W=0.39u.
 * Sweeping Vgs from 0 → 1.8V with step of 0.2V.
 * Sweeping Vds from 0 → 1.8V with step of 0.1V.

![](images/v50.jpg)
 
Run the Simulation

![](images/v51.jpg)

![](images/v52.jpg)

The above graph is Id vs Vds for different values of Vgs. 

* For low values of Vgs → Quadratic behaviour
* For high values of Vgs → Linear behavior

![](images/v53.jpg)

![](images/v54.jpg)

**Peak Current :**

For peak current,press left click on mouse at Vgs=1.8V

Peak current ≈ 197.5µA

**Id-Vgs**

let us simulate transfer characteristics

Now go to the Day 2 Vgs file.

![](images/v55.jpg)

 We are taking same L=0.15u and W=0.39u.

* Keeping Vds constant at 1.8V
* Sweeping Vgs from 0 → 1.8V with step of 0.1V.

![](images/v56.jpg)
 
Run the Simulation

![](images/v57.jpg)

![](images/v58.jpg)

The above graph is Id vs Vgs at Vds = 1.8V. 

Due to Velocity Saturation or Short Channel Effect ,we have linear behaviour for higher Vgs and Vds being constant.

### L6 Labs Sky130 Vt

Now Let us calculate Threshold Voltage Vt for Id vs Vgs curve.

To find Vt we will draw tangent on the curve and see where it touches on the x-axis.

![](images/v59.jpg)

Threshold Voltage (Vt) is the voltage where current increases drastically for small change in Vgs.

It comes at around 0.76V.

Vt ≈ 0.76V


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

Now let's simulate the VTC of CMOS inverter using a SPICE deck. It is a connectivity information (Netlist) which contains information about components of CMOS inverter, which defines:

* Component Connectivity
  
Here,

- M1 is PMOS
- M2 is NMOS

![](images/n2.jpg)
  
* Component values
  
Next, we define W/L ratios and voltage values. Equal sizing means balanced PMOS and NMOS strength.

![](images/n5.jpg)

* Identify Nodes
  
Then we identify nodes which are points where two components connect  to each other. SPICE runs simulations based on these nodes.

![](images/n6.jpg)

* Name Nodes
  
 Naming nodes ensures proper simulation which we can see them in model file 

![](images/n7.jpg)

Now let's write Spice deck

SPICE syntax for MOSFET:

* Drain Gate Source Body(substrate) (**DGSS**)

![](images/n8.jpg)


### L2 SPICE simulation for CMOS inverter

![](images/n9.jpg)

**Load Capacitance C_L**

This can be a simple MOSFET or another CMOS inverter or a logic gate

![](images/n10.jpg)

To find the VTC,we will only be sweeping the input voltage and measuring the output voltage.

* Sweep Vin from 0 → Vdd (2.5V) with step size 0.05.

![](images/n13.jpg)


**Model Files Library**

All the information about the technological parameteres is inside the model files library.

![](images/n14.jpg)

Now Let's do the SPICE simulations to get VTC
* For Wn/ln = Wp/Lp = 1.5

![](images/n18.jpg)

* For Wn/Ln=1.5, Wp/Lp=2.5 (PMOS width is 2.5 times more than NMOS)

![](images/n19.jpg)

![](images/n22.jpg)

We observed that, when PMOS strength = NMOS Strength ,the previous graph has slightly shifted left side

* Increasing PMOS width:

 - Makes PMOS stronger
 - Output stays high longer

### L3 Labs Sky130 SPICE simulation for CMOS

Now Let us plot the VTC of CMOS Inverter

Let us go to the Day 3 file

![](images/n23.jpg)

Here,
* The W/L ratio of PMOS is 2.33 times greater than NMOS.
* Sweeping Vin from 0 to 1.8V with step size of 0.01V and plotting the Vout.

![](images/n24.jpg)
  
Run the simulation type "ngspice file name" and "plot out vs in".

![](images/n25.jpg)

![](images/n26.jpg)

**Switching Threshold (Vm):** A point where Vin = Vout

Let us calculate the Switching Threshold. 

Note : To zoom in the curve; press righ mouse button + hold it.

![](images/n27.jpg)

![](images/n28.jpg)

We get,
* switching threshold (Vm) = 0.876V

**Transient analysis :** Transient analysis defines how input and output changes with respect to time.

Now let's go inside the Day 3 transient SPICE file.

![](images/n29.jpg)

* We are operating this in typical corner and W/L is same as before
* we taking transient pulse from 0v to 1V with shift of 0 with rise time and fall time being 0.1ns and 0.1ns respectively, pulse width of 2ns and total time period of 4ns.

![](images/n30.jpg)
  
Run the Simulation by **plot out vs time in**

![](images/n31.jpg)

We measure Delay at 50% of output curve (VDD) i.e 0.9V which is the switching voltage(Vm).

![](images/n32.jpg)

* **Rise Delay :** Rising part of waveform

![](images/n33.jpg)

![](images/n34.jpg)

**Rise delay = 2.484ns-2.149ns = 0.335ns**

* **Fall Delay :** Falling part of waveform

![](images/n35.jpg)

![](images/n36.jpg)

**Fall Delay = 4.335ns-4.052ns = 0.284ns**

## Static behaviour evaluation-CMOS inverter robustness-Switching Threshold

### L1 Switching Threshold, Vm

CMOS logic can be used in the logic gate designing and CMOS inveretes is a robust device.Let us understand it

![](images/a2.jpg)

Now Let us compare the two different CMOS inverters with different W/L ratios of PMOS and NMOS,we can observe that 
* Shape of the VTC is same
* Switching threshold is different which shows the robustnesss of CMOS inverter.

![](images/a4.jpg)

We can find the Switching threshold(Vm) in both the cases by drawing a 45 degree line.
* At Vin = 0 and Vout = Vdd vice versa No current flows in CMOS inverter
* At Vin = Vout = Vm the maximum current flows in CMOS inverter and it enters into Saturation

![](images/a6.jpg)

At this particular operating region Vm, 
* _Vgs>>Vt_
* There is a posibility of leakage current as the current flows directly from _Power to Load_

  ![](images/a7.jpg)

### L2 Analytical expression of Vm as a function of (W/L)n and (W/L)p
Let us now calculate the value of Switching Voltage (Vm)  w.r.t the width and lengths of NMOS and PMOS.

Ignore the value of Vt as Vgs is far greater

![](images/a8.jpg)

![](images/a9.jpg)

![](images/a10.jpg)

![](images/a13.jpg)

The values kp',Vdsat and W/L etc.. are in model files

By the equation ,We see that Vm depends on:

- Mobility (μn, μp)
- Kn',Kp'
- (W/L)n and (W/L)p
- Vdsatn,Vdsatp

### L3 Analytical expression of (W/L)n and (W/L)p as a function of Vm

Let us now calculate the value of the width and lengths of NMOS and PMOS w.r.t Switching Voltage (Vm) 

We need to choose W/L such that **Vm ≈ Vdd/2** in which CMOS acts as a symmetrical inverter.

From Current Equation **_Idsn=-Idsp_**

![](images/a.16jpg)

![](images/a17.jpg)

![](images/a18.jpg)

![](images/a19.jpg)

![](images/a20.jpg)

Now ,we can find W/L ratios if we know Vm. This will allow us to find out for what value of W/L ratio of PMOS will be greater than NMOS based on
 values of Vm.

![](images/a22.jpg)

### L4 Static and Dynamic simulation of CMOS inverter

Now let us caluculate the Vm for different widths of PMOS and observe the CMOS behaviour

* For (W/L)n = (W/L)p = 1.5

![](images/a22.jpg)

 -  _Static Simulation → VTC curve_
 -  _Dynamic Simulation → Delay behavior_

By doing Transient Analysis, we can calculate the Rise Delay and Fall Delay just like before

![](images/a35.jpg)

### L5 Static and Dynamic simulation of CMOS inverter with increased PMOS width

* (W/L)p = 2(W/L)n

![](images/a42.jpg)

* (W/L)p = 3(W/L)n

![](images/a43.jpg)

* (W/L)p = 4(W/L)n

![](images/a44.jpg)

* (W/L)p = 5(W/L)n

![](images/a45.jpg)

**Observation:**
 - As the PMOS width increases ,the rise delay significantly decreases and Vm increases eventually
 - PMOS has become more stronger which needs more current to charge the output load capacitor
 - We can see that as the Vm is increases,the graph shifted towards right 

**NOTE:** The rise delay is the time that required by output capacitor to charge completely.


### L6 Applications of CMOS inverter in clock network and STA
From the above experiments,we get

![](images/a46.jpg)

**Conclusions :**

* Due to fabrication imperfections, there can be slight variation in sizes of PMOS and NMOS from the required one, but the robustness of CMOS inverter is such that, there is not much difference in the Vm with change in sizes.

  For Example, 1.2V to 1.25V there is barle 50mV difference So, the slight variation in PMOS doesn't cause much change in Vm

* The basic requirement for a cell which lies in clock network is

  **Rise Delay = Fall Delay**

 When (W/L)p = 2(W/L)n, we see that the Rise Delay and Fall Delay are approximately equal to each other  which shows the **symmetric behaviour** of CMOS

If we set Vm, from 1.2V to 1.3V then we will get a inverter which has symmetry of particular cell. 

![](images/a47.jpg)

This is a typical characteristic of Clock Inverter/buffer where we want the rise delay and fall delay to be equal.
But in this case Rise Delay and Fall Delay are not equal 

![](images/a48.jpg)

We design an inverter for which Rise Delay and Fall Delay are equal

where ,
* PMOS resistance = NMOS resistance
  
The other types of PMOS widths will be used according to the data path requirement

![](images/a50.jpg)

![](images/a51.jpg)

* Saved Area
* Reduced delays for datapath

# Day4-CMOS Noise Margin robustness evaluation

## Static behaviour evaluation-CMOS inverter robustness-Noise Margin

### L1 Introduction to Noise Margin

Let us discuss the CMOS robustness towards Noise Margin

**Noise Margin:** It is a measure of how much unwanted electrical noise a logic circuit can tolerate on its input without producing an incorrect output.It explains allowable noise range in CMOS inverter while transmitting logic 0 and 1.

* **Ideal Inverter**
  
We observe that the output instantly switchines from higher voltage to lower voltage (VDD to 0) at VDD/2 and the slope is infinite
  
![](images/p4.jpg)

* **Actual Inverter**
  
In practical, Due to presence of resistances and capacitances of a CMOS inverter there will be delay which gives a finite slope and transition become gradual.Also,unlike ideal the ouput Voltages can't exactly equal to VDD and 0

slope = -1

![](images/p5.jpg)

![](images/p7.jpg)

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

![](images/p10.jpg)

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

![](images/p11.jpg)

Let us calculate the Noise Margin equation 

![](images/p12.jpg)

These are the tolerable levels of noise

* **High Noise Margin (NMH)** = VOH − VIH
* **Low Noise Margin (NML)** = VIL − VOL

In the Undefined region,the output logic level can swing between 'high' and 'low' which is a metastate.

![](images/p13.jpg)

Bumps in the tolerable region won't harm the output of a circuit.

But for the Bumps in between VIL to VIH , the output is undefined.

### L4 Noise margin variation with respect to PMOS width

Now let's observe the Noise Margin for different PMOS widths

For wide range of noises,we consider
- For PMOS : logic 1 on capacitor
- For NMOS : logic 0 on capacitor

* Find the points where slope = -1
* calculate NMH and NML

![](images/p14.jpg)

![](images/p15.jpg)

![](images/p16.jpg)

![](images/p17.jpg)

![](images/p18.jpg)

PMOS width is responsible for getting capacitance charged which creates low resistance path as a result of it is able to hold charges for longtime

![](images/p19.jpg)

Due to fabrication imperfections ,Wp is not exactly equal to the x.Wn ,but CMOS Noise margin won't affect as we observe that NMH increases to certain point w.r.t PMOS width i.e 0.3 to 0.42 and then it doen't change then.This shows the CMOS Robustness towards Noise Margin

The suitable ranges for Digital Design and Analog Design of CMOS VTC are

![](images/p20.jpg)

![](images/p21.jpg)


### L5 Sky130 Noise margin labs

Let us calculate Noise Margin through SPICE simulation

Let us go to the Day 4 file

![](images/p22.jpg)

We are taking 
* Wp/Wn = 2.77
* Tuning/sweeping the Vin from 0 to 1.8V with stepsize of 0.01V

![](images/p23.jpg)

Run the Simulation

![](images/p24.jpg)

![](images/p25.jpg)

We will take the point where the slope is -1 ; 

![](images/p27.jpg)

We get
* VOH = 1.70141
* VIH = 0.97479
* VOL = 0.107042
* VIL = 0.773109

**High Noise margin NMH = VOH - VIH = 1.70141 - 0.97479 = 0.726**

**Low Noise margin NML = VIL - VOL = 0.773109 - 0.107042 = 0.66**


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






