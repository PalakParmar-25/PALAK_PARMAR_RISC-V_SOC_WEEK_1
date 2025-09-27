<div align="center">
 
# Week 1 : Day 3
# Combinational and Sequential Optimization

</div>

<div align="center">
 
[![RISC-V](https://img.shields.io/badge/RISC--V-SoC%20Tapeout-blue?style=for-the-badge&logo=riscv)](https://riscv.org/)
[![VSD](https://img.shields.io/badge/VSD-Program-orange?style=for-the-badge)](https://vsdiat.vlsisystemdesign.com/)
![Week](https://img.shields.io/badge/Week-1-green?style=for-the-badge)

</div>

## Table of Content :
1. introduction to optimizations
2. Combinational logic optimizations
3. Sequential logic optimizations
4.Sequential optimizations for unused outputs

### 1. introduction to optimizations

Optimization during synthesis improves area, power, and performance by simplifying combinational logic through Boolean minimization techniques and by removing unused states or outputs. More advanced methods, like state optimization, retiming, and sequential logic cloning, further enhance the design’s efficiency and operational speed. Post-synthesis, Gate-Level Simulation (GLS) validates the logical correctness and timing of the netlist against the original RTL using delay annotations, ensuring timing requirements are met. Mismatches between RTL and netlist simulations often arise due to non-standard Verilog practices, incomplete sensitivity lists, or misuse of blocking (sequential execution) versus non-blocking (concurrent execution) assignments. Understanding and applying these conventions correctly is necessary to avoid functional discrepancies and achieve reliable synthesis results.

### 2. Combinational logic optimizations
Combinational logic optimizations focus on simplifying logic without changing its behavior. These include techniques like constant propagation, logic reduction (e.g., removing redundant gates), and merging equivalent logic paths. The goal is to reduce gate count and improve performance or area efficiency in synthesized hardware.

- We optimzed conbinational circuits using many gates like AND gate ,NOT gate ,OR gate etc. by simulating and synthesysing the RTL desing of the gate , using appropriate commands in the terminal

<img width="1300" height="736" alt="day3_show_opt_check" src="https://github.com/user-attachments/assets/626a1b19-ec7f-41cd-95c3-9491b1365e93" />

<img width="1300" height="736" alt="day3_show_check3" src="https://github.com/user-attachments/assets/dc2846e9-391d-4a3a-ba77-2afb395bf411" />

 <img width="1300" height="736" alt="day3_show_check4" src="https://github.com/user-attachments/assets/e3aa11ce-3f74-4fbf-82d9-ea515d6945f5" />

### 3. Sequential logic optimizations
Sequential logic optimizations involve improving the design around flip-flops or latches. This may include removing unused or redundant registers, optimizing reset logic, and retiming (moving registers to balance timing). Such optimizations help reduce unnecessary state elements and improve clock performance.

- We optimzed sequential circuits like register , flip flop by simulating and synthesysing the RTL desing of the sequential circuits ,using appropriate commands in the terminal

#### Simulated output :
<img width="1300" height="736" alt="day3_show_const5_seq" src="https://github.com/user-attachments/assets/f27e2999-6dd1-4d0c-ad0f-9733068b5293" />

#### Synthesised output :
  <img width="1300" height="736" alt="day3_show_dff_const1_seq" src="https://github.com/user-attachments/assets/36b0bd25-aff2-4790-b10c-b1d3490703e9" />

<img width="1300" height="736" alt="day3_show_const4_seq" src="https://github.com/user-attachments/assets/2245ad0d-e513-42ab-b81a-367ef956d1c4" />

<img width="1300" height="736" alt="day3_show_dff-const2_seq" src="https://github.com/user-attachments/assets/eb4abfd1-4728-42a8-8362-869362064157" />

<img width="1300" height="736" alt="day3_seq gtkwave_const1" src="https://github.com/user-attachments/assets/97f3b3fa-df58-4d03-b599-95e43b432bf5" />

4.Sequential optimizations for unused outputs
In cases where sequential logic drives outputs that are never used, synthesis tools can identify and eliminate this dead logic. Removing these unused output paths helps reduce register count, logic depth, and power consumption, while also improving synthesis and timing performance.

optimzation of counter by simulating and synthesysing the RTL desing of the counter register ,using appropriate commands in the terminal.

<img width="1300" height="736" alt="day3_show_counter_opt_seq" src="https://github.com/user-attachments/assets/693a2181-7542-4902-a002-050a6b579eda" />
 
flattening of multiple_module_opt.v :
<img width="1300" height="736" alt="day3_fatten_multipule_module" src="https://github.com/user-attachments/assets/a96637ee-94ec-4a44-8b45-3e6b5c902545" />

flattening of multiple_module_opt2.v :
<img width="1300" height="736" alt="day3_multiple_module2_flatten" src="https://github.com/user-attachments/assets/bb9b4055-038a-4278-aae1-7a3614f67b8b" />




