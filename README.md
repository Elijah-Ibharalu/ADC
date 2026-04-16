# ADC
# ⚡ Model-Based Design: Simulink to VHDL for ADC Digital Logic

![MATLAB](https://img.shields.io/badge/MATLAB-Simulink-blue.svg)
![HDL Coder](https://img.shields.io/badge/HDL_Coder-VHDL-orange.svg)
![FPGA](https://img.shields.io/badge/Target-FPGA-success.svg)

## 📌 Project Overview
This repository demonstrates a complete Model-Based Design (MBD) workflow using **MATLAB/Simulink** to model an Analog-to-Digital Converter (ADC) environment and successfully generate synthesizable **VHDL** code for the digital backend. 

The project bridges the gap between high-level analog simulation and low-level digital RTL (Register-Transfer Level) hardware implementation, utilizing the **HDL Coder** toolchain.

## 🏗️ Architecture: Testbench vs. Synthesizable Logic
One of the core architectural achievements of this project was correctly partitioning the model to isolate physical analog behavior from digital hardware logic:

* **Analog Testbench (Outside the DUT):** Models the real-world imperfections of an ADC, including anti-aliasing filters, aperture jitter, time delays, and amplifier non-linearities (using continuous `tanh` and `Gain` functions).
* **Digital Subsystem (The DUT):** The strictly discrete, synthesizable portion of the design targeted for FPGA deployment. This includes data type conversion, quantization, and digital saturation.

## 🚀 Key Accomplishments
* **0 Errors, 0 Warnings:** Achieved a perfectly clean HDL generation report, proving the model's strict compatibility with hardware constraints.
* **Custom Fixed-Point Precision:** Implemented an optimized 16-bit word length with a 14-bit fraction length (`fixdt(1,16,14)`) to balance hardware resource utilization with quantization accuracy.
* **Automated RTL Generation:** Successfully transitioned from graphical signal flows to fully functional `.vhd` source files ready for FPGA synthesis.

## 🚧 Overcoming Technical Hurdles
Generating HDL from a mixed-signal model requires strict adherence to hardware rules. Here are the key challenges solved during development:

### 1. The "Floating-Point to Fixed-Point" Barrier
* **Problem:** Simulink naturally computes analog signals as continuous `Double` floating-point types, which digital logic gates (FPGAs) cannot natively process without massive resource overhead.
* **Solution:** Engineered a robust entry point into the digital subsystem using a precise **Data Type Conversion** block. By enabling the **Native Floating Point** library and explicitly defining the quantization strategy, the continuous analog wave was successfully mapped to a discrete fixed-point bitstream.

### 2. Multirate Timing Synchronization
* **Problem:** The continuous analog simulation ran at an infinite rate, while the digital logic required a strict, discretized clock rate, leading to multirate processing errors during code generation.
* **Solution:** Enforced strict timing boundaries using a **Zero-Order Hold** to sample the analog signal at a specific frequency. Transitioned the model solver to **Fixed-step discrete** and enforced strict constraints on multitask data transfers, ensuring perfect synchronization for hardware deployment.

## 🛠️ Tech Stack & Tools
* **MATLAB / Simulink:** Primary modeling and simulation environment.
* **HDL Coder:** Automated VHDL generation and hardware compatibility checking.
* **VHDL (VHSIC Hardware Description Language):** The target language for the generated RTL.

## 📂 Repository Structure
* `ADC_personal.slx` - The main Simulink model containing both the testbench and the digital subsystem.
* `/hdlsrc/` - The generated VHDL source code directory.
    * `Subsystem.vhd` - The primary RTL file containing the synthesized digital logic.
* `Subsystem_report.html` - The final generation report verifying 0 errors and 0 warnings.

## 💡 How to Explore
To view the generated hardware code, navigate to the `hdlsrc` folder and open the `.vhd` files. To run the simulation, open `ADC_personal.slx` in MATLAB (requires Simulink and HDL Coder toolboxes).
