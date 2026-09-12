# X Algorithm: A Comprehensive Technical Research Report (2026 Edition)

An analytical deep-dive into the architecture, signal mechanics, engagement modeling, and algorithmic optimization framework of X's ranking system. This repository contains the complete research report based on the official `xai-org/x-algorithm` repository releases from January and May 2026.

**Read the report:** [X Algorithm Breakdown (PDF)](<X Algorithm/X_Algorithm_Breakdown.pdf>)

---

## 📌 Executive Summary
Between late 2025 and May 2026, X underwent a foundational architectural pivot, abandoning its legacy Scala-based engineering framework (comprising hundreds of hand-tuned heuristics and manual weighting tables) in favor of a modern **62.9% Rust and 37.1% Python/JAX** multi-task transformer pipeline. 

This report deconstructs how the production system processes ~500 million daily posts and executes roughly 5 billion individual ranking decisions in under 200 milliseconds per request.

---

## 🏗️ Core Architectural Modules Covered

* **Thunder (`thunder/`):** The latency-critical Rust service acting as an in-memory, Kafka-backed post store handling sub-millisecond in-network retrieval.
* **Phoenix Retrieval (`phoenix/`):** The out-of-network discovery engine operating on a two-tower transformer architecture utilizing Approximate Nearest-Neighbor (ANN) similarity search.
* **Grox (`grox/`):** The multimodal content-understanding pipeline executing Vision-Language Model (VLM) inference for asynchronous spam detection, quality scoring (`slop_score`), and brand safety.
* **Home Mixer (`home-mixer/`):** The Rust orchestration layer managing 21 parallel context hydrators, pre-scoring filters, and ad-blending injection.

---

## ⚡ Key Algorithmic Breakthroughs Analyzed

### 1. Complexity Reduction via Candidate Isolation (`make_recsys_attn_mask`)
The report details how `phoenix/grok.py` implements a specific attention masking strategy. By allowing candidates to attend to the user's interaction history and themselves, but *restricting cross-candidate attention*, the network's processing complexity drops quadratically from $O(M^2 \times L)$ to a linear $O(M \times L)$. This renders inference deterministic, highly cacheable, and ultra-fast.

### 2. Deconstruction of the Linear-Weighting Fallacy
Moving past static multiplier assumptions inherited from the 2023 `HeavyRanker` ("a reply is worth 75 likes"), the report proves the non-linear multi-task learning nature of the 15-to-19 distinct engagement probability heads optimized concurrently within the model.

### 3. Signal Mechanics & Suppression Triggers
* **Positive Drivers:** Continuous regression modeling for passive Dwell Time, High-intent DM Sharing (`dm_share`), and Video Quality View (`VQV_WEIGHT`) duration gates.
* **Negative Friction:** Aggressive suppression of external links for non-Premium accounts, hashtag inflation penalties, author diversity decay multipliers, visual unoriginality screening (`slop_score`), and the restrictive `lte_1000` follower bucket filtering reply loops.

---

## 🔬 Reproducibility Framework
The report includes an implementation breakdown of the `phoenix/run_pipeline.py` framework shipped in May 2026, evaluating how researchers can interface with the ~3 GB Git LFS pre-trained mini Phoenix model artifacts (`oss-phoenix-artifacts/`) to test custom user-action interaction sequences.

---

## 📜 Metadata & Citations
* **Author:** Emil Veliyev ([github.com/emillvl](https://github.com/emillvl))
* **Target Repository:** [xai-org/x-algorithm](https://github.com/xai-org/x-algorithm)
* **Reference Commit:** `e414c17` (May 15, 2026)
* **License:** Apache 2.0

---
*For a complete structural review, including temporal complexity matrices and adversarial open-source risk paradoxes, refer directly to the full PDF report in this repository.*
