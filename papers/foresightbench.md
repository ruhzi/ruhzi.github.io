---
layout: paper
title: "ForesightBench: Benchmarking Research Foresight in Large Language Models"
bib_title: "{ForesightBench}: Benchmarking Research Foresight in Large Language Models"
authors: ["Ruhaan Chopra"]
bib_authors: "Chopra, Ruhaan"
bibkey: chopra2026foresightbench
date: 2026-10-20
version: v1
pdf: /papers/foresightbench.pdf
versions:
  - v: v1
    date: 2026-10-20
    pdf: /papers/foresightbench-v1.pdf
    note: Initial version.
doi: ""
permalink: /papers/foresightbench/
description: "A benchmark of 316 controlled comparisons from fresh arXiv preprints, testing whether LLMs can predict the empirical outcome of a research setup before it is run."
links:
  - name: Dataset
    url: https://huggingface.co/datasets/ruhzi/foresightbench
  - name: Benchmark repo
    url: https://github.com/ruhzi/benchmark
  - name: Pipeline
    url: https://github.com/ruhzi/autoresearchrl
  - name: Harness
    url: https://github.com/ruhzi/autoresearch-harness
abstract: >-
  Mainstream autoresearch loops largely search by brute force, which works only while the search space is small. Beyond that threshold, an agent needs research taste: a sense of which experiments will pay off before running them. To assess frontier models on their ability to forecast empirical outcomes of given research setups, we introduce ForesightBench, built by an agentic pipeline that turns recent arXiv preprints into contamination-controlled benchmark samples, and evaluate thirteen frontier models on 42 samples from papers submitted after every published training cutoff. We reveal that while most models get the direction of the prediction right, they are consistently conservative in their predictions. Error is also highest on safety and interpretability papers (0.45) and agent papers (0.42), against 0.16 on vision. We then identify three failure modes of reasoning, and discover that faulty predictions often happen when models fail to take cognizance of the entire provided experimental setup and imprudently rely on their priors. Using this as our guiding principle, we introduce a lightweight inference-time intervention which decomposes the setup into threads and sub-threads. We test this harness built on top of pi against vanilla, out-of-the-box pi, tasking them both with creating a long-horizon agent. Across two seeds, the agent built by our custom harness outperforms the one built by vanilla pi on DABstep by 8 percentage points on average.
---
