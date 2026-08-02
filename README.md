# FPGA-Fabric-Design-and-Architecture
refers to the internal structure and organization of a Field-Programmable Gate Array (FPGA) and other variants like OpenFPGA,SOFA and Customisation

# Day 1 – Introduction to FPGA Architecture, Programming and Vivado Design Flow using Basys board. FPGA operating remotely.

## Objective

The objective of Day 1 was to understand the fundamentals of FPGA architecture, FPGA design flow, and implementation of a simple digital design using Xilinx Vivado on the Basys 3 FPGA board and remotely too

The complete flow covered:
- Introduction to FPGA and programmable logic
- FPGA vs ASIC comparison
- FPGA internal architecture
- LUTs, CLBs, Flip-Flops and routing resources
- Vivado RTL-to-Bitstream flow
- Behavioral simulation
- Synthesis
- Implementation
- Timing analysis
- Power analysis
- Device utilization
- Constraints and pin mapping
- Virtual I/O (VIO)

---
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

# FPGA Architecture
![FPGA Architecture](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/FPGAArchitecture.png)
An FPGA mainly consists of:
- Configurable Logic Blocks (CLBs)
- Look-Up Tables (LUTs)
- Flip-Flops (FFs)
- Programmable Interconnects
- Block RAM
- DSP Blocks
- I/O Blocks

## Configurable Logic Block (CLB)

CLBs are the fundamental building blocks of an FPGA.

A CLB contains:
- LUTs for combinational logic
- Flip-flops for sequential logic
- Carry chains for arithmetic operations
- Multiplexers and routing resources

## Look-Up Tables (LUTs)

A LUT implements logic using a truth table approach.

For an N-input LUT:
- There are \(2^N\) memory entries
- Each entry represents output for one input combination

A 3-input LUT can implement any logic function of 3 variables.

---

# FPGA Programming Flow

The FPGA design methodology followed in Vivado includes:

1. RTL Design
2. Testbench Creation
3. Behavioral Simulation
4. Synthesis
5. Constraints Assignment
6. Placement and Routing
7. Timing Analysis
8. Bitstream Generation
9. FPGA Programming

---

# Basys 3 FPGA Board

The FPGA board used throughout the experiments was the **Basys 3 Artix-7 FPGA Board**.

## Major Components on Basys 3

| Component | Description |
|---|---|
| Push Buttons | User input |
| Slide Switches | Input control |
| LEDs | Output display |
| 7-Segment Display | Numerical output |
| VGA Port | Display output |
| USB/JTAG Port | FPGA programming |
| Clock Oscillator | System clock source |

---

# Vivado Counter Design

A 4-bit up counter was implemented in Verilog HDL.

The counter:
- Increments on every positive edge of clock
- Resets when reset signal is active
- Displays output on LEDs

---

# Verilog Counter Code

```verilog
module counter_clk_div(
    input clk,
    input rst,
    output reg [3:0] counter_out
);

reg div_clk;
reg [25:0] delay_count;

always @(posedge clk) begin
    if(rst) begin
        delay_count <= 26'd0;
        div_clk <= 1'b0;
    end
    else begin
        if(delay_count == 26'd212) begin
            delay_count <= 26'd0;
            div_clk <= ~div_clk;
        end
        else begin
            delay_count <= delay_count + 1;
        end
    end
end

always @(posedge div_clk) begin
    if(rst)
        counter_out <= 4'b0000;
    else
        counter_out <= counter_out + 1;
end

endmodule
```

---

# Behavioral Simulation

Behavioral simulation was performed in Vivado simulator to verify the functionality of the counter before synthesis.

The waveform confirmed:
- Proper clock toggling
- Counter increment operation
- Reset functionality

## Simulation Output

<img width="940" height="486" alt="image" src="https://github.com/user-attachments/assets/456706e8-76f9-4326-ace5-82c53904f2f8" />


*Behavioral simulation waveform of 4-bit up counter in Vivado.*

---

# RTL Elaboration

RTL elaboration converts Verilog HDL into an RTL schematic representation.

This stage verifies:
- Logical connectivity
- Module hierarchy
- Register structure
- Signal flow

## RTL Schematic

<img width="940" height="509" alt="image" src="https://github.com/user-attachments/assets/f7dc36a1-9bd2-4ff7-a69d-f282429c0aaa" />


*RTL elaborated schematic of the counter design.*

---

# Constraints and Pin Mapping

Constraints were added using the `.xdc` file.

The constraints file maps:
- Clock input pin
- Reset input pin
- Output LEDs

## Constraint.xdc:

<img width="940" height="534" alt="image" src="https://github.com/user-attachments/assets/49e5f102-7d0c-445a-bc7d-977c337e64a7" />


## Package Mapping

<img width="940" height="508" alt="image" src="https://github.com/user-attachments/assets/6d116368-aa34-4a35-a568-6161a491ee69" />


*FPGA package view showing physical pin assignments.*

---

# Synthesis

Synthesis converts RTL into FPGA primitives such as:
- LUTs
- Flip-Flops
- Carry Chains

During synthesis:
- Logic optimization is performed
- Resource estimation is generated
- Timing estimation begins

---

# Timing Analysis

Timing analysis verifies whether signals reach destination registers within required clock periods.

Two important checks:
- Setup Timing
- Hold Timing

## Setup Timing

Setup timing ensures data reaches before the active clock edge.

Condition:

\[
T_{cq} + T_{logic} < T_{clock} - T_{setup}
\]

## Hold Timing

Hold timing ensures data remains stable after clock edge.

## Slack

Slack is defined as:

\[
Slack = Required\ Time - Arrival\ Time
\]

Positive slack indicates timing is met.

## Design Timing Summary

<img width="940" height="178" alt="image" src="https://github.com/user-attachments/assets/ec86bbd1-37fb-46ef-8bce-af7fc6712ddf" />


*Timing analysis summary.*

---

# Device Utilization

Vivado generates utilization reports showing FPGA resource consumption.

Resources analyzed:
- LUTs
- Flip-Flops
- I/O pins
- BRAM

## Utilization Report

<img width="800" height="318" alt="image" src="https://github.com/user-attachments/assets/cb63ddd1-a63c-4668-802f-bbf1ef172858" />


*FPGA resource utilization report after implementation.*

---

# Power Analysis

Power analysis estimates:
- Dynamic power
- Static power
- Clock power
- Signal power

## Power Report

<img width="778" height="384" alt="image" src="https://github.com/user-attachments/assets/606e0804-d336-4ec0-b000-e4b85e7c67bb" />


*Power analysis report generated after FPGA implementation.*

---

# Virtual Input/Output (VIO)

Virtual Input/Output (VIO) allows internal FPGA signals to be monitored and controlled in real-time using Vivado Hardware Manager.

Applications:
- Internal debugging
- Signal monitoring
- Runtime testing
# Day 2 - Exploring OpenFPGA, VPR and VTR Flow

## Introduction

Day 2 focused on understanding the open-source FPGA CAD flow using:
- OpenFPGA
- VPR (Versatile Place and Route)
- VTR (Verilog-To-Routing)

The complete flow from Verilog RTL to FPGA routing and timing analysis was explored. Timing constraints, post-synthesis simulation, power analysis and generated reports were also studied.

---

# Introduction to OpenFPGA

OpenFPGA is an open-source FPGA framework used for:
- FPGA architecture exploration
- Bitstream generation
- FPGA fabric generation
- CAD automation
- Verification and testing

The framework supports:
- Verilog-to-Bitstream flow
- Custom FPGA architecture exploration
- Automated FPGA fabric generation

---

# Introduction to VPR

VPR (Versatile Place and Route) is an open-source CAD tool used for:
- Packing
- Placement
- Routing
- Timing Analysis

The VPR flow consists of:

1. Packing  
2. Placement  
3. Routing  
4. Timing Analysis  

---

# VPR Flow Command

```bash
$VTR_ROOT/vpr/vpr \
$VTR_ROOT/vtr_flow/arch/timing/EArch.xml \
<blif-file-path> \
--route_chan_width 100 \
--disp on
```

---

# VPR Architecture Flow

The VPR flow performs:

## Packing
Combines logic primitives into FPGA logic blocks.

## Placement
Places logic blocks inside FPGA grid.

## Routing
Creates interconnections between FPGA logic blocks.

## Timing Analysis
Analyzes setup and hold timing paths.

---

# VPR GUI Visualization

VPR GUI was used to visualize:
- FPGA grid
- Routing resources
- Logic placement
- Nets and critical paths

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
