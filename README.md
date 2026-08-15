# **RTL DESIGN WORKSHOP**

> **A hands-on journey through RTL design, simulation, synthesis, timing libraries, and digital hardware implementation.**

This repository documents my practical learning journey through **RTL Design, Verilog Simulation, Waveform Analysis, Logic Synthesis, Technology Libraries, Timing Concepts, and Sequential Circuit Design**.

Each workshop day contains the concepts studied, practical experiments, commands used, results, screenshots, and key observations.

---

## **WORKSHOP PROGRESS**

|  **Day**  | **Topics Covered**                                            |   **Status**  |
| :-------: | ------------------------------------------------------------- | :-----------: |
| **Day 1** | Verilog RTL Design, Icarus Verilog, GTKWave & Yosys Synthesis | **Completed** |
| **Day 2** | Timing Libraries, Synthesis Methods & Flip-Flop RTL Coding    | **Completed** |

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
└── Day_2/
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

# **RTL DESIGN JOURNEY**

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
          TECHNOLOGY MAPPING
                   │
                   ▼
          GATE-LEVEL NETLIST
                   │
                   ▼
             DIGITAL HARDWARE
```

---


## **AUTHOR**

**Laxmi Rathna**

*Electronics & Communication Engineering*

