---
layout: default
title: ""
date: 2025-10-31
tags: [ML]
mathjax: true
---


# When AI Stopped Talking and Started Thinking  

Reasoning — the holy grail of AI — has long been the gap between memorization and actual understanding. Most modern large language models (LLMs) simulate reasoning through **Chain-of-Thought (CoT)** prompting, a clever trick where the model narrates its logic step-by-step. Unfortunately, this is more like a magician describing the illusion rather than truly performing it. CoT depends on verbose linguistic scaffolding, brittle decomposition, and mountains of training data. If you drop one logical domino, the entire thought process collapses.

<!--more-->

The **Hierarchical Reasoning Model (HRM)** proposes a brain-inspired alternative that performs reasoning *internally* rather than through words. It introduces two interacting recurrent modules: a **high-level “think slowly” module** that plans abstractly, and a **low-level “act fast” module** that performs detailed computation. They communicate like an executive and an intern — the executive decides “what” to do, the intern rapidly iterates on “how.” Once the intern stabilizes (reaches equilibrium), the executive updates its strategy and the process repeats. Voilà — hierarchy, temporal separation, and feedback loops, just like the human cortex.

The magic lies in the architecture’s **computational depth without instability**. Traditional Transformers have a fixed number of layers; HRM gains virtual depth via cycles of interaction between its modules. With only 27 million parameters and 1,000 training samples, HRM achieves near-perfect performance on logic-heavy benchmarks such as Sudoku-Extreme and 30×30 Maze puzzles — tasks where even 175 M-parameter Transformers fail miserably. On the **ARC-AGI benchmark** (meant to test abstraction and reasoning), HRM outperforms Claude 3.7, DeepSeek R1, and o3-mini-high, all of which are vastly larger and pre-trained.

HRM trains from scratch, without CoT supervision or gigantic datasets. It also avoids **Backpropagation Through Time (BPTT)** — the memory-hungry villain of recurrent models — using a one-step gradient approximation that keeps training efficient and biologically plausible.  

In short: HRM doesn’t “think out loud,” it *thinks in silence.* And somehow, this quiet thinker beats the loudest transformers at their own reasoning games.

## Introduction

Deep learning’s story has always been: “add more layers, get smarter.” Yet LLMs — our crown jewels — are surprisingly *shallow* in computational terms. Their fixed-depth Transformers can only perform constant-time transformations; they can’t simulate algorithmic processes that require multiple dependent steps. In computational-complexity lingo, Transformers sit somewhere in the “AC⁰” club — fast but fundamentally shallow. If intelligence were an onion, LLMs are currently slicing only the outer layer.

This depth problem explains why LLMs struggle with tasks requiring **deliberate reasoning** — Sudoku, path-finding, symbolic logic — even when scaled up. Increasing width (more neurons per layer) barely helps. Increasing depth (more layers) leads to vanishing gradients and unstable training.  

Enter **Chain-of-Thought** reasoning, the band-aid solution: get the model to *write down* its thinking. While it looks impressive (“let’s break this down step-by-step”), CoT relies on textual decomposition crafted by humans. One incorrect step derails everything. Worse, it’s inefficient: each reasoning token costs compute, and complex puzzles balloon into hundreds of tokens of self-talk.

The authors propose **latent reasoning** — computation done inside hidden states rather than external language. The idea: thought doesn’t need to be narrated. The brain doesn’t spell out every neuron’s activity in Morse code; it computes silently in a high-dimensional latent space. However, building models that can do this has been hard. Deep recurrent networks suffer from early convergence (they stop updating after a few steps) and require backpropagation-through-time, which is biologically implausible and GPU-unfriendly.

The **Hierarchical Reasoning Model** fixes these issues by taking cues from the brain’s architecture. The cortex operates in **hierarchies and timescales**: slow-changing regions (like the prefrontal cortex) manage goals and context, while fast ones (like sensory areas) perform quick computations. HRM mimics this with two recurrent modules — high-level and low-level — that alternate updates. The low-level computes multiple steps before the high-level intervenes, achieving “hierarchical convergence.” This generates *effective depth* far beyond the number of parameters.

The model also introduces a **one-step gradient approximation**, removing the heavy memory burden of BPTT. Training becomes stable, scalable, and almost… zen-like. With these innovations, HRM learns algorithmic reasoning from scratch, solving tasks like Sudoku-Extreme and ARC puzzles with tiny data and zero pre-training. The message is simple but profound: *intelligence may not need more data; it needs better structure.*

## The Hierarchical Reasoning Model

The HRM formalizes three neuroscience principles — hierarchy, temporal separation, and recurrence — into a computational framework. The system has four networks:

1. **Input encoder** \(f_I(x)\) converts input \(x\) into a latent representation \(\tilde{x}\).  
2. **Low-level recurrent module** \(f_L\) processes fast dynamics.  
3. **High-level recurrent module** \(f_H\) governs slower abstract reasoning.  
4. **Output head** \(f_O\) produces predictions \(\hat{y}\).

The model evolves through \(N\) high-level cycles, each containing \(T\) low-level steps. During a cycle, the low-level state \(z_L\) updates repeatedly using its previous state, the fixed high-level state \(z_H\), and the input. Only after those \(T\) micro-steps does \(z_H\) update using the final \(z_L\). After all cycles, the output is \(f_O(z_H^{NT})\).

This structure creates **hierarchical convergence.** Regular RNNs quickly fall asleep — their hidden states converge too fast, leading to zero gradient flow. HRM keeps things lively: the low-level module converges locally, the high-level resets its context, and the low-level wakes up again for a new round of computation. This nested rhythm — think of it as “fast thoughts nested inside slower meta-thoughts” — sustains long reasoning chains without instability.

Training such a model without BPTT requires mathematical sorcery. The authors borrow from **Deep Equilibrium Models (DEQ)**: if a recurrent system converges to a fixed point \(z^*\), gradients can be computed implicitly using the **Implicit Function Theorem**:

{% raw %}
$$
\frac{\partial z^*}{\partial \theta} = (I - J_F)^{-1} \frac{\partial F}{\partial \theta}
$$
{% endraw %}

Since inverting \((I - J_F)\) is expensive, HRM approximates it by keeping only the first term of the Neumann series — the famous “1-step gradient.” The result: constant-memory backpropagation that’s efficient and biologically plausible (because the brain doesn’t store gigabytes of past states either).

HRM also includes two elegant add-ons:
- **Deep Supervision:** After each reasoning “segment,” the model makes an intermediate prediction, gets feedback, and detaches the hidden state (like taking notes but not overthinking the last step). This stabilizes training.
- **Adaptive Computation Time (ACT):** A mini Q-learning head decides when to halt reasoning. It learns a policy: keep thinking if uncertain, stop if confident. In practice, the model “thinks fast and slow” — short reasoning for easy problems, longer for hard ones. Average compute stays low, accuracy high.

Architecturally, both modules are Transformer blocks (with RMSNorm, rotary embeddings, GLU activations), but the recurrent coupling turns them into something deeper than their layer count suggests. HRM essentially teaches a Transformer to loop intelligently, not endlessly.

## Results

The authors evaluated HRM on three reasoning tests, each a different flavor of brain pain:

1. **ARC-AGI (Abstraction and Reasoning Corpus):** IQ-test-style puzzles demanding rule induction and compositional reasoning.  
2. **Sudoku-Extreme:** Brutal 9×9 puzzles requiring multi-step logic and backtracking.  
3. **Maze-Hard:** Optimal path-finding on 30×30 grids — a search-heavy, algorithmic challenge.

Despite being trained from scratch on ~1,000 samples, HRM annihilated baselines. On ARC-AGI-1, it scored 40.3%, beating o3-mini-high (34.5%) and Claude 3.7 8K (21.2%). On Sudoku-Extreme, where most models flatline at 0%, HRM achieved ~75% accuracy. On Maze-Hard, it hit 55% — again, zero for everyone else.

To visualize reasoning, the authors examined intermediate outputs at each timestep. In mazes, HRM first explores multiple paths, prunes dead ends, and gradually sharpens the optimal route — a neural echo of breadth-first search. In Sudoku, it fills candidates, detects violations, and backtracks — like a symbolic solver. On ARC tasks, it performs incremental pixel transformations, climbing toward the goal rather than guessing blindly. The reasoning pattern adapts by domain, suggesting HRM learns *algorithms*, not just correlations.

They also measured **inference-time scaling**: increasing the computation limit (letting the model “think longer”) improves accuracy without retraining — the model literally benefits from more time. A Transformer can’t do that; its layers are fixed. HRM, by contrast, scales gracefully, mirroring how humans take more seconds to solve tougher problems.

## Brain Correspondence

Neuroscience suggests that higher brain areas (like the prefrontal cortex) maintain **higher-dimensional representations**, allowing flexible, context-rich cognition. This can be quantified via the **Participation Ratio (PR):**

{% raw %}
$$
PR = \frac{(\sum_i \lambda_i)^2}{\sum_i \lambda_i^2}
$$
{% endraw %}

where \(\lambda_i\) are eigenvalues of the neural activity’s covariance matrix. A higher PR means information is spread across more dimensions — more “mental workspace.”

When computed on HRM’s hidden states (trained on Sudoku-Extreme), the results are striking:  
- Low-level module \(z_L\): PR ≈ 30  
- High-level module \(z_H\): PR ≈ 90  

That’s a 3× difference — almost identical to empirical ratios observed in the mouse cortex between sensory and associative areas. Furthermore, as task variety increases, \(z_H\)’s PR expands, while \(z_L\)’s stays steady — exactly like biological systems adapting representational capacity to cognitive demand.

## Discussion

HRM edges closer to **practical Turing-completeness.** Like the Universal Transformer, given unlimited memory and time, HRM can emulate any algorithm. But the difference lies in *efficiency:* it achieves deep computation without the instability or memory explosion of previous recurrent architectures. Its adaptive compute lets it allocate effort dynamically — more cycles for harder problems, fewer for easy ones.

It may also challenge the “bigger-is-better” dogma of AI. If a 27 M-parameter network trained on 1,000 examples can outperform billion-parameter models trained on terabytes, the future might not be about scaling data but *scaling thought*. This marks a conceptual pivot — from data-centric scaling laws to architecture-centric reasoning laws.

## Conclusion

The Hierarchical Reasoning Model stands as a small network with a big statement: **reasoning is architectural, not statistical.** It proves that deep, adaptive computation can emerge from hierarchical feedback rather than gigantic parameter counts. With 27 M parameters and 1,000 examples, HRM solves puzzles that confound models a hundred times its size.

The broader implication is revolutionary. HRM flips the narrative — intelligence may come from *structure* and *recurrence*, not size. If current LLMs are talented talkers, HRM is the quiet genius in the back of the room — smaller, humbler, but far deeper in thought.

**📘 Reference:**  
[Original Paper – *The Hierarchical Reasoning Model (Sapient Intelligence, 2025)*](https://arxiv.org/abs/2506.21734)
