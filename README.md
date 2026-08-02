# FPGA-Fabric-Design-and-Architecture
refers to the internal structure and organization of a Field-Programmable Gate Array (FPGA) and other variants like OpenFPGA,RISCV Core on Vivado,SOFA and RISC-V core on custom SOFA fabric
![FPGA_Architecture](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/FPGA_Architecture.png).

# Day 1 – Introduction to FPGA Architecture, Programming and Vivado Design Flow using Basys board. FPGA operating remotely.

## Objective

The objective of Day 1 was to understand the fundamentals of FPGA architecture, FPGA design flow, and implementation of a simple digital design using Xilinx Vivado on the Basys 3 FPGA board and remotely too
# Introduction to FPGA (Field Programmable Gate Array)
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
# The complete flow for a FPGA Programming on Vivado:
- Simulation
  ![Simulation_Counter](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/154bce8823589f8bde3c9c76d8171f75f6857e01/Simulation_Counter.png)
- Elaboration Verilog HDL into an RTL schematic representation.
  ![Pin_assignmentsetup](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Pinassignment(setup.png)
- Synthesis
- ![Synthesis](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/RTL%20Schematic.png)
- Implementation
- Timing analysis
- ![Design_Timing Summary](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Design_Timing%20Summary.jpg)
- Power analysis gives dynamic,static,clock and signal power
- ![Poweranalysis_report](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Power%20analysis_report.png)
- Device utilization
- ![Resource](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Resource_Utilization_report.png)
- Constraints and pin mapping
![Constraint_file](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Constraint_file.png)

# Virtual Input/Output (VIO)

Virtual Input/Output (VIO) allows internal FPGA signals to be monitored and controlled in real-time using Vivado Hardware Manager.

Applications:
- Internal debugging
- Signal monitoring
- Runtime testing
# Day 2 - Study on OpenFPGA, VPR and VTR Flow

Day 2 focused on understanding the open-source FPGA CAD flow Tools:
- OpenFPGA
- VPR (Versatile Place and Route)
- VTR (Verilog-To-Routing)

The complete flow from Verilog RTL to FPGA routing and timing analysis was explored. Timing constraints, post-synthesis simulation, power analysis and generated reports were also studied.

## VPR Visualization

<img width="940" height="771" alt="image" src="https://github.com/user-attachments/assets/af78fc1d-c4be-40b9-9910-12a2a8a96c13" />


*VPR placement and routing visualization.*

---

# EArch FPGA Architecture Analysis using VPR

The `EArch.xml` FPGA architecture file was analyzed using the VPR flow. The architecture visualization, routing resources, nets, logical connections and timing reports were generated and studied.

The VPR flow was executed using the following command:

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
$VTR_ROOT/vtr_flow/benchmarks/blif/tseng.blif \
--route_chan_width 100 \
--disp on
```

---

# FPGA Architecture Visualization

The FPGA architecture generated using `EArch.xml` contains:
- Logic blocks
- Routing channels
- Switch blocks
- Interconnect resources
- FPGA grid structure


# Nets Analysis

The nets report contains:
- Source and destination nodes
- Interconnect paths
- Net routing information
- Routing resource usage

## Nets Report

<img width="940" height="720" alt="image" src="https://github.com/user-attachments/assets/709a35a3-7938-491d-8c06-9a9130a1cfbf" />

*Net connections generated after routing.*

---

# Logical Connections

Logical connections show:
- Signal interconnections
- Routing connectivity
- Logic block communication

## Logical Connections Report

<img width="940" height="709" alt="image" src="https://github.com/user-attachments/assets/cdf823c2-2ea1-4c48-85c4-6ad9fe6846de" />

*Logical interconnections generated during routing.*

---

# Critical Path Analysis

Critical path analysis determines:
- Longest timing path
- Maximum propagation delay
- Maximum operating frequency

The critical path directly impacts FPGA performance.

## Critical Path Report

<img width="940" height="711" alt="image" src="https://github.com/user-attachments/assets/44e6e49c-3f2b-412e-81ba-1414fa151c5b" />

*Critical timing path generated during timing analysis.*

---

# Routing Utilization

Routing utilization shows:
- Channel occupancy
- Routing efficiency
- Congestion distribution
- Resource utilization

## Routing Utilization Report

<img width="940" height="721" alt="image" src="https://github.com/user-attachments/assets/42dc365d-d40b-463f-8e63-024111186a4c" />

*Routing utilization generated after placement and routing.*

---

# Timing Analysis using Constraints

Timing constraints were added using an SDC file.

The constraints file defines:
- Clock period
- Input delays
- Output delays

---

# SDC Constraint File

## Constraint File

```tcl
create_clock -period 10 -name pclk
set_input_delay -clock pclk -max 0 [get_ports {*}]
set_output_delay -clock pclk -max 0 [get_ports {*}]
```

---

# Running VPR with Constraints

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
$VTR_ROOT/vtr_flow/benchmarks/blif/tseng.blif \
--route_chan_width 100 \
--sdc_file tseng.sdc \
--disp on
```

---

# Setup Timing Analysis

Setup timing checks whether data reaches destination registers before the active clock edge.

After adding constraints:
- Setup slack improved
- Timing closure was achieved
- Violations were reduced

## Setup Timing Report

<img width="940" height="486" alt="image" src="https://github.com/user-attachments/assets/d1ab7bd8-25ce-4826-8ce1-c80a23df133c" />

*Setup timing report after applying timing constraints.*

---

# Hold Timing Analysis

Hold timing ensures data remains stable after the active clock edge.

## Hold Timing Report

<img width="940" height="194" alt="image" src="https://github.com/user-attachments/assets/eb897b40-8f6c-4ddf-98ee-b4b0dee00bfa" />

*Hold timing report generated using VPR timing analysis.*

---

# Introduction to VTR

VTR (Verilog-To-Routing) is a complete open-source FPGA CAD flow.

The VTR flow performs:
- RTL Elaboration
- Synthesis
- Technology Mapping
- Packing
- Placement
- Routing
- Timing Analysis

Tools involved:
- ODIN II
- ABC
- VPR

---

# VTR Flow Command

```bash
$VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py \
counter.v \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
-temp_dir . \
-route_chan_width 100
```

---

# Counter Design for VTR Flow

The following counter design was used for VTR implementation.

## Counter Verilog Code

```verilog
/*Important: Once you run ./a.out, it will keep running infinitely, because it is in an always block. You need to hit Ctrl + Z to stop it, else, the vcd will become a large file and will never end.
*/

module up_counter (
out      ,
enable   ,
clk      ,
reset
);

output [3:0] out;

input enable, clk, reset;

reg [3:0] out;

always @(posedge clk)

if (reset) begin
    out = 4'b0 ;
end

else if (enable) begin
    out = out + 1;
end

endmodule
```

---

# VTR Flow Stages

The VTR flow performed the following stages:

1. Elaboration and Synthesis using ODIN II  
2. Logic Optimization using ABC  
3. Packing using VPR  
4. Placement using VPR  
5. Routing using VPR  
6. Timing Analysis  

---

# Generated VTR Outputs

The VTR flow generated:
- `.net`
- `.place`
- `.route`
- `.blif`
- Timing reports
- Routing reports
- Placement reports

---

# Nets and Logical Connections

The generated reports included:
- Nets
- Routing utilization
- Critical paths
- Logical interconnections

## Nets Report

<img width="940" height="691" alt="image" src="https://github.com/user-attachments/assets/fcb0ac03-e295-4f8c-a568-46af29977e44" />

*Generated net connections*

## Logical Connections

<img width="940" height="756" alt="image" src="https://github.com/user-attachments/assets/f27bb6e5-088c-49f3-8603-83bcca565393" />

*Logical routing connections generated by VPR.*

---

# Critical Path Analysis

Critical path analysis was performed after routing.

The critical path determines:
- Maximum operating frequency
- Worst-case delay path

## Critical Path Report

!<img width="940" height="782" alt="image" src="https://github.com/user-attachments/assets/0cb2e8be-232b-4ce8-9d5f-af98a2b580e7" />


*Critical timing path generated during routing.*

---

# Timing Analysis using Constraints

Timing constraints were added using `.sdc` file.

The constraints define:
- Clock period
- Input delays
- Output delays

---
# Setup Timing Report

## Setup Timing

<img width="940" height="473" alt="image" src="https://github.com/user-attachments/assets/789f07a7-4fdf-4651-bcdc-943e2f095a66" />


*Setup timing report before applying constraints.*

---

# Hold Timing Report

## Hold Timing

<img width="940" height="550" alt="image" src="https://github.com/user-attachments/assets/702ba1e6-d8ab-4793-a580-681d6d97afb0" />


*Hold timing report generated by VPR before constraints.*

---

# SDC Constraint File

## Constraint File

```tcl
create_clock -period 10 up_counter_clk
set_input_delay -clock up_counter_clk -max 0 [get_ports {*}]
set_output_delay -clock up_counter_clk -max 0 [get_ports {*}]
```

---

# Running VPR with Constraints

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--route_chan_width 100 \
--sdc_file counter.sdc
```

---

# Setup Timing Report

After adding timing constraints:
- Setup slack improved
- Timing requirements were met

## Setup Timing

<img width="940" height="619" alt="image" src="https://github.com/user-attachments/assets/71a1a2fa-7149-4880-9904-13b02eb13a0b" />


*Setup timing report after applying constraints.*

---

# Post Synthesis Simulation

Post synthesis simulation was performed using:
- Generated post synthesis netlist
- SDF timing file
- Vivado simulator

The generated files:
- `up_counter_post_synthesis.v`
- `up_counter_post_synthesis.sdf`

---

# Generating Post Synthesis Netlist

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--gen_post_synthesis_netlist on
```

---

# Testbench for Post Synthesis Simulation

## Counter Testbench

```verilog
`timescale 1ns/1ps

module upcounter_testbench();

reg clk, reset, enable;
wire [3:0] out;

up_counter dut(
    .\up_counter^enable (enable),
    .\up_counter^clk (clk),
    .\up_counter^reset (reset),
    .\up_counter^out~0 (out[0]),
    .\up_counter^out~1 (out[1]),
    .\up_counter^out~2 (out[2]),
    .\up_counter^out~3 (out[3])
);

initial $sdf_annotate("up_counter_post_synthesis.sdf", dut);

initial begin

clk=0;
enable=0;
reset=1;

#20;

reset=0;
enable=1;

end

always
#5 clk=~clk;

endmodule
```

---

# Post Synthesis Simulation Results

The generated post synthesis simulation verified:
- Timing behavior
- Routing delays
- Gate-level functionality

## Post Synthesis Waveform

<img width="871" height="278" alt="image" src="https://github.com/user-attachments/assets/b656b2b0-47f4-4870-b87b-b8f8eaeb0307" />


*Post synthesis simulation waveform.*

---

# Power Analysis using VTR

Power estimation was performed using VTR power analysis flow.

The flow estimated:
- Dynamic power
- Routing power
- Clock power
- Leakage power

---

# Power Analysis Command

```bash
$VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py \
counter.v \
$VTR_ROOT/vtr_flow/arch/timing/k6_frac_N10_mem32K_40nm.xml \
-power \
-temp_dir . \
-route_chan_width 100
```

---

## stdout.log report

<img width="940" height="389" alt="image" src="https://github.com/user-attachments/assets/15846909-5252-489f-a236-85ffe7b730ce" />

*stdout.log report generated using VTR.*

# Power Report

## Power Analysis

<img width="940" height="463" alt="image" src="https://github.com/user-attachments/assets/a3728e51-12e1-46be-b4a8-7c16d556aec0" />

*Power estimation report generated using VTR.*

---

# VTR Generated Reports

Generated reports included:
- Timing reports
- Routing reports
- Power reports
- Placement reports
- Post synthesis netlists

---

# Important Commands Used

## Running VTR Flow

```bash
$VTR_ROOT/vtr_flow/scripts/run_vtr_flow.py \
counter.v \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
-temp_dir . \
-route_chan_width 100
```

---

## Running VPR GUI

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--route_chan_width 100 \
--disp on
```

---

## Running VPR with Constraints

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--route_chan_width 100 \
--sdc_file counter.sdc
```

---

## Generating Post Synthesis Netlist

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
counter.pre-vpr.blif \
--gen_post_synthesis_netlist on
```
