---
title: "GAN发展时间轴"
date: 2020-02-29T17:10:06+08:00
lastmod: 2020-02-29T17:10:06+08:00
show_in_homepage: true

tags: ['GAN']
categories: ['机器学习']

featured_image: ''
featured_image_preview: ''

comment: true
toc: false
autoCollapseToc: true
---

<!--more-->

GAN发展时间轴

2020年2月26日星期三

下午2:57

提升效果gpu要求较大重点   工具链接：https://paperswithcode.com

# 2014 GAN:Generative Adversarial Networks

​      https://paperswithcode.com/paper/generative-adversarial-networks

# 2014 CGAN: Conditional Generative Adversarial Network

不再用噪声作为输入

cGAN使用了辅助的标签信息来增强原始GAN，对生成器和判别器都使用标签数据进行训练，从而实现模型具备产生特定条件数据的能力。

https://paperswithcode.com/paper/conditional-generative-adversarial-nets

# 2015 DCGAN: Deep Convolutional Generative Adversarial Network

DCGAN 的思路可以简单概括为：

- 卷积神经网络=处理图像效果好
- 生成对抗网络=生成数据效果好
- ⟹卷积神经网络+生成对抗网络=生成图像效果好

https://paperswithcode.com/paper/unsupervised-representation-learning-with-1

# 2016： CoGAN: Coupled Generative Adversarial Networks

同样是两组判别器和生成器，相比 DCGAN 清晰度更高，更为真实。

https://paperswithcode.com/paper/coupled-generative-adversarial-networks

# 2016 ：infoGAN:Interpretable Representation Learning by Information Maximizing Generative Adversarial Nets

这是Generative Adversarial网络的信息理论扩展，它能够以完全无监督的方式学习解缠结的表示形式。 InfoGAN是一个生成型对抗网络，也可以最大化一小部分潜在变量和观测值之间的相互信息。 我们得出了可以有效优化的互信息目标的下限，并表明我们的训练过程可以解释为Wake-Sleep算法的一种变体。 具体而言，InfoGAN成功地将书写样式从MNIST数据集上的数字形状中解脱出来，从3D渲染图像的光照中构成姿势，并从SVHN数据集上的中心数字中解脱了背景数字。 它还可以在CelebA脸部数据集上发现视觉概念，包括发型，眼镜的有无以及情感。 实验表明，InfoGAN学习的可解释表示形式与通过现有完全监督方法学习的表示形式具有竞争性。

https://arxiv.org/abs/1606.03657

# 2016: SGAN: Semi-Supervised Learning with Generative Adversarial Networks

​     SGAN的结构来利用辅助标签信息（少量标签），利用判别器或者分类器的末端重建标签信息。

​    https://paperswithcode.com/paper/semi-supervised-learning-with-generative

# 2016-2017: EBGAN:Energy-based Generative Adversarial Network

We introduce the "Energy-based Generative Adversarial Network" model (EBGAN) which views the discriminator as an energy function that attributes low energies to the regions near the data manifold and higher energies to other regions. Similar to the probabilistic GANs, a generator is seen as being trained to produce contrastive samples with minimal energies, while the discriminator is trained to assign high energies to these generated samples. Viewing the discriminator as an energy function allows to use a wide variety of architectures and loss functionals in addition to the usual binary classifier with logistic output. Among them, we show one instantiation of EBGAN framework as using an auto-encoder architecture, with the energy being the reconstruction error, in place of the discriminator. We show that this form of EBGAN exhibits more stable behavior than regular GANs during training. We also show that a single-scale architecture can be trained to generate high-resolution images.

https://arxiv.org/abs/1609.03126

# 2016-2017：ACGAN: Conditional Image Synthesis With Auxiliary Classifier GANs

CGAN+SGAN

https://paperswithcode.com/paper/conditional-image-synthesis-with-auxiliary

# 2016-2018：pix2pix: Image-to-Image Translation with Conditional Adversarial Networks

​    https://paperswithcode.com/paper/image-to-image-translation-with-conditional

# 2017:BEGAN: Boundary Equilibrium Generative Adversarial Networks

我们提出了一种新的平衡执行方法，并结合了从Wasserstein距离得出的损耗，用于训练基于自动编码器的生成对抗网络。 该方法在训练过程中平衡了生成器和鉴别器。 此外，它还提供了一种新的近似收敛方法，快速稳定的训练和高视觉质量。 我们还导出了一种控制图像多样性与视觉质量之间权衡的方法。 我们专注于图像生成任务，即使在更高的分辨率下，也为视觉质量树立了新的里程碑。 这是在使用相对简单的模型架构和标准培训过程的同时实现的。

https://arxiv.org/abs/1703.10717

# 2017：WGAN: Wasserstein Generative Adversarial Networks

提出新的代价函数，防止梯度消失，有更能好的稳定性

没有解决1-Lipshcitz限制问题

https://paperswithcode.com/paper/wasserstein-gan

# 2017-2018：CycleGAN：Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks

两组判别器和生成器，同样以图片作为输入而不是噪声

https://paperswithcode.com/paper/unpaired-image-to-image-translation-using

# STGAN 人脸属性编辑

 

# 2017-2018：BicycleGAn:Toward Multimodal Image-to-Image Translation

BicycleGAN在图像变换生成的基础上实现了多类风格的生成， 比如给出一张鞋的草图在保持确定的前提下，生成出各式各样不同纹理风格的图像。

https://arxiv.org/abs/1711.11586

# 2017-2018: AttGAN: Facial Attribute Editing by Only Changing What You Want

面部属性编辑旨在操纵面部图像的单个或多个属性，即在保留其他细节的同时生成具有所需属性的新面部。 最近，生成对抗网络（GAN）和编码器-解码器体系结构通常被合并来处理此任务，并获得可喜的结果。 基于编码器-解码器体系结构，通过对以所需属性为条件的给定面部的潜在表示进行解码来实现面部属性编辑。 一些现有方法试图建立与属性无关的潜在表示，以进行进一步的属性编辑。 但是，这种对潜在表示的独立于属性的约束是过分的，因为它限制了潜在表示的容量，并且可能导致信息丢失，从而导致生成过程过于平滑和失真。 在这项工作中，我们没有在潜在表示上施加约束，而是将属性分类约束应用于生成的图像，以仅保证所需属性的正确更改，即“更改所需内容”。 同时，引入了重构学习来保留属性排除的细节，换句话说，就是“仅更改您想要的内容”。 此外，对抗学习用于视觉逼真的编辑。 这三个组件相互配合，形成了用于高质量面部属性编辑（称为AttGAN）的有效框架。 此外，我们的方法也直接适用于属性强度控制，并且可以自然地扩展为属性样式操作。 在CelebA数据集上进行的实验表明，在保留完备的面部细节的情况下，我们的方法在现实属性编辑方面的表现优于最新技术。

https://arxiv.org/abs/1711.10678

# 2017-2018：ProGAN: Progressive growing of Generative Adversarial Networks

我们描述了一种生成对抗网络的新训练方法。 关键思想是逐渐增加生成器和鉴别器：从低分辨率开始，我们添加新层，以随着训练的进行对越来越细的细节建模。 这既加快了训练速度，又大大稳定了训练速度，使我们能够产生前所未有的质量

https://paperswithcode.com/paper/progressive-growing-of-gans-for-improved

# 2017-2018：StarGAN: Unified Generative Adversarial Networks for Multi-Domain Image-to-Image Translation

可以仅使用一个模型就可以对多个域执行图像到图像的转换。 StarGAN的这种统一模型架构允许在单个网络中同时训练具有不同域的多个数据集

https://paperswithcode.com/paper/stargan-unified-generative-adversarial

# 2018：SNGAN: Spectral Normalization for Generative Adversarial Networks

频谱归一化

通过对真正的解决判别器f(x)的1-Lipshcitz限制问题，方法非常朴素且巧妙。

​    https://paperswithcode.com/paper/spectral-normalization-for-generative

# 2018-2019： SAGAN: Self-Attention Generative Adversarial Networks

SAGAN论文中给出的解释是GAN擅长合成几乎没有结构约束的图像类别，例如海洋、天空和景观类别。但是无法捕获在某些类别中始终如一地出现的几何或结构模式， 例如狗的毛皮。这种原因并不是GAN自身带来的，而是以前的GAN模型在很大程度上依赖于卷积来模拟不同图像区域之间的依赖关系。

效果好于ACGAN

在本文中，我们提出了一种自我注意生成对抗网络（SAGAN），该网络可以为图像生成任务提供注意力驱动的远程依赖关系建模。 传统的卷积GAN生成高分辨率细节，仅作为低分辨率特征图中空间上局部点的函数。 在SAGAN中，可以使用来自所有要素位置的提示来生成细节。 此外，鉴别器可以检查图像的远处部分中的高度详细的特征是否彼此一致。 此外，最近的工作表明，发电机调节会影响GAN性能。 利用这种见解，我们将频谱归一化应用于GAN生成器，并发现这可以改善训练动态。 拟议中的SAGAN达到了最先进的结果，在具有挑战性的ImageNet数据集上，最佳已发布的Inception得分从36.8提高到52.52，Frechet Inception距离从27.62降低到18.65。 注意层的可视化显示生成器利用了与对象形状相对应的邻域，而不是固定形状的局部区

https://paperswithcode.com/paper/self-attention-generative-adversarial

# 2018-2019：BigGAN：Large Scale GAN Training for High Fidelity Natural Image Synthesis

生成效果高度逼真而被誉为「史上最强 GAN 图像生成器」。需要TPU集群

需要很大的数据集效果才更好

目前需要4-8块gpu,暂时不考虑了

首先，DeepMind使用SAGAN作为基线，并附加了一个叫做光谱标准化的特性。

从那里开始，他们将批量缩放50%，宽度(通道数)缩放20%。起初，增加层数似乎没有帮助。

在其他一些一位数百分比的改进之后，作者使用“截断技巧”来提高采样图像的质量。

https://paperswithcode.com/paper/large-scale-gan-training-for-high-fidelity

# 2018-2019：StyleGAN: Style-based Generative Adversarial Networks

StyleGAN 来自英伟达的一项研究，关注的是损失函数、稳定性、架构等。

因此，StyleGAN 没有专注于生成更加逼真的图像，而是致力于提高 GAN 对生成图像的精确控制能力。

如前所述，StyleGAN并不是在架构和损失函数上开发的。 取而代之的是一套可与任何GAN一起使用的技术，使您可以做各种很酷的事情，例如混合图像，在多个级别上更改细节以及执行样式转换的更高级版本。

换句话说，StyleGAN就像是photoshop插件，而大多数GAN开发都是photoshop的新版本

![GPI's  2  4  8  1024x1024  41 days 4 hours  21 days 22 hours  11 days 8 hours  6 days 14 hours  512x512  24 days 21 hours  13 days 7 hours  7 days O hours  4 days 10 hours  256x256  14 days 22 hours  9 days 5 hours  4 days 21 hours  3 days 8 hours ](file:///C:/Users/Haillk/AppData/Local/Temp/msohtmlclip1/01/clip_image001.png)

感觉时间还是有点长啊

https://paperswithcode.com/paper/a-style-based-generator-architecture-for

> *参考：**2018**、**6**、**21**https://blog.floydhub.com/gans-story-so-far/*