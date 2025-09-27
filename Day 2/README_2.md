# Week 1 : Day 2
# Timing Libraries, Synthesis Approaches, and Efficient Flip-Flop Coding

## Overview :

## Table of Content :
1. Introduction to timing .libs
2. Hierarchical vs Flat Synthesis
3. Various Flop Coding Styles and optimization

### 1. Introduction to timing .libs
Synthesis relies on standard cell libraries, typically provided as .lib files, which contain detailed information about the logical cells available for implementation. Each cell may exist in different flavors—slow, medium, or fast—to accommodate the critical timing and hold requirements of the design. Fast cells have larger transistors offering higher current drive to charge load capacitances quickly, thus minimizing delay but increasing area and power consumption. Conversely, slow cells reduce power and area but contribute to longer delay, useful for mitigating hold time violations. In digital circuit design, these contravening factors of area, power, and timing form the design constraints that synthesis tools must balance to achieve efficient and reliable operation.

### 2. Hierarchical vs Flat Synthesis

Design Structuring, Sequential Logic, and Timing Considerations
Designs can be synthesized in a hierarchical or flat manner. Hierarchical synthesis breaks the design into manageable submodules, typically used when the design includes repeated instances. 

- We simulated the multiple_modules.v
- then genrated the Gtkwave
- Synthesised it using Yosys
- We generated individual netlist graphical representation for each module including all sub modules using commands such as hierarchy

  <img width="1366" height="768" alt="day2_heir1_part1" src="https://github.com/user-attachments/assets/72b2ce7e-f5ad-4dde-b559-c2b0ea5f8a2c" />
Sub module :
  <img width="662" height="699" alt="day2_heir_sub_module" src="https://github.com/user-attachments/assets/03100cec-b90d-464d-9f4c-6f101f1112a1" />

Multiple module :
<img width="1366" height="768" alt="day2_heir_part1" src="https://github.com/user-attachments/assets/da0ce7c5-1c15-43ef-b413-9319dd57392d" />

- Same way we also flattened the multiple_module_opt and multiple_module_opt2 including code "flatten" during synthesis in Yosys 


### 3. Various Flop Coding Styles and optimization
Flip-flops are crucial elements in synchronous design, serving to store data between clock cycles and eliminate glitches caused by propagation delays in combinational logic paths. Sequential elements are triggered on either the positive or negative edge of the clock, which stabilizes timing and ensures correct data flow. Flop initialization typically uses synchronous or asynchronous set/reset signals. Timing information in .lib files includes variations in process, voltage, and temperature (PVT), which influence delay and performance margins, making the timing-aware synthesis and simulation crucial for realistic hardware verification.

- We simulated the syncres.v , async_set and asyncres.v
- then genrated the Gtkwave
- Synthesised it using Yosys
- We generated individual netlist graphical representation for each design

simulation :
  <img width="1300" height="736" alt="DAY2_FLOP_asyncres_PART1" src="https://github.com/user-attachments/assets/596bf2cb-0335-4b51-8948-de88e2144ce6" />

synthesis :
  <img width="1300" height="736" alt="day2_show_asyncres_flop" src="https://github.com/user-attachments/assets/31705df8-74c7-407d-b40e-d86db4b32238" />



