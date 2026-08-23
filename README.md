# **RTL DESIGN AND SYNTHESIS WORKSHOP**

> **A hands-on journey through RTL design, simulation, synthesis, timing libraries, optimization, and digital hardware implementation.**

This repository documents my learning journey through **RTL Design, Verilog Simulation, Waveform Analysis, Logic Synthesis, Technology Libraries, Timing Concepts, RTL Optimization, Gate-Level Simulation, and Digital Hardware Design**.

Each workshop day contains the concepts studied, practical work, commands used, simulation results, synthesis outputs, screenshots, and key observations.

---

## **WORKSHOP PROGRESS**

|  **Day**  | **Topics Covered**                                                                  |   **Status**  |
| :-------: | :---------------------------------------------------------------------------------- | :-----------: |
| **Day 1** | Verilog RTL Design, Icarus Verilog, GTKWave & Yosys Synthesis                       | **Completed** |
| **Day 2** | Timing Libraries, Synthesis Methods & Flip-Flop RTL Coding                          | **Completed** |
| **Day 3** | RTL Optimization, Logic Simplification, Constant Propagation & Counter Optimization | **Completed** |
| **Day 4** | RTL Design, MUX, Gate-Level Simulation & Simulation-Synthesis Mismatch              | **Completed** |
| **Day 5** | IF-ELSE, CASE, Latches, Looping Constructs, MUX, DEMUX & RCA                        | **Completed** |

---

## **REPOSITORY STRUCTURE**

```text
RTL_Design_Workshop/
│
├── README.md
│
├── Day_1/
│   └── README.md
│
├── Day_2/
│   └── README.md
│
├── Day_3/
│   └── README.md
│
├── Day_4/
│   └── README.md
│
└── Day_5/
    └── README.md
```

---

# **DAY 1 — RTL DESIGN, SIMULATION & SYNTHESIS**

Day 1 introduced the fundamentals of the **RTL-to-Gate-Level design flow**.

The work started with writing Verilog RTL and creating a testbench, followed by simulation, waveform analysis, and synthesis using open-source digital design tools.

### **TOPICS COVERED**

* Simulator, Design and Testbench
* Verilog RTL Design
* Icarus Verilog Simulation
* 2:1 Multiplexer Implementation
* GTKWave Waveform Analysis
* RTL Design Flow
* Introduction to Logic Synthesis
* Introduction to Yosys
* Understanding `.lib` Technology Libraries
* Standard Cell Concepts
* Faster and Slower Cell Flavors
* Cell Selection Based on Design Requirements
* Yosys Synthesis Flow
* Synthesis Statistics
* Gate-Level Representation
* Generated Gate-Level Netlist

### **DAY 1 DESIGN FLOW**

```text
        ┌───────────────────┐
        │    Verilog RTL    │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │    Testbench      │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │  Icarus Verilog   │
        │    Simulation     │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │      GTKWave      │
        │ Waveform Analysis │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │       Yosys       │
        │     Synthesis     │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │  Gate-Level       │
        │     Netlist       │
        └───────────────────┘
```

### **DAY 1 DOCUMENTATION**

The complete Day 1 documentation contains:

* RTL code
* Simulation commands
* GTKWave analysis
* Yosys commands
* Synthesis results
* Screenshots
* Gate-level netlist
* Observations

---

## **TOOLS USED — DAY 1**

| **Tool**           | **Purpose**                       |
| ------------------ | --------------------------------- |
| **Verilog**        | RTL hardware description          |
| **Icarus Verilog** | RTL simulation                    |
| **GTKWave**        | Waveform visualization            |
| **Yosys**          | Logic synthesis                   |
| **Linux / Ubuntu** | Design environment                |
| **Git & GitHub**   | Version control and documentation |

---

# **DAY 2 — TIMING LIBRARIES, SYNTHESIS & FLIP-FLOP RTL**

Day 2 moved deeper into the digital implementation flow by exploring **technology libraries, timing information, synthesis strategies, and sequential RTL design**.

Special attention was given to different flip-flop coding styles and how RTL descriptions are transformed into technology-specific standard cells.

### **TOPICS COVERED**

* SKY130 Technology Library
* Understanding `.lib` Timing Libraries
* Standard Cell Timing Information
* Process, Voltage and Temperature Conditions
* Hierarchical Synthesis
* Flattened Synthesis
* Hierarchical vs. Flattened Design
* Asynchronous Reset D Flip-Flop
* Asynchronous Set D Flip-Flop
* Synchronous Reset D Flip-Flop
* Icarus Verilog Simulation
* GTKWave Waveform Analysis
* Yosys Synthesis
* `dfflibmap` for Flip-Flop Mapping
* Technology Mapping using `abc`
* Gate-Level Representation

### **DAY 2 DESIGN FLOW**

```text
        ┌───────────────────┐
        │     RTL Design    │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │  Timing Library   │
        │      (.lib)       │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │     Synthesis     │
        │                   │
        │  Hierarchical /   │
        │     Flattened     │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │       Yosys       │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │   dfflibmap +     │
        │       ABC         │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Technology-Mapped │
        │     Netlist       │
        └───────────────────┘
```

### **DAY 2 DOCUMENTATION**

The complete Day 2 documentation contains:

* Timing library analysis
* PVT information
* Synthesis approaches
* Flip-flop RTL implementations
* Simulation waveforms
* Yosys synthesis
* Flip-flop mapping
* Technology mapping
* Gate-level results
* Screenshots and observations

---

## **TOOLS & TECHNOLOGIES — DAY 2**

| **Tool / Technology** | **Purpose**                      |
| --------------------- | -------------------------------- |
| **Verilog**           | RTL design                       |
| **Icarus Verilog**    | Functional simulation            |
| **GTKWave**           | Waveform analysis                |
| **Yosys**             | RTL synthesis                    |
| **SKY130**            | Standard-cell technology library |
| **Linux / Ubuntu**    | Design environment               |
| **Git & GitHub**      | Version control                  |

---

# **DAY 3 — RTL OPTIMIZATION & SYNTHESIS**

Day 3 focused on understanding how synthesis tools analyze RTL and simplify the resulting hardware while maintaining the required functionality.

The main concepts included **Boolean logic optimization, constant propagation, D flip-flop optimization, sequential logic optimization, and counter optimization**.

### **TOPICS COVERED**

* RTL Optimization
* Boolean Simplification
* AND Logic
* OR Logic
* Three-Input AND Logic
* Constant Propagation
* D Flip-Flop Optimization
* Constant-Driven D Flip-Flops
* Sequential Logic Optimization
* Counter Optimization
* Synthesized Gate-Level Structures
* RTL vs. Synthesized Hardware
* Simulation Waveform Verification
* Area, Power and Timing Considerations

### **DAY 3 DESIGN FLOW**

```text
        ┌───────────────────┐
        │     RTL Design    │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │  Logic Analysis   │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │    Optimization   │
        │                   │
        │ Boolean / Constant│
        │   Propagation     │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │       Yosys       │
        │     Synthesis     │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Gate-Level        │
        │ Representation    │
        └───────────────────┘
```

### **KEY CONCEPTS**

#### **Logic Optimization**

Boolean expressions can often be simplified using identities such as:

```text
A AND 1 = A
A AND 0 = 0
A OR 0  = A
A OR 1  = 1
```

The synthesis tool uses such relationships to remove unnecessary logic.

#### **Constant Propagation**

Known constant values can be propagated through a circuit.

For example:

```text
A AND 0 = 0
A AND 1 = A
A OR 0  = A
A OR 1  = 1
```

This can reduce unnecessary hardware in both combinational and sequential circuits.

#### **D Flip-Flop Optimization**

Constant values connected to the input of a D flip-flop can provide information that allows the synthesis tool to simplify the sequential structure.

#### **Counter Optimization**

Counters contain flip-flops and next-state logic, making them useful examples for understanding sequential optimization.

A 3-bit counter follows:

```text
000 → 001 → 010 → 011
     → 100 → 101 → 110 → 111
     → 000
```

### **DAY 3 DOCUMENTATION**

The complete Day 3 documentation contains:

* RTL optimization concepts
* Boolean logic examples
* Constant propagation
* DFF optimization
* Counter optimization
* Synthesized circuit analysis
* Simulation waveforms
* Key observations
* Optimization impact on area, power and timing

---

## **TOOLS USED — DAY 3**

| **Tool / Technology** | **Purpose**                      |
| --------------------- | -------------------------------- |
| **Verilog**           | RTL design                       |
| **Yosys**             | RTL synthesis and optimization   |
| **Icarus Verilog**    | Simulation                       |
| **GTKWave**           | Waveform analysis                |
| **SKY130**            | Standard-cell technology library |

---

# **DAY 4 — RTL DESIGN, SYNTHESIS & GATE-LEVEL SIMULATION**

Day 4 focused on the transition from **RTL code to synthesized hardware and Gate-Level Simulation**.

The concepts covered included MUX design using the ternary operator, RTL simulation, synthesis, standard-cell mapping, gate-level netlist generation, sensitivity lists, blocking assignments, and simulation-synthesis mismatch.

### **TOPICS COVERED**

* Ternary Operator Based MUX
* RTL Simulation
* Yosys Synthesis
* SKY130 Standard-Cell Mapping
* Gate-Level Netlist
* Gate-Level Simulation
* GTKWave Waveform Analysis
* Incomplete Sensitivity Lists
* `always @(*)`
* Blocking Assignments
* Non-Blocking Assignments
* RTL vs. Gate-Level Waveforms
* Simulation-Synthesis Mismatch

### **DAY 4 DESIGN FLOW**

```text
        ┌───────────────────┐
        │    Verilog RTL    │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │  RTL Simulation   │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │       Yosys       │
        │     Synthesis     │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Technology        │
        │ Mapping           │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Gate-Level        │
        │ Netlist           │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Gate-Level        │
        │ Simulation        │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │     GTKWave       │
        │ Waveform Analysis │
        └───────────────────┘
```

### **MUX USING TERNARY OPERATOR**

A 2:1 MUX can be described using:

```verilog
assign y = sel ? i1 : i0;
```

The select signal determines which input appears at the output.

The design can then be synthesized and mapped to a SKY130 standard cell such as:

```text
sky130_fd_sc_hd__mux2_1
```

### **INCOMPLETE SENSITIVITY LIST**

An incomplete sensitivity list can cause incorrect RTL simulation behaviour.

For example:

```verilog
always @(sel)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

Here, `i0` and `i1` also affect the output but are missing from the sensitivity list.

A safer combinational coding style is:

```verilog
always @(*)
begin
    if (sel)
        y = i1;
    else
        y = i0;
end
```

### **BLOCKING AND NON-BLOCKING ASSIGNMENTS**

Blocking assignment:

```verilog
=
```

Non-blocking assignment:

```verilog
<=
```

Blocking assignments are commonly used for combinational procedural logic, while non-blocking assignments are commonly used for sequential logic.

### **DAY 4 DOCUMENTATION**

The complete Day 4 documentation contains:

* Ternary MUX design
* RTL simulation
* Yosys synthesis
* Standard-cell mapping
* Gate-level netlist
* Gate-Level Simulation
* GTKWave analysis
* Sensitivity-list concepts
* Blocking vs. non-blocking assignments
* RTL vs. GLS comparison
* Simulation-synthesis mismatch
* Key observations

---

## **TOOLS USED — DAY 4**

| **Tool / Technology** | **Purpose**                      |
| --------------------- | -------------------------------- |
| **Verilog**           | RTL design                       |
| **Icarus Verilog**    | RTL and Gate-Level Simulation    |
| **Yosys**             | Synthesis                        |
| **GTKWave**           | Waveform analysis                |
| **SKY130**            | Standard-cell technology library |

---

# **DAY 5 — IF-ELSE, CASE & LOOPING CONSTRUCTS**

Day 5 focused on Verilog coding constructs used to describe **conditional logic and repetitive hardware structures**.

The topics included `if-else`, `case`, `casez`, incomplete assignments, latch inference, synthesis optimization, procedural `for` loops, and generate loops.

Practical designs such as **MUX, DEMUX, and Ripple Carry Adder (RCA)** were also studied to understand how these coding constructs translate into hardware.

### **TOPICS COVERED**

* IF-ELSE Statements
* Priority Logic
* CASE Statements
* IF-ELSE vs. CASE
* Inferred Latches
* Incomplete IF Statements
* Incomplete CASE Statements
* Complete CASE Statements
* Partial Output Assignment
* Overlapping `casez` Conditions
* Boolean Logic Optimization
* Procedural `for` Loops
* Generate `for` Loops
* MUX using Procedural Loop
* DEMUX using CASE
* DEMUX using Procedural Loop
* Ripple Carry Adder using Generate Loop
* RTL and Gate-Level Verification

### **DAY 5 DESIGN FLOW**

```text
        ┌───────────────────┐
        │     RTL Coding    │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ IF-ELSE / CASE    │
        │      / LOOPS      │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ RTL Simulation    │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │     Synthesis     │
        │       Yosys       │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Gate-Level        │
        │     Netlist       │
        └─────────┬─────────┘
                  │
                  ▼
        ┌───────────────────┐
        │ Waveform Analysis │
        └───────────────────┘
```

### **IF-ELSE**

`if-else` statements are useful when conditions need to be evaluated according to priority.

```verilog
always @(*)
begin
    if (condition_1)
        y = value_1;
    else if (condition_2)
        y = value_2;
    else
        y = value_3;
end
```

### **CASE**

`case` statements are useful for multi-way selection.

```verilog
always @(*)
begin
    case (sel)
        2'b00: y = a;
        2'b01: y = b;
        2'b10: y = c;
        default: y = d;
    endcase
end
```

### **LATCH INFERENCE**

An incomplete combinational assignment can result in unintended latch inference.

For example:

```verilog
always @(*)
begin
    if (enable)
        y = data;
end
```

When `enable` is LOW, `y` is not assigned.

A complete version is:

```verilog
always @(*)
begin
    if (enable)
        y = data;
    else
        y = 1'b0;
end
```

### **OVERLAPPING CASEZ CONDITIONS**

Wildcard conditions should be written carefully.

For example:

```text
2'b1?
```

can match both:

```text
2'b10
2'b11
```

Overlapping patterns can make the intended selection behaviour ambiguous.

### **LOOPING CONSTRUCTS**

Two important loop styles were studied:

**Procedural `for` loop**

Used inside an `always` block for repeated RTL operations.

**Generate `for` loop**

Used to create repeated structural hardware during elaboration.

| **Feature** | **Procedural `for`**       | **Generate `for`**        |
| ----------- | -------------------------- | ------------------------- |
| Location    | Inside `always` block      | Outside procedural blocks |
| Purpose     | Repeats RTL operations     | Creates repeated hardware |
| Typical use | MUX, DEMUX, bit operations | RCA, repeated modules     |
| Description | Behavioral                 | Structural                |

### **MUX AND DEMUX**

Loop-based implementations were used to understand how repetitive selection logic can be described more compactly.

A DEMUX routes one input to one of several outputs based on the select signal.

### **RIPPLE CARRY ADDER**

A Ripple Carry Adder is built by connecting multiple Full Adders.

```text
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
```

A generate loop allows the same Full Adder structure to be repeated for each bit.

```verilog
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
```

This makes the design easier to scale by changing the width parameter.

### **DAY 5 DOCUMENTATION**

The complete Day 5 documentation contains:

* IF-ELSE concepts
* CASE statements
* Latch inference
* Incomplete and complete conditional logic
* Overlapping CASE conditions
* Synthesis optimization
* Procedural loops
* Generate loops
* MUX implementation
* DEMUX implementation
* Ripple Carry Adder
* Simulation waveforms
* Synthesized netlists
* Key observations

---

## **TOOLS USED — DAY 5**

| **Tool / Technology** | **Purpose**                      |
| --------------------- | -------------------------------- |
| **Verilog**           | RTL design                       |
| **Icarus Verilog**    | Simulation                       |
| **GTKWave**           | Waveform analysis                |
| **Yosys**             | RTL synthesis and optimization   |
| **SKY130**            | Standard-cell technology library |

---

# **RTL DESIGN JOURNEY**

The workshop gradually builds the understanding of how a Verilog description becomes digital hardware.

```text
              RTL DESIGN
                   │
                   ▼
              SIMULATION
                   │
                   ▼
          WAVEFORM ANALYSIS
                   │
                   ▼
               SYNTHESIS
                   │
                   ▼
          LOGIC OPTIMIZATION
                   │
                   ▼
          TECHNOLOGY MAPPING
                   │
                   ▼
          GATE-LEVEL NETLIST
                   │
                   ▼
         GATE-LEVEL SIMULATION
                   │
                   ▼
             DIGITAL HARDWARE
```

---

# **KEY LEARNING AREAS**

Through the five days, the main areas covered were:

* Verilog RTL Design
* Testbench Development
* RTL Simulation
* Waveform Analysis
* Logic Synthesis
* Yosys
* Icarus Verilog
* GTKWave
* SKY130 Technology Library
* Standard Cells
* Timing Libraries
* PVT Concepts
* Hierarchical and Flattened Synthesis
* Flip-Flop RTL Coding
* RTL Optimization
* Constant Propagation
* Sequential Logic Optimization
* Gate-Level Simulation
* Simulation-Synthesis Mismatch
* IF-ELSE and CASE Coding
* Latch Inference
* Procedural Loops
* Generate Loops
* MUX and DEMUX Design
* Ripple Carry Adder

---

## **AUTHOR**

**Laxmi Rathna**

*Electronics & Communication Engineering*
