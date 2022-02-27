# Design of an Adjustable Low Drop-out Regulator
3 Week hands on [Cloud Based Analog IC Design Hackathon](https://www.iith.ac.in/events/2022/02/15/Cloud-Based-Analog-IC-Design-Hackathon/) conducted by Electrical Engineering Department of IIT Hyderabad inassociation with VLSI System Design (VSD) Corp. Pvt. Ltd India and sponsored by Synopsys India.
The Hackathon provided an oppurtunity to implement proposed LDO design using Synopsis Custom Compiler on 28nm CMOS Technology. The below repository is the final submission for completion of Adjustable LDO design

## Table of Contents
1. [Abstract](#abstract)
2. [Working](#working)
3. [Reference Circuit](#reference-circuit)
4. [Tools Used](#tools-used)
5. [Implementation](#implementation)
6. [LDO Schematic](#ldo-schematic)
7. [Simulation result](#simulation-result)
8. [Challenge](#challenge)
9. [Limitations](#limitations)
10. [References](#references)
11. [Acknowledgements](#acknowledgements)
12. [Author](#author)

## Abstract
Low Drop-out regulator (LDO) is a linear voltage regulator designed to provide stable DC voltage. To satisfy evolving on chip power delivery requirements, a CMOS LDO capable of providing two adjustable voltage levels by using a binary-input control signal is implemented. It supplies a 0.6V default output voltage from a 1.0V battery input and a maximum load of 20mA.

## Working
Linear regulators convert one voltage level to a lower voltage level by dissipating the excess energy as waste heat in the output device. The pass element gate voltage is controlled to regulate the output voltage at a stable state. 
A high gain OPAMP is used as error amplifier, with a stable voltage reference at one input and feedback voltage is connected to the other input of the differential amplifier. A current mirror is used to bias the error amplifier with current IREF.
In order to achieve high gain a second stage common source amplifier is used. In order to keep the LDO stable miller compensation is used. Miller compensation is used to split the poles and bring the second pole below UGB and achieve better stability.
 NMOS acts as a strong pull down device, yielding rail-to-rail swing. Operational amplifiers with rail-to-rail output stage achieve the maximum output signal swing in systems with low single-supply voltages. They are capable of generating an output signal up to the supply rails.
 
<p align="center">
	<img width="600" Height="350" src="LDO/reftop.png" alt="reftop"> 
	<h5 align="center">Figure 1: Top level schematic</h5>
</p>

## Reference Circuit
<p align="center">
	<img width="600" Height="350" src="LDO/refckt.png" alt="reftop"> 
	<h5 align="center">Figure 2: Proposed reference schematic</h5>
</p>

## Tools Used
- Synopsys Custom Compiler: Custom Compiler provides a highly productive environment for design entry
and simulation, with strong features for mixed-signal design, debug, simulation
management, analysis, and reporting. 
- Synopsys Primewave: The PrimeWave Design Environment is a comprehensive environment for all Synopsys simulation engines and analysis capabilities.It provides an easy-to-use simulation setup cockpit, support for grid-based job distribution and monitoring of simulation jobs, and a powerful graphical waveform viewer. Whether designing analog, mixed-signal, or RF designs on mature or advanced nodes,
PrimeWave is a unified solution for all applications. 
- Synopsys 28nm PDK: LDO is designed using Synopsys 28nm PDK.

## Implementation

**1. Sizing the Pass Element:**
- The pass element is designed to carry the full load of 20mA at all PVT corners.
- The device is designed to operate in saturation region.
- PMOS is used as pass element. The pass element is sized for  Max load current, Minimum VDD, Minimum tolerable Vg and SS corner at-40 Deg C.
- Inorder to ensure that Pass element isnt over sized, It is ensured to have leakage current less than the maximun current allowed across resistor divider circuit. Pass element is tested at FF corner at 85 Deg C.
- PMOS Element is designed to operate at -40 to 85 deg C.
  <p align="center">
	<img width="500" Height="300" src="LDO/passele_SS.png" alt="reftop"> 
	<h5 align="center">Figure 3: ID across Pass Element at SS Testbench</h5>
</p>
  <p align="center">
	<img width="500" Height="300" src="LDO/passele_SS_res.png" alt="reftop"> 
	<h5 align="center">Figure 4:ID across Pass Element at SS result</h5>
</p>
  <p align="center">
	<img width="500" Height="300" src="LDO/Passele_FF.png" alt="reftop"> 
	<h5 align="center">Figure 5: Leakage across Pass Element at FF Testbench</h5>
</p>
  <p align="center">
	<img width="500" Height="300" src="LDO/passele_FF_res.png" alt="reftop"> 
	<h5 align="center">Figure 6: Leakage across Pass Element at FF result</h5>
</p>

**2. Sizing the feedback network:**
- Current across resistor feedback network is taken as 10uA and Vref is taken as 0.6V.
- Inorder to obtain 0.6V RFB2 network needs to be OPEN which is obtained bt CTRL=0V.
- Inorder to obtain 0.8V: RFB1+RFB2 = 0.8/10u = 80K ohm.
- Vout/Vref = 1+(Rfb2/Rfb1). Therefore, Rfb1 = 20K ohm & Rfb2 = 60K ohm.

**3. Error Amplifier design:**
- The minimum gain required across PVT corner for the device to regulate is
  
  Vout/Vref = A/(1+AB)
  
  at Vref=0.6V, B=1, Vout=0.589V: A=54.54=34.735dB
  
  at Vref=0.6V, B=0.75, Vout=0.789V: A=95.6363=39.612dB
- This is the minimum gain targetted while designing the error amplifier.
- Inorder to improve the over all BW and gain of the error amplifier two stage differential amplifier is designed.
- common source amplifier is implemented as second stage of tther error amplifier.
- Bias current is set as 0.1uA
</p>
  <p align="center">
	<img width="600" Height="400" src="LDO/ErrAMp_loopgain_tb.png" alt="reftop"> 
	<h5 align="center">Figure 7: Error amplifier design</h5>
</p>

## LDO Schematic
- All the blocks are connected together to complete the LDO design. Inorder to improve the phase margin of the device a miller cap of 30pF is added to split the poles.
- Below is the scematic and test bench set up.

</p>
  <p align="center">
	<img width="600" Height="400" src="LDO/LDO SCHEMATIC.png" alt="reftop"> 
	<h5 align="center">Figure 8: LDO Schematic</h5>
</p>
</p>
  <p align="center">
	<img width="600" Height="400" src="LDO/LDO TESTBench.png" alt="reftop"> 
	<h5 align="center">Figure 9:LDO Testbench</h5>
</p>

## Simulation results
