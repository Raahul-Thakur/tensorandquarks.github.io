---
layout: default
title: ""
date: 2025-03-06
tags: [ML]
---

# The Random Illusion: Why Adversarial Defenses Aren’t as Robust as They Seem

The field of adversarial machine learning is built on a paradox: models that perform impressively on natural data can be shockingly vulnerable to small, human-imperceptible perturbations. These adversarial examples expose a fragility in deep networks that could have serious consequences in security-critical domains like autonomous driving, medical imaging, or biometric authentication. Naturally, defenses against these attacks have been the subject of intense research. Among them, a seemingly simple strategy has gained popularity: **random transformations**. By applying random, often non-differentiable perturbations to input images—such as resizing, padding, cropping, JPEG compression, or color quantization—these methods hope to break the adversary’s control over the gradients that guide attacks. At first glance, it seems effective. Robust accuracy increases. Attacks fail. But is this robustness genuine?

<!--more-->

This question forms the heart of the paper *“Demystifying the Adversarial Robustness of Random Transformation Defenses”* by Bai et al. (2022). The authors critically re-examine the perceived robustness of these defenses. Their work builds a theoretical and empirical foundation to investigate whether random transformation (RT) defenses provide true robustness or merely present an illusion by obfuscating gradients and making attacks harder to execute. Through a combination of analytical modeling and rigorous adaptive attacks, they show that what looks like security is often just an artifact of incomplete threat models.

## What Are Random Transformation Defenses?

RT defenses work by applying a stochastic transformation `T(x)` to the input `x` before feeding it into a model `f`. This transformation is sampled from a distribution `𝒯`, and the model predicts based on `f(T(x))`. The hope is that even if an adversary perturbs the original image `x`, the transformation might distort or nullify that perturbation’s effect.

There’s an intuitive logic behind this: if the adversarial perturbation is tightly tailored to a specific input instance, applying random augmentations or corruptions might “shake it loose.” Moreover, some transformations—like JPEG compression—are non-differentiable, which can interfere with the gradient-based optimization that underlies most modern adversarial attacks.

But intuition isn't proof. And it turns out that when attackers are properly equipped, they can pierce the veil of these defenses.

## Full-Information Adversaries and EOT Attacks

A key contribution of the paper is defining a strong and realistic **threat model**. Here, the attacker knows the transformation distribution `𝒯`, has access to model gradients, and optimizes:

$$
\begin{aligned}
\text{maximize} \quad & \mathbb{E}_{T \sim \mathcal{T}} \left[ \ell(f(T(x + \delta)), y) \right] \\
\text{subject to} \quad & \|\delta\| \leq \epsilon
\end{aligned}
$$

This is the **Expectation Over Transformation (EOT)** attack. It was introduced by Athalye et al. in 2018 and works by averaging gradients over multiple transformation samples. Many prior defenses were tested against weak, non-adaptive attacks. But once EOT is used, the perceived robustness of these RT defenses collapses.

## Theory: What Random Transformations Actually Do

The paper rigorously proves that RT defenses do **not** remove adversarial examples. Instead, they convolve the loss landscape with the transformation distribution, creating a smoothed version. This changes gradient directions slightly and slows down naive attacks—but an adaptive attacker can still find vulnerabilities.

Moreover, the theory dispels a major misconception: random transformations do **not** push inputs out of the adversarial cone. The adversarial directions remain close. RTs may distort the path to the boundary but not its existence.

This insight is crucial: any perceived robustness is an illusion created by **gradient obfuscation**, not true geometric changes.

## Experiments: When Defenses Fail

The authors evaluate common RT defenses like:

- Random Resize & Pad (R&P)
- JPEG Compression
- Bit Depth Reduction

Across CIFAR-10 and ImageNet, they compare robust accuracy under non-adaptive and adaptive EOT attacks. Results are stark:

| Defense               | Robust Acc (naive) | Robust Acc (EOT) |
|-----------------------|--------------------|------------------|
| JPEG Compression      | ~50%               | ~5–10%           |
| Resize and Padding    | ~40–45%            | ~2–8%            |
| Bit Depth Reduction   | ~30%               | ~1–5%            |

Clearly, RT defenses fall apart under proper attack conditions. The illusion of security is broken when the adversary adapts to the defense.

## Geometry & Visualizations

The paper also explores adversarial geometry. Using visualizations and decision boundary proximity metrics, they show that adversarial examples persist across transformations. The decision boundaries themselves aren’t significantly altered.

This dismantles the notion that RTs “reshape” the input space. Instead, they merely add noise—noise that adaptive attackers can average out.

## What the Paper Doesn’t Cover

Despite its rigor, the paper leaves a few areas underexplored:

1. **Query-Limited Settings:** Real-world attackers may not have access to unlimited samples. RTs could offer some practical robustness by increasing attack cost.
2. **Training-Time Use:** What happens if RTs are used during adversarial training? The paper only explores test-time application.
3. **Perceptual Defense:** RTs like JPEG may make adversarial examples less perceptible. The paper doesn’t evaluate human perception.
4. **Learnable Transformations:** Modern augmentation techniques (AutoAugment, RandAugment) might offer better protection than handcrafted RTs.

## Final Thoughts

The message from this paper is loud and clear: **robustness must be measured under adaptive, full-information attacks.** RT defenses appear to work only when attackers aren’t trying hard enough. Once you account for this, the robustness vanishes.

The takeaway? If we want trustworthy models, we must move beyond random patches and toward principled defenses—certified training, robust optimization, and transparent evaluation.

---

Read the Paper: [arXiv:2207.03574](https://arxiv.org/abs/2207.03574)

