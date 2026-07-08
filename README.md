readme_content = """# 16-Point Fast Fourier Transform (FFT) Processor: ASIC Flow Design

## Executive Summary
This repository contains the complete RTL-to-GDSII implementation of a hardware-accelerated 16-point Radix-2 Fast Fourier Transform (FFT) processor. Designed to address the high latency and power consumption limitations of software-based FFT implementations, this ASIC design is optimized for real-time Digital Signal Processing (DSP) and embedded systems. The processor was developed using Verilog HDL and synthesized into a physical layout using the open-source OpenLane ASIC flow and the SkyWater 130nm Process Design Kit (PDK).

## Technologies & EDA Tools
* **Hardware Description Language (HDL):** Verilog
* **Simulation & Functional Verification:** Xilinx Vivado, ModelSim, GTKWave
* **Logic Synthesis:** Yosys
* **Physical Design (Place & Route):** OpenLane Flow
* **Process Design Kit (PDK):** SkyWater 130nm (Sky130)

## Hardware Architecture & Methodology
The architecture is inherently modular, ensuring scalability, reuse, and efficient routing across multiple FFT computation stages. 

### 1. Core Computational Blocks
* **Radix-2 Algorithm:** Decomposes the Discrete Fourier Transform (DFT) to minimize computational complexity.
* **Butterfly Computation Unit:** The primary arithmetic engine, executing complex addition and subtraction for both real and imaginary components.
* **Twiddle Factor Multipliers:** Dedicated, precision-optimized signed multiplication blocks for complex exponential coefficients.

### 2. Memory & Storage
* **Dual-Port Memory Block:** Facilitates organized, synchronous read/write operations for stage-wise intermediate data.
* **Lookup Tables (LUTs):** * *Twiddle Factor LUT:* Stores precomputed complex exponentials, eliminating real-time trigonometric calculations and drastically reducing latency.
  * *Address Generation LUT:* Computes fetch sequences for accurate bit-reversed and natural-order routing.

### 3. FSM Control Logic
A robust Finite State Machine (FSM) synchronizes the entire datapath. It dictates multiplexer selections, stage transitions, memory access timing, and the propagation of valid signals to ensure stall-free execution.

## RTL-to-GDSII ASIC Flow (Physical Design)
The processor was driven through a complete physical design pipeline to generate a fabrication-ready GDSII layout.

1. **Floorplanning:** Defined core silicon area and I/O pin placements for optimal standard cell density.
2. **Standard Cell Placement:** Minimized routing congestion and wirelength.
3. **Clock Tree Synthesis (CTS):** Generated a balanced clock distribution network to mitigate clock skew across sequential FSM and memory elements.
4. **Routing & Optimization:** Executed global and detailed routing while resolving IR drop and timing violations.
5. **GDSII Generation:** Exported the final verified layout for tape-out simulation.

## Performance, Power, and Area (PPA) Results
Extensive synthesis exploration was conducted using Yosys to identify the optimal PPA configuration before committing to Place and Route (PnR). Seven distinct area-driven and delay-driven strategies were evaluated.

**Selected Configuration: AREA 1 Strategy**
* **Total Silicon Area:** ~68,576 µm² (Optimal compactness for embedded footprint).
* **Power Consumption:** ~3.47 mW (Highly efficient for low-power DSP applications).
* **Timing & Frequency:** Achieved positive setup and hold slack across all paths; operates reliably at the synthesized target clock frequency.
* **Trade-off Analysis:** AREA 1 was chosen over the AREA 0 baseline as it yielded the smallest die footprint while simultaneously exhibiting a slight reduction in signal delay (12.24 ns vs. 12.40 ns). 

## 📂 Repository Structure
