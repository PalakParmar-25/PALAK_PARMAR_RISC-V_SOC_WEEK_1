v align="center">
 
# Week 1 : Day 3
# Combinational and Sequential Optimization

</div>

<div align="center">
 
[![RISC-V](https://img.shields.io/badge/RISC--V-SoC%20Tapeout-blue?style=for-the-badge&logo=riscv)](https://riscv.org/)
[![VSD](https://img.shields.io/badge/VSD-Program-orange?style=for-the-badge)](https://vsdiat.vlsisystemdesign.com/)
![Week](https://img.shields.io/badge/Week-1-green?style=for-the-badge)

</div>

## Table of Content :
1. If Case constructs
2. Labs for If-Else and Case Statements
3. For Loops and for generated
4. RCA (Ripple Carry Adder)


### 1. If Case constructs

An if-else statement is a conditional construct used in RTL (Register Transfer Level) code to describe different behaviors based on a condition.

#### Example in Verilog:

```bash
always @(posedge clk) begin
    if (reset)
        q <= 0;
    else
        q <= d;
end
```
In the above, if reset is high, q is reset to 0; otherwise, it gets the value of d. This structure synthesizes to a flip-flop with reset behavior.

#### What is an inferred latch?

An inferred latch is an unintended sequential storage element (like a latch) that a synthesis tool creates when RTL code doesn't fully specify what should happen in all conditions.
It usually happens in combinational always blocks when else or default is missing.

##### Example that causes an inferred latch:
```bash
always @(a or b) begin
    if (a)
        y = b;  // What happens when a == 0?
end
```
In this case:
If a is 1, y gets b.
But when a is 0, the code says nothing about y, so the synthesizer assumes y must remember its previous value — and creates a latch to hold it.

#### How to Avoid Inferred Latches:
Use else or default branches in if or case statements.
Make sure every possible input condition assigns a value to every output.
For pure combinational logic, always assign all outputs in all paths.

##### Correct version (no latch):
```bash
always @(a or b) begin
    if (a)
        y = b;
    else
        y = 0;  // Now y is always assigned → no latch
end
```

### 2. Labs for If-Else and Case Statements
#### a.Incomplete If Statement
```bash
module incomp_if (input i0, input i1, input i2, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
end
endmodule
```
simulation :
<img width="1300" height="736" alt="day5_incomp_case_wave" src="https://github.com/user-attachments/assets/8ee716dc-e6f8-4fc3-bd83-8ceb8d64e4b0" />

synthesis :
<img width="1300" height="736" alt="DAY5_INCOMP_CASE_SHOW" src="https://github.com/user-attachments/assets/d3aa2e63-7225-493f-8a9e-c6eacdc380a7" />

#### b.nested if-else
```
module incomp_if2 (input i0, input i1, input i2, input i3, output reg y);
always @(*) begin
    if (i0)
        y <= i1;
    else if (i2)
        y <= i3;
end
endmodule
```
simulation :
<img width="1300" height="736" alt="day5_incomp_if2_wave" src="https://github.com/user-attachments/assets/662cb6eb-55e4-45f9-9ed5-96fcb9c1a569" />

synthesis :
<img width="1300" height="736" alt="day5_incomp_if2_show" src="https://github.com/user-attachments/assets/db9f12dd-7eca-4e41-ab4a-0d83343e8902" />

#### c. Complete Case Statement
```bash
module comp_case (input i0, input i1, input i2, input [1:0] sel, output reg y);
always @(*) begin
    case(sel)
        2'b00 : y = i0;
        2'b01 : y = i1;
        default : y = i2;
    endcase
end
endmodule
```
simulation :
<img width="1300" height="736" alt="day5_comp_case_wave" src="https://github.com/user-attachments/assets/719196ef-6d68-4634-b510-af75a5cd0e6e" />

synthesis :
<img width="1300" height="736" alt="day5_comp_case_show" src="https://github.com/user-attachments/assets/0fac926e-66e8-4890-9a25-bfb414fdb91d" />

#### d. Incomplete case Handling
```
module bad_case (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
always @(*) begin
    case(sel)
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        2'b1?: y = i3; // '?' is a wildcard; be careful with incomplete cases!
    endcase
end
endmodule
```
simulation :
<img width="1300" height="736" alt="day5_bad_case_net_gtkwave" src="https://github.com/user-attachments/assets/c9c8b92d-eed0-45b6-a2d4-983fb3fad819" />

#### e. Partial Assignment in case
```bash
module partial_case_assign (
    input i0, input i1, input i2,
    input [1:0] sel,
    output reg y, output reg x
);
always @(*) begin
    case(sel)
        2'b00: begin
            y = i0;
            x = i2;
        end
        2'b01: y = i1;
        default: begin
            x = i1;
            y = i2;
        end
    endcase
end
endmodule
```
simulation :
<img width="1300" height="736" alt="day5_partial_bad_case_gtkwave" src="https://github.com/user-attachments/assets/42987aa8-1a3a-4c99-97a6-a18697806840" />

synthesis :
<img width="1300" height="736" alt="day5_partial_comp_show" src="https://github.com/user-attachments/assets/fe08e8de-dce6-4e7e-82be-7eb6c47f3f90" />

### 3. For Loops and for generated

code :
```
module mux_generate (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
wire [3:0] i_int;
assign i_int = {i3, i2, i1, i0};
integer k;
always @(*) begin
    for (k = 0; k < 4; k = k + 1) begin
        if (k == sel)
            y = i_int[k];
    end
end
endmodule
```
#### For a 4x1 MUX using for loop :
simulation :
<img width="1300" height="736" alt="day5_mux_generate_gtkwave" src="https://github.com/user-attachments/assets/04c8411a-7c62-40de-b74a-5b79d574f005" />

#### 8x1 De-Mux using case:
```
code :
module mux_generate (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
wire [3:0] i_int;
assign i_int = {i3, i2, i1, i0};
integer k;
always @(*) begin
    for (k = 0; k < 4; k = k + 1) begin
        if (k == sel)
            y = i_int[k];
    end
end
endmodule
```

simulation :
<img width="1300" height="736" alt="day5_demux_case_gtkwave" src="https://github.com/user-attachments/assets/79670479-5a26-4520-9258-ea4c450bc808" />

synthesis:
<img width="1300" height="736" alt="day5_demux_case_show" src="https://github.com/user-attachments/assets/a022c852-94a0-496f-8916-a09e42ae22bb" />

#### De-Mux using For loop
code :
```bash
module demux_generate (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
integer k;
always @(*) begin
    y_int = 8'b0;
    for (k = 0; k < 8; k = k + 1) begin
        if (k == sel)
            y_int[k] = i;
    end
end
endmodule
```

simulation:
<img width="1300" height="736" alt="day5_demux_generate_gtwave" src="https://github.com/user-attachments/assets/d214b69b-f8bb-472b-b589-34fced40d7c3" />

synthesis :
<img width="1300" height="736" alt="day5_demux_generate_show" src="https://github.com/user-attachments/assets/42f02d50-8ad9-4b6f-9bda-fc5986a4889e" />

### 5. RCA (Ripple Carry Adder)

code :
```
module rca (
    input [7:0] num1,
    input [7:0] num2,
    output [8:0] sum
);
wire [7:0] int_sum;
wire [7:0] int_co;

genvar i;
generate
    for (i = 1; i < 8; i = i + 1) begin
        fa u_fa_1 (.a(num1[i]), .b(num2[i]), .c(int_co[i-1]), .co(int_co[i]), .sum(int_sum[i]));
    end
endgenerate

fa u_fa_0 (.a(num1[0]), .b(num2[0]), .c(1'b0), .co(int_co[0]), .sum(int_sum[0]));

assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```
#### Full adder module :
code :
```
module fa (input a, input b, input c, output co, output sum);
    assign {co, sum} = a + b + c;
endmodule
```
simulation :

<img width="1300" height="736" alt="day5_rca_gtkwave" src="https://github.com/user-attachments/assets/27c14f8e-190a-4ea3-b6ca-d6583fb37f61" />


