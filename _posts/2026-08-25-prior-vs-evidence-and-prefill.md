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

Over the cycles I have spent reading these trajectories, I am arriving at a fuzzy conclusion about research taste in large language models. Crudely put, how tasteful a model is depends heavily on when it relies on its priors, and when it doesn't.

## Four definitions

<!-- Skeleton. Each definition = (a) a one-line definition in a .defn block,
     optionally with a "swap test" line, (b) one or two verbatim trace cards,
     (c) a one-sentence gloss on what the move did to the prediction.

     Trace card anatomy:
       <div class="trace lethal">          variants: prior | evidence | applicable | lethal
         <p class="trace-head"><span class="tag">lethal prior</span><span class="meta">model · sample · channel</span></p>
         <pre>verbatim excerpt</pre>
         <p class="gloss">what it did</p>
       </div>
-->

### 1 · A prior

<div class="defn" markdown="1">
<span class="term">Prior.</span> <!-- one-line definition: a proposition whose warrant comes from outside the instance -->

<span class="test">Swap test: <!-- replace this paper with any other paper using the same technique; if the justification is unchanged, it is a prior --></span>
</div>

### 2 · Evidence

<div class="defn" markdown="1">
<span class="term">Evidence.</span> <!-- one-line definition: a proposition whose warrant comes from this instance, checkable against the prompt alone -->

<span class="test">Swap test: <!-- swap the paper and the claim becomes uncheckable --></span>
</div>

### 3 · An applicable prior

An applicable prior is always bounded by evidence, i.e the model applies its prior knowledge of the domain with respect to the setup.

<div class="trace applicable">
<p class="trace-head"><span class="tag">applicable prior</span><span class="meta">claude-fable-5 · 2608.10777-s004 · reasoning</span></p>
<pre>I'm weighing whether PI-VM's short-horizon TD bootstrapping avoids the expensive adjoint ODE solve that AM requires, which could make it notably faster per training iteration—maybe in the 10-30ms range depending on task, though this</pre>
<p class="gloss">The prior <em>“removing a per-step solve saves time”</em> is licensed by the prompt's description of the baseline as adjoint matching.</p>
</div>

<!-- optional: per-slot table (truth 8 / 7 / 7 ms vs baseline 41 / 21 / 21) -->

### 4 · A lethal prior

A lethal prior is unbounded by any evidence, and while it may be technically and scientifically correct, it is applied blindly.

The prior is consumed before its evidence — and the evidence is either never surfaced, or surfaced afterward and not allowed to bind.

#### Evidence never mentioned

<div class="trace lethal">
<p class="trace-head"><span class="tag">lethal prior</span><span class="meta">deepseek-v4-pro · 2608.10777-s004 · reasoning</span></p>
<pre>if learned exactly. Time? PI-VM uses N sample trajectories, so slower. If baseline AM 41ms, PI-VM maybe 100-200ms? Need quantify.

- OU Quadratic (eas</pre>
<p class="gloss">The sampling-cost prior fires and the arithmetic runs on it (41 ms × ~2.5). The question <em>“what got removed?”</em> is never generated. Shipped <strong>98 / 48 / 48 ms</strong> against a truth of <strong>8 / 7 / 7</strong>.</p>
</div>

#### Evidence surfaced after the prior, not allowed to bind

<!-- second lethal case (probe transfer, 2608.10627-s016): a "lethal prior" card,
     then an "evidence" card from the same model's raw answer, then the gloss -->

## The rule

<!-- what separates §3 from §4 is not whether the prior is held but whether the
     licensing evidence is in the conditioning set before the prior is consumed -->

## The prefill experiment

<!-- hypothesis / design / quantitative results / qualitative analysis -->

## Threats to validity

<div class="warn" markdown="1">

</div>
