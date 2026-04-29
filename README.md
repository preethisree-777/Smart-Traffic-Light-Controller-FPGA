# Real-Time Smart 4-Way Traffic Light Controller using Verilog HDL

## Project Overview

This project implements a Real-Time Smart 4-Way Traffic Light Controller on Zybo Z7-10 FPGA using Verilog HDL in Xilinx Vivado.

The system manages four-direction traffic flow with:

* Finite State Machine (FSM)
* Emergency Vehicle Priority Handling
* Pedestrian Crossing Support
* System Fault Detection
* Adaptive Green Signal Timing based on Time of Day
* FPGA Synthesis and Implementation Verification

This project improves traffic efficiency, safety, and smart city traffic management.

---

## Features

### Normal Traffic Operation

Sequential traffic flow for:

* North
* East
* South
* West

with proper:

* Green
* Yellow
* Red transitions

---

### Emergency Vehicle Priority

When an emergency vehicle is detected:

* Normal cycle is interrupted
* Emergency mode is activated
* Priority path is assigned immediately

---

### Pedestrian Safety Mode

When pedestrian request is detected:

* Traffic stops safely
* Pedestrian crossing signal is activated

---

### Adaptive Signal Timing

Green light duration changes based on:

* Day Mode
* Peak Hour Mode
* Night Mode

for better traffic optimization.

---

### System Fault Detection

If system fault occurs:

* Safe mode activates
* System health monitoring responds immediately

---

## Tools Used

* Verilog HDL
* Xilinx Vivado
* Zybo Z7-10 FPGA
* FSM Design
* Digital Logic Design

---

## FPGA Results

### Timing Summary

* WNS: 3.936 ns
* TNS: 0.000 ns
* WHS: 0.199 ns
* All timing constraints met

---

### Power Summary

* Total On-Chip Power: 0.101 W

---

### Resource Utilization

* LUT: 54
* FF: 13
* DSP: 0
* BRAM: 0

---

## Project Files

* RTL Design
* Testbench
* Constraint File (.xdc)
* Simulation Results
* Synthesis Reports
* Implementation Reports
* Timing Reports
* Power Reports

---

## Author

ECE Student | FPGA | Verilog | VLSI | Digital Design Enthusiast
