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

I have been benchmarking frontier models on how well they predict empirical outcomes of AI research setups. Upon some qualitative readings on the reasoning trajectories of Fable, Sol, K3, Inkling, DeepSeek Pro, Flash and Grok, a very common failure mode that emerged was harmful reliance on priors. A _prior_ is an assumption a language model makes on the basis of its knowledge of the domain, for example:

> *Monte-Carlo estimation is expensive.*

For the task type, these priors can end up derailing the reasoning trace of the model and culminating in a really wrong prediction.

Over the cycles I have spent reading these trajectories, I am arriving at a fuzzy conclusion about research taste in large language models. Crudely put, how tasteful a model is depends heavily on when it relies on its priors, and when it doesn't. \

A _prior_ is an assumption a language model makes based on its knowledge of the domain.


<div class="method" markdown="1">
**A note on the sample.** The traces below come from one training sample built from *Path Integral Value Matching for Linear Quadratic Stochastic Optimal Control* ([arXiv 2608.10777](https://arxiv.org/abs/2608.10777)). I rewrote the paper's method section in the first person with all results removed, gave the model the adjoint-matching (AM) baseline's numbers, and asked it to predict PI-VM's control L2 error and the paper's reported "Time" (in ms) on the three unimodal OU tasks. The paper does not define "Time" beyond the table header; from context it is per-iteration runtime on a single RTX 4060, and the 8 vs 41 ms gap reflects PI-VM dropping the baseline's forward simulation and backpropagation through the dynamics.
</div>

### An applicable prior

An applicable prior is always bounded by evidence, i.e the model applies its prior knowledge of the domain with respect to the setup.

<div class="trace applicable">
<p class="trace-head"><span class="tag">applicable prior</span><span class="meta">claude-fable-5 · 2608.10777-s004 · reasoning</span></p>
<pre>I'm weighing whether PI-VM's short-horizon TD bootstrapping avoids the expensive adjoint ODE solve that AM requires, which could make it notably faster per training iteration—maybe in the 10-30ms range depending on task, though this</pre>
<p class="gloss">The prior <em>“removing a per-step solve saves time”</em> is licensed by the prompt's description of the baseline as adjoint matching.</p>
</div>

<span class="score-tag"><b>claude-fable-5</b> · mean sample error <b>0.94</b> · field median 0.94</span>

Fable's prediction was very close to the target values, because it bound its prior by the fact that the baseline method (adjoin matching) requires simulating the controlled SDE forward and then solving an adjoint ODE backward through the whole trajectory (plus backprop through the simulator) on every training step. 

### A lethal prior

A lethal prior is unbounded by any evidence, and while it may be technically and scientifically correct, it is applied blindly.

The prior is consumed before its evidence — and the evidence is either never surfaced, or surfaced afterward and not allowed to bind.

<div class="trace lethal">
<p class="trace-head"><span class="tag">lethal prior</span><span class="meta">deepseek-v4-pro · 2608.10777-s004 · reasoning</span></p>
<pre>if learned exactly. Time? PI-VM uses N sample trajectories, so slower. If baseline AM 41ms, PI-VM maybe 100-200ms? Need quantify.

- OU Quadratic (eas</pre>
<p class="gloss">The sampling-cost prior fires and the arithmetic runs on it (41 ms × ~2.5). The question <em>“what got removed?”</em> is never generated. Shipped <strong>98 / 48 / 48 ms</strong> against a truth of <strong>8 / 7 / 7</strong>.</p>
</div>

<span class="score-tag"><b>deepseek-v4-pro</b> · mean sample error <b>3.34</b> · field median 0.94</span>

Deepseek-v4-pro never took cognizance of the expensive adjoint matching baseline, and instead relied on its prior in an unbounded (lethal) manner and ended up being very off.

Neither prior is false. MC sampling does cost compute; AM's adjoint solve is expensive. But the setup only allows for one of these to be consumed in a justified manner.

## The Prefill Experiment 

To test the effect that a scientifically valid but inapplicable prior has on the reasoning trajectory, I ran 3 forward passes on Qwen3.5-4B per rollout, one prefilled with a prior, one prefilled with the setup reinstated, and one prefilled with nothing. 

<div class="method" markdown="1">
**A note on the sample.** This trace comes from a training sample built from *EgoHieraLoc: A Cortically Inspired Hierarchical Segmentation-Guided Framework for Egocentric Visual Query Localization* ([arXiv 2608.09656](https://arxiv.org/abs/2608.09656)). The task is 3D object localization from first-person video: several camera frames each vote on where an object is, and the votes are averaged with confidence weights. The paper's baseline weights each frame only by how semantically confident it is. The intervention adds one more multiplicative weight — a *reprojection check* that down-weights a frame if projecting the consensus 3D point back into that frame lands far from where the object was actually seen. I gave the model the baseline's five numbers on the Ego4D-VQ3D validation set (Succ. 81.42%, Succ.* 96.93%, L2 1.37 m, Ang. 0.60 rad, QwP 84.48%) and asked it to predict all five after the geometric weight is added. Truth: small gains on four (81.82 / 98.11 / 1.31 / 0.57) and no change on QwP.
</div>

The three prefills, each opening the model's <code>&lt;think&gt;</code> block:

<div class="prefills">
<div class="prefill neutral"><span class="ptag">neutral</span><p>(no prefill — the model begins its own reasoning)</p></div>
<div class="prefill prior"><span class="ptag">prior_only</span><p>Reprojection-consistency weighting is a well-founded geometric constraint -- multiplicatively down-weighting frames whose back-projection disagrees with the consensus suppresses outlier views -- so adding it should improve localization.</p><p class="pcite">Supported by: Black &amp; Rangarajan, <a href="https://doi.org/10.1007/BF00131148">“On the unification of line processes, outlier rejection, and robust statistics with applications in early vision,”</a> IJCV 19(1), 1996 · Mai et al., <a href="https://arxiv.org/abs/2212.06969">“EgoLoc: Revisiting 3D object localization from egocentric videos with visual queries,”</a> ICCV 2023.</p></div>
<div class="prefill evidence"><span class="ptag">evidence_only</span><p>To restate the setup: the tolerance is ε_r = max(10, 0.02 × bbox_diag), typically 10--25 pixels, and the new weight multiplies into the already-full semantic weight, clamped at ε_w.</p></div>
</div>

<figure class="wide">
  <img src="/assets/img/prefill-attention-s040.png" alt="Attention mass over normalized position for the s040 sample under the neutral, prior_only and evidence_only arms">
  <figcaption><strong>Figure 1.</strong> Attention paid to the prefix sentence (solid: orange = prior, green = evidence) and to the prompt's own evidence facts (dashed blue) as the trace extends, s040, four rollouts overlaid per panel.</figcaption>
</figure>

### Results (s040, four rollouts per arm)

| arm | mean err | mean length (tok) | min / max length | closed think | parsed |
|---|---:|---:|:---:|:---:|:---:|
| `neutral` | 0.026 | 1,582 | 1,340 / 1,802 | 4/4 | 4/4 |
| `prior_only` | 0.028 | 860 | 737 / 919 | 4/4 | 4/4 |
| `evidence_only` | **0.018** | 4,426 | 3,890 / 4,934 | 4/4 | 4/4 |
