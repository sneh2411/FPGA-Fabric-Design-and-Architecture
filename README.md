# FPGA-Fabric-Design-and-Architecture
refers to the internal structure and organization of a Field-Programmable Gate Array (FPGA) and other variants like OpenFPGA,RISCV Core on Vivado,SOFA and RISC-V core on custom SOFA fabric
![FPGA_Architecture](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/FPGA_Architecture.png).

### Day 1 – Introduction to FPGA Architecture, Programming and Vivado Design Flow using Basys board. 
## Objective

The objective of Day 1 was to understand the fundamentals of FPGA architecture, FPGA design flow, and implementation of a simple digital design using Xilinx Vivado on the Basys 3 FPGA board and remotely too

#### Introduction to FPGA (Field Programmable Gate Array)
A field-programmable gate array (FPGA) is a type of configurable integrated circuit that can be repeatedly programmed . 

Significance of FPGA:
- Hardware acceleration
- Signal processing
- Embedded systems
- Machine learning
- Aerospace systems
- High-performance computing

## Comparison  

Feature| FPGA | ASIC |
|---|---|---|
Design Process| simpler design | long and complex
Flexibility| Reprogrammable | Fixed after fabrication |
Time to market| Faster prototyping | Long fabrication cycle |
Cost| Lower initial cost | High initial fabrication cost |
Performance| slower but more versatile | faster and more efficient |
Hardware Design| RTL to Bitstream | RTL to Layout |

---
### The complete flow for a FPGA Programming on Vivado:
- Simulation
  ![Simulation_Counter](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/154bce8823589f8bde3c9c76d8171f75f6857e01/Simulation_Counter.png)
- Elaboration Verilog HDL into an RTL schematic representation.

   ![Pinassignmentsetup](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Pinassignmentsetup.png)
- Synthesis
- ![Synthesis](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/RTL%20Schematic.png)
- Implementation
  
- Timing analysis
- ![Design_Timing Summary](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Design_Timing%20Summary.jpg)

- Power analysis gives dynamic,static,clock and signal power
- ![Poweranalysis_report](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Power%20analysis_report.png)

- Resource utilization
- ![Resource](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Resource_Utilization_report.png)
- Constraints and pin mapping
![Constraint_file](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Constraint_file.png)

# Virtual Input/Output (VIO)

Virtual Input/Output (VIO) allows internal FPGA signals to be monitored and controlled in real-time using Vivado Hardware Manager.

# Day 2 - Study on OpenFPGA, VPR and VTR Flow

Day 2 focused on understanding the open-source FPGA CAD flow Tools:
- OpenFPGA
- VPR (Versatile Place and Route)
- VTR (Verilog-To-Routing)

The complete flow from Verilog RTL to FPGA routing and timing analysis was explored. Timing constraints, post-synthesis simulation, power analysis and generated reports were also studied.

## VPR Visualization

![VPR_Visualisation](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/VPR_Visualisation.png)

---

### EArch FPGA Architecture Analysis using VPR

There are about more than 20 architecture of that #EArch.xml# FPGA architecture file was analyzed using the VPR flow. The architecture visualization, routing resources, nets, logical connections and timing reports were generated and studied.
### Nets Analysis - net connections after routing

![Nets report](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Nets%20report.png)

### Logical Connections Report
![LogicalConnections](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Logical%20Connections.png)


### Critical Path Analysis Report
## Routing Utilization Report
![Critical Path Analysis ](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Critical%20Path%20Analysis.png)

![RoutingUtilization_Report](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Routing%20Utilization_report.png)
## Timing Analysis before using constraints
![Setup_Timing_bc](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Setup_Timing_bc.png)
![Hold_Timing_bsdc](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Hold_Timing_bsdc.png)
# Timing Analysis using Constraints

Timing constraints were added using an SDC file. The constraints file gives Clock period,Input and Output delays

## SDC Constraint File
```tcl
create_clock -period 10 -name pclk
set_input_delay -clock pclk -max 0 [get_ports {*}]
set_output_delay -clock pclk -max 0 [get_ports {*}]
```
# Running VPR with Constraints

## Setup Timing Analysis - Slack improved and violations reduced
## Setup Timing Report

![Setup_Timingreport](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Setup_Timing%20report.png)


# Hold Timing Analysis
Hold timing ensures data remains stable after the active clock edge.

## Hold Timing Report
![Hold_TimingReport_afterconsrt](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Hold_Timing%20Report_afterconsrt.png)

## Post Synthesis Simulation Waveform
![Post_Synthesis_sim_Waveform](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Post_Synthesis_sim_Waveform.png)
## stdout.log report using VTR
![stdout.log](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/stdout.log_report.png)
## Power Analysis using VTR
![Power_Analysis_report](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Power_analysis_report.png)

# Day 3: Mythcore Processor Implementation and FPGA Analysis
### RTL Simulation waveform of Mythcore Processor using Vivado software on Basys 3 board


