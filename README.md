# 16-Point Fast Fourier Transform (FFT) Processor: ASIC Flow Design

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
* **Lookup Tables (LUTs):** * *Twiddle Factor LUT:* Stores precomputed complex exponentials, avoiding real-time computation and improving speed.
  * *Address Generation LUT:* Generates appropriate read addresses for fetching data in the correct order.

### 3. FSM Control Logic
A robust Finite State Machine (FSM) synchronizes the entire datapath. It dictates multiplexer selections, stage transitions, memory access timing, and the data flow between modules to ensure stall-free execution.

## RTL-to-GDSII ASIC Flow (Physical Design)
The processor was driven through a complete physical design pipeline to generate a fabrication-ready GDSII layout.

* **Floorplanning:** Initialized the die/core bounding boundaries, configured the Power Distribution Network (PDN) straps (VDD/GND grid), and locked input/output interface pins along the perimeter layout edges.
* **Standard Cell Placement:** Legalized and snapped synthesized target gates cleanly onto horizontal placement tracks, distributing the logical footprint uniformly to avoid congestion.
* **Clock Tree Synthesis (CTS):** Engineered an optimized clock distribution tree to preserve uniform, high-speed clock delivery across all pipeline stages while minimizing global skew.
* **Routing & Optimization:** Interconnected standard cell pins across multiple physical metal layers (`li1`, `met1`, `met2`, `met3`), validating interconnect tracks and solving remaining maximum-slew timing anomalies.
* **GDSII Generation:** Streamed out the final fully routed physical layout configuration data to an industry-standard GDSII data stream format for silicon sign-off.

## Performance, Power, and Area (PPA) Results
Extensive synthesis exploration was conducted using Yosys to sweep area-driven and delay-driven constraints to isolate the optimal structural architecture before executing physical floorplanning. 

**Selected Configuration: DELAY 4 Strategy**
* **Total Mapped Standard Cells:** 2,944 cells cleanly mapped to physical technology primitives.
* **Total Core Logic Area:** 34,128.98 µm²
* **Sequential Footprint Distribution:** 651 D-Flip Flops (`sky130_fd_sc_hd__dfxtp_2`) occupying 13,847.03 µm², accounting for 40.57% of the active logic area.
* **Dynamic & Static Power:** 1.13 mW total power estimation, maintaining high power efficiency suitable for resource-constrained embedded systems.
* **Microarchitectural Gate Density:**
  * *Data-Path Routing:* Employs over 700 custom multiplexer configurations (including 128 high-density `mux4_2` cells) to execute the parallel radix butterfly array shuffle paths.
  * *Fanout Buffering:* Integrates a calculated distribution of drive-strength buffers (including heavy-duty `buf_6` and `buf_12` configurations) to eliminate propagation skew and bound maximum slew parameters safely prior to sign-off routing.

## Repository Structure

    ├── src/                    # Verilog RTL source files (Butterfly, Multiplier, FSM, etc.)
    ├── tb/                     # Testbenches for module-level and top-level verification
    ├── openlane_runs/          # PnR configuration scripts and physical design summaries
    ├── docs/                   # PDF Report, FSM diagrams, and schematic visualizations
    ├── images/                 # Heatmaps (IR Drop, Routing Congestion) and GDSII layouts
    └── README.md               # Project documentation

## Local Simulation Instructions
To verify the frequency-domain conversions locally:

1. Clone this repository and open it in Visual Studio Code.
2. Compile the Verilog testbenches using your preferred simulator (like Icarus Verilog). Note: For Apple Silicon (M1) systems, ensure your simulator binaries and GTKWave are natively compiled for the arm64 architecture to prevent translation bottlenecks.
3. Run the simulation to generate the .vcd waveform dump.
4. Open the waveform in GTKWave to validate the real and imaginary FFT output vectors against theoretical mathematical models.

## Future Scope
* **Scalability:** Extend the RTL architecture to support 32-point and 64-point FFT computations.
* **Throughput Optimization:** Implement multi-stage pipelining and parallel processing architectures.
* **Low-Power Enhancements:** Integrate clock gating to further suppress dynamic power consumption.
* **Hardware Integration:** Tape-out for fabrication and post-silicon testing.
