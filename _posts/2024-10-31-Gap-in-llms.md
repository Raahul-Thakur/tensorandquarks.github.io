---
layout: default
title: "From Facts to Insight: Bridging the Compositionality Gap in Language Models"
date: 2024-10-31
tags: [ML]
---

# From Facts to Insight: Bridging the Compositionality Gap in Language Models

Large language models (LLMs) such as GPT-3 have transformed natural language understanding by memorizing vast amounts of text. Yet, when faced with questions that require **combining** multiple pieces 
of knowledge—so-called *compositional reasoning*—even the biggest models stumble. In their paper *Measuring and Narrowing the Compositionality Gap in Language Models*, Press et al. introduce a new metric
for this shortfall, show that it persists despite model scale, and propose practical prompting techniques to close it.  

<!--more-->

---

## The Compositionality Gap: What and Why

**Compositional reasoning** asks a model not simply to recall one fact but to stitch together two or more facts in novel ways. For example:

> “What is the calling code of the birthplace of Frida Kahlo?”

Even if an LLM knows:  
1. *Frida Kahlo was born in Coyoacán.*  
2. *Mexico’s calling code is +52.*  

it may still fail to produce **+52** as the combined answer. To quantify this failure mode, Press et al. define the **compositionality gap**:

> **Compositionality Gap** =  
> (Number of 2-hop questions answered incorrectly despite both sub-questions being answered correctly)  
> ÷ (Number of 2-hop questions for which both sub-questions are answered correctly)

This metric exposes how often an LM “knows” the pieces yet cannot compose them into the final answer.

---

## Crafting a Diagnostic Benchmark: Compositional Celebrities

To systematically measure the gap, the authors introduce **Compositional Celebrities (CC)**—an 8.6 K-question, automatically generated dataset of *2-hop* queries:

1. **Select a celebrity** (e.g., Justin Bieber).  
2. **Retrieve two facts** about their birth (e.g., birth year = 1994; Masters Tournament champion = José María Olazábal).  
3. **Combine them** into a compositional query:  
   > “Who won the Master’s Tournament in the year Justin Bieber was born?”

Key properties of CC:

- **Unlikely combinations**: The paired facts are common individually, but their conjunction rarely appears in any text corpus.  
- **Decomposability**: Every question naturally splits into two sub-questions, allowing measurement of memorization vs. composition.

---

## Scaling Alone Doesn’t Solve Composition

A natural hope is that simply **increasing model size** would improve both factual recall and reasoning. Press et al. evaluate the GPT-3 family (Ada → Davinci) on CC:

- **1-hop accuracy** climbs steeply with size.  
- **2-hop accuracy** improves only marginally.  
- **Compositionality gap** stays near ~40 % across all sizes (0.35 B → 175 B parameters).

This reveals that while larger models **memorize** ever more facts, they do **not** proportionally enhance their ability to **compose** learned knowledge.

---

## Elicitive Prompting: Encouraging “Thought” in LLMs

To tackle this, the authors borrow and extend the idea of prompting LMs to **“think aloud.”** Two key methods emerge:

### 1. Chain-of-Thought (CoT)
- The model generates a free-form reasoning trace before the final answer.  
- Improves multi-hop accuracy, but can be verbose or hard to parse.

### 2. Self-Ask Prompting
- A **structured** approach:
  1. Model decides if follow-up questions are needed.  
  2. It explicitly emits each `Follow up:` sub-question.  
  3. It provides each `Intermediate answer:` in turn.  
  4. Finally, it states `So the final answer is:` in concise form.
- By scaffolding decomposition and retrieval, self-ask dramatically narrows the compositionality gap.

---

## Hybrid Reasoning: Plugging in a Search Engine

Self-ask’s clear sub-question boundaries make it trivial to integrate an external search API:

1. **LM** generates `Follow up: When was X born?`  
2. **System** sends that to a search engine and retrieves “1994.”  
3. **LM** continues, using the fetched answer as if it were its own.  

This **Self-Ask + Search Engine (SA+SE)** requires **no fine-tuning** or prompt changes—and yields further accuracy gains (often +10 pp) on CC and complementary benchmarks like 2WikiMultiHopQA, Musique, and Bamboogle.

---

## Empirical Highlights

| Method                       | 2-hop Accuracy | Compositionality Gap |
|------------------------------|---------------:|--------------------:|
| Direct Prompt                |           ~45% |               ~40%  |
| Chain-of-Thought             |           ~60% |             ~25–30%  |
| **Self-Ask (ours)**          |           ~75% |             ~10–15%  |
| **Self-Ask + Search (ours)** |           ~85% |              ~5–10%  |

---

## Implications for LLM Development

1. **Diagnostic Clarity**  
   The compositionality gap provides a targeted metric for multi-step reasoning beyond aggregate accuracy.

2. **Prompt Engineering Matters**  
   Structured elicitive prompts like self-ask can unlock reasoning abilities that mere scale cannot.

3. **Hybrid Systems Are Powerful**  
   Seamlessly combining LMs with external tools (search, databases) can bridge knowledge or reasoning shortfalls without retraining.

---

## Future Directions

- **Deeper Multi-Hop**: Extend evaluation to 3-, 4-, or higher-order reasoning tasks.  
- **Multilingual & Multimodal**: Test compositional abilities across languages and modalities (e.g., text + images).  
- **Architectural Innovations**: Design model architectures that internalize explicit decomposition, rather than relying solely on prompting.

---

**Conclusion**  
Press et al.’s work shines a spotlight on a critical blind spot of today’s LLMs: the gulf between *knowing* facts and *combining* them intelligently. By defining the compositionality gap and demonstrating that structured prompting and smart tool integration can dramatically narrow it, they offer both a diagnostic lens and a practical toolkit for the next generation of reasoning-capable language models.

---

🔗 **Read the full paper:**  
[Measuring and Narrowing the Compositionality Gap in Language Models (arXiv:2210.03350)](https://arxiv.org/abs/2210.03350)
