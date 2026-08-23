# Day 4 – RTL Design, Synthesis and Gate-Level Simulation

## Overview

Day 4 focused on understanding what happens to an RTL design after it is written in Verilog and how it is converted into actual hardware through synthesis.

The main concepts covered were RTL simulation, synthesis using Yosys, standard-cell mapping, gate-level netlist generation, Gate-Level Simulation (GLS), waveform analysis, sensitivity lists, and blocking assignments.

A major focus of this session was understanding the difference between **RTL simulation and Gate-Level Simulation**, and how incorrect RTL coding can sometimes cause differences between the two.

### Topics Covered

* Ternary operator based MUX
* RTL simulation
* Logic synthesis using Yosys
* Standard-cell mapping
* Gate-level netlist generation
* Gate-Level Simulation
* Incomplete sensitivity lists
* Blocking assignments
* RTL vs Gate-Level waveforms
* Simulation-synthesis mismatch

---

## Table of Contents

1. [RTL to Gate-Level Simulation Flow](#1-rtl-to-gate-level-simulation-flow)
2. [Ternary Operator MUX](#2-ternary-operator-mux)
3. [Bad MUX – Incomplete Sensitivity List](#3-bad-mux--incomplete-sensitivity-list)
4. [Blocking Assignment](#4-blocking-assignment)
5. [Blocking vs Non-Blocking Assignments](#5-blocking-vs-non-blocking-assignments)
6. [Importance of `always @(*)`](#6-importance-of-always-)
7. [Simulation-Synthesis Mismatch](#7-simulation-synthesis-mismatch)
8. [RTL Simulation vs Gate-Level Simulation](#8-rtl-simulation-vs-gate-level-simulation)
9. [Tools Used](#9-tools-used)
10. [Key Observations](#10-key-observations)
11. [Learning Outcomes](#11-learning-outcomes)
12. [Conclusion](#12-conclusion)

---

# 1. RTL to Gate-Level Simulation Flow

The overall flow studied in Day 4 can be represented as:

```text
RTL Verilog Code
       |
       v
RTL Simulation
       |
       v
Yosys Synthesis
       |
       v
Technology Mapping
       |
       v
Gate-Level Netlist
       |
       v
Gate-Level Simulation
       |
       v
GTKWave
       |
       v
Waveform Analysis
```

RTL simulation is used to check whether the Verilog code behaves as expected.

After that, synthesis converts the RTL description into a hardware implementation. During technology mapping, the logic is represented using standard cells from the target library.

The generated gate-level netlist can then be simulated again. This helps verify whether the synthesized hardware still behaves as intended.

---

# 2. Ternary Operator MUX

## 2.1 Working Principle

A multiplexer, or MUX, is a combinational circuit that selects one of several inputs and sends the selected input to the output.

For a 2:1 MUX, there are two data inputs, one select signal, and one output.

```text
        i0 --------\
                    \
                     >---- y
                    /
        i1 --------/
                    |
                   sel
```

The operation is:

```text
sel = 0  →  y = i0
sel = 1  →  y = i1
```

The same functionality can be written in Verilog using the ternary operator:

```verilog
assign y = sel ? i1 : i0;
```

The ternary operator provides a simple way to describe conditional selection in combinational logic.

---

## 2.2 RTL Simulation

The MUX was first considered at the RTL simulation level.

The main signals observed were:

```text
i0
i1
sel
y
```

When `sel` is LOW, the output follows `i0`.

When `sel` is HIGH, the output follows `i1`.

### RTL Waveform

The RTL waveform can be used to confirm that the MUX is selecting the correct input for each value of the select signal.
<img width="1920" height="896" alt="sim_ternary_operator_mux" src="https://github.com/user-attachments/assets/98849869-f0cb-49fe-bdb4-1d75747a97e3" />

---

## 2.3 Synthesis

After RTL verification, the MUX design can be synthesized using Yosys.

During synthesis, the RTL description is converted into a hardware structure using cells from the SKY130 standard-cell library.

The MUX is mapped to:

```text
sky130_fd_sc_hd__mux2_1
```

### Synthesized Netlist

The synthesized netlist shows how the simple Verilog statement:

```verilog
assign y = sel ? i1 : i0;
```

is represented using a standard-cell implementation.

This provides a clear example of how an RTL description is transformed into hardware.

---

## 2.4 Gate-Level Simulation

The synthesized netlist can then be used for Gate-Level Simulation.

The testbench applies different input combinations, and the resulting signals can be observed using GTKWave.

### Gate-Level Waveform

The RTL and Gate-Level waveforms can be compared to check whether the synthesized implementation maintains the expected MUX functionality.

The complete flow can be summarized as:

```text
RTL MUX
   |
   v
Ternary Operator
   |
   v
Yosys Synthesis
   |
   v
sky130_fd_sc_hd__mux2_1
   |
   v
Gate-Level Simulation
```

---

# 3. Bad MUX – Incomplete Sensitivity List

## 3.1 Problem

A MUX can also be described using an `always` block.

An incorrect version can be written as:

```verilog
always @(sel)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

The problem here is that only `sel` is included in the sensitivity list.

However, the output depends on:

```text
sel
i0
i1
```

If `sel` changes, the block executes and the output is updated.

But if `i0` or `i1` changes while `sel` remains unchanged, the block may not execute.

This can result in incorrect RTL simulation behaviour.

---

## 3.2 RTL Simulation
<img width="955" height="625" alt="bad_mux_tb_sim" src="https://github.com/user-attachments/assets/63e686dc-52d4-4c2c-8584-7532115ecb70" />

The Bad MUX example helps demonstrate the effect of an incomplete sensitivity list.

The waveform may show that changes in `i0` or `i1` do not immediately appear at the output when `sel` remains unchanged.

This happens because the simulator only triggers the `always` block when a signal present in its sensitivity list changes.

---

## 3.3 Synthesis and Gate-Level Simulation

The sensitivity list mainly affects simulation. It does not represent a physical hardware component.

During synthesis, the tool analyzes the logic inside the procedural block and determines the corresponding hardware structure.

As a result, an incomplete sensitivity list can create a difference between the behaviour seen during RTL simulation and the actual synthesized hardware behaviour.

Comparing the RTL waveform with the Gate-Level waveform helps make this difference easier to understand.

This is an important example of how incorrect RTL coding can lead to a **simulation-synthesis mismatch**.

---

## 3.4 Correct Coding Style

For combinational logic, the safer coding style is:

```verilog
always @(*)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

The `@(*)` construct automatically includes the signals referenced inside the block in the sensitivity list.

Therefore, instead of:

```verilog
always @(sel)
```

the preferred form is:

```verilog
always @(*)
```

This reduces the possibility of accidentally leaving out input signals that affect the output.

---

# 4. Blocking Assignment

## 4.1 Blocking Assignment

Verilog provides two commonly used procedural assignment operators:

```text
Blocking assignment       =
Non-blocking assignment   <=
```

A blocking assignment updates the value immediately within the procedural flow.

For example:

```verilog
always @(*)
begin
    x = a | b;
    d = x & c;
end
```

Here, the first statement executes before the second statement.

Therefore, the updated value of `x` is available when the next statement is evaluated.

This means that the order of statements matters when blocking assignments are used.

---

## 4.2 RTL Simulation

The blocking-assignment example can be examined using RTL simulation.

The logic can be represented as:

```text
a ----\
       OR ---- x ----\
b ----/               \
                       AND ----> d
c -------------------/
```

### RTL Waveform
<img width="955" height="890" alt="blocking_caveat_sim v" src="https://github.com/user-attachments/assets/5924b988-bae6-4300-8393-67604484a712" />

The waveform shows the behaviour of the intermediate signal `x` and the final output `d`.

Since blocking assignments execute in sequence, the updated value of `x` can be used immediately by the following statement.

---

## 4.3 Synthesis

The design can then be synthesized using Yosys.

The RTL is mapped to cells available in the SKY130 standard-cell library.

One of the relevant cells is:

```text
sky130_fd_sc_hd__o21a_1
```

### Synthesized Netlist
<img width="955" height="343" alt="blocking_caveat_synth" src="https://github.com/user-attachments/assets/57ac1b89-d59b-4a1a-ab05-8403e88bf8b7" />

The synthesized netlist represents the actual combinational hardware generated from the RTL.

The procedural statements in the Verilog description are converted into connections between hardware cells.

---

## 4.4 Gate-Level Simulation

The synthesized netlist can also be simulated at the gate level.

### Gate-Level Waveform

The Gate-Level Simulation waveform represents the behaviour of the synthesized circuit.

Comparing the RTL and GLS waveforms helps show the difference between the way procedural RTL executes and how the final hardware operates.

The basic idea is:

```text
Blocking Assignment
        |
        v
Sequential RTL Execution
        |
        v
Intermediate Signal Updated
        |
        v
Next Statement Uses Updated Value
```

This shows why the ordering of blocking assignments should be considered carefully when writing combinational RTL.

---

# 5. Blocking vs Non-Blocking Assignments

## Blocking Assignment

The blocking assignment operator is:

```text
=
```

Example:

```verilog
always @(*)
begin
    x = a | b;
    d = x & c;
end
```

The statements execute sequentially, and the updated value of `x` is immediately available to the next statement.

Blocking assignments are commonly used for combinational procedural logic.

---

## Non-Blocking Assignment

The non-blocking assignment operator is:

```text
<=
```

Example:

```verilog
always @(posedge clk)
begin
    q <= d;
end
```

Non-blocking assignments schedule the update rather than immediately changing the value during the current procedural evaluation.

They are commonly used when describing sequential logic such as flip-flops and registers.

### Comparison

| Blocking `=`                                   | Non-Blocking `<=`                                  |
| ---------------------------------------------- | -------------------------------------------------- |
| Executes immediately                           | Update is scheduled                                |
| Statements execute sequentially                | Updates occur after evaluation                     |
| Commonly used for combinational logic          | Commonly used for sequential logic                 |
| Statement order can affect intermediate values | Useful for modelling simultaneous register updates |

---

# 6. Importance of `always @(*)`

When combinational logic is written using an `always` block, all signals that can affect the output need to be considered.

An incomplete sensitivity list such as:

```verilog
always @(sel)
```

can cause incorrect RTL simulation behaviour.

A safer approach is:

```verilog
always @(*)
```

For example:

```verilog
always @(*)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

This allows changes in `sel`, `i0`, and `i1` to trigger the block.

Using `always @(*)` therefore reduces the possibility of simulation-synthesis mismatches caused by missing signals in the sensitivity list.

---

# 7. Simulation-Synthesis Mismatch

A simulation-synthesis mismatch occurs when the behaviour observed during RTL simulation differs from the behaviour of the synthesized hardware.

The Bad MUX example provides a clear illustration of this issue.

## Incomplete Sensitivity List

```verilog
always @(sel)
```

The RTL simulator responds only when `sel` changes, even though `i0` and `i1` also affect the output.

The synthesized hardware, however, is based on the actual logic relationship between the inputs and output.

## Blocking Assignment

```verilog
always @(*)
begin
    x = a | b;
    d = x & c;
end
```

Blocking assignments execute sequentially during RTL simulation.

Therefore, the order in which statements are written can affect the intermediate values observed during simulation.

The overall idea can be represented as:

```text
RTL Coding Issue
       |
       v
RTL Simulation Behaviour
       |
       v
Synthesis
       |
       v
Hardware Implementation
       |
       v
RTL vs GLS Comparison
```

This highlights the importance of writing RTL that is both synthesizable and simulation-correct.

---

# 8. RTL Simulation vs Gate-Level Simulation

| Feature         | RTL Simulation          | Gate-Level Simulation        |
| --------------- | ----------------------- | ---------------------------- |
| Input           | RTL Verilog             | Synthesized netlist          |
| Stage           | Before synthesis        | After synthesis              |
| Main purpose    | Functional verification | Post-synthesis verification  |
| Representation  | RTL description         | Standard-cell implementation |
| Timing          | Mainly functional       | Can include cell/gate delays |
| Simulator       | Icarus Verilog          | Icarus Verilog               |
| Waveform viewer | GTKWave                 | GTKWave                      |

RTL simulation checks whether the original Verilog description behaves as expected.

Gate-Level Simulation checks the behaviour of the synthesized implementation.

Comparing both waveforms helps verify that the synthesized hardware still represents the intended RTL functionality.

---

# 9. Tools Used

| Tool               | Purpose                                              |
| ------------------ | ---------------------------------------------------- |
| **Yosys**          | RTL synthesis and netlist generation                 |
| **Icarus Verilog** | Verilog compilation and simulation                   |
| **GTKWave**        | Waveform viewing and analysis                        |
| **SKY130 PDK**     | Standard-cell library used during technology mapping |

---

# 10. Key Observations

### Ternary Operator MUX

The MUX can be described using:

```verilog
assign y = sel ? i1 : i0;
```

The RTL description is synthesized and mapped to:

```text
sky130_fd_sc_hd__mux2_1
```

The resulting netlist can then be used for Gate-Level Simulation.

### Bad MUX

The following coding style can cause problems:

```verilog
always @(sel)
```

The sensitivity list does not include `i0` and `i1`, even though both signals affect the output.

For combinational logic, the preferred form is:

```verilog
always @(*)
```

### Blocking Assignment

The following example demonstrates sequential execution of blocking assignments:

```verilog
always @(*)
begin
    x = a | b;
    d = x & c;
end
```

The updated value of `x` is available to the next statement immediately.

### Gate-Level Simulation

The overall verification flow is:

```text
RTL
 |
 v
Synthesis
 |
 v
Gate-Level Netlist
 |
 v
Gate-Level Simulation
 |
 v
Waveform Analysis
```

Gate-Level Simulation provides an additional way to check the synthesized design after synthesis.

---

# 11. Learning Outcomes

By the end of Day 4, the following concepts were understood:

* RTL-to-Gate-Level Simulation flow
* Ternary operator based MUX design
* RTL simulation
* Synthesis using Yosys
* Standard-cell mapping
* Gate-level netlist generation
* Gate-Level Simulation
* Waveform analysis using GTKWave
* Sensitivity lists in Verilog
* Importance of `always @(*)`
* Blocking assignments
* Non-blocking assignments
* Simulation-synthesis mismatch
* Proper coding practices for combinational RTL
* Comparison between RTL and synthesized hardware behaviour

The concepts also help connect Verilog coding practices with the actual hardware structure produced during synthesis.

---

# 12. Conclusion

Day 4 helped build a clearer understanding of the flow from RTL design to synthesized hardware and Gate-Level Simulation.

The MUX example showed how a simple Verilog description can be mapped to a standard cell. The Bad MUX example highlighted the importance of using a complete sensitivity list when describing combinational logic. The blocking-assignment example showed how statement ordering can affect RTL simulation.

Looking at RTL waveforms, synthesized netlists, and Gate-Level Simulation results makes the connection between Verilog code and actual hardware much easier to understand.

The session also provided practical exposure to **Yosys, Icarus Verilog, GTKWave, and the SKY130 standard-cell library**.

Overall, Day 4 strengthened the understanding of RTL coding, synthesis, standard-cell mapping, simulation, and post-synthesis verification, while also showing why careful RTL coding is important for reliable digital hardware design.

