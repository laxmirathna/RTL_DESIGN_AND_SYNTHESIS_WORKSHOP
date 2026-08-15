# **Day 02 — Timing Libraries, Synthesis & Flip-Flop RTL**

## **Experiment Objective**

The objective of Day 02 was to understand how **technology libraries and timing information** support the RTL synthesis process.

The experiment also explored the difference between **hierarchical and flattened synthesis** and examined how different flip-flop behaviors can be described using Verilog RTL, including asynchronous reset, asynchronous set, and synchronous reset.

The designs were simulated using **Icarus Verilog**, analyzed using **GTKWave**, and synthesized using **Yosys** with the **SKY130 standard-cell library**.

---

## **Contents**

* [Technology Libraries](#1-technology-libraries)
* [Hierarchical and Flattened Synthesis](#2-hierarchical-and-flattened-synthesis)
* [Flip-Flop RTL Coding](#3-flip-flop-rtl-coding)
* [Simulation and Synthesis](#4-simulation-and-synthesis)
* [Conclusion](#5-conclusion)

---

# **1. Technology Libraries**

RTL code describes the required hardware behavior, but synthesis eventually needs to translate that description into actual cells available in a target technology.

A **technology library** provides the information required by synthesis tools to understand these standard cells. This information can include:

* Cell functionality
* Timing characteristics
* Power information
* Area information
* Operating conditions
* Input and output characteristics

For this experiment, the **SKY130 high-density standard-cell library** was used.

### **Library Used**

```text
sky130_fd_sc_hd__tt_025C_1v80.lib
```

The filename provides information about the operating conditions represented by the library.

| **Library Field** | **Meaning**                   |
| ----------------- | ----------------------------- |
| `sky130`          | SKY130 technology             |
| `fd_sc_hd`        | Standard-cell library variant |
| `tt`              | Typical process condition     |
| `025C`            | Temperature of 25°C           |
| `1v80`            | Supply voltage of 1.8 V       |

The Liberty file can be opened from the terminal using:

```bash
gedit sky130_fd_sc_hd__tt_025C_1v80.lib
```

Examining the file makes it possible to see the available standard cells and the timing information associated with them.

### **SKY130 Liberty File**
**<img width="1646" height="819" alt="day2_fistimage" src="https://github.com/user-attachments/assets/4abb919d-ae2b-45fa-bad0-62ad71e9c2ee" />


---

# **2. Hierarchical and Flattened Synthesis**

When a design contains multiple RTL modules, synthesis can either preserve the module structure or combine the logic into a single representation.

Understanding this distinction is important because the synthesis structure can affect **optimization, readability, debugging, and design organization**.

## **Hierarchical Synthesis**

In hierarchical synthesis, the relationships between the RTL modules are retained.

```text
             Top Module
             /        \
            /          \
       Module A      Module B
```

The individual blocks remain identifiable after synthesis.

### **Advantages**

* Maintains the original design organization.
* Makes individual blocks easier to identify.
* Can simplify debugging and analysis.
* Useful for designs divided into reusable modules.

---

## **Flattened Synthesis**

Flattening removes the boundaries between the individual modules and combines their logic into a unified representation.

```text
Module A ──┐
           ├──► Flat Design
Module B ──┘
```

With the hierarchy removed, the synthesis tool has greater freedom to optimize logic across module boundaries.

### **Advantages**

* Allows optimization across module boundaries.
* Can remove unnecessary intermediate logic.
* Produces a unified implementation.
* May improve optimization opportunities for certain designs.

## **Comparison**

| **Feature**          | **Hierarchical** | **Flattened**            |
| -------------------- | ---------------- | ------------------------ |
| Module hierarchy     | Preserved        | Removed                  |
| Optimization         | More localized   | Across module boundaries |
| Debugging            | Easier           | Relatively harder        |
| Design structure     | Modular          | Unified                  |
| Block identification | Easier           | Less direct              |

### **Hierarchical / Flattened Synthesis**

**<img width="859" height="178" alt="day2_secpic" src="https://github.com/user-attachments/assets/e5d1d9a0-8997-4a68-940e-b8511785ea20" />


---

# **3. Flip-Flop RTL Coding**

A **flip-flop** is a sequential storage element that retains a value and normally updates its output according to a clock event.

The RTL description of a flip-flop depends on how its control signals are intended to operate.

In this experiment, three common forms were implemented:

1. Asynchronous reset
2. Asynchronous set
3. Synchronous reset

---

## **3.1 Asynchronous Reset D Flip-Flop**

An asynchronous reset does not have to wait for the clock edge.

When the reset signal is asserted, the output is immediately forced to the reset state.

```verilog
module dff_asyncres (
    input clk,
    input async_reset,
    input d,
    output reg q
);

always @(posedge clk, posedge async_reset)
begin
    if (async_reset)
        q <= 1'b0;
    else
        q <= d;
end

endmodule
```

### **Operation**

* `async_reset = 1` → `q` is immediately cleared to `0`
* `async_reset = 0` → `q` captures `d` on the rising edge of `clk`

---

## **3.2 Asynchronous Set D Flip-Flop**

An asynchronous set operates independently of the normal clocked data transfer.

When the set signal is asserted, the output is immediately forced to `1`.

```verilog
module dff_async_set (
    input clk,
    input async_set,
    input d,
    output reg q
);

always @(posedge clk, posedge async_set)
begin
    if (async_set)
        q <= 1'b1;
    else
        q <= d;
end

endmodule
```

### **Operation**

* `async_set = 1` → `q` becomes `1` immediately
* `async_set = 0` → `q` captures `d` on the rising edge of `clk`

---

## **3.3 Synchronous Reset D Flip-Flop**

A synchronous reset is evaluated only when the active clock edge occurs.

Unlike an asynchronous reset, changing the reset signal alone does not immediately change the output.

```verilog
module dff_syncres (
    input clk,
    input sync_reset,
    input d,
    output reg q
);

always @(posedge clk)
begin
    if (sync_reset)
        q <= 1'b0;
    else
        q <= d;
end

endmodule
```

### **Operation**

At every rising edge of `clk`:

* `sync_reset = 1` → `q` becomes `0`
* `sync_reset = 0` → `q` captures `d`

---

## **Reset Behavior Comparison**

```text
Asynchronous Reset

Reset ─────────────► Output changes immediately


Synchronous Reset

Reset ─────► Clock Edge ─────► Output changes
```

| **Type**               | **When control is effective**      |
| ---------------------- | ---------------------------------- |
| **Asynchronous Reset** | Immediately when reset is asserted |
| **Asynchronous Set**   | Immediately when set is asserted   |
| **Synchronous Reset**  | Only at the active clock edge      |

### **Flip-Flop RTL / Code**

<img width="1252" height="157" alt="day2_3" src="https://github.com/user-attachments/assets/ea867587-3b08-4b34-817a-1a21f1dcb155" />


---

# **4. Simulation and Synthesis**

The flip-flop designs were first verified through **RTL simulation**. After confirming their behavior, the designs were synthesized using **Yosys** and mapped to cells from the SKY130 technology library.

This provides a complete path from **RTL coding → simulation → synthesis → technology mapping**.

---

## **4.1 Icarus Verilog Simulation**

The RTL design and testbench were compiled using:

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
```

The generated simulation executable was then run using:

```bash
./a.out
```

The waveform was opened using:

```bash
gtkwave tb_dff_asyncres.vcd
```

GTKWave was used to observe the relationship between the **clock, reset, input, and output signals** and verify the expected flip-flop behavior.

### **Simulation Result**
**<img width="1258" height="652" alt="day2_4" src="https://github.com/user-attachments/assets/604a2ff9-4306-4f91-a6ce-1bfacfaeb131" />


---

## **4.2 Yosys Synthesis**

Yosys was used to convert the flip-flop RTL into a technology-mapped representation.

### **Launch Yosys**

```bash
yosys
```

### **Load the Technology Library**

```bash
read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
```

### **Read the RTL**

```bash
read_verilog /path/to/dff_asyncres.v
```

### **Perform Synthesis**

```bash
synth -top dff_asyncres
```

### **Map the Flip-Flop**

```bash
dfflibmap -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
```

The `dfflibmap` command maps the inferred flip-flop to a suitable flip-flop cell available in the selected technology library.

### **Technology Mapping**

```bash
abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
```

The `abc` step performs technology mapping and optimizes the synthesized logic using the selected library.

### **Display the Synthesized Design**

```bash
show
```

This displays the resulting synthesized circuit graphically.

### **Gate-Level Representation**

**<img width="1251" height="194" alt="da2_last" src="https://github.com/user-attachments/assets/8fa6b462-885c-4028-9727-4cf4daa6a52b" />



---

## **Command Summary**

| **Command**    | **Purpose**                               |
| -------------- | ----------------------------------------- |
| `read_liberty` | Loads the selected standard-cell library  |
| `read_verilog` | Reads the RTL source                      |
| `synth -top`   | Performs RTL synthesis                    |
| `dfflibmap`    | Maps inferred flip-flops to library cells |
| `abc`          | Performs technology mapping               |
| `show`         | Displays the synthesized circuit          |

---

## **RTL-to-Implementation Flow**

```text
       Flip-Flop RTL
             │
             ▼
        Icarus Verilog
             │
             ▼
       GTKWave Analysis
             │
             ▼
           Yosys
             │
             ▼
        dfflibmap
             │
             ▼
            ABC
             │
             ▼
      Technology-Mapped
        Flip-Flop
```


---

# **5. Conclusion**

Day 02 provided a deeper understanding of how **technology libraries influence RTL synthesis and implementation**.

The experiment introduced the structure and purpose of the **SKY130 Liberty library**, including process, temperature, and voltage information. It also demonstrated how synthesis can either preserve module hierarchy or flatten the design for broader optimization.

Different D flip-flop behaviors were then described in Verilog using **asynchronous reset, asynchronous set, and synchronous reset**. These designs were verified using **Icarus Verilog and GTKWave** and subsequently synthesized using **Yosys**, including flip-flop mapping with `dfflibmap` and technology mapping using `abc`.

Overall, Day 02 strengthened the connection between **RTL coding, timing libraries, simulation, synthesis, and technology-specific hardware implementation**.

