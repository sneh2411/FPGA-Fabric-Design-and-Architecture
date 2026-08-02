# FPGA-Fabric-Design-and-Architecture
refers to the internal structure and organization of a Field-Programmable Gate Array (FPGA) and other variants like OpenFPGA,RISCV Core on Vivado,SOFA and RISC-V core on custom SOFA fabric
![FPGA_Architecture](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/FPGA_Architecture.png).

# Day 1 – Introduction to FPGA Architecture, Programming and Vivado Design Flow using Basys board. FPGA operating remotely.

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
- 
  ![Pin_assignmentsetup](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/Pinassignment(setup.png)
- Synthesis
- ![Synthesis](https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/blob/main/RTL%20Schematic.png)
- Implementation
- 
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

# DAY 3 – Mythcore Processor Implementation and FPGA Analysis

## Objective

The objective of Day 3 was to work with a more complex RTL design instead of a simple counter circuit.  
A Mythcore RISC-V based processor design was taken and implemented on FPGA.  

The work involved:

- RTL simulation
- FPGA synthesis
- Schematic generation
- Package and pin mapping
- Adding ILA (Integrated Logic Analyzer)
- Timing analysis
- Utilization analysis
- Power analysis
- Observing simulation outputs

This experiment helped in understanding how larger digital systems behave on FPGA architectures and how FPGA tools analyze timing, routing, area, and power.

---

# Mythcore Design Source

The following files were used:

- `mythcore_test.v`
- `mythcore_test_gn.v`

Reference repository:

https://github.com/shivanishah269/risc-v-core/tree/master

---

# FPGA Design Flow

The complete FPGA implementation flow followed was:

1. RTL Design
2. Functional Simulation
3. Synthesis
4. Netlist Generation
5. Placement
6. Routing
7. Timing Analysis
8. Bitstream Generation
9. Hardware Debugging using ILA

---

# RTL Simulation

The Mythcore processor was first simulated before synthesis to verify proper functionality.

## Observations

- Clock signal toggled correctly
- Reset initialized the processor
- Outputs changed according to processor execution
- Functional behavior matched expected results

---

## RTL Simulation Output

<img width="940" height="227" alt="image" src="https://github.com/user-attachments/assets/c733cc34-ec14-4639-bb95-c7be878720ff" />


*RTL simulation waveform of Mythcore processor*

---

# Schematic Generation

After synthesis, Vivado generated the RTL schematic for the processor design.

The schematic displayed:

- LUT structures
- Flip-flops
- Internal routing
- Processor datapath
- Logic interconnections

Since Mythcore is significantly more complex than a simple counter, the generated schematic was large and densely interconnected.

---

## RTL Schematic Output

<img width="940" height="311" alt="image" src="https://github.com/user-attachments/assets/5e46f9d8-6b2e-40ba-8a09-0940976834af" />

*RTL schematic generated for Mythcore processor*

---

# Package View

The package view displayed FPGA resource placement and I/O pin allocation.

This helped in understanding:

- Physical FPGA layout
- Resource mapping
- Pin assignments
- FPGA routing regions

---

## Package View Output

<img width="940" height="437" alt="image" src="https://github.com/user-attachments/assets/1fb84060-2fb8-4aba-8707-3b6410bceb27" />

*FPGA package view showing FPGA resource utilization*

---

# Integrated Logic Analyzer (ILA)

## Objective

ILA was added to debug and monitor internal FPGA signals in real time.

Instead of only viewing external outputs, ILA allows internal processor signals to be captured directly using Vivado Hardware Manager.

---

# ILA Instantiation

```verilog
ila_0 your_instance_name (
    .clk(clk),

    .probe0(reset),
    .probe1(out)
);
```

---

# ILA Connections

The following signals were connected:

| Signal | Purpose |
|--------|----------|
| clk | Clock input |
| reset | Reset signal |
| out | Output probe |

---

# Constraint File

The XDC constraint file was created to define:

- Clock pin mapping
- Reset pin mapping
- Timing constraints
- Debug hub configuration
- Clock frequency

---

## Constraints Used

```tcl
set_property PACKAGE_PIN V6 [get_ports clk]
set_property IOSTANDARD LVCMOS33 [get_ports clk]

set_property IOSTANDARD LVCMOS33 [get_ports reset]
set_property PACKAGE_PIN P2 [get_ports reset]

create_clock -period 10.000 -name clk -waveform {0.000 5.000} [get_ports clk]

set_property C_CLK_INPUT_FREQ_HZ 300000000 [get_debug_cores dbg_hub]
set_property C_ENABLE_CLK_DIVIDER false [get_debug_cores dbg_hub]
set_property C_USER_SCAN_CHAIN 1 [get_debug_cores dbg_hub]

connect_debug_port dbg_hub/clk [get_nets clk_IBUF_BUFG]
```

---

# Utilization Analysis

The utilization report displayed FPGA resource consumption.

## Resources Used

- LUTs
- LUTRAM
- Flip-Flops
- BRAM
- IO Blocks

Compared to the counter design, Mythcore consumed significantly more FPGA resources due to processor complexity.

---

## Utilization Report

<img width="778" height="405" alt="image" src="https://github.com/user-attachments/assets/9a21a045-3139-4f65-9d5a-92367041cb9c" />

*FPGA utilization report for Mythcore processor.*

---

# Timing Analysis

Timing analysis was performed after placement and routing.

Timing analysis ensures:

- Proper setup timing
- Proper hold timing
- Correct clock synchronization

<img width="940" height="218" alt="image" src="https://github.com/user-attachments/assets/f54df0f0-8c2e-4235-8dba-cf1e4f0b3ef1" />

*Timing analysis summary*

---

# Power Analysis

Power analysis was performed after implementation.

The report included:

- Dynamic power
- Static power
- Clock power
- Logic power
- Signal power

---

## Observations

- Clock network consumed significant dynamic power
- Logic switching activity contributed to power usage
- Static FPGA power remained nearly constant

---

## Power Analysis Output

<img width="776" height="327" alt="image" src="https://github.com/user-attachments/assets/dcc1ed07-e60d-4c0d-a01b-da582730bece" />

*Power analysis report of Mythcore FPGA implementation.*

---

# Important Observations

## Increased Design Complexity

Compared to the counter design:

- Routing complexity increased heavily
- Resource utilization became much larger
- Timing paths became more critical

---

## FPGA Resource Usage

The Mythcore processor used:

- Large number of LUTs
- Multiple flip-flops
- Additional routing resources
- More clock resources

---

## Importance of Constraints

Without proper timing constraints:

- Timing violations can occur
- Placement becomes inefficient
- Routing delays increase
- FPGA timing closure may fail

Constraints help FPGA tools optimize the design properly.

---

# Conclusion

In Day 3, a Mythcore RISC-V processor was successfully implemented and analyzed on FPGA.

The work included:

- RTL simulation
- Synthesis
- Schematic analysis
- Package mapping
- ILA debugging
- Timing analysis
- Utilization analysis
- Power analysis

This experiment provided deeper understanding of how complex processor-based RTL designs are implemented, routed, timed, and debugged inside FPGA architectures.

---
# Day 4 - Introduction to SOFA FPGA Fabric

## Overview

Day 4 focused on understanding and implementing designs on the **SOFA FPGA Fabric** using the **OpenFPGA Framework**.  
The complete flow involved:

- Running OpenFPGA
- Generating FPGA fabric
- Running VTR flow on the generated architecture
- Performing placement and routing
- Timing analysis
- Power analysis
- Post-synthesis simulation

The design used for experimentation was a **4-bit Up Counter**.

---

# Introduction to SOFA FPGA Fabric

SOFA (**Skywater Open-source FPGAs**) is an open-source FPGA framework developed using:

- Skywater 130nm PDK
- OpenFPGA Framework
- VTR Flow

The architecture used:

- `FPGA1212_QLSOFA_HD_PNR`
- Maximum frequency: **50 MHz**
- 1152 LUTs
- 2304 Flip-Flops
- 1152 Soft Adders

---

# OpenFPGA Flow Execution

## Running OpenFPGA

The OpenFPGA framework was executed to generate the FPGA fabric and run the counter design on the custom FPGA architecture.

### Command Used

```bash
make runOpenFPGA
```

---

## OpenFPGA Execution Output

<img width="940" height="258" alt="image" src="https://github.com/user-attachments/assets/a9ca4995-b440-4e4d-9604-0a28fac81af8" />

*OpenFPGA execution flow and task completion log*

---

## Generated Files

The OpenFPGA flow generated multiple reports and implementation files.

<img width="940" height="84" alt="image" src="https://github.com/user-attachments/assets/c306773f-9854-4752-89b1-73eb56daf93a" />

*Generated implementation reports and output files after OpenFPGA execution*

---

# Counter Design Used

## Counter Verilog Code

```verilog
/*Important: Once you run ./a.out, it will keep running infinitely,
because it is in an always block.
You need to hit Ctrl+Z to stop it,
else, the vcd will become a large file and will never end.
*/

module up_counter (
    out ,      // Output of the counter
    enable ,   // enable for counter
    clk ,      // clock Input
    reset      // reset Input
);

output [3:0] out;

//you can alternately write this as output reg [7:0] out;

input enable, clk, reset;

//----------Internal Variables--------------

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

# Counter Area Report

The implementation reports generated by the SOFA FPGA Fabric were analyzed to determine:

- LUT utilization
- Flip-Flop usage
- Resource occupancy
- FPGA fabric mapping

<img width="940" height="433" alt="image" src="https://github.com/user-attachments/assets/a4becbf9-7d1b-4111-8009-304db251c916" />

*Area utilization report of the counter mapped on SOFA FPGA Fabric*

---

<img width="940" height="527" alt="image" src="https://github.com/user-attachments/assets/8feb8ee3-00b8-4891-83ec-8d4fef4bda1d" />

*Logical elements implemented from vpr_stdout.log*

---

# Constraint File

The timing constraints were provided using an `.sdc` file.

## Example Constraint File

```tcl
create_clock -period 10 up_counter_clk

set_input_delay  -clock up_counter_clk -max 0 [get_ports {*}]

set_output_delay -clock up_counter_clk -max 0 [get_ports {*}]
```

---

# Modifying VPR Command

The VPR command was modified to include timing constraints.

## Updated VPR Command

<img width="940" height="200" alt="image" src="https://github.com/user-attachments/assets/adb3cec8-f6c1-4f47-b4cf-3038d7a83b61" />

---

# Post-Synthesis Simulation

Post-synthesis simulation was performed using the generated netlist and timing files.

The generated files included:

- `.blif`
- `.net`
- `.place`
- `.route`
- `.sdf`
- Post-synthesis Verilog netlist

---

## Post-Synthesis Simulation Command

<img width="940" height="210" alt="image" src="https://github.com/user-attachments/assets/6ae14644-2110-4c63-840d-cd28cca7c964" />

---

## Post-Synthesis Simulation Output

<img width="999" height="173" alt="image" src="https://github.com/user-attachments/assets/59ae682a-9728-4364-b829-757837d50eb7" />

*Post-synthesis simulation waveform of the up counter.*

---

# Timing Violation Analysis

Initially, setup timing violations were observed during implementation.

After adding proper constraints:

- Setup timing was met
- Hold timing was satisfied
- Positive slack was achieved

---

## Setup Timing Report

<img width="940" height="536" alt="image" src="https://github.com/user-attachments/assets/8f23f3b6-bf62-41a6-941d-8afac4deb0f4" />

*Setup timing analysis report after implementation.*

---

## Hold Timing Report

<img width="940" height="538" alt="image" src="https://github.com/user-attachments/assets/9c518ab4-4c20-4f50-bf7e-cfd6e3e9aa3d" />

*Hold timing analysis report after implementation.*

---

# Power Analysis

Power analysis was performed using the generated reports from the VTR flow.

The report included:

- Dynamic Power
- Static Power
- Clock Power
- Logic Power
- Signal Power

---

## Power Report Screenshot

<img width="940" height="320" alt="image" src="https://github.com/user-attachments/assets/b43ec986-e60f-4008-8a64-1725df5af1ea" />

*Power analysis report of the counter on SOFA FPGA Fabric*

---

# Important Files Generated

The following important files were generated during the OpenFPGA flow:

| File | Description |
|------|-------------|
| `counter.blif` | BLIF netlist generated after synthesis |
| `counter.net` | Netlist connectivity |
| `counter.place` | Placement report |
| `counter.route` | Routing report |
| `counter.sdf` | Timing delay information |
| `counter_post_synthesis.v` | Post synthesis Verilog netlist |
| `report_timing.setup.rpt` | Setup timing report |
| `report_timing.hold.rpt` | Hold timing report |
| `vpr_stdout.log` | VPR execution log |
| `power.rpt` | Power analysis report |

---

# Key Learnings

- Understood SOFA FPGA architecture
- Learned OpenFPGA execution flow
- Performed FPGA fabric generation
- Analyzed timing reports
- Observed setup and hold violations
- Applied timing constraints using SDC
- Performed post-synthesis simulation
- Analyzed FPGA power consumption
- Explored generated implementation files
- # Day 5 - RISCV Core on Custom SOFA Fabric

The RVMYTH RISC-V core was integrated with the custom SOFA FPGA fabric and executed through the complete OpenFPGA and VTR flow. The implementation generated various timing, utilization, routing, and simulation reports which were analyzed to verify the correctness of the design and FPGA mapping process.

---

# SOFA RVMYTH Utilization Report

The utilization report shows the FPGA resource usage of the RVMYTH core after synthesis and mapping onto the SOFA FPGA architecture.

The design consumes LUTs, latches, CLBs, routing resources, and logic elements. This report helps in understanding the hardware cost of implementing the processor core on the FPGA fabric.

## Circuit Statistics

- Total Blocks: 5526
- Inputs: 2
- Latches: 1807
- Outputs: 8
- 0-LUTs: 4
- 4-LUTs: 3705

## Net Statistics

- Total Nets: 5518
- Average Fanout: 3.1
- Maximum Fanout: 1807
- Minimum Fanout: 1.0

## Timing Graph

- Timing Graph Nodes: 22705
- Netlist Clocks: 1

<img width="737" height="508" alt="image" src="https://github.com/user-attachments/assets/17be3dde-1973-4f0d-b3ef-595d39615bb9" />

*Detailed FPGA primitive block usage and logic element statistics for the RVMYTH implementation.*

---

# Constraints File

An SDC (Synopsys Design Constraints) file was created to define the clock timing constraints for the design.

## Constraint File Used

```tcl
create_clock -period 200 clk
set_input_delay -clock clk -max 0 [get_ports {*}]
set_output_delay -clock clk -max 0 [get_ports {*}]
```

The constraint file defines:

- Clock period
- Input delay constraints
- Output delay constraints

These constraints are used by the timing analysis engine during placement and routing.

<img width="940" height="149" alt="image" src="https://github.com/user-attachments/assets/189d0a21-364a-44cf-8984-dbb105813b27" />

*SDC constraints file used for RVMYTH implementation on the SOFA FPGA fabric.*

---

# Timing Analysis

Timing analysis was performed after placement and routing to verify whether the design satisfies setup and hold timing constraints.

The setup timing report shows that the data arrival time is within the required timing limits and timing slack is positive.

<img width="658" height="562" alt="image" src="https://github.com/user-attachments/assets/322adca8-4b7e-4e0d-9ce2-d97e25f2f13f" />

*Setup timing analysis report showing positive timing slack for the RVMYTH core.*

---

# Hold Timing Analysis

Hold timing analysis verifies that the data remains stable for the required duration after the clock edge.

<img width="703" height="608" alt="image" src="https://github.com/user-attachments/assets/56ddd848-a357-4390-9668-ffed0fdb4a0e" />

*Hold timing analysis report showing successful hold timing closure.*

---

# Post Implementation Simulation

Post implementation simulation was performed after synthesis, placement, and routing of the RVMYTH core.

The generated waveform confirms the correct execution of the processor logic after FPGA implementation. The simulation validates that the synthesized and routed netlist behaves correctly under the given clock and reset conditions.

The waveform also demonstrates proper output transitions and stable processor execution after implementation.

<img width="973" height="258" alt="image" src="https://github.com/user-attachments/assets/0faf8ae4-f2db-4f7f-886a-d55028ed1691" />


*Post implementation simulation waveform of the RVMYTH core on SOFA FPGA fabric.*

---

# Observations

- The RVMYTH processor was successfully mapped onto the custom SOFA FPGA fabric.
- FPGA resource utilization increased significantly compared to smaller counter-based designs.
- Timing constraints were successfully met with positive setup and hold slack values.
- The OpenFPGA and VTR flow successfully generated all required reports and simulation outputs.
- Post implementation simulation verified the correctness of the synthesized FPGA netlist.

---

# Conclusion

The RVMYTH RISC-V core was successfully implemented on the custom SOFA FPGA architecture using the OpenFPGA and VTR framework. The generated utilization reports, timing analysis, and simulation waveforms confirm that the processor operates correctly on the FPGA fabric while satisfying all timing constraints.

This experiment demonstrated the complete FPGA implementation flow starting from RTL design to post implementation verification using an open-source FPGA toolchain.


--gen_post_synthesis_netlist on
```
