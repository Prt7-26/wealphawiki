---
layout: post
title:  "A glance on deep learning by Westfox"
date:   2025-03-04 12:21:00 +0800
categories: AI DeepLearning Note
tags: AI DeepLearning Note
---

*Author: Westfox, Gryffindor, Class of 2023*

#### Link:[ A glance on deep learning (Basics)](/raw_file/A%20glance%20on%20Deep%20Learning(Basics).pdf)

这篇文档概述了深度学习的基础知识和其基本概念：

1. **人工智能与深度学习**：文档解释了我们通常所说的“人工智能”实际上主要是机器学习，而深度学习是其更高级的形式。深度学习通过神经网络构建复杂的函数，通过调整众多参数来逼近现实世界中的函数。

2. **深度学习的基本概念**：引入了函数可以表示现实的观点，数据（如体重、身高或图片等）可以用数字表示。计算机通过这些数字（数据）来构造函数，从而逼近现实世界的现象。

3. **神经网络**：文档解释了神经网络是由多个线性函数层组成，通过权重和偏置连接。网络的输出通过引入像sigmoid函数这样的非线性函数，变成能够学习复杂模式的神经网络。

4. **神经网络的类型**：除了全连接神经网络，还提到了其他类型的神经网络，如图神经网络（GNN）、循环神经网络（RNN）和卷积神经网络（CNN），它们分别适用于不同的应用场景。

5. **神经网络的训练**：深度学习的关键在于通过调整神经网络的参数来减小构建的函数与真实函数之间的差异。这个过程通过使用损失函数（如均方误差或二元交叉熵损失）和梯度下降算法来实现。

6. **梯度下降与反向传播**：梯度下降是一种通过迭代调整参数以最小化损失函数的方法。反向传播指的是在神经网络中计算梯度并向后传播，从而高效地更新网络的参数。

This note provides an overview of the basics of deep learning and its fundamental concepts:

1. **Artificial Intelligence and Deep Learning**: It clarifies that AI, as commonly understood, is mostly machine learning, with deep learning being its advanced form. Deep learning uses complex functions, constructed by neural networks, to approximate real-world functions through the adjustment of numerous parameters.

2. **Basic Concepts of Deep Learning**: It introduces the idea of functions representing reality, where data (like weight, height, or pictures) can be expressed as numbers. These numbers (data) can be manipulated by computers to create functions that approximate real-world phenomena.

3. **Neural Networks**: The document explains that neural networks are composed of layers of linear functions connected by weights and biases. The network's output is adjusted through the inclusion of non-linear functions like the sigmoid function, turning the network into a neural network capable of learning complex patterns.

4. **Types of Neural Networks**: Besides fully connected networks, it mentions other types like Graph Neural Networks (GNN), Recurrent Neural Networks (RNN), and Convolutional Neural Networks (CNN), each suited for different applications.

5. **Training Neural Networks**: The key to deep learning is adjusting the parameters of a neural network to reduce the difference between the constructed function and the real function. This is done using **loss functions** (such as Mean Squared Error or Binary Cross-Entropy Loss) and **gradient descent** algorithms. 

6. **Gradient Descent and Backpropagation**: Gradient descent is a method used to minimize the loss function by iteratively adjusting the parameters. Backpropagation refers to the process in neural networks where gradients are calculated and propagated backward to update the network's parameters efficiently.

![1](/assets/images/article/deep-learning-wzb/1.jpg)

![2](/assets/images/article/deep-learning-wzb/2.jpg)