---
title: "Does the prior follow the evidence, or precede it?"

date: 2026-08-26

tags: [autoresearch, research-taste, evaluation]
toc: true
cite: true
---

<div class="note" markdown="1">
This small investigation is part of a larger research project on research taste in large language models, and benchmarking and improving downstream autoresearch capabilities.
</div>

I have been benchmarking frontier models on how well they predict empirical outcomes of ai research setups. Upon some qualitiatve readings on the reasoning trajectories of fable, sol, k3, inkling, deepseek pro and flash and grok, a very common failure mode was harmful reliance on priors. A prior is a an assumption a language model makes on the basis of its knowledge of the domain, example - "Monte-Carlo estimation costs time". For the task type, these priors can end up derailing the reasoning trace of the model and culminating in a really wrong prediction. 

Over the cycles I have spent reading these trajectories, I am arriving at a fuzzy conclusion about research taste in large language models. Crudely put, how tasteful a model is depends heavily on when it relies on its priors, and when it doesn't.



[^1]: 
