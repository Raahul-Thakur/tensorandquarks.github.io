---
layout: default
title: ""
date: 2025-10-16
tags: [ML]
mathjax: true
---

# From Intuition to Axiom: The Story of an AI That Learned to Prove  

Mathematics has always been the ultimate test of structured reasoning. While large language models (LLMs) like GPT-4 or DeepSeek can reason through complex text, they often stumble where human mathematicians shine — constructing airtight, verifiable proofs.  

<!--more-->

In 2025, researchers at ByteDance introduced **Seed-Prover**, a system that finally bridged this gap. It proved **five out of six problems** at the **International Mathematical Olympiad (IMO 2025)** and achieved state-of-the-art results on nearly every major formal mathematics benchmark.  

At its heart lies a simple idea: combine **the formal precision of Lean**, a proof-verification language, with **the broad reasoning abilities of modern LLMs**. Where previous models relied on intuition-like text reasoning, Seed-Prover reasons *formally* — step by step, lemma by lemma — and lets the Lean compiler serve as the final judge.  

Accompanied by its geometry counterpart **Seed-Geometry**, which fills Lean’s long-standing geometric gap, the system represents a new frontier where artificial intelligence doesn’t just *approximate truth* — it **proves it**.  

## **Introduction**

The evolution of AI reasoning has mirrored the progression of mathematics itself — from intuition to formality. Natural language reasoning gave LLMs a remarkable fluency in solving equations or explaining logic, but it lacked **verification**.  

In mathematics, verification is non-negotiable. Every claim must be provably true. This is where **formal languages** like **Lean** come in. In Lean, proofs aren’t essays — they are **programs** that can be compiled, tested, and certified.  

Previous theorem-proving AIs fell into two camps:  

<table style="width:100%; border-collapse: collapse; text-align: left;">
  <thead>
    <tr style="border-bottom: 2px solid #ccc;">
      <th style="padding: 8px; border: 1px solid #ddd;">Type</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Approach</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Limitation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>Step-Level Provers</strong></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Generate Lean proofs line-by-line</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Too slow, often fail mid-proof</td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>Whole-Proof Provers</strong></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Generate the entire proof in one go</td>
      <td style="padding: 8px; border: 1px solid #ddd;">No feedback during proof</td>
    </tr>
  </tbody>
</table>

**Seed-Prover** merges both. It generates entire proofs but refines them *iteratively* using **Lean’s compiler feedback** — essentially letting the AI “debug” its own proofs.  

The system introduces three defining features:  
1. **Lemma-Style Reasoning** — the model builds smaller provable components first, then uses them to prove the main theorem.  
2. **Iterative Refinement** — continuous feedback loops between model and compiler.  
3. **Test-Time Scaling** — three reasoning tiers (light, medium, heavy) balancing speed and depth.  

The result? A model that not only outperforms its predecessors — **DeepSeek-Prover-V2**, **Kimina-Prover**, and **Goedel-Prover** — but one that begins to **mirror human mathematical strategy**.

## **Approach**

At its core, Seed-Prover builds upon two complementary systems:  

- **Seed-Geometry** — a fast, neuro-symbolic engine for geometric reasoning.  
- **Seed-Prover** — a large formal reasoning model built on Lean 4.  

### **The Reinforcement Loop**

Training formal provers is tricky because there’s no “partial credit.” Either a proof compiles or it doesn’t. Seed-Prover uses a reinforcement learning loop based on **VAPO (Verified Automatic Proof Optimization)** where the reward is:

{% raw %}
$$
R = 
\begin{cases}
1, & \text{if Lean verifies the proof;} \\
0, & \text{otherwise.}
\end{cases}
$$
{% endraw %}

The model updates its parameters to maximize the probability of producing compilable proofs:

{% raw %}
$$
\mathcal{L} = -\sum_i R_i \log P_\theta(a_i | s_i)
$$
{% endraw %}

It learns through millions of trials — generating, failing, fixing, and refining.

### **Three Inference Modes**

<table style="width:100%; border-collapse: collapse; text-align: left;">
  <thead>
    <tr style="border-bottom: 2px solid #ccc;">
      <th style="padding: 8px; border: 1px solid #ddd;">Mode</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Description</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Duration</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>Light</strong></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Quick iterative fixes using compiler feedback</td>
      <td style="padding: 8px; border: 1px solid #ddd;">~1 hour</td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>Medium</strong></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Nested lemma refinement</td>
      <td style="padding: 8px; border: 1px solid #ddd;">~6–12 hours</td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>Heavy</strong></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Thousands of conjectures explored in parallel</td>
      <td style="padding: 8px; border: 1px solid #ddd;">2–3 days</td>
    </tr>
  </tbody>
</table>

These tiers allow Seed-Prover to adjust its reasoning depth like a human — skimming for easy problems or diving deep into multi-day proofs.

## **Seed-Geometry**

Geometry has long been the Achilles’ heel of automated provers. Unlike algebraic manipulation, geometry demands **spatial construction and inference**.  

**Seed-Geometry** addresses this through a dual-architecture approach: a **neural proposal model** that imagines possible constructions and a **symbolic reasoning engine** that verifies them.  

### **Extended Domain-Specific Language**

Instead of step-by-step ruler-and-compass moves, Seed-Geometry uses **composite geometric actions** such as:

{% raw %}
$$
\text{IsogonalConjugate}(P, \triangle ABC), \quad \text{ExsimilitudeCenter}(O_1, O_2)
$$
{% endraw %}

These high-level abstractions drastically shorten proof chains and make reasoning interpretable.

### **Performance Highlights**

<table style="width:100%; border-collapse: collapse; text-align: left;">
  <thead>
    <tr style="border-bottom: 2px solid #ccc;">
      <th style="padding: 8px; border: 1px solid #ddd;">Benchmark</th>
      <th style="padding: 8px; border: 1px solid #ddd;">AlphaGeometry 2</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Seed-Geometry</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;">IMO-AG-50</td>
      <td style="padding: 8px; border: 1px solid #ddd;">42/50</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>43/50</strong></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;">IMO Shortlist (2000–2022)</td>
      <td style="padding: 8px; border: 1px solid #ddd;">19/39</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>22/39</strong></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;">IMO 2025 P2</td>
      <td style="padding: 8px; border: 1px solid #ddd;">—</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>Solved in 2 s</strong></td>
    </tr>
  </tbody>
</table>

By rewriting its backend in **C++**, the team achieved a **100× speedup** over prior Python implementations. The engine performs forward-chaining until closure:

{% raw %}
$$
F_{t+1} = F_t \cup \{ r(f) \mid f \in F_t, r \in \mathcal{R} \}
$$
{% endraw %}

This allows complete derivation of geometric relationships before final verification.

## **Seed-Prover**

While Seed-Geometry handles spatial problems, Seed-Prover dominates algebraic and combinatorial domains.  

Its defining characteristic is **lemma-style proving** — building proofs hierarchically. The model generates intermediate lemmas, proves them independently, then uses them to complete the final theorem.

```lean
lemma L1 : a^2 ≥ 0 := by ring
lemma L2 : a^2 + b^2 ≥ 2ab := by nlinarith
theorem main : (a - b)^2 ≥ 0 := by linarith [L1, L2]

Each lemma is stored in a **lemma pool** with metadata on difficulty, dependencies, and success rate. Failed lemmas are retried or reformulated.
Over time, this builds a reusable knowledge base — much like a mathematician’s notebook.
```

## **Test-Time Scaling**

<table style="width:100%; border-collapse: collapse; text-align: left;">
  <thead>
    <tr style="border-bottom: 2px solid #ccc;">
      <th style="padding: 8px; border: 1px solid #ddd;">Setting</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Focus</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>Light</strong></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Syntax correction</td>
      <td style="padding: 8px; border: 1px solid #ddd;">IMO 2022 P2</td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>Medium</strong></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Nested refinement</td>
      <td style="padding: 8px; border: 1px solid #ddd;">IMO 2025 P5</td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>Heavy</strong></td>
      <td style="padding: 8px; border: 1px solid #ddd;">Massive conjecture expansion</td>
      <td style="padding: 8px; border: 1px solid #ddd;">IMO 2025 P3–P4</td>
    </tr>
  </tbody>
</table>

Under the heavy setting, **Seed-Prover** can maintain **thousands of active conjectures**, refining them iteratively over days.  
The final output can span **over 1000 lines of Lean code**, each line verified by the compiler.

## **Evaluation**

The system was tested across the world’s toughest mathematical benchmarks.

<table style="width:100%; border-collapse: collapse; text-align: left;">
  <thead>
    <tr style="border-bottom: 2px solid #ccc;">
      <th style="padding: 8px; border: 1px solid #ddd;">Dataset</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Metric</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Previous SOTA</th>
      <th style="padding: 8px; border: 1px solid #ddd;">Seed-Prover</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;">IMO 2025</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Problems Solved</td>
      <td style="padding: 8px; border: 1px solid #ddd;">3 (AlphaProof)</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>5 / 6</strong></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;">Past IMO</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Success Rate</td>
      <td style="padding: 8px; border: 1px solid #ddd;">—</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>78.1 %</strong></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;">MiniF2F</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Valid/Test</td>
      <td style="padding: 8px; border: 1px solid #ddd;">92.2 %</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>99.6 %</strong></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;">PutnamBench</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Problems Solved</td>
      <td style="padding: 8px; border: 1px solid #ddd;">86</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>331 / 657</strong></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;">CombiBench</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Accuracy</td>
      <td style="padding: 8px; border: 1px solid #ddd;">10 %</td>
      <td style="padding: 8px; border: 1px; border: 1px solid #ddd;"><strong>30 %</strong></td>
    </tr>
    <tr>
      <td style="padding: 8px; border: 1px solid #ddd;">MiniCTX-v2</td>
      <td style="padding: 8px; border: 1px solid #ddd;">Accuracy</td>
      <td style="padding: 8px; border: 1px solid #ddd;">44.3 %</td>
      <td style="padding: 8px; border: 1px solid #ddd;"><strong>81.8 %</strong></td>
    </tr>
  </tbody>
</table>

**Seed-Geometry** complements these achievements with **43 / 50** on IMO-AG-50 and **22 / 39** on shortlist problems.

Together, the systems demonstrate both **depth** and **generalization** — solving Olympiad-level puzzles while handling abstract research-grade proofs.

The result is clear: reinforcement learning guided by **formal verification** is more effective than imitation learning or natural-language reasoning.  
Seed-Prover doesn’t just *guess*; it *knows*.

## **Conclusion**

**Seed-Prover** and **Seed-Geometry** together represent a paradigm shift — from **statistical reasoning** to **formal cognition**.  
They mark the first time a single AI system has achieved near-complete coverage of Olympiad-level mathematics with **provable correctness**.

### **Key Takeaways**
- **Formal supervision** provides deterministic rewards for reinforcement learning.  
- **Lemma modularity** enables compositional reasoning and knowledge reuse.  
- **Iterative refinement** allows dynamic correction beyond fixed token limits.  
- **Multi-tier inference** balances computational depth with flexibility.  

In proving **5 of 6 IMO 2025 problems**, Seed-Prover didn’t just pass a test — it demonstrated a new kind of machine understanding.  

If **AlphaGo** taught AI to master *games*, Seed-Prover teaches it to master *logic*.  
The next challenge? Applying this framework to **open conjectures**, **mathematical research**, and even **physics proofs**.


### **Original Paper**

📄 [*Seed-Prover: Deep and Broad Reasoning for Automated Theorem Proving (ByteDance Seed AI4Math, 2025)*](https://arxiv.org/abs/2507.23726)
