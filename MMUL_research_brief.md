# Hardware Matrix Multiplier Units (MMUL) — Technical Research Brief

Dense raw material for slides / README. Cited specs are from primary vendor docs and ISCA/Hot Chips papers where possible. Contradictions and 2025–26 items flagged at the bottom.

---

## 1. History (chronological)

### Pre-history: MAC roots
- 1960s–70s: digital signal processing leans on multiply-accumulate (MAC) as the kernel for FIR filters, FFTs, correlators. Discrete TTL multipliers, then single-chip MACs (TRW MPY-016 multiplier-accumulators, 1976).
- TI TMS320 series DSPs (1982 onward) make MAC a single-cycle ISA primitive — the immediate ancestor of every "tensor" instruction.

### 1978–1982: Systolic origins (CMU)
- Kung & Leiserson, "Systolic Arrays (for VLSI)", *Sparse Matrix Proceedings 1978* (SIAM, pp. 245–282) — the foundational paper.
- H. T. Kung, "Why Systolic Architectures?", *Computer* (IEEE), Jan 1982 — the canonical citation, often misdated as 1979 in secondary sources. Argues for arrays of locally-connected PEs that "pump" data in lockstep, amortising memory bandwidth across many ops.

### 1984–1990: Warp / iWarp
- Warp (CMU + GE/Honeywell, 1984) — 10-cell linear systolic array, 100 MFLOPS per cell at 10 MHz.
- iWarp (CMU + Intel, first 12-node system online March 1990) — single-chip 700K-transistor LIW processor with integrated routing; 20 MFLOPS / 20 MIPS per cell. Direct ancestor of "compute + on-chip mesh routing" in modern accelerators.

### 1980s–90s: Other spatial machines
- GAPP (NCR Geometric Arithmetic Parallel Processor, 1984) — 6×12 bit-serial SIMD array on one die; targeted image processing.
- MasPar MP-1/MP-2 (1990–92), Thinking Machines CM-1/2/5 — dense fine-grained SIMD/SPMD; not strictly systolic but the same "compute close to memory" thesis.
- MIT Cilk (1994) — software side: structured parallelism that later informed dataflow compilers (TensorFlow XLA lineage).

### 2000s: DSP / VPU heritage
- Movidius (founded Dublin 2005, acquired by Intel Sept 2016) — Myriad 1/2/X VPUs with SHAVE VLIW vector cores, on-chip CMX scratchpad. Myriad X (2017) added a "Neural Compute Engine" — an early dedicated MMUL block (~1 TOPS, INT8) for edge vision. Direct lineage to Intel's later Movidius/Habana product line and DSP-style NPUs in phones.

### 2015–2017: TPU v1 makes MMUL mainstream
- Google TPU v1: deployed internally 2015, paper Jouppi et al. ISCA 2017 "In-Datacenter Performance Analysis of a Tensor Processing Unit".
- 256×256 INT8 MAC systolic array = 65,536 PEs; 700 MHz → 92 TOPS peak INT8.
- Output-stationary variant: weights load and stay; activations stream from the left; partial sums flow downward into 4 MiB of 32-bit accumulators (4096 × 256-element rows). 28 MiB on-chip unified buffer; 8 GiB DDR3 weights; 75 W TDP; 28 nm.
- Designed strictly for inference, INT8 only; latency-bounded by Google's 99th-percentile SLA for serving.

### 2017: NVIDIA Volta V100 — Tensor Cores
- GV100 (May 2017, 12 nm) — 84 SMs, 8 Tensor Cores per SM = 672 TC, 5,376 FP32 CUDA cores.
- Each TC computes D = A·B + C on 4×4 matrices; A, B FP16; C, D FP16 or FP32 accumulator. 64 FMAs per clock per TC ⇒ ~125 TFLOPS FP16 mixed-precision per V100.
- First broad-deployment "matrix-as-instruction" inside a GPU. Conceptually a small dot-product tree per TC, not a Kung systolic.

### 2018–2024: Tensor Core generations
| Gen | GPU (year) | New formats | Peak dense (per-GPU) |
|---|---|---|---|
| 1st | V100 (2017) | FP16 mixed-precision | 125 TFLOPS FP16 |
| 2nd | Turing T4 / RTX 20 (2018) | INT8, INT4 | 130 TOPS INT8 (T4) |
| 3rd | A100 Ampere (2020) | TF32, BF16, FP64 TC, **2:4 structured sparsity** | 312 TFLOPS BF16 / 624 sparse |
| 4th | H100 Hopper (2022) | **FP8 (E4M3, E5M2)**, TMA, Transformer Engine | 1979 TFLOPS FP8 / **3958 sparse** |
| 5th | B200 Blackwell (2024) | **FP4 (E2M1), MXFP, NVFP4**, dual-die | 4.5 PF FP8, **20 PF FP4** |

(2:4 structured sparsity = compress 4 elements to keep 2 nonzero → 2× throughput on Sparse Tensor Cores.)

### 2017–2025: TPU v2 → Trillium
- TPU v2 (2017): float MXU, 128×128, BF16 multiply / FP32 accumulate; ~46 TFLOPS BF16/chip, 4 chips/board.
- TPU v3 (2018): liquid-cooled, ~123 TFLOPS BF16/chip; 2 cores × 2 MXUs.
- TPU v4 (2021): ~275 TFLOPS BF16/INT8 per chip; optical circuit-switched interconnect at pod scale.
- TPU v5e (2023): inference-tuned, ~197 TFLOPS BF16, 393 TOPS INT8.
- TPU v5p (2023): training, ~459 TFLOPS BF16, 918 TOPS INT8 per chip.
- TPU v6e "Trillium" (2024): MXU enlarged from 128×128 to **256×256**; ~4.7× v5e per-chip compute, ~926 TFLOPS BF16, 1.8 POPS INT8.
- TPU v7 "Ironwood" (2025, public preview): inference-focused, FP8 supported, ~4.6 PFLOPS FP8/chip — Google's H100/B200 counter.

### Ecosystem 2018–2026
- **Cerebras WSE-1/2/3** — wafer-scale: WSE-3 (2024) is 46,225 mm², 4 trillion transistors at TSMC 5 nm, 900,000 cores, 44 GB on-die SRAM, 21 PB/s SRAM bandwidth, 125 PFLOPS sparse FP16.
- **Graphcore IPU** Colossus MK2 GC200 (2020) → Bow IPU (2022, wafer-on-wafer power-delivery die): 1,472 cores, 8,832 threads, 900 MB in-processor SRAM, **350 TFLOPS FP16** per Bow IPU. Stochastic rounding native.
- **Groq LPU / TSP** (2020 onward): functionally-sliced deterministic compiler-scheduled architecture. 14 nm, 25×29 mm die, 230 MB SRAM, 80 TB/s on-chip BW, 188 TFLOPS FP16 / 750 TOPS INT8. No DRAM — model sharded across chips.
- **Tesla Dojo D1** (Hot Chips 34, 2022): 7 nm, 645 mm², 50 B transistors, 354 nodes/chip, 22.6 TFLOPS FP32, ~362 TFLOPS BF16/CFloat8; 2 GHz, ~400 W. 25-chip Training Tile = 9 PFLOPS BF16, 11 GB SRAM, 36 TB/s I/O, 15 kW liquid-cooled.
- **AWS Inferentia2** (2022): 2 NeuronCores; 190 TFLOPS BF16/FP16, 380 TOPS INT8, 32 GB HBM/chip.
- **AWS Trainium2** (2024): ~667 TFLOPS dense BF16/chip, 96 GB HBM3e, ~500 W. Tensor Engine = 128×128 systolic. Trn2 UltraServer scales to 64 chips with NeuronLink.
- **Apple Neural Engine**: 8-core in A11 (2017, 0.6 TOPS); 16-core ANE in A12 (2018) onward; M4 (2024) ANE = **38 TOPS** (INT8); A18 = 35 TOPS. Hardware actually FP16-native — INT8 figure is FP16 ops × 2 by industry convention.
- **Qualcomm Hexagon NPU** — DSP heritage (HVX 1024-bit vectors) plus HMX tensor block. 32×32 FP16 tile = 2 KiB; supports INT4/8/16, FP16, FP8 (Snapdragon 8 Gen 3 onward). Snapdragon X2 Elite Extreme NPU 6 = 80 TOPS INT8.
- **Intel AMX** (Sapphire Rapids Xeon, Jan 2023): 8 tile registers TMM0–7, each 16 rows × 64 bytes (1 KiB, 8 KiB total). TMUL accelerator: BF16 and INT8 ops; ~1024 INT8 MACs/cycle/core. AVX10 / AMX-FP16 added in Granite Rapids.
- **ARM SME / SME2** (Armv9-A 2021 spec, shipping in Apple M4 2024): streaming-SVE matrix mode with a ZA tile, vector-length-agnostic outer-product instructions FMOPA / BFMOPA / SMOPA (FP32, BF16, FP16, INT8). 2024 Armv9.6 added 2:4 structured sparsity outer products and quarter-tile ops.
- **Etched Sohu** (announced June 2024, Sept 2024 funding): TSMC 4 nm, transformer-only ASIC; hardwires the transformer dataflow graph. 144 GB HBM3e/chip; claimed 500K tokens/s on Llama-70B and an 8-Sohu server replacing 160 H100s. Vendor numbers only — no shipping silicon as of late 2025.
- **Tenstorrent Wormhole** (12 nm, 2023): 80 Tensix+ cores, 292 TFLOPS FP8, 12 GB GDDR6, 16×100 GbE. Grayskull → Wormhole → **Blackhole** (6 nm, 2024): 140 Tensix++ cores, **774 TFLOPS FP8**, 32 GB GDDR6, 4× QSFP-DD 800G. RISC-V control cores (5 per Tensix), supports Block-FP2/4/8 in addition to FP8/FP16/BF16.

### 2024–2026 directions
- Block-scaled microformats (OCP MX) becoming the dominant inference numeric: NVIDIA NVFP4 (Blackwell), AMD MI355X, ARM SME2, Microsoft Maia 100.
- **Training in FP4** (NVIDIA papers Q4 2024 onward) using stochastic rounding + per-tensor scales.
- 3D stacking: SRAM-on-logic (AMD MI300X 3D V-Cache lineage; future Blackwell-NeXT rumoured).
- Optical I/O (TPU v4 OCS, Lightmatter Passage) supplanting copper SerDes between cabinets.
- Transformer-specialised silicon (Etched Sohu, MatX, Cerebras CS-3 dense KV-cache) trading generality for area efficiency.
- AMD MI300X / MI325X / MI355X (CDNA3/4, 2023–25), Intel Gaudi 3 (2024) all converge on FP8 + MXFP6/4.

---

## 2. Why MMUL exists

### Operational / arithmetic intensity
- Roofline (Williams et al. 2009): performance is min(peak FLOP/s, AI · peak BW), where **AI = FLOPs / byte of DRAM traffic**.
- Matmul of two N×N matrices: 2N³ FLOPs, 3N² bytes touched ⇒ AI ≈ 2N/3. For N=1024 that's ~683 FLOPs/byte — deeply compute-bound on any modern HBM device (~5–10 FLOPs/byte ridge).
- Compare to vector add (AI = 1/12 with FP32): firmly memory-bound. Matmul is uniquely amenable to spatial reuse.

### Energy: the actual driver
- Horowitz, ISSCC 2014, "Computing's Energy Problem" (45 nm, normalised numbers, still cited everywhere):
  - 32-bit FP MUL ≈ 3.7 pJ; 32-bit FP ADD ≈ 0.9 pJ; FP MAC ≈ 4.6 pJ.
  - 32-bit SRAM read ≈ 5 pJ; 32-bit DRAM access ≈ **640 pJ** (~200× a MAC).
- An MMUL only wins if each operand fed from DRAM is reused **O(N)** times across PEs. That reuse — temporal in registers/scratchpads, **spatial** across PEs — is the entire reason for the architecture.
- Eyeriss measurements (MIT, 2016): DRAM access = 200× the energy of a register-file read; on-chip global buffer = 6× a register read; PE-to-PE = 2×. Hence "stationary" dataflows that maximise the cheapest movement.

### Spatial reuse principle
- A 256×256 PE array sees each input activation reused 256× and each weight reused 256× per cycle without re-fetching from SRAM. That's the multiplier on which the whole AI hardware industry rides.

---

## 3. Core architectures

### Output-stationary systolic array (TPU v1 style)
- A flows L→R, B flows T→B, partial sum **stays in PE** and accumulates in place; result drains at the end.
- Used by: TPU v1 (256×256), TPU v2–v5p (128×128), Trillium (256×256), AWS Trainium/Inferentia (128×128 Tensor Engine).
- Pros: minimal accumulator movement, high utilisation on dense GEMM. Cons: fill/drain latency = 2N–1 cycles (the "fill bubble"); poor on small / non-square tiles.

### Weight-stationary
- Weights pre-loaded into PE registers and held; activations stream; partial sums flow.
- Used by: NVIDIA NVDLA reference design, most edge/mobile NPUs (Apple ANE, ARM Ethos-N), Hailo-8.
- Optimal when weights >> activations and reused across a batch — typical for inference.

### Input-stationary
- Activations pinned, weights stream, partial sums flow.
- Less common; appears in some training accelerators where activation reuse dominates (back-prop ∂L/∂W).

### Row-stationary (Eyeriss, MIT 2016)
- 1-D row of filter weights pinned in a PE; 1-D row of ifmap streams horizontally; partial sums accumulate vertically. Maximises *all three* data-type reuse simultaneously by mapping CONV's natural 1-D primitive.
- 168 PEs, 12×14 array, 108 KB GLB. Demonstrated 1.4–2.5× energy-efficiency improvement over weight-/output-stationary on AlexNet.

### Broadcast / dot-product trees (NVIDIA Tensor Core)
- Each TC is conceptually a tree of FMAs computing 4×4×4 outer-product chunks, not a cell-to-cell systolic. Operands are fetched from registers per warp, processed in 4 cycles, written back.
- Wider Tensor Cores in Hopper/Blackwell expose larger M×N×K shapes (e.g. 16×8×16 mma.sync) but the structure is parallel reduction trees, not Kung-style pumping.
- Trade-off: lower latency, more flexible shapes; higher SRAM bandwidth needed to feed the trees.

### 2D vs 3D / TMA / Tensor Memory
- 2D arrays — TPU MXU, AMX TMUL.
- 3D / cube arrays — Tesla Dojo (2D mesh of cores each running its own SIMD matrix); Cerebras WSE (effectively 2D wafer-mesh).
- **Tensor Memory Accelerator (TMA)** in H100 (Hopper, 2022): async multi-dim DMA engine that slides tiles between global memory and shared memory without warp involvement. Blackwell adds **TMEM** — 256 KB per SM of dedicated tensor-core-private memory, decoupling MMA inputs from shared memory pressure.
- Block-scaled formats (MX) require a scale broadcast network parallel to the dot-product fabric — see §4.

---

## 4. Number formats

| Format | Bits | S/E/M | Range (≈) | Used in |
|---|---|---|---|---|
| FP32 (IEEE 754 binary32) | 32 | 1/8/23 | ±3.4e38 | Baseline; accumulators everywhere |
| TF32 (NVIDIA) | 19 (stored as 32) | 1/8/10 | same as FP32 | A100+ Tensor Cores; FP32 in/out |
| FP16 (IEEE binary16) | 16 | 1/5/10 | ±65,504 | V100 onwards |
| BF16 (Google brain float) | 16 | 1/8/7 | ±3.4e38 | TPU v2+, all modern; "FP32 truncated" |
| FP8 E4M3 (OFP8) | 8 | 1/4/3 | ±448, NaN, no Inf | Hopper, MI300X — weights & activations |
| FP8 E5M2 (OFP8) | 8 | 1/5/2 | ±57,344, ±Inf, NaN | Hopper, MI300X — gradients |
| FP6 E3M2 | 6 | 1/3/2 | ±28 | OCP MX, Blackwell |
| FP6 E2M3 | 6 | 1/2/3 | ±7.5 | OCP MX, Blackwell |
| FP4 E2M1 | 4 | 1/2/1 | ±6 | OCP MX, Blackwell, AMD MI355X |
| MXFP8/6/4 | 8/6/4 + shared E8M0 scale per 32-element block | – | element format × 2^E8M0 | OCP MX v1.0 (Sept 2023) |
| NVFP4 | 16 elements share **FP8 (E4M3) scale**, plus per-tensor FP32 scale | – | per-block | Blackwell (NVIDIA-specific) |
| INT8 / INT4 | 8 / 4 | – | ±127 / ±7 | TPU v1, NPUs; quantised inference |
| Posit / LNS | varies | – | tapered precision | research only — not in shipping accelerators |

### Bit-layout details (sign / exp / mantissa, bias)
- **FP32**: bias 127. Cost reference for everything below.
- **TF32**: same exponent as FP32 (range), mantissa truncated to 10 bits before TC multiply; result accumulated in FP32. Memory footprint stays 32 bits (it's a TC compute mode, not a storage format).
- **BF16**: bias 127. Same exponent as FP32 means trivial truncation/round-to-bf16. Wide range (handles gradients) at the cost of mantissa precision (~3 decimal digits).
- **FP8 E4M3**: bias 7, no Inf, only 2 NaN encodings; e_max raised by 1 binade vs naive IEEE → max ±448. Used for forward weights/activations.
- **FP8 E5M2**: bias 15, has Inf and NaN per IEEE rules. Max ±57,344. Used for gradients (wider range tolerates outliers).
- **FP6 E3M2 / E2M3, FP4 E2M1**: defined in OCP MX spec (Sept 2023) — no Inf, simplified NaN.

### MX (Microscaling) — OCP v1.0 (Sept 2023)
- Block of **k = 32** elements share one **E8M0** scale (8-bit unsigned biased exponent, no sign / mantissa).
- Storage: MXFP8 block = 32×8 + 8 = 264 bits (33 B); MXFP6 = 32×6 + 8 = 200 bits (25 B); MXFP4 = 32×4 + 8 = 136 bits (17 B); MXINT8 = 32×8 + 8 = 264 bits.
- Scale acts as a per-block exponent → 1 bit of dynamic range expanded to ~256 binades.
- Backed by AMD, Arm, Intel, Meta, Microsoft, NVIDIA, Qualcomm.

### NVFP4 (Blackwell-specific, narrower block)
- Block size **16** (vs OCP MX's 32) → finer granularity.
- Scale type: **FP8 E4M3** (vs OCP's E8M0) → mantissa bits in the scale, much better precision near the boundary.
- Plus a per-tensor FP32 global scale.
- Largest-magnitude element in each 16-block normalised to FP4 max — improves SNR vs MXFP4 in practice (NVIDIA papers show NVFP4 within 0.5% of FP8 perplexity on Llama-class models).

### Stochastic rounding & two-stage accumulation
- **Stochastic rounding**: round up with probability proportional to fractional part. Crucial for low-precision *training* — replaces the systematic bias of round-to-nearest with zero-mean noise. Native in Graphcore IPU, supported in NVIDIA Hopper/Blackwell for FP8/FP4 paths.
- **Two-stage / Kahan accumulation**: keep a high-precision running residual; each MAC accumulates into an FP32 accumulator with periodic spill to FP64 / fixed-point. Used in TPU MXU (FP32 accum), H100 (FP32 accum from FP8 input), NVFP4 (FP32 accum).

### Posit / LNS (research)
- Posits (Gustafson 2017): regime + exp + mantissa, tapered precision near 1.0. Hardware MAC ~22–83% lower power than FP32 in research papers; no shipping AI accelerator uses them.
- Log-number system: replaces multiplies with adds. Used in some FPGA NN cores; not commercial.

---

## 5. Real-world examples (one-pager)

| Device (year) | Process | Array shape | Datatypes | Peak (dense) | On-chip mem |
|---|---|---|---|---|---|
| **Google TPU v1** (2015 / 2017 paper) | 28 nm | 256×256 systolic, output-stationary | INT8 mul, INT32 acc | **92 TOPS INT8 @ 700 MHz** | 28 MiB UB + 4 MiB acc |
| **TPU v4** (2021) | 7 nm | 4× 128×128 MXU per core, 2 cores | BF16, INT8 | 275 TFLOPS BF16 / 275 TOPS INT8 | 32 MiB CMEM, 32 GiB HBM |
| **TPU v5p** (2023) | 5 nm | 128×128 MXU | BF16, INT8 | ~459 TFLOPS BF16 | 96 GiB HBM |
| **Trillium TPU v6e** (2024) | 4 nm | **256×256 MXU** | BF16, INT8 | ~926 TFLOPS BF16 | 32 GiB HBM3 |
| **NVIDIA H100 SXM5** (2022) | TSMC 4N | 4th-gen TC, dot-product trees | FP64 / FP32 / TF32 / FP16 / BF16 / INT8 / **FP8 E4M3, E5M2** | **3958 TF FP8 sparse / 1979 dense** | 50 MB L2, 80 GB HBM3 |
| **NVIDIA B200** (2024) | TSMC 4NP, dual-die | 5th-gen TC + TMEM | + **FP6, FP4 (NVFP4 / MXFP)** | 4.5 PF FP8 / **20 PF FP4** dense | 192 GB HBM3e, 8 TB/s |
| **Apple M4 ANE** (2024) | TSMC N3E | 16-core, weight-stationary | FP16 native; INT8 ×2 by convention | 38 TOPS INT8 | shared with SoC |
| **AWS Inferentia2** (2022) | – | 2 NeuronCores, 128×128 systolic | FP16/BF16/cFP8/TF32 | 190 TF BF16 / 380 TOPS INT8 | 32 GB HBM |
| **AWS Trainium2** (2024) | 4 nm | 8 NeuronCores, systolic Tensor Engine | BF16, FP8 | ~667 TFLOPS BF16 | 96 GB HBM3e |
| **Groq LPU (TSP)** | 14 nm | functional slices, deterministic | FP16, INT8 | 188 TF FP16 / 750 TOPS INT8 | 230 MB SRAM, 80 TB/s, **no DRAM** |
| **Cerebras WSE-3** (2024) | TSMC 5 nm, wafer | 900,000 cores, mesh | FP16, BF16, FP8 | 125 PF (sparse FP16) | **44 GB on-die SRAM**, 21 PB/s |
| **Tesla Dojo D1** (2022) | TSMC 7 nm | 354 nodes/chip, 4-way SMT | FP32, BF16, CFloat8 | 22 TF FP32, ~362 TF BF16 (chip) / 9 PF/tile | 11 GB SRAM/tile |
| **Etched Sohu** (announced 2024) | TSMC 4 nm | hardwired transformer dataflow | claimed FP8 | claim: 500K tok/s Llama-70B | 144 GB HBM3e |
| **Intel AMX** (Sapphire Rapids, 2023) | Intel 7 | 8 tiles × 16×64 B (1 KiB each) | BF16, INT8; FP16 added Granite Rapids | ~1024 INT8 MACs/cyc/core | per-core |
| **ARM SME / SME2** (Apple M4, 2024) | – | streaming-SVE ZA tile, VL-agnostic | FP32, BF16, FP16, INT8; +sparsity 2024 | depends on VL | per-core |
| **Tenstorrent Wormhole n300** | 12 nm | 128 Tensix+ cores | FP8/16, BF16, FP32, BLOCKFP2/4/8, INT8/32 | 466 TF FP8 (n300) | 192 MB SRAM, 24 GB GDDR6 |
| **Tenstorrent Blackhole p150a** (2024) | 6 nm | 140 Tensix++ cores | as Wormhole + larger BFP | 774 TF FP8 | 210 MB SRAM, 32 GB GDDR6 |

---

## 6. Key tradeoffs

### Numerical (accuracy vs. power/area)
- Each halving of bit-width ≈ 4× fewer transistors per multiplier (MUL is ~quadratic in mantissa width) and ~2× lower energy per MAC.
- INT8 / FP8 inference now near-lossless via per-channel + per-tensor scaling and calibration.
- FP8 *training* requires Transformer Engine-style automatic per-tile scaling and stochastic rounding; FP4 training only viable with block-scaled MXFP4/NVFP4 plus careful loss-scaling.
- Dynamic-range collapse is the failure mode — block scales (MX) are the engineering answer.

### Area: PE size vs PE count
- TPU v1: 65,536 small 8-bit MACs, 700 MHz, 28 nm → ~330 mm².
- B200 GPU: hundreds of larger TCs at >2 GHz; trades count for clock + flexibility.
- Wafer-scale (Cerebras) goes the other way: 900k tiny cores, no off-die hop. Yield handled by core-level redundancy and routing around defects.
- Big arrays (256×256 in Trillium) maximise reuse but suffer worst on small batch / odd shapes; small arrays (4×4×4 V100 TC) under-utilise the operand-reuse argument but handle irregular shapes.

### Bandwidth: the memory wall
- Even with HBM3e (~8 TB/s on B200), a 20 PF FP4 engine has ridge AI ≈ 2,500 FLOPs/byte. Below that, you're memory-bound.
- Mitigations: KV-cache compression, FlashAttention-style fused kernels, on-die SRAM (Groq, Cerebras), wafer-on-wafer power+memory dies (Bow IPU), HBM4 (2025+), in-package SRAM.

### Utilisation: workload mapping
- Real-world Tensor Core utilisation on LLM training often 40–60% of peak — a function of tile-shape mismatch, attention quadratic, and pipeline bubbles.
- Larger tiles (Trillium 256×256) require larger inner GEMM dimensions to reach ~95% utilisation; small batches expose tail effects.
- Compiler is half the chip: TPUs lean on XLA, NVIDIA on cuBLAS/CUTLASS, Tenstorrent on TT-Metalium, Groq on its deterministic compiler.

### Tail effects
- Fill / drain bubbles in systolic arrays = 2N–1 cycles at start and end of every K-tile.
- Solution: pipelined K-loop and tile fusion (cuBLAS GEMM-K), or shorter dimensions (TPU's 128 vs Trillium's 256 trade-off).

### Reconfiguration overhead
- Switching numeric mode (e.g. FP16 ↔ FP8) on Hopper is essentially free per-kernel (compile-time selected MMA op). Switching weights between layers is the real cost — hence weight-stationary on edge, output-stationary for training.
- Etched Sohu eliminates reconfiguration entirely by hardwiring one model class — the asymptote of this trade-off.

---

## 7. Key references / further reading

### Landmark papers
- H. T. Kung & C. E. Leiserson, *Systolic Arrays (for VLSI)*, Sparse Matrix Proceedings 1978 (SIAM).
- H. T. Kung, *Why Systolic Architectures?*, **Computer (IEEE), Jan 1982**.
- N. P. Jouppi et al., *In-Datacenter Performance Analysis of a Tensor Processing Unit*, **ISCA 2017**.
- Y.-H. Chen, J. Emer, V. Sze, *Eyeriss: A Spatial Architecture for Energy-Efficient Dataflow for CNNs*, **ISCA 2016**.
- M. Horowitz, *Computing's Energy Problem (and what we can do about it)*, **ISSCC 2014**, pp. 10–14.
- S. Williams, A. Waterman, D. Patterson, *Roofline: An Insightful Visual Performance Model*, CACM 2009.
- P. Micikevicius et al., *FP8 Formats for Deep Learning*, arXiv:2209.05433 (2022) — the joint NVIDIA/Arm/Intel FP8 paper that became OFP8.
- Rouhani et al., *Microscaling Data Formats for Deep Learning*, arXiv:2310.10537 (2023).
- *OCP Microscaling Formats (MX) Specification v1.0*, Open Compute Project, Sept 2023.
- *OCP 8-bit Floating Point Specification (OFP8) Rev 1.0*, Open Compute Project, Dec 2023.
- NVIDIA, *Hopper Architecture In-Depth* whitepaper (2022); *Blackwell Architecture* whitepaper (2024).

### TPU sources
- Jouppi et al., *A Domain-Specific Supercomputer for Training Deep Neural Networks*, CACM 2020 (TPU v2/v3).
- Jouppi et al., *TPU v4: An Optically Reconfigurable Supercomputer for Machine Learning*, ISCA 2023.

### Books / surveys
- V. Sze, Y.-H. Chen, T.-J. Yang, J. Emer, *Efficient Processing of Deep Neural Networks*, Morgan & Claypool 2020 — the canonical textbook; row-stationary dataflow analysis.
- D. Patterson & J. Hennessy, *Computer Architecture: A Quantitative Approach*, 6th ed., Ch. 7 (DSAs).

---

## Key facts I confirmed (and flags)

- **"Why Systolic Architectures?" date**: the paper is **1982** (IEEE Computer), not 1979. The 1978/79 work is the earlier "Systolic Arrays (for VLSI)" Sparse Matrix Proceedings paper. Many secondary sources (incl. several listed in the user prompt) conflate them. Cite both.
- **TPU v1 array size**: 256×256 = 65,536 MACs at 700 MHz → 92 TOPS INT8 ✅ (Jouppi 2017).
- **V100 Tensor Core shape**: 4×4×4, FP16 multiply / FP16 or FP32 accumulate, 64 FMAs/clk/TC ✅.
- **Trillium MXU**: confirmed expanded from 128×128 (v2–v5p) to **256×256** in v6e — the first MXU resize since TPU v2 (2017).
- **MX block size**: **32 elements**, **E8M0 scale** (per OCP v1.0 Sept 2023). NVFP4 differs: **16-element** blocks with **FP8 E4M3** scale plus FP32 per-tensor — flag the divergence between OCP and NVIDIA-internal NVFP4.
- **FP8 E4M3 max value**: ±448 (no Inf, only 2 NaN encodings) — sources occasionally and incorrectly state ±240 or ±240·2 conflating with E5M2; the OFP8 spec is unambiguous.
- **Apple ANE TOPS**: 38 TOPS for M4 is **INT8 by convention = 19 TFLOPS FP16 × 2**; the hardware is FP16-native and does not actually run INT8 at 2× rate. Worth flagging in the slides.
- **H100 FP8 dense**: 1979 TFLOPS dense / 3958 sparse (NVIDIA datasheet); some web pages cite "989 TFLOPS" — that is the *FP16/BF16* tensor figure, not FP8. Common confusion.
- **B200 FP4 dense**: 9–10 PFLOPS per die; **20 PFLOPS dense** is the dual-die B200 package figure (40 sparse). Earlier marketing slides cited 20 PF "with sparsity" — NVIDIA's later datasheet revised to 20 PF dense / 40 PF sparse. Use the most recent datasheet number.
- **Groq die size**: 14 nm, ~725 mm² (25×29 mm), 230 MB SRAM. Confirmed 188 TF FP16 / 750 TOPS INT8.
- **Cerebras WSE-3**: 4 trillion transistors, 900,000 cores, 44 GB SRAM, 21 PB/s, **125 PFLOPS sparse FP16** (not dense — a frequent press-release mis-cite).
- **Trainium2**: 667 TFLOPS dense BF16/chip; the Tensor Engine itself is a 128×128 systolic, not a tree-style TC.
- **Etched Sohu**: all numbers are vendor-self-reported (June 2024); no third-party silicon as of late 2025. Do not present on slides as confirmed.
- **Recent (2025–26) items to add late if presenting fresh**: TPU v7 "Ironwood" (2025), AMD MI355X (CDNA4, 2025) with FP4 / MXFP6, Intel Gaudi 3 (2024), NVIDIA Blackwell Ultra B300 (announced 2025), Microsoft Maia 100 (2024).
- **2:4 sparsity nomenclature**: NVIDIA "structured sparsity" (Ampere onward); ARM SME 2024 added 1:2 and 2:4 outer-product sparsity instructions — suggests the pattern is becoming an ISA primitive across vendors.

---

*Prepared as raw research material; not a publishable article. Verify all numbers against the cited primary docs before slide-ware.*
