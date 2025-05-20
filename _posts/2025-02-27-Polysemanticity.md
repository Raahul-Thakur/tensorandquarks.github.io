---
layout: default
title: ""
date: 2025-02-27
tags: [ML]
---

# Block Geometry & Everything-Bagel Neurons: Decoding Polysemanticity  

## When Neurons Speak in Tongues: Why Polysemanticity Demands a Theory of Capacity  

Crack open a modern vision or language model and you’ll run into a curious spectacle: the **same** unit flares for “cat ears,” “striped shirts,” **and** “the Eiffel Tower.” This phenomenon—**polysemanticity**—is more than a party trick. It frustrates attribution, muddies interpretability dashboards, and complicates any safety guarantee that relies on isolating *the* “terrorism neuron” or “privacy-violation neuron.”  

<!--more-->

Scherlis et al. begin by arguing that previous explanations—random weight-space rotations, quirks of stochastic gradient descent, or mere dataset noise—describe *symptoms*, not *causes*. The real culprit, they claim, is **capacity**. Picture every embedding dimension as a square foot in a cramped city apartment; features are tenants who “pay” rent proportional to how much they lower the loss. When the building is spacious (wide layers), each tenant can live alone—monosemantic luxury. When space is scarce, the loss-minimising landlord forces roommates to bunk together, entangling unrelated features in a single dimension.  

This reframing yields two research goals:

1. **Define feature capacity** in a way that plugs cleanly into optimisation theory.  
2. **Derive quantitative predictions** (sharp phase boundaries) and **qualitative geometry** (block-semi-orthogonal embeddings).

Polysemantic neurons, then, are cast as an **inevitable budgeting hack** whenever the set of important features outnumbers cheap dimensions. That premise powers everything that follows.

---

## 2&nbsp;·&nbsp;The Math of Capacity Allocation: From Definitions to Diminishing Returns  

Section 2 (“Capacity and Superposition”) translates the apartment-sharing metaphor into algebra. Each input feature \(i\) acquires an **embedding vector** \(v_i\) in a \(d\)-dimensional pre-activation space. Its **capacity**

\[
c_i \;=\;\frac{\lVert v_i\rVert^{2}}{\sum_{j=1}^{d}\langle v_i,\;v_j\rangle^{2}}
\]

measures the fraction of a basis vector it “rents,” and obeys \(0 \le c_i \le 1\). Summing over all features gives the **hard budget**

\[
\sum_i c_i \;\le\; d.
\]

Because more capacity never *increases* loss in their setup, the network spends its budget via a Lagrange multiplier \(\lambda\). The Karush–Kuhn–Tucker conditions imply:

* If \(0 < c_i < 1\), then \(\partial \mathcal{L}/\partial c_i = \lambda\).  
* If \(c_i = 1\), its marginal curve stays above \(\lambda\).  
* If \(c_i = 0\), its marginal curve sits below \(\lambda\).

Visualised, each feature’s diminishing-returns curve rises steeply at first and flattens with capacity. The optimiser allocates slivers until all *partially funded* curves land on a common horizontal line \(\lambda\). Features whose curves never dip that low monopolise a dimension; those that never rise that high disappear; the rest share neurons—polysemanticity incarnate.  

**Data statistics matter.** High-kurtosis or highly sparse inputs flatten marginal-benefit curves quickly, inviting sharing; dense Gaussian-like inputs keep curves steep, favouring monosemantic codes.  

**Architecture matters too.** Certain activations (quadratic, tanh) satisfy the monotonic-benefit assumption exactly, whereas ReLU approximates it, partly explaining why ReLU layers look cleaner at the same width. In a sentence: Section 2 turns a fuzzy interpretability headache into a crisp *capacity-allocation problem* solved by classical optimisation.

---

## 3&nbsp;·&nbsp;The Quadratic Toy Model: Analytic Phase Diagrams and the Role of Sparsity  

To ground the theory, the authors build a **one-layer quadratic network**. Ground-truth labels come from  

\[
y \;=\; \sum_i \alpha_i x_i^{2},
\]

with independent random features \(x_i\) whose variance and kurtosis are known. The model must learn this map with only \(d\) neurons—fewer than features—so the capacity constraint bites.  

Quadratic activations square linear embeddings, letting expected MSE loss split neatly into a *correlation* term (good alignment with \(\alpha_i\)) and a *hallucination* term (spurious overlap). Sparse “spike-and-slab” inputs have large kurtosis, shrinking hallucination penalties and encouraging aggressive sharing; Gaussian inputs hurt more when features overlap, so the network prefers orthogonal monosemantic embeddings.  

Solving the optimisation yields a **phase diagram**:

| Region | Capacity Allocation | Behaviour |
|--------|--------------------|-----------|
| **A – Saturated Elite** | Top-\(k\) features claim \(c_i=1\); all others \(0\). | Purely monosemantic; minor features vanish. |
| **B – Plateau** | Mid-tier features share \(0 < c_i < 1\). | Classic polysemantic neurons mix equal-weight features. |
| **C – Abundant** | Almost every feature reaches \(c_i \approx 1\). | Superposition fades away. |

Empirical tests on synthetic data and Anthropic’s sparse autoencoders honour these boundaries, demonstrating the mechanism’s generality. Two take-aways:

* **Sparsity ↔ Polysemanticity.** Rarely active features happily overlap, so the network economises by mixing them.  
* **Importance Diversity.** Polysemanticity flourishes when several features have comparable importance; a steep importance spectrum lets elites monopolise neurons.

Even in this toy setting, some superposition is **loss-optimal** whenever representational resources are tight.

---

## 4&nbsp;·&nbsp;Block-Semi-Orthogonal Geometry: From Diagonal Luxury to Everything-Bagel Chaos  

Given optimal capacities, what geometry do weight matrices adopt? Section 4 shows every optimum decomposes into **block-semi-orthogonal embeddings**:

\[
V \;=\; \bigoplus_{b=1}^{B} Q_b S_b,
\]

where each block \(Q_b\) is orthogonal and \(S_b\) rescales feature lengths. Four archetypes emerge:

1. **Diagonal (Monosemantic).** Each feature owns a private axis.  
2. **Everything-Bagel Semi-Orthogonal.** All features crowd one giant block, interfering freely.  
3. **Mixed / Tegum Product.** Several medium blocks—interference within, orthogonality across.  
4. **Arbitrary Combination.** Any embedding rotates into a direct sum of such blocks.

Capacity can shuffle freely *inside* a block, yielding many equivalent minima—hence the “everything-bagel” flexibility. Architecture turns out to be a toggle: Quadratic > tanh > GELU > ReLU in typical block size, reflecting activation curvature and saturation.  

Practical engineering insights:

* **Orthogonality regularisers** (e.g., Björck, weight whitening) shrink blocks toward size 1, promoting monosemanticity.  
* **Sparse-coding layers** replace a giant bagel with crisp crumbs at equal parameter cost.  
* **Width tuning** relaxes capacity precisely where auditability matters—safety-critical features can live alone even while benign ones share.

Block geometry thus offers both a *diagnostic language* and a *toolbox* for steering how meaning distributes across neurons.

---

## 5&nbsp;·&nbsp;Experiments, Practitioner Take-aways, and Open Frontiers  

The final section reconnects theory with practice. Two-layer networks (ReLU, GELU, tanh, quadratic) are trained on synthetic tasks where sparsity and feature importance are dial-a-knob. Results:

* **Phase transitions**—monosemantic → polysemantic → ignored—*match theory* but smear a bit under optimisation noise.  
* **Activation-specific block spectra.** Quadratic layers spawn huge blocks; ReLU keeps them tiny, confirming geometric claims.  
* **Kurtosis drives superposition.** Holding width fixed, raising input kurtosis increases polysemanticity almost linearly.

**Key practitioner lessons**

1. **Selective width increases** untangle safety-critical features.  
2. **Data curation** (transforms that tame kurtosis) curbs unnecessary superposition.  
3. **Loss penalties** favouring orthogonality or penalising large blocks trade a small loss uptick for clearer semantics.  
4. **Capacity-aware probes** locate blind-spot domains (features with \(c_i \approx 0\)) and entanglement zones (mixed-capacity blocks).

**Open problems**

* **Depth dynamics.** How does capacity propagate or re-allocate across layers?  
* **Optimisation paths.** Does SGD march directly toward capacity-optimal blocks or loiter in messy rotations?  
* **Correlated features.** Extending capacity definitions beyond independent features is fertile ground, intersecting disentanglement research.

Polysemantic neurons, in sum, are a *budget decision*, not an accident. By quantifying capacity and mapping block geometry, Scherlis et al. turn spooky superposition into a predictable, steerable property—handing interpretability researchers a fresh compass.

---

### Further Reading  

Original paper: **“Polysemanticity and Capacity in Neural Networks”** — <https://arxiv.org/abs/2210.01892>
