PROJECT SHIVAM 2.0
Breaking the Energy Wall in Large Language Model Inference

Author: Preet Shukla
Organization: Sivan AI Research Lab

Overview

PROJECT SHIVAM 2.0 is a systems-level research project investigating the true source of energy inefficiency in Large Language Model (LLM) inference and proposing a practical, deployable solution.

Contrary to common belief, our experiments show that arithmetic precision (INT8 / INT4) is not the dominant factor in inference energy. Instead, repeated movement of large model weights — the memory wall — dominates energy consumption.

SHIVAM 2.0 demonstrates that by reducing the number of large-model weight fetches per generated token, inference energy can be reduced by 2–3×, even on 7B-parameter models running on modern inference GPUs.

Key Result (TL;DR)

LLM inference energy is dominated by memory movement, not computation.
By amortizing weight fetches across multiple tokens using bounded speculative decoding, we reduce energy per token by 2–3× without changing model accuracy or hardware.

Motivation

Most efficiency efforts focus on:

Lower precision (INT8, INT4)

Faster kernels

More FLOPs

However, real measurements show that:

INT4 often fails to reduce total energy

Sometimes INT4 increases energy due to overhead

GPUs are increasingly memory-bound during inference

SHIVAM 2.0 asks a different question:

How often do we fetch the same weights from memory per token — and can we reduce that?

Core Insight

In standard autoregressive decoding:

The full model weights are fetched once per token

This dominates energy cost

SHIVAM 2.0 reframes inference as a memory reuse problem, not a compute problem.

Solution Approach
Memory-Aware Speculative Decoding

We use speculative decoding not as a latency trick, but as a memory-energy optimization.

A small draft model proposes multiple tokens

The large model verifies them in one forward pass

The same large weights are reused across multiple tokens

This converts:

1 weight fetch → 1 token


into:

1 weight fetch → k tokens

Experimental Validation
Stage I — Microbenchmarks (Memory Proof)

Forced reload vs reuse of large tensors

Up to ~45% energy reduction

No change in arithmetic precision

Stage II — Mid-Scale Models (1.3B)

GPU: Tesla T4

Energy per token reduced by ~3×

Confirms behavior on real transformers

Stage III — Large-Scale Models (7B)

GPU: NVIDIA L4

Model: Mistral-7B-Instruct

Energy per token reduced by ~2.2×

Confirms scalability and deployment relevance

Why Compression Alone Fails (SHIVAM 1)

Earlier work (PROJECT SHIVAM 1) showed that:

INT4 reduces arithmetic cost

But does not reduce weight fetch frequency

Dequantization and kernel overhead offset gains

Conclusion:

Compression without reducing memory movement cannot solve the energy wall.

SHIVAM 2.0 directly addresses this limitation.

Key Contributions

Experimental proof that memory movement dominates inference energy

Demonstration that precision reduction alone is insufficient

Reframing speculative decoding as an energy optimization

Validation on 7B models + inference GPUs

Identification of an optimal speculation window (k ≈ 3–5)

What This Is (and Is Not)
This IS

Systems research

Hardware-backed experimentation

Deployment-relevant optimization

Energy-first analysis

This is NOT

A new model architecture

A training optimization

A hardware proposal

A speculative theory without measurements

Practical Impact

This approach:

Requires no new hardware

Preserves correctness and accuracy

Works with existing LLM stacks

Can be adopted incrementally in production

It provides a clear path to reducing inference cost and energy for large models.

Repository Contents

Experimental notebooks

Benchmark scripts

Energy measurement code

Research documentation

This repository is intended for research, discussion, and further exploration.

Status

Active research project.
Open to feedback, critique, and collaboration.

Citation

If you reference this work, please cite as:

Preet Shukla. PROJECT SHIVAM 2.0: Breaking the Energy Wall in Large Language Model Inference. Sivan AI Research Lab, 2025.

Contact

Preet Shukla
Sivan AI Research Lab
