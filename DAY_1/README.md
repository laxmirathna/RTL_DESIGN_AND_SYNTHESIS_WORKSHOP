# **Day 01 — RTL Design, Simulation & Synthesis**

## **Experiment Objective**

The objective of Day 01 was to understand the fundamentals of **RTL design and digital design verification using Verilog**.

The experiment covered the complete flow from writing the RTL design and testbench to compiling, simulating, analyzing waveforms, and synthesizing the design into a gate-level representation.

A **2:1 Multiplexer** was used as the practical design example throughout the experiment.

---

## **Contents**

* [Digital Design Verification](#1-digital-design-verification)
* [Simulation Workflow with Icarus Verilog](#2-simulation-workflow-with-icarus-verilog)
* [Practical Exercise – 2:1 Multiplexer](#3-practical-exercise--21-multiplexer)
* [Multiplexer RTL Design](#4-multiplexer-rtl-design)
* [Introduction to Yosys](#5-introduction-to-yosys)
* [RTL Design and Synthesis](#6-rtl-design-and-synthesis)
* [Understanding `.lib` Files and Cell Flavors](#7-understanding-lib-files-and-cell-flavors)
* [Yosys Synthesis of the Multiplexer](#8-yosys-synthesis-of-the-multiplexer)
* [Synthesis Results and Gate-Level Representation](#9-synthesis-results-and-gate-level-representation)
* [Generated Gate-Level Netlist](#10-generated-gate-level-netlist)
* [Conclusion](#11-conclusion)

---

# **1. Digital Design Verification**

Before a digital circuit is implemented in hardware, its functionality must be verified through simulation.

The basic verification setup consists of a **design, testbench, and simulator**.

### **Simulator**

A simulator executes the Verilog design in a virtual environment. It allows signal behavior and circuit responses to be observed under different input conditions before hardware implementation.

### **Design**

The design is the Verilog module that describes the intended digital circuit. It defines the inputs, outputs, and logic required to implement the desired functionality.

### **Testbench**

A testbench is a separate verification module used to apply different input combinations to the design and observe the resulting outputs.

### **Verification Flow**

```text
Design + Testbench
        ↓
    Simulator
        ↓
 Output / Waveform

```

### **Testbench**

![image](PASTE_TESTBENCH_IMAGE_URL_HERE)

<img width="1227" height="709" alt="testbench" src="https://github.com/user-attachments/assets/72e9b40a-fb6d-4a85-bc28-73a03ae004c8" />

---

# **2. Simulation Workflow with Icarus Verilog**

**Icarus Verilog (`iverilog`)** is an open-source Verilog compiler and simulator.

It compiles the RTL design and testbench, executes the simulation, and can generate a **Value Change Dump (`.vcd`)** file containing signal transitions.

The generated waveform file can then be opened using **GTKWave** for detailed analysis.

## **Simulation Flow**

```text
Verilog Design
      +
  Testbench
      ↓
Icarus Verilog
      ↓
  Simulation
      ↓
   .vcd File
      ↓
   GTKWave
      ↓
Waveform Analysis
```

### **Simulation Flow Diagram**

![image](PASTE_SIMULATION_FLOW_IMAGE_URL_HERE)
<img width="1263" height="778" alt="simulationflow" src="https://github.com/user-attachments/assets/a8a55fba-44e3-4e00-8c24-57c7734fb923" />


---

# **3. Practical Exercise — 2:1 Multiplexer**

A **2:1 Multiplexer** was selected as the practical design for understanding the Verilog simulation process.

The experiment was performed using **Icarus Verilog** for simulation and **GTKWave** for waveform visualization.

## **Step 1 — Install Required Tools**

```bash
sudo apt install iverilog
sudo apt install gtkwave
```

---

## **Step 2 — Compile the Design**

```bash
iverilog good_mux.v tb_good_mux.v
```

This command compiles the multiplexer RTL file together with its testbench.

---

## **Step 3 — Run the Simulation**

```bash
./a.out
```

The generated simulation executable runs the testbench and produces the waveform data.

---

## **Step 4 — Open the Waveform**

```bash
gtkwave tb_good_mux.vcd
```

GTKWave can then be used to inspect the transitions of the input, select, and output signals.

### **GTKWave Output**

![image](PASTE_WAVEFORM_IMAGE_URL_HERE)
<img width="960" height="1020" alt="muxtbscreenshot" src="https://github.com/user-attachments/assets/e9b134f7-c066-4187-85b2-af0e247872b9" />


---

# **4. Multiplexer RTL Design**

The following Verilog code describes the behavior of the 2:1 Multiplexer.

## **Verilog Implementation**

```verilog
module good_mux (
    input i0,
    input i1,
    input sel,
    output reg y
);

always @(*)
begin
    if (sel)
        y <= i1;
    else
        y <= i0;
end

endmodule
```

## **Signal Description**

| **Signal** | **Description**    |
| ---------- | ------------------ |
| `i0`       | First data input   |
| `i1`       | Second data input  |
| `sel`      | Selection signal   |
| `y`        | Multiplexer output |

## **Operation**

The select signal determines which input appears at the output:

* `sel = 0` → `y = i0`
* `sel = 1` → `y = i1`


### **Verilog Code**

![image](PASTE_VERILOG_CODE_IMAGE_URL_HERE)<img width="600" height="180" alt="MUXcode" src="https://github.com/user-attachments/assets/506be1fe-988a-4b10-be9b-f5921a5ba22d" />


---

# **5. Introduction to Yosys**

**Yosys** is an open-source synthesis framework used to transform Verilog RTL into a synthesized gate-level representation.

The synthesis process converts the behavioral RTL description into an implementation based on available logic or technology-library cells.

## **Basic Yosys Flow**

```text
RTL Design
     ↓
Technology Library
     ↓
    Yosys
     ↓
Logic Synthesis
     ↓
Technology Mapping
     ↓
Gate-Level Netlist
```

A basic synthesis sequence is:

```bash
read_liberty -lib <library>.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty <library>.lib
write_verilog synthesized_mux.v
```

The generated netlist can then be inspected and further verified.
### **Yosys Output**

![image](PASTE_YOSYS_OUTPUT_1_IMAGE_URL_HERE)<img width="1256" height="695" alt="yoyssetuo" src="https://github.com/user-attachments/assets/01dbff92-713b-494f-9a8e-298ca92e0259" />


![image](PASTE_YOSYS_OUTPUT_2_IMAGE_URL_HERE)<img width="1250" height="703" alt="yosyssim" src="https://github.com/user-attachments/assets/f1d9a81c-f71e-4f1f-9798-388b5a180a74" />

---

# **6. RTL Design and Synthesis**

Synthesis creates the connection between **RTL design and gate-level implementation**.

The RTL describes the intended functionality, while synthesis converts that description into a hardware structure that can be implemented using available cells.

```text
RTL Design
     ↓
  Synthesis
     ↓
Logic Structure
     ↓
Technology Mapping
     ↓
Gate-Level Netlist
```

## **RTL vs Gate-Level Design**

| **RTL Design**                                 | **Gate-Level Design**                               |
| ---------------------------------------------- | --------------------------------------------------- |
| Describes circuit functionality using Verilog. | Represents the synthesized hardware structure.      |
| Easier to write, understand, and modify.       | Represents the implementation using cells or gates. |
| Used during functional simulation.             | Used for implementation and further verification.   |


### **Gate-Level Representation**

![image](PASTE_GATE_LEVEL_IMAGE_URL_HERE)<img width="975" height="623" alt="yosyssynthesis" src="https://github.com/user-attachments/assets/29433796-c86e-4f3a-8315-b02a2937f281" />

---

# **7. Understanding `.lib` Files and Cell Flavors**

A **Liberty (`.lib`) file** contains information about the standard cells available in a particular technology library.

The library can contain information related to:

* Cell functionality
* Timing characteristics
* Power characteristics
* Area
* Operating conditions
* Input and output behavior

Different versions of cells may be available to satisfy different implementation requirements.

## **Faster and Slower Cell Variants**

### **Faster Cells**

* Provide lower propagation delay.
* Are useful for timing-critical paths.
* May require higher power or area.

### **Slower Cells**

* Have higher delay compared with faster variants.
* Can be used where timing requirements are less demanding.
* May provide advantages in power or area.

Therefore, using the fastest cell everywhere is not necessarily optimal. Cell selection depends on design requirements such as **timing, power, and area**.

---

# **8. Yosys Synthesis of the Multiplexer**

Yosys can be launched from the Linux terminal using:

```bash
yosys
```


### **Launching Yosys**

![image](PASTE_YOSYS_LAUNCH_IMAGE_URL_HERE)<img width="744" height="160" alt="yosysinvoke" src="https://github.com/user-attachments/assets/7f0493e8-7e49-43c6-9960-c7a71bb6d421" />


The `good_mux` design was synthesized using:

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr good_mux_net.v
```

## **Command Breakdown**

| **Command**     | **Purpose**                                             |
| --------------- | ------------------------------------------------------- |
| `read_liberty`  | Loads the selected technology library.                  |
| `read_verilog`  | Reads the RTL Verilog design.                           |
| `synth -top`    | Synthesizes the specified top module.                   |
| `abc`           | Performs technology mapping using the selected library. |
| `write_verilog` | Generates the synthesized Verilog netlist.              |

---

# **9. Synthesis Results and Gate-Level Representation**

After synthesis, Yosys provides statistics describing the resulting design, including information about ports, wires, and cells.

### **Synthesis Statistics**

![image](PASTE_SYNTHESIS_STATISTICS_IMAGE_URL_HERE)<img width="975" height="623" alt="yosyssynthesis" src="https://github.com/user-attachments/assets/a7fb47a8-b837-4ffa-9ef4-8f756a5ebe50" />


The synthesized circuit can also be visualized using:

```bash
show
```

This displays a graphical representation of the synthesized gate-level design.

### **Gate-Level Logic**

![image](PASTE_GATE_LEVEL_LOGIC_IMAGE_URL_HERE)


---

# **10. Generated Gate-Level Netlist**

The synthesized Verilog netlist was generated using:

```bash
write_verilog -noattr good_mux_net.v
```

The generated file can be viewed from the terminal using:

```bash
cat good_mux_net.v
```

The resulting netlist represents the original RTL functionality using cells from the selected technology library.

### **Gate-Level Logic**

![image](PASTE_GATE_LEVEL_LOGIC_IMAGE_URL_HERE)<img width="1254" height="407" alt="yosysgoodmux" src="https://github.com/user-attachments/assets/2de83798-882e-4e4f-85d3-67a08489589c" />



---

# **11. Conclusion**

Day 01 provided a practical introduction to the **RTL-to-netlist design flow**.

Through the 2:1 Multiplexer experiment, I learned how to write and verify a Verilog RTL design, compile it using **Icarus Verilog**, analyze simulation waveforms using **GTKWave**, and synthesize the design using **Yosys**.

The experiment also introduced **Liberty (`.lib`) technology libraries**, different cell variants, technology mapping, synthesis statistics, and gate-level netlists.

Overall, Day 01 established the foundation for understanding how **Verilog RTL progresses from a functional description to a technology-mapped hardware representation**.
