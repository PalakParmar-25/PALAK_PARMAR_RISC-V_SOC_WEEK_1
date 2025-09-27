<div align="center">
 
# Week 1 : Day 4
# Gate-Level Simulation (GLS), Blocking vs. Non-Blocking in Verilog, and Synthesis-Simulation Mismatch

</div>

<div align="center">
 
[![RISC-V](https://img.shields.io/badge/RISC--V-SoC%20Tapeout-blue?style=for-the-badge&logo=riscv)](https://riscv.org/)
[![VSD](https://img.shields.io/badge/VSD-Program-orange?style=for-the-badge)](https://vsdiat.vlsisystemdesign.com/)
![Week](https://img.shields.io/badge/Week-1-green?style=for-the-badge)

</div>

## Overview :
Post-synthesis, Gate-Level Simulation (GLS) validates the logical correctness and timing of the netlist against the original RTL using delay annotations, ensuring timing requirements are met. Mismatches between RTL and netlist simulations often arise due to non-standard Verilog practices, incomplete sensitivity lists, or misuse of blocking (sequential execution) versus non-blocking (concurrent execution) assignments. Understanding and applying these conventions correctly is necessary to avoid functional discrepancies and achieve reliable synthesis results.

## Table of Content :
1. Gate-Level Simulation (GLS)
2. Synthesis-Simulation Mismatch
3. Blocking vs. Non-Blocking Assignments in Verilog
4. Lab work

###  1. Gate-Level Simulation (GLS)

Gate-Level Simulation (GLS) is an essential verification phase in the VLSI design process. It involves simulating the synthesized netlist of a digital design—i.e., the version made up of actual logic gates—to verify that the design behaves as expected. 
- It checks whether the gate-level version still performs the correct logic.
- Ensures timing constraints are met, especially when delays are applied.
- Provides more accurate power estimation than RTL simulation.Verifies scan chains and other test logic are properly implemented.

How we run it :
Post-Synthesis: After the RTL has been compiled into a gate-level netlist.
Pre-Layout (and sometimes Post-Layout): Helps catch logic and timing bugs early, before place-and-route.

### 2. Synthesis-Simulation Mismatch
synthesis-simulation mismatch happens when the behavior observed in the **RTL simulation (before synthesis)** differs from the results seen in the gate-level simulation (after synthesis) or actual hardware testing.

 Common Causes of Mismatch:
Non-synthesizable Code:
  Using constructs like delays, initial blocks, or other elements that synthesis tools do not support.

Ambiguous or Incomplete RTL:
  Examples include missing `else` statements or incorrect sensitivity lists in always blocks, which lead to unintended simulation behavior.

Differences in Tool Interpretation:
  Simulation and synthesis tools may handle unclear or ambiguous RTL code differently, causing discrepancies.
  
Important Takeaway
To avoid mismatches, it’s crucial to write **clear, synthesizable, and unambiguous RTL code**. Following good coding standards helps ensure consistent behavior across simulation, synthesis, and hardware implementation.

3. Blocking vs. Non-Blocking Assignments in Verilog

#### There are two types of procedural assignments :
a.Blocking Statements (=)
Syntax: =
It is executed sequential andimmediately.
It is also suitable for Combinational logic (e.g., always @(*) ,temp variables.

b.Non-Blocking Statements (<=)
Syntax: <=
It's execution is Scheduled, it executes concurrently at the end of the time step.
It is suitable Sequential logic (e.g., always @(posedge clk),registers/flip-flops.

### Lab work :

#### Verilog code for a 2:1 multiplexer using a ternary operatoris given below:

```bash
module ternary_operator_mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```
we used the function : y = i1 if sel = 1; else y = i0.

#### simulated the rtl_design of ternary_operator_mux.v and got waveforms in the gtkwave :
<img width="1300" height="736" alt="day4_ternary_mux net" src="https://github.com/user-attachments/assets/7afb734a-abbb-47a6-ab99-a368f09951f2" />

#### synthesised it using Yosys :

<img width="1300" height="736" alt="day4_ternary_mux net" src="https://github.com/user-attachments/assets/d524973e-686a-4775-904e-b4b3d666508b" />

#### GLS for 2x1 MUX :

<img width="1300" height="736" alt="day4_ternary_good_mux_gtkwave" src="https://github.com/user-attachments/assets/e3816ae1-9dd4-4a39-8f16-477b186e663b" />

#### Bad MUX Example (Common Pitfalls)
let's take a code with mistakes

```bash
module bad_mux (input i0, input i1, input sel, output reg y);
  always @ (sel) begin
    if (sel)
      y <= i1;
    else 
      y <= i0;
  end
endmodule
```
##### Major issues:
Incomplete sensitivity list: Should include i0, i1, and sel.
Non-blocking assignment in combinational logic: Should use blocking assignments (=).

#### The correct code is :
```bash
always @ (*) begin
  if (sel)
    y = i1;
  else
    y = i0;
end
```

#### simulation of bad_mux :

<img width="1300" height="736" alt="day4_bad_mux_gtkwave" src="https://github.com/user-attachments/assets/9b87fa93-eaf4-4367-ad91-36b813dc6b59" />

#### Performing GLS for bad_mux :

<img width="1300" height="736" alt="day4_bad_mux_net_gtkwave" src="https://github.com/user-attachments/assets/a5905017-d7a0-4138-b7d6-64668fe18813" />

#### Blocking Assignment Caveat

Verilog code of blocking_caveat :

```bash
module blocking_caveat (input a, input b, input c, output reg d);
  reg x;
  always @ (*) begin
    d = x & c;
    x = a | b;
  end<img width="1300" height="736" alt="day4_block_gtkwave" src="https://github.com/user-attachments/assets/26061217-eb61-4585-b8b7-568f28369ff8" />

endmodule
```
#### Fault in the code :
The order of assignments causes d to use the old value of x—not the newly computed value.
To avoid this assign intermediate variables before using them.

#### Correct Code :
```bash
always @ (*) begin
  x = a | b;
  d = x & c;
end
```
simulation of blocking_caveat :
<img width="1300" height="736" alt="day4_blocking_net_gtkwave" src="https://github.com/user-attachments/assets/0e50c9b6-a1db-482d-99db-58592f2dfe21" />

<img width="1300" height="736" alt="day4_block_gtkwave" src="https://github.com/user-attachments/assets/4dee1fc1-eb02-420d-a851-b85b58535428" />

Synthesis of blocking_caveat :
<img width="1300" height="736" alt="day4_block_show" src="https://github.com/user-attachments/assets/52b19282-a954-4eff-a8ef-b4924dd890d6" />





