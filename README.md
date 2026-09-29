# Models Engine: Continuous-Time Neuromorphic Intelligence Without Backpropagation

### Open Empirical Research Whitepaper & Diagnostic Telemetry

> **Status:** Active Research & Commercial Development  
> **Author / Lead Architect:** Andrii Shumko (with the assistance of autonomous AI agents)  
> **Substrate:** Pure Rust, Single CPU Core, Zero GPUs, Zero Matrices, Zero PyTorch  
> **Key Benchmark:** 91-token continuous-time natural language sequence prediction (`data/bukvar.txt`, 13 discrete categories)  
> **Peak Performance:** **+84.80% L1 advantage** over the optimal constant median baseline (0.1667) on a single CPU core without backpropagation  

---

## Executive Overview

This repository contains the complete, unredacted scientific audit, architectural specifications, diagnostic telemetry logs, and commit-by-commit research notes for our autonomous **continuous-time neuromorphic AI engine**.

Unlike standard deep learning models which rely on static weight matrices and global Backpropagation Through Time (BPTT), this engine operates on biological principles:
1. **Zero Backpropagation / Zero Tensors:** Strictly local delta-rule plasticity and spike-timing traces. No weight transport, no matrix multiplication.
2. **Biological Physical Delays:** Signal transmission traverses heterogeneous axonal delays ($\tau \in [1, 50]$ ticks), enabling rich spatio-temporal attractor dynamics.
3. **Autonomous Morphogenesis & Autophagy:** Neurons (somas) and micro-columns sprout and resorb dynamically based on metabolic energy budgets and local prediction error.
4. **Single-Core CPU Efficiency:** The entire continuous-time simulation runs at **500 to 1,000 ticks per second on a single ordinary consumer CPU core**.


> [!NOTE]
> ### Language & Community Translations
> The primary empirical audit log (`HARDCODE_AUDIT_NETWORK_CONSTRUCTION.md`) was authored in **Ukrainian** by our research team during live lab experiments. To preserve the unedited historical integrity, mathematical exactness, and raw thought process of our experimental notes, we publish it in its original language.
> 
> The executive summary article (**[`DEV_TO_ARTICLE.md`](DEV_TO_ARTICLE.md)**) and repository architecture overview are fully written in **English**. 
> 
> If you are interested in translating sections or technical reports into other languages, **community Pull Requests are warmly welcomed!**

---

## Repository Structure & Documents

This open research repository serves as a verifiable, reproducible whitepaper for our findings:

| Document | Description |
|---|---|
| **[`DEV_TO_ARTICLE.md`](DEV_TO_ARTICLE.md)** | **Executive Summary Article:** *"Why Nature Grows Brains from Embryos — The Failure of Forced Scale in Neuromorphic AI"*. High-level narrative of our breakthrough, the hyperplasia paradox, and the $do(N)$ causal test. |
| **[`HARDCODE_AUDIT_NETWORK_CONSTRUCTION.md`](HARDCODE_AUDIT_NETWORK_CONSTRUCTION.md)** | **Master Empirical Audit (6,500+ lines):** Complete step-by-step experimental record from Section 1 to Section 84. Contains loss-surface probes, gradient additivity & coherence measurements, metabolic budgets, and the 89-run correlation meta-analysis. |
| **[`RESEARCH_COMMIT_HISTORY.md`](RESEARCH_COMMIT_HISTORY.md)** | **Chronological Commit Changelog:** The raw, unedited experimental notes, hypothesis pre-registrations, and measurement verdicts logged by the autonomous engineering agent at every step of development. |
| **[`ZERO_HARDCODE_CONSTITUTION.md`](ZERO_HARDCODE_CONSTITUTION.md)** | **Foundational Constitution:** The rigorous rules prohibiting arbitrary heuristics, synthetic task priors, or hardcoded dataset knowledge. |
| **[`ENGINE_SYSTEM_ARCHITECTURE_BLUEPRINT.md`](ENGINE_SYSTEM_ARCHITECTURE_BLUEPRINT.md)** | **Architecture Blueprint:** Detailed system design of the continuous-time spiking graph, event queues, and synaptic buffers. |
| **[`BIOLOGICAL_MORPHOGENIC_NEURAL_ENGINE_SPECIFICATION.md`](BIOLOGICAL_MORPHOGENIC_NEURAL_ENGINE_SPECIFICATION.md)** | **Biological Specification:** Biophysical equations governing somatic integration, dynamic thresholds, and micro-column growth drives. |
| **[`AUTONOMOUS_SELF_SURGERY_AND_METABOLIC_PROTOCOL.md`](AUTONOMOUS_SELF_SURGERY_AND_METABOLIC_PROTOCOL.md)** | **Metabolic Self-Surgery Protocol:** Principles of tissue turnover, energy accounting, and candidate connection stabilization. |
| **[`DYNAMIC_ECOSYSTEM_TRAINING_PROTOCOL.md`](DYNAMIC_ECOSYSTEM_TRAINING_PROTOCOL.md)** | **Dynamic Ecosystem Training & Budding:** Multi-strategy elite pooling, competence milestones, and asynchronous core re-allocation across CPU threads. |
| **[`PURE_COMPUTATIONAL_SUBSTRATE_AND_SAFETY_BARRIER.md`](PURE_COMPUTATIONAL_SUBSTRATE_AND_SAFETY_BARRIER.md)** | **Safety Barrier & Purity:** Boundaries ensuring the computational substrate remains strictly domain-agnostic. |
| **[`telemetry/`](telemetry/)** | **Raw Execution Logs:** Unedited run transcripts (`S77_s7.txt`, `S83b_summary.txt`, etc.) with per-tick errors, energy sheets, and streak counters across independent random seeds. |
| **[`data/bukvar.txt`](data/bukvar.txt)** | **Benchmark Sequence:** The 91-character cyclic natural language dataset used across all runs. |

---

## Key Breakthroughs & Milestones

1. **Resolution of Gradient Interference (§77):**  
   Diagnostic probes revealed an 11:1 destructive self-cancellation ratio on 1D scalar inputs (gradient coherence $C = 9.5\%$). Replacing 1D scalar encoding with **orthogonal sensory population coding** surged coherence to $61.4\%$ and lifted 9-seed median performance from $+0.00\%$ to **$+67.60\%$**, with **Seed 7 hitting +84.80%** (23 consecutive characters predicted error-free).
2. **Refutation of the "Cancer / Hyperplasia" Hypothesis (§83a):**  
   Meta-analysis across 89 independent historical runs demonstrated that body size correlates positively with intelligence at **$r = +0.882$ ($p < 10^{-10}$)**. Large recurrent somas are not pathological tumors; they are necessary reservoir capacity.
3. **Falsification of Forced Scale & The Ontogenetic Imperative (§83b):**  
   A pre-registered causal intervention test ($do(N=60)$) proved that pre-allocating an adult brain at tick zero causes an catastrophic spike-noise avalanche, cutting the score of our champion seed in half (+84.8% $\to$ +42.3%). Neuromorphic systems **must grow from an embryo**, anchoring new columns into an already-stabilized functional core.
4. **Multi-Core Darwinian Clutch Selection (§84):**  
   Rather than forcing a single embryo to succeed, multi-core CPUs incubate a clutch ($K=3..4$ embryos) with varying exploration rates ($\eta$). Natural selection guarantees capturing the >85% attractor deterministically.

---

## Substrate Architecture & Collaboration

To protect the underlying runtime implementation for future edge computing, robotics, and low-power embedded deployment, the core Rust computational engine substrate (`src/`) remains **closed-source and proprietary**.

### Evaluation Access & Collaboration
If you are an AI researcher, roboticist, or systems engineer working on continuous-time models, edge computing, or neuromorphic dynamics:
* We are happy to provide pre-compiled engine binaries for independent benchmarking.
* We are open to research discussions and exploratory hardware pilots.

Feel free to connect or reach out directly at: [andrii@feedo.ink](mailto:andrii@feedo.ink) or open a discussion in GitHub Issues.
