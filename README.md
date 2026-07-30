# FPGA-Fabric-Design-and-Architecture
refers to the internal structure and organization of a Field-Programmable Gate Array (FPGA) and other variants 

# Day 1 – Introduction to FPGA Architecture and Vivado Design Flow

## Objective

The objective of Day 1 was to understand the fundamentals of FPGA architecture, FPGA design flow, and implementation of a simple digital design using Xilinx Vivado on the Basys 3 FPGA board.

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

# Introduction to FPGA ( Field Programmable Gate Array)

An FPGA is an integrated circuit that can be configured by the user after manufacturing. Unlike ASICs, which are fabricated for a fixed functionality, FPGAs can be reprogrammed multiple times.

Significance of FPGA:
- Hardware acceleration
- Signal processing
- Embedded systems
- Machine learning
- Aerospace systems
- High-performance computing

## Comparison  

| FPGA | ASIC |
|---|---|
| Reprogrammable | Fixed after fabrication |
| Faster prototyping | Long fabrication cycle |
| Lower initial cost | High initial fabrication cost |
| Higher flexibility | Optimized performance |
| RTL to Bitstream | RTL to Layout |

---

# FPGA Architecture
![FPGA Architecture] (https://github.com/sneh2411/FPGA-Fabric-Design-and-Architecture/FPGA Architecture.png)
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
