---
layout: default
title: "How AlexNet Lit the Spark and ResNet Fanned the Flames"
date: 2025-01-23
tags: [ML]
---

# How AlexNet Lit the Spark and ResNet Fanned the Flames

In the ever-evolving landscape of deep learning, certain architectures have defined turning points in how neural networks are designed, trained, and understood. Among these, **AlexNet** and **ResNet** stand out as monumental contributions that shifted the paradigm of computer vision and image classification. Though separated by just three years, these two architectures reflect fundamentally different eras of deep learning—AlexNet laid the groundwork for deep convolutional networks, while ResNet solved the pressing problems that deeper architectures introduced.

<!--more-->

## The Birth of AlexNet: When Deep Learning Went Mainstream

In 2012, AlexNet, developed by Alex Krizhevsky, Ilya Sutskever, and Geoffrey Hinton, took the world of computer vision by storm when it dramatically outperformed all previous approaches at the ImageNet Large Scale Visual Recognition Challenge (ILSVRC). The network achieved a top-5 error rate of just 15.3%, nearly 10 percentage points better than the runner-up. But AlexNet's significance went far beyond just its performance—it was a proof-of-concept that deep learning could scale with modern hardware, specifically GPUs.

Architecturally, AlexNet was composed of eight layers—five convolutional and three fully connected. The input was a 227x227 RGB image, and the network utilized ReLU activations instead of traditional sigmoid or tanh, enabling faster convergence. It also incorporated dropout in the fully connected layers to reduce overfitting and local response normalization (LRN), which was intended to mimic lateral inhibition in real neurons. These innovations made AlexNet a formidable model, yet despite its depth and complexity at the time, it now appears relatively shallow when compared to modern architectures.

One of the critical limitations exposed by AlexNet and similar networks was the **vanishing gradient problem**. As networks grew deeper in pursuit of better representations, they became increasingly difficult to train. Gradients diminished as they were propagated backward, leading to ineffective learning in the initial layers. This created a paradox: deeper networks theoretically had more expressive power, but in practice, they often performed worse.

![AlexNet Architecture](https://tse3.mm.bing.net/th?id=OIP.LArC4u1tbwMGgUVuC82OTwHaEK&pid=Api)

## ResNet: Going Deeper Without Getting Lost

By 2015, the deep learning community had started to feel the limitations of depth. That’s when ResNet—short for Residual Network—was introduced by Kaiming He and his team at Microsoft Research. ResNet fundamentally redefined how networks could grow in depth while remaining trainable and performant. Its architecture won the ILSVRC 2015 competition by a large margin and introduced a deceptively simple yet powerful concept: residual connections.

The central idea behind ResNet is the use of **skip connections**, or shortcuts that allow the gradient to bypass certain layers entirely. Instead of learning a direct mapping from input to output, each residual block in the network learns a residual function: the difference between the desired output and the input. This small shift has a profound impact. It allows gradients to flow more freely during backpropagation, alleviating the vanishing gradient problem and enabling networks to exceed 100 layers without degradation in training performance.

The original ResNet models come in varying depths—ResNet-18, ResNet-34, ResNet-50, ResNet-101, and ResNet-152—each offering deeper networks with bottleneck structures or identity mappings, depending on the configuration. The deeper models introduced batch normalization to stabilize learning and retained ReLU activations. Unlike AlexNet, which relied on a monolithic structure of sequential layers, ResNet’s modular residual blocks made it both scalable and robust.

![ResNet Architecture](https://tse2.mm.bing.net/th?id=OIP.aLvULJeweGPIJYVlLgCydgHaFL&pid=Api)

## Comparing Two Generations of Thought

AlexNet and ResNet represent two distinct generations of deep learning thought. AlexNet was a bold leap, showing the world what was possible with deep convolutional neural networks and GPU acceleration. It was a hand-crafted architecture with large kernel sizes and heavy fully connected layers, making it quite parameter-heavy. In fact, it had over 60 million parameters, which was staggering for its time.

ResNet, on the other hand, was an answer to the scaling problem. It retained similar parameter counts (ResNet-50 has ~25 million, ResNet-152 closer to 60 million), but due to its residual learning mechanism, it could go much deeper without suffering from training degradation. Instead of learning features from scratch at every layer, ResNet learned how to improve upon previously learned features, which turned out to be more efficient and stable, particularly for tasks that require hierarchical feature abstraction.

While AlexNet’s strength lay in pioneering practices like dropout and ReLU, it was still relatively shallow and inflexible by today's standards. Its use in modern practice is now limited to educational purposes or small-scale transfer learning. ResNet, on the other hand, has become a default backbone for numerous state-of-the-art models across computer vision applications—ranging from classification and detection to segmentation and even neural style transfer.

## Real-World Performance and Use Cases

Studies comparing the performance of these two architectures in practical domains, such as remote sensing image classification, consistently show ResNet outperforming AlexNet. For instance, a recent comparative analysis on remote sensing datasets reported ResNet-18 achieving an accuracy of nearly 65%, while AlexNet lagged behind at just over 60%. The skip connections in ResNet allow it to generalize better, especially on high-resolution, complex datasets where deeper contextual understanding is needed.

Transfer learning applications also tend to favor ResNet. Because ResNet’s structure enables deep but stable feature learning, fine-tuning pre-trained ResNet backbones on specific tasks yields better results in most modern scenarios than using AlexNet. Moreover, the community support, ongoing research, and availability of ResNet variants across frameworks make it a more production-friendly choice.

## Final Thoughts: Evolution Over Revolution

AlexNet will always be remembered as the network that launched deep learning into the mainstream. Its architectural simplicity, GPU-optimized training, and competition dominance made it iconic. But as the field matured, the need for more depth and better training dynamics gave rise to ResNet—a model that not only addressed the shortcomings of its predecessors but redefined the standards of modern CNN design.

In essence, AlexNet was the **spark**, and ResNet became the **blueprint** for building scalable, deep neural networks. Understanding both models is essential—not just for appreciating the history of deep learning, but also for grasping the design choices that influence current and future architectures.

---

*References:*

- [The W3H of AlexNet, VGGNet, ResNet, and Inception](https://medium.com/data-science/the-w3h-of-alexnet-vggnet-resnet-and-inception-7baaaecccc96)  
- [AlexNet, VGGNet, ResNet, and Inception Overview](https://medium.com/@karandeepdps/alexnet-vggnet-resnet-and-inception-11880a1ed3cd)  
- [Comparison of AlexNet and ResNet Models for Remote Sensing](https://www.researchgate.net/publication/383112789_Comparison_of_AlexNet_and_ResNet_Models_for_Remote_Sensing_Image_Recognition)  
- [Understanding ResNet](https://www.geeksforgeeks.org/residual-networks-resnet-deep-learning/)  
- [Transfer Learning with AlexNet](https://towardsdatascience.com/transfer-learning-using-pre-trained-alexnet-model-and-fashion-mnist-43898c2966fb)  
- [CV Tricks: Understanding CNN Architectures](https://cv-tricks.com/cnn/understand-resnet-alexnet-vgg-inception/)
