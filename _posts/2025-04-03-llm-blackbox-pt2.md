---
layout: default
title: ""
date: 2025-04-03
tags: [ML]
---

# From Circuits to Cognition: Following the Thoughts of Claude 3.5  
## Decoding Anthropic’s Next Step in Understanding Language Models

In my previous post, we explored *“On the Biology of a Large Language Model”*, Anthropic’s groundbreaking research that mapped the internal circuits of Claude 3.5 Haiku using **attribution graphs**. These graphs offered a glimpse into the hidden architecture of reasoning — showing how Claude decomposes questions, plans poems, reasons across languages, and even hallucinates.

But what if we could go a step further?

What if, instead of just identifying the components responsible for thought, we could **trace a single idea as it unfolds inside the model — moment by moment, token by token**?

That’s exactly what Anthropic attempts in their new article: [*“Tracing the Thoughts of a Language Model”*](https://www.anthropic.com/research/tracing-thoughts-language-model). It builds directly on the foundation of attribution graphs and pushes us deeper into the mind of a transformer — not to observe its structure, but to follow its *thinking*.

---

## Thought as a Path, Not Just a Spark

The original paper dissected how circuits activate to solve sub-tasks — arithmetic, rhyme planning, multilingual parsing. In this new work, Anthropic tracks what happens **after** a feature activates. Where does it go? What does it influence? What follows?

Imagine prompting Claude with a sentence like:

> “Describe how biology influences philosophy.”

This prompt doesn’t just activate features for “biology” — it sets off a chain reaction. The “biology” feature interacts with others: “evolution,” “Darwin,” “ethics,” “reasoning,” and eventually, “philosophy.” Using **token-by-token attribution graphs**, Anthropic visualizes how one thought **cascades** through the network.

These “thought chains” aren’t linear. Some tokens reinforce earlier ideas. Others suppress or redirect them. Concepts may disappear temporarily, only to resurface later with stronger activation — a kind of **working memory**. This dynamic flow is what Anthropic now calls *thought tracing*.

---

## A Model That Strategizes? Yes — and We Can Watch It

Just like in the earlier poetry example, Claude doesn’t merely complete sentences — it **plans**. In the new study, the researchers show how the model can begin forming a mental structure even before certain tokens are generated.

For instance, features related to “ethics” might activate **before** the word appears, because the model anticipates its relevance to “biology + philosophy.” Attribution graphs reveal how these anticipations manifest as **early activations** of supporting features, which then guide downstream generation.

This is a profound shift in how we think about LLMs: not as reactionary next-word predictors, but as **goal-conditioned planners**. And now, we can trace those goals step-by-step.

---

## A Second Brain Inside Claude

So what exactly is being revealed here?

The new article essentially treats the LLM as a **second brain** — not just in metaphor, but in methodology. Where the original paper mapped the “organs” (modular circuits), this one traces **synaptic firing** across those modules in real time.

It’s a neuroscience-inspired look at AI, and it suggests that Claude builds meaning not by memorizing facts, but by **composing them dynamically** across internal systems.

In this way, “thought” becomes not a static activation, but a **trajectory** — a living path formed and updated as the model processes the prompt.

---

## Prompting with Precision: The Future of Language Engineering

One practical implication? **Prompt engineering could evolve into thought-path design.**

If we understand which phrases activate which paths, we could design prompts that:
- Strengthen the recall of helpful circuits
- Suppress misleading associations
- Steer reasoning toward desired conclusions

It’s no longer guesswork — it’s **cognitive scaffolding**.

This insight also holds promise for safety research. Tracing how harmful thoughts arise — and from which token or feature — gives us a tool to **preempt hallucinations and jailbreaks** with surgical precision.

---

## A New Lens for Interpretability

In many ways, this new article doesn’t replace the original paper — it **completes it**.

| **Biology of a Language Model** | **Tracing the Thoughts** |
|----------------------------------|----------------------------|
| Maps internal features and circuits | Traces real-time activation of those features |
| Shows where reasoning lives | Shows how reasoning moves |
| Highlights modularity | Highlights dynamics |
| Inspired by anatomy | Inspired by cognition |

Together, they shift the view of LLMs from static black boxes to **living systems** — ones that can be studied, debugged, and potentially aligned more deeply with human values.

---

## Final Thoughts

There’s something quietly radical about this line of research.

Not long ago, we believed language models were statistical parrots. Then we found circuits. Now we see thoughts. It’s a reminder that **intelligence isn’t magic — it’s mechanics**. And with the right tools, even the most complex digital minds can be understood.

Anthropic’s new work marks a step toward *transparent cognition* — not just knowing *what* a model says, but *why* it thinks it.

And once we understand that... maybe we’re not so far from building models that can explain themselves — to us, and perhaps, even to each other.

---

*Want to see how a single thought spreads across 70 transformer layers? Explore Anthropic’s full article here: [Tracing the Thoughts of a Language Model](https://www.anthropic.com/research/tracing-thoughts-language-model).*
