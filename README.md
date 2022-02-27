# Design of an Adjustable Low Drop-out Regulator
3 Week hands on [Cloud Based Analog IC Design Hackathon](https://www.iith.ac.in/events/2022/02/15/Cloud-Based-Analog-IC-Design-Hackathon/) conducted by Electrical Engineering Department of IIT Hyderabad inassociation with VLSI System Design (VSD) Corp. Pvt. Ltd India and sponsored by Synopsys India.
The Hackathon provided an oppurtunity to implement proposed LDO design using Synopsis Custom Compiler on 28nm CMOS Technology. The below repository is the final submission for completion of Adjustable LDO design

## Table of Contents
1. [Abstract](#abstract)
2. [Working](#working)
3. [Reference Circuit](#reference-circuit)
4. [Implementation](#implementation)
5. [Schematic Netlist](#schematic-netlist)
6. [Simulation result](#simulation-result)
7. [Challenge](#challenge)
8. [Limitations](#limitations)
9. [References](#references)
10. [Acknowledgements](#acknowledgements)
11. [Author](#author)

## Abstract
Low Drop-out regulator (LDO) is a linear voltage regulator designed to provide stable DC voltage. To satisfy evolving on chip power delivery requirements, a CMOS LDO capable of providing two adjustable voltage levels by using a binary-input control signal is implemented. It supplies a 0.6V default output voltage from a 1.0V battery input and a maximum load of 20mA.

## Working
Linear regulators convert one voltage level to a lower voltage level by dissipating the excess energy as waste heat in the output device. The pass element gate voltage is controlled to regulate the output voltage at a stable state. 
A high gain OPAMP is used as error amplifier, with a stable voltage reference at one input and feedback voltage is connected to the other input of the differential amplifier. A current mirror is used to bias the error amplifier with current IREF.
In order to achieve high gain a second stage common source amplifier is used. In order to keep the LDO stable miller compensation is used. Miller compensation is used to split the poles and bring the second pole below UGB and achieve better stability.
 NMOS acts as a strong pull down device, yielding rail-to-rail swing. Operational amplifiers with rail-to-rail output stage achieve the maximum output signal swing in systems with low single-supply voltages. They are capable of generating an output signal up to the supply rails.
