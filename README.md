# EXPERIMENT 3A: Simulation of All Flip-Flops using Non Blocking Statement

## AIM
To design and simulate basic flip-flops (SR, D, JK, and T) using **Non blocking statements** in Verilog HDL, and verify their functionality through simulation in Vivado 2023.1.

## APPARATUS REQUIRED
- Vivado 2023.1
- Computer with HDL Simulator

## DESCRIPTION
Flip-flops are the basic memory elements in sequential circuits.  
In this experiment, different types of flip-flops (SR, D, JK, T) are modeled using **behavioral modeling** with **Non blocking assignment (`<=`)** inside the `always` block.  
Non Blocking assignments execute sequentially in the given order, which makes it easier to describe simple synchronous circuits.

## PROCEDURE
1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** (e.g., `FlipFlop_Simulation`).  
3. Add Verilog source files for each flip-flop (SR, D, JK, T).  
4. Add a testbench file to verify all flip-flops.  
5. Run **Behavioral Simulation**.  
6. Observe waveforms of inputs and outputs for each flip-flop.  
7. Verify that outputs match the truth table.  
8. Save results and capture simulation screenshots.

---

## VERILOG CODE

### SR Flip-Flop (Non Blocking)
```module sr_flipflop (
    input  wire clk,
    input  wire reset,   
    input  wire S,
    input  wire R,
    output reg  Q,
    output wire Qbar
);

assign Qbar = ~Q;

always @(posedge clk or posedge reset) 
begin
    if (reset)
        Q <= 1'b0;
    else begin
        case ({S,R})
            2'b00: Q <= Q;      
            2'b01: Q <= 1'b0;  
            2'b10: Q <= 1'b1;  
            2'b11: Q <= 1'bx;   
        endcase
    end
end

endmodule
```
### SR Flip-Flop Test bench 
```
module sr_flipflop_tb;

reg clk;
reg reset;
reg S;
reg R;
wire Q;
wire Qbar;

sr_flipflop uut (
    .clk(clk),
    .reset(reset),
    .S(S),
    .R(R),
    .Q(Q),
    .Qbar(Qbar)
);
always #5 clk = ~clk;
initial 
begin
    clk = 0;
    reset = 1;
    S = 0;
    R = 0;

    #10 reset = 0;
    #10 S = 0; R = 0;
    #10 S = 1; R = 0;
    #10 S = 0; R = 1;
    #10 S = 1; R = 1;
    #10 S = 0; R = 0;
    #20 $finish;
end

endmodule
```
#### SIMULATION OUTPUT

<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/6a2557dd-12f0-4591-8045-fe905d7c8edc" />


### JK Flip-Flop (Non Blocking)
```
module jk_flipflop (
    input  wire clk,
    input  wire reset,   
    input  wire J,
    input  wire K,
    output reg  Q,
    output wire Qbar
);

assign Qbar = ~Q;

always @(posedge clk or posedge reset)
begin
    if (reset)
        Q <= 1'b0;
    else begin
        case ({J,K})
            2'b00: Q <= Q;      
            2'b01: Q <= 1'b0;   
            2'b10: Q <= 1'b1;   
            2'b11: Q <= ~Q;     
        endcase
    end
end

endmodule
```
### JK Flip-Flop Test bench 
```verilog
module jk_flipflop_tb;
reg clk;
reg reset;
reg J;
reg K;
wire Q;
wire Qbar;

jk_flipflop uut (
    .clk(clk),
    .reset(reset),
    .J(J),
    .K(K),
    .Q(Q),
    .Qbar(Qbar)
);
always #5 clk = ~clk;

initial 
begin
    clk = 0;
    reset = 1;
    J = 0;
    K = 0;
    #10 reset = 0;
    #10 J = 0; K = 0;
    #10 J = 0; K = 1;
    #10 J = 1; K = 0;
    #10 J = 1; K = 1;
    #20 J = 1; K = 1;
    #20 $finish;
end

endmodule
```
#### SIMULATION OUTPUT
<img width="1909" height="1074" alt="image" src="https://github.com/user-attachments/assets/f29d64c4-bf82-4a33-be4d-1c240f35d6bd" />

---
### D Flip-Flop (Non Blocking)
```verilog

```
### D Flip-Flop Test bench 
```
module d_flipflop_tb;
reg clk;
reg reset;
reg D;
wire Q;
wire Qbar;
d_flipflop uut (
    .clk(clk),
    .reset(reset),
    .D(D),
    .Q(Q),
    .Qbar(Qbar)
);
always #5 clk = ~clk;

initial 
begin   
    clk = 0;
    reset = 1;
    D = 0;
    #10 reset = 0;
    #10 D = 1;
    #10 D = 0;
    #10 D = 1;
    #10 D = 0;

    #20 $finish;
end

endmodule
```

#### SIMULATION OUTPUT
<img width="1918" height="1074" alt="image" src="https://github.com/user-attachments/assets/d752f37c-868e-4298-b0f4-1b970e22959e" />

### T Flip-Flop (Non Blocking)
```
module t_flipflop_nb (
    input  wire clk,
    input  wire T,
    output reg  Q,
    output reg  Q_bar
);

always @(posedge clk)
begin
    if (T == 1'b0) 
    begin
        Q     <= Q;        
        Q_bar <= Q_bar;
    end
    else 
    begin
        Q     <= ~Q;      
        Q_bar <= ~Q_bar;
    end
end

endmodule
```
### T Flip-Flop Test bench 
```
module tb_t_flipflop_nb;
reg clk;
reg T;
wire Q;
wire Q_bar;
t_flipflop_nb uut (
    .clk(clk),
    .T(T),
    .Q(Q),
    .Q_bar(Q_bar)
);

always #5 clk = ~clk;

initial 
begin
    clk = 0;
    T   = 0;
    #10 T = 0; 
    #20 T = 1;
    #40 T = 0; 
    #20 T = 1; 
    #20 $stop;
end
endmodule
```

#### SIMULATION OUTPUT
![Image](https://github.com/user-attachments/assets/190d08d6-5c30-4828-9679-d5bf706309c7)
### RESULT

All flip-flops (SR, D, JK, T) were successfully simulated using Non blocking statements in Verilog HDL.
The outputs matched the expected truth table values, demonstrating correct sequential behavior.
