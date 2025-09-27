# Week 1 : Day 1
# Introduction to Verilog RTL design and Synthesis

## Overview :
On this first day, I was introduced to Verilog, a key hardware description language, and discover how to simulate your digital circuits using the open-source tool Icarus Verilog (iverilog). I learnt about logic synthesis by experimenting with Yosys, another open-source tool. This session includes hands-on labs, straightforward explanations, and essential background knowledge—all designed to help you establish a solid understanding of Register Transfer Level (RTL) design fundamental.


## Table of content :
1. Introduction to Verilog RTL design and Synthesis
2. Introduction to open-source simulator iverilog
3. Labs using iverilog and gtkwave
4. Introduction to Yosys and Logic synthesis

### 1.Introduction to Verilog RTL design and Synthesis
Begin by exploring the fundamental ideas behind digital circuits and systems. This forms the essential groundwork needed before diving into more advanced topics, making sure the guiding principles and real-world importance of digital design are clear at the very start.

### 2. Introduction to open-source simulator iverilog
 Get started with Verilog, a powerful language used by engineers to describe digital systems. Through clear explanations and interactive exercises, you’ll learn how Verilog enables you to model circuits at the Register Transfer Level (RTL), bringing your design ideas to life on paper and in simulation.

### 3. Labs using iverilog and gtkwave
Practice running your Verilog code with Icarus Verilog (iverilog), a widely used open-source simulator. This step lets you see your written logic in action, spot errors or unexpected behaviors, and gain practical confidence through direct experimentation.

#### Compiling the RTL code and testbench code using iverilog
```bash
iverilog good_mux.v tb_good_mux.v
```
#### Simulation
```bash
./a.out
```
#### View Waveform in Gtkwave
```bash
gtkwave tb_good_mux.vcd
```
<img width="1300" height="736" alt="day1_gtkwave" src="https://github.com/user-attachments/assets/b00037af-f22b-4cc5-bf6a-dc5f251a01a4" />


### 4. Introduction to Yosys and Logic synthesis
Step into the basics of logic synthesis by using Yosys. You will learn how hardware description gets converted into gate-level logic, setting the stage for future implementation on actual hardware like FPGAs or ASICs. Understanding this flow is a cornerstone of the digital design process.

open the verilog_files directory in the terminal using the below given code :
```bash
cd VLSI
cd verilog_files
```
open yosys
```bash
yosys
```
now ,in yosys enter these codes :

Reading liberty library
```bash
read_liberty -lib /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```
Reading Verilog code
```bash
read_verilog /home/vsduser/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files/good_mux.v
```
Synthesizing the RTL code/design
```bash
synth -top good_mux
```

Technology mapping
```
abc -liberty /address/to/your/sky130/file/sky130_fd_sc_hd__tt_025C_1v80.lib
```

Visualize the gate-level netlist
```
show
```
*We got error while running show so we first installed needed python packages :
```bash
sudo apt-get install python3-distutils
```






   
   
