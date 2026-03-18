# Hardware

A collection of synthesisable RTL designs, power electronics projects, and educational resources covering CPU architecture, arithmetic hardware, SoC design, and DC-DC converter control.

---

## RISC-V SoC Platform

A complete RISC-V System-on-Chip built from independently verified subsystems.

| Project | Description |
|---------|-------------|
| [RISC-V SoC — System Integration](https://github.com/BrendanJamesLynskey/RISCV_SoC) | **Top-level SoC** integrating CPU, crossbar, MMU, cache, DMA, and IOMMU into a unified bus-connected system with PLIC, SRAM, and peripheral bridge |

### SoC Subsystems

| Project | Role in SoC | Description |
|---------|-------------|-------------|
| [BRV32P — 5-Stage Pipelined RV32IMC](https://github.com/BrendanJamesLynskey/RISCV_RV32IMC_5stage) | CPU Core | In-order 5-stage pipeline with full forwarding, 2-way set-associative caches, AXI4-Lite bus, branch prediction, M and C extensions |
| [AXI4 Crossbar](https://github.com/BrendanJamesLynskey/AXI4_Crossbar) | Interconnect | Parameterised NxM AXI4 crossbar — round-robin arbitration, ID-based response routing, error slave, independent read/write paths |
| [MMU (Sv32)](https://github.com/BrendanJamesLynskey/MMU) | Virtual Memory | Sv32 MMU with fully-associative TLB, hardware page table walker, permission checking, SFENCE.VMA support |
| [Cache Controller (MESI)](https://github.com/BrendanJamesLynskey/Cache_Controller_MESI) | L2 Cache | 4-way set-associative cache with MESI coherence, write-back policy, snoop interface, AXI4 bus interface |
| [DMA Controller](https://github.com/BrendanJamesLynskey/RISCV_DMA) | DMA Engine | 4-channel DMA with scatter-gather, AXI4 master, per-channel interrupts |
| [IOMMU (Sv32)](https://github.com/BrendanJamesLynskey/RISCV_IOMMU) | I/O Translation | I/O address translation for DMA isolation — IOTLB, device context cache, fault handling |

---

## RISC-V CPUs

| Project | Description |
|---------|-------------|
| [BRV32 — Single-Cycle RV32I](https://github.com/BrendanJamesLynskey/RISCV_RV32I_SingleCycle) | Complete single-cycle RV32I SoC in Verilog-2001 — CPU, GPIO, UART, Timer, machine-mode CSRs. 32/32 tests passing |

## Arithmetic Units

| Project | Description |
|---------|-------------|
| [Integer Dividers](https://github.com/BrendanJamesLynskey/Integer_dividers) | Five SystemVerilog divider architectures — restoring, non-performing, non-restoring, SRT radix-4, and Newton-Raphson |
| [Floating-Point Dividers](https://github.com/BrendanJamesLynskey/Floating_Point_Dividers) | Six IEEE 754 FP32 divider architectures — restoring, non-restoring, SRT-2, SRT-4, Newton-Raphson, and Goldschmidt |
| [CORDIC](https://github.com/BrendanJamesLynskey/CORDIC) | Synthesisable SystemVerilog implementations of the CORDIC algorithm |
| [Neural Network Data Types](https://github.com/BrendanJamesLynskey/NN_data_types) | SystemVerilog implementations of 9 numerical formats (FP32 down to FP4) used in NN training and inference hardware |

## ML Accelerator Hardware

| Project | Description |
|---------|-------------|
| [Transformer Decoder — RTL Accelerator](https://github.com/BrendanJamesLynskey/LLM_Transformer_Decoder_RTL) | Synthesisable SystemVerilog implementation of a pre-norm decoder block with KV-cache, plus full verification suite (83 tests) |

## Power Electronics

| Project | Description |
|---------|-------------|
| [DC-DC Converter Control Techniques](https://github.com/BrendanJamesLynskey/DCDC_Control_Techniques) | Interactive Reveal.js presentation covering PWM (voltage-mode, peak/valley/average current-mode), PFM, hysteretic, and constant on-time (COT) control — with interactive waveform and efficiency graphics, tradeoff comparisons, and future directions including digital control and GaN |
| [COT DC-DC Converter](https://github.com/BrendanJamesLynskey/COT_DCDC_Simulink) | MATLAB/Simulink constant on-time DC-DC converter model, adapted from the NPTEL course on switched mode power converter control |

## SoC Design

| Project | Description |
|---------|-------------|
| [Modern SoC Design](https://github.com/BrendanJamesLynskey/SoC) | Interactive presentation series — advanced packaging, chiplets, on-chip interconnect/NoC, memory hierarchies, and high-speed SerDes |

## HDL Examples

| Project | Description |
|---------|-------------|
| [VHDL Example Code](https://github.com/BrendanJamesLynskey/VHDL_example_code) | Example VHDL coding samples |
