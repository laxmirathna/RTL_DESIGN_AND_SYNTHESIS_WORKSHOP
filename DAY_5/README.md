# Day 5 – IF-ELSE, CASE, and Looping Constructs

## Overview

Day 5 focused on different Verilog RTL coding styles used to describe decision-making and repeated hardware structures.

The main topics covered were if-else, case, casez, incomplete assignments, latch inference, synthesis optimization, procedural for loops, and generate loops.

The session also looked at practical examples such as MUXes, DEMUXes, and a Ripple Carry Adder. Simulation waveforms and synthesized netlists were used to understand how these RTL descriptions are interpreted and converted into hardware.

---

## Table of Contents

[RTL Coding Styles](#1-rtl-coding-styles)
[Inferred Latches](#2-inferred-latches)
[Labs 1–2 – Incomplete IF Statements](#3-labs-1-2--incomplete-if-statements)
[Labs 3–5 – CASE Statements](#4-labs-3-5--case-statements)
[Lab 6 – Overlapping CASE Statements](#5-lab-6--overlapping-case-statements)
[Synthesis Optimization](#6-synthesis-optimization)
[Looping Constructs in Verilog](#7-looping-constructs-in-verilog)
[Labs 7–10 – MUX, DEMUX, and RCA](#8-labs-7-10--mux-demux-and-rca)
[Overall Summary](#9-overall-summary)
[Conclusion](#10-conclusion)

---

# 1. RTL Coding Styles

RTL describes how a digital circuit should behave before synthesis converts the description into hardware.

The way Verilog code is written has a direct effect on how the synthesis tool understands the intended circuit.

Conditional statements are commonly used when a circuit needs to make decisions based on input signals.

## IF-ELSE

An if-else statement checks conditions one after another.

For example:

verilog
always @(*)
begin
    if (condition_1)
        y = value_1;
    else if (condition_2)
        y = value_2;
    else
        y = value_3;
end


When more than one condition is true, the first true condition gets priority.

| Statement | Priority |
| --------- | -------- |
| if      | Highest  |
| else if | Next     |
| else    | Lowest   |

Because of this priority behaviour, if-else is useful for circuits such as priority encoders and control logic.

---

## CASE

A case statement compares a selector with different possible values.

Example:

verilog
always @(*)
begin
    case (sel)
        2'b00: y = a;
        2'b01: y = b;
        2'b10: y = c;
        default: y = d;
    endcase
end


This style is useful when different selector values need to produce different outputs.

Common applications include:

Multiplexers
Decoders
State machines
Control logic

### IF-ELSE vs CASE

| Feature             | IF-ELSE                             | CASE                              |
| ------------------- | ----------------------------------- | --------------------------------- |
| Main use            | Priority-based decisions            | Multi-way selection               |
| Evaluation          | Conditions checked in order         | Selector compared with case items |
| Common applications | Priority logic, control             | MUX, decoder, FSM                 |
| Main concern        | Condition ordering and completeness | Coverage and overlapping patterns |

---

# 2. Inferred Latches

A latch is a level-sensitive storage element that can hold its previous value.

In combinational RTL, an unintended latch can be created when an output is not assigned for every possible condition.

For example:

verilog
always @(*)
begin
    if (enable)
        y = data;
end


When enable is 1, y receives data.

But when enable is 0, there is no assignment to y. The circuit therefore needs to remember the previous value, which can cause the synthesis tool to infer a latch.

A complete version is:

verilog
always @(*)
begin
    if (enable)
        y = data;
    else
        y = 1'b0;
end


Another common method is to assign a default value first:

verilog
always @(*)
begin
    y = 1'b0;

    if (enable)
        y = data;
end


## Latch vs Flip-Flop

Unintended latch inference in combinational logic should not be confused with intentional storage in sequential logic.

For example:

verilog
always @(posedge clk or posedge reset)
begin
    if (reset)
        count <= 0;
    else if (enable)
        count <= count + 1;
end


Here, count is stored using flip-flops because the block is triggered by a clock edge.

If enable is low, the flip-flop keeps its previous value. This is expected sequential behaviour, not unintended latch inference.

---

# 3. Labs 1–2 – Incomplete IF Statements

## Lab 1 – Incomplete IF Statement

The first example demonstrates what happens when a combinational if statement does not cover all possible conditions.

Example:

verilog
always @(*)
begin
    if (i0)
        y = i1;
end


The behaviour can be summarized as:

| i0 | Output        |
| ---- | ------------- |

| 1  | y = i1      |
| 0  | No assignment |

When i0 is 0, the output is not given a new value.

As a result, the synthesis tool may infer storage so that the previous value of y is retained.

### Waveform
<img width="1920" height="923" alt="incomp_if_simulation png" src="https://github.com/user-attachments/assets/c52eb501-c0eb-4612-a349-bdb7d647509b" />
The waveform can be used to observe how the output behaves when the condition is not satisfied.

### Synthesized Netlist
<img width="1153" height="539" alt="incomp_if_latch_synthesis png" src="https://github.com/user-attachments/assets/c54cc8f3-8071-4bbe-a52a-48dfd4f10fa3" />

The synthesized netlist shows the hardware structure generated from the incomplete RTL.

### Learning Outcome

A combinational output should have a defined value for every required input condition. Missing assignments can result in unintended latch inference.

---

## Lab 2 – Incomplete IF-ELSE Statement

The second example adds another condition:

verilog
always @(*)
begin
    if (i0)
        y = i1;
    else if (i2)
        y = i3;
end


The possible behaviour is:

| i0 | i2 | Output        |
| ---- | ---- | ------------- |
| 1  | X    | y = i1      |
| 0  | 1  | y = i3      |
| 0  | 0  | No assignment |

The final condition is still not covered.

Therefore, the output may retain its previous value, leading to latch inference.

### Complete Version

verilog
always @(*)
begin
    if (i0)
        y = i1;
    else if (i2)
        y = i3;
    else
        y = 1'b0;
end


The final else gives y a defined value for the remaining condition.

### Learning Outcome

Adding an else if does not automatically make combinational logic complete. Every possible execution path should provide an output value.

---

# 4. Labs 3–5 – CASE Statements

## Lab 3 – Incomplete CASE Statement

Consider the following case statement:

verilog
always @(*)
begin
    case (sel)
        2'b00: y = i0;
        2'b01: y = i1;
    endcase
end


A 2-bit selector has four possible combinations, but only two are covered.

| sel   | Output        |
| ------- | ------------- |
| 2'b00 | y = i0      |
| 2'b01 | y = i1      |
| 2'b10 | No assignment |
| 2'b11 | No assignment |

The uncovered cases can result in storage being inferred.

### Learning Outcome

When using case for combinational logic, all required selector values should be covered or a suitable default condition should be provided.

---

## Lab 4 – Complete CASE Statement

The previous example can be completed by adding a default branch:

verilog
always @(*)
begin
    case (sel)
        2'b00: y = i0;
        2'b01: y = i1;
        default: y = i2;
    endcase
end


The resulting behaviour is:

| sel   | Output   |
| ------- | -------- |
| 2'b00 | y = i0 |
| 2'b01 | y = i1 |
| 2'b10 | y = i2 |
| 2'b11 | y = i2 |

The default branch ensures that the output always receives a value.

### Learning Outcome

Using a default branch is a simple way to provide defined behaviour for selector values that are not explicitly listed.

---

## Lab 5 – Partial Output Assignment

It is also possible for one output to be assigned completely while another output remains incomplete.

Example:

verilog
always @(*)
begin
    case (sel)

        2'b00: begin
            y = i0;
            x = i2;
        end

        2'b01: begin
            y = i1;
        end

        default: begin
            y = i3;
            x = i4;
        end

    endcase
end


Here, y receives a value in every branch, but x is not assigned when sel = 2'b01.

| sel   | y  | x            |
| ------- | ---- | -------------- |
| 2'b00 | i0 | i2           |
| 2'b01 | i1 | Previous value |
| Default | i3 | i4           |

Because x does not receive a value in every branch, storage can be inferred for that output.

### Learning Outcome

Every output controlled by combinational logic should be assigned appropriately in every execution path.

---

# 5. Lab 6 – Overlapping CASE Statements

The sixth example focuses on wildcard matching with casez.

verilog
always @(*)
begin
    casez (sel)
        2'b00: y = i0;
        2'b01: y = i1;
        2'b10: y = i2;
        2'b1?: y = i3;
    endcase
end


The ? represents a don't-care position.

Therefore:

text
2'b1?


can match both:

text
2'b10
2'b11


This means that 2'b10 matches two case patterns.

| sel   | Matching patterns   |
| ------- | ------------------- |
| 2'b00 | 2'b00             |
| 2'b01 | 2'b01             |
| 2'b10 | 2'b10 and 2'b1? |
| 2'b11 | 2'b1?             |

This is an overlap issue rather than a latch issue.

### Learning Outcome

Wildcard patterns should be written carefully because overlapping conditions can make the intended selection behaviour unclear and may lead to unexpected results.

---

# 6. Synthesis Optimization

Synthesis tools do more than simply translate RTL into gates. They also analyze the logic and remove unnecessary or redundant portions while preserving the required functionality.

For example:

text
F = A + A'B


can be simplified to:

text
F = A + B


The simplified expression contains less redundant logic.

Optimization can help with:

Reducing gate count
Reducing area
Lowering logic complexity
Improving timing
Reducing switching activity
Potentially reducing power consumption

The general process is:

text
RTL Description
      ↓
Logic Analysis
      ↓
Boolean Optimization
      ↓
Technology Mapping
      ↓
Gate-Level Netlist


Because of these optimizations, the synthesized netlist may look quite different from the original RTL while still implementing the same logical function.

---

# 7. Looping Constructs in Verilog

Loops are useful when the same operation needs to be described multiple times.

Two important types of loops in RTL design are:

*Procedural for loop*
*Generate for loop*

Although their syntax looks similar, they are used for different purposes.

## Procedural for Loop

A procedural loop is written inside an always block.

Example:

verilog
integer i;

always @(*)
begin
    for (i = 0; i < 4; i = i + 1)
    begin
        y[i] = a[i];
    end
end


Procedural loops are useful for:

MUX logic
DEMUX logic
Bit-wise operations
Array processing
Repeated combinational operations

## Generate for Loop

A generate loop is used outside procedural blocks to create repeated hardware structures during elaboration.

Example:

verilog
genvar i;

generate
    for (i = 0; i < WIDTH; i = i + 1)
    begin
        // Repeated hardware instance
    end
endgenerate


Generate loops are especially useful for:

Ripple Carry Adders
Full Adder arrays
Register arrays
Repeated module instances
Parameterized designs

### Procedural FOR vs Generate FOR

| Feature     | Procedural for           | Generate for                 |
| ----------- | -------------------------- | ------------------------------ |
| Location    | Inside always block      | Outside procedural blocks      |
| Purpose     | Repeats RTL operations     | Replicates hardware structures |
| Typical use | MUX, DEMUX, bit operations | RCA, repeated modules          |
| Stage       | Procedural evaluation      | Elaboration                    |
| Description | Behavioral                 | Structural                     |

---

# 8. Labs 7–10 – MUX, DEMUX, and RCA

## Lab 7 – MUX Using Procedural for Loop

A multiplexer selects one input from several available inputs based on the select signal.

Using a loop avoids writing separate conditions for every input and makes the RTL easier to scale.

The basic idea is:

text
Multiple Inputs
      ↓
Select Signal
      ↓
     MUX
      ↓
Single Output


### Waveform
<img width="1920" height="905" alt="mux_generate_simulation png" src="https://github.com/user-attachments/assets/2c78cd49-c302-46ab-a187-2f5ed7187bda" />

The waveform verifies that the output follows the input selected by the select signal.

### Learning Outcome

A procedural loop can reduce repetitive RTL code while still allowing the synthesis tool to generate the required combinational hardware.

---

## Lab 8 – DEMUX Using CASE

A demultiplexer takes one input and routes it to one of several outputs depending on the select signal.

For a 4-output DEMUX:

text
sel = 2'b00 → Output 0
sel = 2'b01 → Output 1
sel = 2'b10 → Output 2
sel = 2'b11 → Output 3


A case statement can represent these conditions directly.

The selected output receives the input, while the remaining outputs remain inactive.

### Waveform
<img width="1920" height="898" alt="demux_case_simulation png" src="https://github.com/user-attachments/assets/d3dd4f9e-39db-4c32-9025-76d0567ff228" />

The waveform can be used to verify that the input is routed to the correct output for each select value.

### Learning Outcome

For a DEMUX with a small number of outputs, a case statement provides a simple and readable implementation.

---

## Lab 9 – DEMUX Using Procedural for Loop

The same DEMUX functionality can also be described using a procedural loop.

The general process is:

text
Initialize Outputs
      ↓
Read Select Signal
      ↓
Loop Through Outputs
      ↓
Find Selected Index
      ↓
Activate Selected Output


This avoids writing a separate case branch for every output.

### CASE vs Procedural LOOP

| Feature         | CASE              | Procedural for   |
| --------------- | ----------------- | ------------------ |
| Coding approach | Explicit branches | Repeated operation |
| Small designs   | Simple            | Simple             |
| Scalability     | More manual       | More convenient    |
| Repetition      | Higher            | Lower              |

### Learning Outcome

A procedural loop provides a compact and scalable way to describe repetitive DEMUX logic.

---

## Lab 10 – Ripple Carry Adder Using Generate for Loop

A Ripple Carry Adder (RCA) is created by connecting several Full Adder stages together.

Each Full Adder receives:

One bit from operand A
One bit from operand B
Carry from the previous stage

It produces:

One sum bit
Carry for the next stage

The carry moves from the least significant bit toward the most significant bit.

The structure can be represented as:

text
A0 ──┐
B0 ──┤
Cin ─┤
     ↓
  Full Adder
     │
     ├── Sum0
     │
     └── Carry1
           ↓
      Full Adder
           │
           ├── Sum1
           │
           └── Carry2
           ↓
          ...


A generate loop can be used to create one Full Adder for each bit.

Example:

verilog
genvar i;

generate
    for (i = 0; i < WIDTH; i = i + 1)
    begin

        full_adder FA (
            .a(a[i]),
            .b(b[i]),
            .cin(carry[i]),
            .sum(sum[i]),
            .cout(carry[i+1])
        );

    end
endgenerate


The advantage of this approach is scalability. The number of Full Adder stages can be changed by changing the width parameter.

### RTL Simulation Waveform
<img width="1920" height="923" alt="rca_simulation_detailed png" src="https://github.com/user-attachments/assets/4d65376b-817d-4992-bae9-33201cd820dc" />


The RTL waveform can be used to verify the addition operation and observe the resulting sum and carry signals.

### Synthesized Netlist
<img width="977" height="828" alt="rca_synthesis png" src="https://github.com/user-attachments/assets/51cd599c-1080-45af-b44f-015e82b09ca5" />
The synthesized netlist shows the hardware structure generated from the RTL description.

### Gate-Level Verification

The RTL implementation can be compared with its synthesized version during Gate-Level Simulation to check whether the intended functionality has been preserved.

### Learning Outcome

The RCA example demonstrates how generate loops can be used to create repeated structural hardware. The same idea can be applied to larger arithmetic circuits, register arrays, and other parameterized designs.

---

# 9. Overall Summary

The Day 5 concepts show how different Verilog coding styles influence the way synthesis tools interpret and implement digital hardware.

| Topic                  | Main Learning                                          |
| ---------------------- | ------------------------------------------------------ |
| IF-ELSE                | Useful for ordered and priority-based conditions       |
| CASE                   | Useful for multi-way selection                         |
| Incomplete IF          | Can result in unintended latch inference               |
| Incomplete CASE        | Can result in unintended latch inference               |
| Partial assignment     | Can cause storage for incompletely assigned outputs    |
| Overlapping CASE       | Can create multiple matching conditions                |
| Synthesis optimization | Removes redundant logic while preserving functionality |
| Procedural for       | Describes repeated RTL operations                      |
| Generate for         | Creates repeated structural hardware                   |
| MUX                    | Selects one input based on a control signal            |
| DEMUX                  | Routes one input to a selected output                  |
| RCA                    | Performs binary addition using cascaded Full Adders    |

---

# 10. Conclusion

Day 5 expanded the understanding of RTL coding by focusing on conditional statements and looping constructs used in Verilog.

The incomplete if-else and case examples showed why every possible condition needs to be handled when describing combinational logic. Missing assignments can cause the synthesis tool to infer unintended storage.

The casez example highlighted another important issue: wildcard conditions need to be written carefully because overlapping patterns can result in more than one possible match.

The synthesis optimization section showed how redundant Boolean logic can be simplified without changing the required functionality.

The final part introduced procedural and generate loops. Procedural loops are useful for describing repeated operations such as MUX and DEMUX logic, while generate loops are more suitable for creating repeated hardware structures such as the Full Adders in a Ripple Carry Adder.

Overall, Day 5 helped build a better understanding of how to write *complete, clear, scalable, and synthesis-friendly RTL*, and how simulation and synthesis results can be used to verify the resulting hardware.
