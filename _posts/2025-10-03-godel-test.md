---
layout: default
title: ""
date: 2025-10-03
tags: [ML]
mathjax: True
---


# From Prompts to Proofs: Can ChatGPT Pass the Gödel Test?

## My Introduction
ChatGPT has become a part of our daily lives in ways we could not have imagined just a few years ago. From writing emails and polishing presentations to generating working code for side projects, it has become a universal assistant. But beyond these everyday tasks, what are the true capabilities of models like ChatGPT and its successors? Can they go beyond imitating human output and actually contribute to fields that demand creativity and rigor—like mathematics? This question is at the heart of a recent research paper, *Gödel Test: Can Large Language Models Solve Easy Conjectures?* published just a week ago. The paper does not ask whether large language models can memorize or recall results, but whether they can engage in something far more ambitious: creating new mathematics. In this blog, I want to walk through what the paper does, why it matters, and what it means for the future of artificial intelligence and mathematical discovery.

<!--more-->

## The Paper’s Introduction
The authors introduce the Gödel Test as a benchmark designed to evaluate whether large language models can prove new mathematical conjectures. Unlike competition-style problems such as the International Mathematical Olympiad, these conjectures are not widely known or memorized but are instead freshly constructed problems. The aim is to test if a model like GPT-5 can take minimal background information and reason through a problem in a way that resembles the way mathematicians make progress. The focus of the study is on submodular maximization, a branch of optimization theory with deep connections to computer science, game theory, and machine learning. Submodular functions capture the principle of diminishing returns, making them both mathematically elegant and practically important. To evaluate GPT-5, the authors designed five conjectures that, while unsolved, are considered manageable by a strong graduate student. They then tested whether GPT-5, given references to one or two related papers, could reason about these conjectures without external assistance. The results were mixed but revealing: GPT-5 demonstrated promising signs of reasoning and even originality in some cases but fell short in problems requiring the synthesis of multiple proof techniques.

## The Point of the Research
The motivation for this research comes from an observation made by the mathematician Terence Tao, who suggested that current AI systems resemble “mediocre graduate students.” They can follow hints, copy known techniques, and sometimes surprise you with clever connections, but they are rarely capable of originating the truly novel step that marks a breakthrough. The authors wanted to see whether GPT-5 had crossed that line, and whether it could solve problems that are new but still easy in the grand scheme of mathematics. Instead of looking at famous unsolved problems, which are far too deep for current AI, the authors created five conjectures that would test whether GPT-5 could handle fresh ideas. These problems were deliberately chosen in submodular maximization because the area is well-structured, easy to explain, and rich in techniques that a student—or an AI—can attempt to adapt. In short, the paper is not about whether AI can solve the Riemann Hypothesis or P versus NP, but whether it can behave like a competent mathematician working on manageable problems. The results suggest that GPT-5 is on its way, but the path toward true theorem-proving remains incomplete.

## Explaining the Math
To understand the Gödel Test, it helps to know what submodular functions are. A submodular function is a special type of set function that captures the idea of diminishing returns. Imagine placing WiFi routers in a building. The first router greatly expands coverage, the second helps but less dramatically, and by the tenth router, the gain is minimal. Mathematically, submodularity is expressed by the inequality that adding an element to a small set helps more than adding it to a large set. Maximizing such functions means finding the best subset of elements under certain constraints. This is relevant in many real-world problems like selecting the best locations for sensors, choosing diverse news articles to summarize, or maximizing viral spread in social networks. Modular functions, by contrast, are perfectly additive and have no interactions. The difficulty in optimization comes when we impose constraints: perhaps we can only choose \(k\) elements, or our choices must obey rules modeled by structures like matroids and p-systems. Submodular maximization is often NP-hard, but greedy algorithms offer provable approximation guarantees. This makes it an ideal testing ground for AI reasoning: the problems are hard enough to be interesting, but structured enough to have known methods and guarantees.

## The Five Conjectures in Simple Terms

### Conjecture 1 – Mixing Monotone and Non-Monotone Functions
A monotone function always improves when you add more elements, while a non-monotone function can sometimes get worse. This conjecture asks: what if your objective combines both? For example, imagine planning a music festival. Adding more performers (monotone) always increases audience interest. But adding more food stalls (non-monotone) might sometimes hurt if they overcrowd the space or duplicate menus. Can algorithms still guarantee that the overall plan is close to optimal? The conjecture suggests yes: we can secure most of the benefit from the monotone part while salvaging some value from the non-monotone part.

### Conjecture 2 – Bicriteria Optimization under p-Systems
Constraints often limit how many or what type of elements you can choose. A p-system generalizes these rules, and they can be very strict. Bicriteria optimization says: what if we allow a small relaxation, picking a few extra items beyond the rules? For example, if a school science fair says “choose 5 projects,” bicriteria might allow 6, ensuring we capture nearly all the best ones. This conjecture asked whether greedy algorithms still perform well in such looser systems. The result showed that relaxing constraints just a little can yield solutions almost as good as the true optimum.

### Conjecture 3 – γ-Weak Submodularity
Not every function follows diminishing returns perfectly. A γ-weak submodular function is one where diminishing returns hold only approximately, controlled by γ (1 = strong, smaller values = weaker). The conjecture asks whether greedy algorithms still work in this weaker setting. Imagine choosing movies for a film festival lineup: normally, adding new films gives smaller and smaller gains in diversity. But if the genres overlap messily, this “diminishing returns” property is weaker. The conjecture proposed that the quality of the greedy solution smoothly decreases with γ, meaning the less structured the problem, the weaker—but still predictable—the guarantee.

### Conjecture 4 – Partial Monotonicity plus Weak Submodularity
This conjecture combines two relaxations: partial monotonicity (adding items usually, but not always, helps, measured by \(m\)) and weak submodularity (measured by γ). The challenge is whether guarantees can interpolate between these extremes. Think about designing a mobile app. Adding new features usually helps (monotone), but too many popups may reduce usability (non-monotone). At the same time, features overlap in usefulness, so the diminishing returns property is imperfect (weak submodular). This conjecture asked if performance guarantees could smoothly depend on both \(m\) and γ. The expected answer: yes, the approximation ratio blends these two factors into a single bound.

### Conjecture 5 – Two Matroid Constraints
A matroid captures independence rules, like “no cycles in a graph” or “only \(k\) items allowed.” Optimization under one matroid is already hard, but what if two apply simultaneously? That’s the essence of this conjecture. Imagine organizing a sports team under two rules: pick at most three players from each region (rule 1), and stay within a salary cap (rule 2). Balancing both makes the problem much harder. The conjecture asked whether greedy-like algorithms could still give good guarantees. GPT-5 proposed a bound, but its proof fell apart, highlighting the true difficulty of handling intersecting constraints.

## Overview on Submodular Maximization
Submodular maximization is a central problem in optimization and computer science because it captures the challenge of balancing diminishing returns with constraints. A submodular function can be seen as measuring the value of subsets of a ground set, and the goal is to pick a subset that maximizes value while obeying restrictions. Constraints might be as simple as a cardinality bound—pick at most \(k\) items—or as complex as matroid independence systems, which generalize notions of independence from linear algebra and graph theory. One of the most important results is that for monotone submodular functions under cardinality constraints, the greedy algorithm achieves a

{% raw %}
$$
1 - \frac{1}{e}
$$
{% endraw %}

approximation, about 63% of the optimum. This is remarkable because the exact problem is NP-hard. For non-monotone functions, the guarantees are weaker but still possible. Researchers have extended these results to continuous settings, convex relaxations, and approximate versions of submodularity. In practice, this theory underpins problems like sensor placement, influence maximization, and summarization. For the Gödel Test, submodular maximization provided the perfect balance: problems are nontrivial, require real proof techniques, but are still solvable within a day by a competent graduate student. That makes them ideal for testing whether AI can engage in real mathematical reasoning.

## Detailed Summary of the Five Conjectures

### Conjecture 1 – Mixing Monotone and Non-Monotone Functions
The first conjecture explored what happens when we maximize a function that is the sum of two parts: one monotone (adding elements always increases value) and one non-monotone (adding elements can sometimes reduce value). This is important because many real-world problems combine “always good” and “sometimes harmful” elements. The authors hypothesized that a variant of the Measured Greedy Frank–Wolfe (MGFW) algorithm, a continuous optimization method, could still guarantee a split performance:

{% raw %}
$$
(1 - \tfrac{1}{e}) \ \text{of the monotone component} \quad \text{and} \quad \tfrac{1}{e} \ \text{of the non-monotone component}.
$$
{% endraw %}

GPT-5 attempted this conjecture by adapting proofs from a related NeurIPS 2021 paper, where the second term was concave rather than non-monotone. Its adaptation was broadly correct: it reproduced the expected split bound with an additional small error term linked to smoothness parameters. However, its proof was “lazy,” often copying steps wholesale, skipping justifications, and leaving inequalities vague. While the result aligned with the conjecture, it revealed GPT-5’s tendency to mimic rather than refine. In essence, GPT-5 showed that it could extend known algorithms to this hybrid setting, but it lacked the mathematical elegance to streamline or improve the argument. Still, this problem demonstrated the model’s ability to handle incremental generalizations reliably.

### Conjecture 2 – Bicriteria Optimization under p-Systems
The second conjecture asked whether greedy bicriteria algorithms could handle optimization under p-systems, which generalize matroids and allow more complex independence structures. Bicriteria solutions are slightly “cheating”: instead of strictly obeying the constraint, they relax it by a factor but achieve nearly full value. The authors originally conjectured a bound of

{% raw %}
$$
(1-\epsilon, \ \lceil \log_{p+1}(1/\epsilon) \rceil).
$$
{% endraw %}

GPT-5 approached the problem by extending Feldman–Kuhnle’s multi-pass greedy framework. It reasoned that if one runs greedy repeatedly and unions the resulting independent sets, the accumulated solution could approximate the optimum to within

{% raw %}
$$
1 - \epsilon.
$$
{% endraw %}

For feasibility, GPT-5 derived a factor

{% raw %}
$$
g_p(\epsilon) \;=\; \left\lceil \frac{\ln(1/\epsilon)}{\ln\!\left(\frac{p+1}{p}\right)} \right\rceil \;\approx\; (p+1)\ln\!\big(1/\epsilon\big),
$$
{% endraw %}

which was actually better than the conjectured bound because it naturally captured the dependence on \(p\) rather than fixing the base of the logarithm. The analysis showed how the residual gap decreases exponentially with passes, which was mathematically valid. While it made small mistakes, like skipping simplifications and overlooking the neat reduction when \(p=1\), GPT-5’s result was sharper and more intuitive than the authors’ original guess. This was the model’s strongest performance: not just adapting proofs but slightly improving on them, suggesting the beginnings of genuine mathematical creativity.

### Conjecture 3 – γ-Weak Submodularity
The third conjecture examined γ-weak submodular functions, which generalize submodularity by relaxing diminishing returns with a parameter γ. When γ=1, the function is perfectly submodular; smaller γ values weaken the property. The conjecture was whether approximation guarantees degrade smoothly with γ. Using the continuous greedy Frank–Wolfe method, the authors expected an approximation of

{% raw %}
$$
1 - e^{-\gamma}.
$$
{% endraw %}

GPT-5’s proof closely mirrored known analysis for exact submodularity but multiplied progress terms by γ. It argued that at each iteration, the improvement is scaled by γ, leading to an exponential decay bound. Its final result matched the conjecture, with small additive error terms. However, the proof was unnecessarily convoluted: it used variable step sizes, lengthy recurrences, and confusing terminology (“value factor” instead of approximation ratio). In effect, GPT-5 gave the right answer but in the style of a student copying too many lines from a textbook rather than writing a clean adaptation. Nevertheless, the model demonstrated that it could correctly handle the weakening of structure and produce the expected bound. This problem reinforced that GPT-5 excels at “single-parameter” generalizations where the proof closely parallels a known argument.

### Conjecture 4 – Partial Monotonicity with Weak Submodularity
The fourth conjecture combined two relaxations: partial monotonicity, where adding elements usually helps but not always (measured by \(m\)), and weak submodularity, where diminishing returns are imperfect (measured by γ). The challenge was to see whether approximation guarantees could interpolate smoothly between these two axes. The expected result was something like

{% raw %}
$$
m \big(1 - e^{-\gamma}\big) \;+\; (1-m)\,\frac{\gamma}{e}.
$$
{% endraw %}

GPT-5 first defaulted to re-stating known results without integrating \(m\). When pushed, it attempted to prove the interpolated bound using Randomized Greedy with dummy elements. Its analysis leaned on occupancy arguments and local submodularity ratios, eventually deriving the desired interpolating formula. However, the proof was riddled with flaws: missing constants, unjustified inequalities, and an overgeneralized reliance on local rather than global ratios. Despite these problems, the high-level structure was correct, and GPT-5 produced the formula that matched expectations. This case highlighted both promise and weakness: GPT-5 could see the shape of the result but failed to execute the detailed mathematics rigorously. It illustrated the model’s difficulty when two distinct relaxations must be combined in one argument, requiring synthesis beyond straightforward adaptation.

### Conjecture 5 – Two Matroid Constraints
The final conjecture tackled maximizing monotone γ-weakly submodular functions under the intersection of two matroids. Even for one matroid, the problem is challenging, but solvable with algorithms like Residual Random Greedy. The conjecture asked if such algorithms could extend to two matroids. GPT-5 suggested adapting the random greedy algorithm: in each step, compute a maximum-weight independent set under the contracted matroids, pick one element randomly, and repeat. It attempted to analyze progress but incorrectly assumed only one element from the optimum would be blocked at each step, overlooking the fact that with two matroids, two elements could be blocked. Its bound,

{% raw %}
$$
\left(\frac{\gamma}{\gamma+2}\right)^{\!2},
$$
{% endraw %}

resembled a natural extension of the single-matroid case, but the analysis was invalid. The algorithmic idea was on target, but the proof collapsed under feasibility and exchange property issues. Even the authors noted this problem may be fundamentally harder than expected, revealing limits for both human and AI reasoning. GPT-5’s failure here underscored its core weakness: when multiple complex proof techniques must interact, it cannot maintain coherence, producing plausible but incorrect mathematics.

## Terminology Explained
To navigate this research, it helps to unpack some of the technical terms. A submodular function is one with diminishing returns, while a modular function is simply additive. A greedy algorithm is the natural process of picking the element that helps the most at each step, and remarkably, this simple strategy performs well for submodular maximization. Matroids are combinatorial structures that generalize independence and make greedy algorithms work; a p-system generalizes matroids further, with a parameter \(p\) controlling complexity. Bicriteria algorithms relax constraints slightly to achieve stronger guarantees. A γ-weakly submodular function is one that is approximately submodular, with γ measuring how close it is to the real thing. Monotonicity describes whether adding elements always helps (monotone), sometimes hurts (non-monotone), or partly helps depending on an interpolation parameter \(m\). Together, these concepts provide the vocabulary of the paper. They are not just abstract ideas: each reflects a real-world tradeoff between constraints, resources, and returns. Understanding them is essential to seeing how the conjectures extend known results.

## The Process of Solving Conjectures
Solving these conjectures is not about producing exact sets or numbers but about proving approximation guarantees. A graduate student tackling one of these problems would begin by identifying the closest known result and the standard algorithm used there, usually greedy or continuous greedy. The next step is to adapt the analysis: for example, scaling progress inequalities by γ in the case of weak submodularity, or introducing dummy elements for partial monotonicity. Then comes the key step: writing and solving the recurrence that tracks how much closer the algorithm gets to the optimum with each step. This often reveals exponential decay, leading to bounds like

{% raw %}
$$
1 - \frac{1}{e}
$$
{% endraw %}

or

{% raw %}
$$
1 - e^{-\gamma}.
$$
{% endraw %}

The final result is a theorem stating that a given algorithm guarantees at least a certain fraction of the optimal value. GPT-5 followed a similar process: it was able to mimic and adapt proofs for problems closely related to existing ones, but it failed where synthesis of multiple techniques was needed. In this sense, it performed like a competent but unimaginative graduate student—capable of handling straightforward variations, but struggling with deeper creativity.

## Conclusion
The Gödel Test is an ambitious new way of benchmarking AI in mathematics. Instead of asking whether models can solve known problems, it asks whether they can prove new conjectures that are simple but fresh. GPT-5’s performance on the test is a mixed story. On the positive side, it solved or nearly solved three conjectures, improved on the authors’ original bound in one case, and showed genuine reasoning ability. On the negative side, it failed on the hardest two problems, especially where multiple proof ideas had to be combined. What this tells us is that GPT-5 is inching closer to the level of a strong graduate student: it can adapt proofs, follow known structures, and even surprise us occasionally, but it lacks the deeper originality and integration skills of a mathematician. The significance of the Gödel Test lies in setting a benchmark for when AI might achieve this next leap. Passing it would mean not just problem-solving but true theorem-proving—an indication that AI can meaningfully contribute to mathematical discovery. Until then, we can say GPT-5 has shown sparks of brilliance but still has a long way to go.

---

## References
- Gödel Test: Can Large Language Models Solve Easy Conjectures? — [Original paper (arXiv PDF)](https://arxiv.org/pdf/2509.18383)
