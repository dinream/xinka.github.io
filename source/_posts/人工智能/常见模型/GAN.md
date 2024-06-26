# GAN
## 关键
1. 生成对抗网络（GANs）将当前用于判别机器学习的深度神经网络的进步转化为（隐式）生成建模。
2. GAN 训练一个生成式深度学习网络 G，将多维随机样本 z （来自高斯分布或者均匀分布）作为输入，从所需分布中生成一个样本。
3. GANs 将密度估计问题转换为 二元分类问题，其中对 G 参数的学习是通过能耐区分真假数据的判别深度神经网络 D 来实现的。
4. 更正式地说，GAN 解决的优化问题可以概括如下：
	![[Pasted image 20240415162616.png]]
	  HashCat Per position Markov Chains. 

## 分类
1. 基于积分概率指标：IPM-based GAN
	为 GAN 训练 提供稳定性，学习过程中相对稳定，
2. 基于非积分概率指标：non-IPM-based GAN
## 缺点
在学习阶段不稳定，模型优化困难。

# WGAN
## 概述
Arjovsky 等人 2017 提出的 WGAN 通过采用 Wasserstein 距离作为损失来提高标准 GAN 的训练稳定性。 这种方法的好处包括减少模式崩溃和有意义的学习曲线，这有助于确定最佳超参数。 WGAN 纳入了新的成本函数； 然而，WGAN 的实验重点是生成逼真的图像。 古尔拉贾尼等人。
## 优化
与传统的GAN相比，WGAN通过引入Wasserstein距离取代了传统GAN中使用的JS散度或KL散度，从而在训练过程中提供了更稳定的梯度信号。

## 策略
为了实现Wasserstein距离的近似，WGAN引入了一个判别器的参数范数约束，即Lipschitz限制。这通过对判别器的权重进行剪裁或权重正则化来实现。然而，这种限制方法可能难以实施并且效果不稳定。


# IWGAN 
可以更有效地找到全局最优值。 他们引入了梯度惩罚的概念来代替 WGAN 的梯度裁剪。 古尔拉贾尼等人。 提出使用IWGAN来解决文本生成问题。 在 Gulrajani 等人的 IWGAN 中，G 和 D 都由简单的残差 CNN 组成。 残差架构使得 GAN 的训练快速且稳定[30,31]。 G 将潜在噪声向量作为输入，通过将其转发到其卷积层来对其进行转换，并输出 32 个 one-hot 字符向量的序列。 G的输出层采用softmax非线性函数，并将其转发给D。假样本的每个输出特征由argmax函数的结果决定，argmax函数将G生成的每个输出向量作为输入。 

## Relativistic Average GAN

相对论平均生成对抗网络（Relativistic Average GAN）是生成对抗网络（GAN）的一种变体，旨在改善生成对抗网络的训练和生成效果。

## 背景
传统的GAN中，生成器试图生成逼真的样本，而判别器则根据样本的真实性进行分类。然而，这种方式可能导致生成器和判别器陷入不稳定的训练过程，因为生成器的更新依赖于判别器的反馈，而判别器的反馈又依赖于生成器生成的样本。
## 特点
相对论平均GAN通过引入相对论平均策略来解决这个问题。在这种策略下，生成器和判别器之间的对抗性比较被重新定义，以便更准确地反映出生成样本的真实性。具体而言，相对论平均GAN引入了两个新的损失函数：相对论平均生成器损失和相对论平均判别器损失。

1. 相对论平均生成器损失（Relativistic Average Generator Loss）是通过对真实样本和生成样本进行比较来度量生成器的性能。它通过计算生成样本在判别器给出真实样本的概率上的平均值来评估生成器的生成能力。

2. 相对论平均判别器损失（Relativistic Average Discriminator Loss）用于度量判别器的性能。它通过比较真实样本和生成样本之间的相对概率来评估判别器的辨别能力。

## 训练过程

1. 从真实样本中随机采样。
2. 使用生成器生成一批样本。
3. 计算相对论平均生成器损失和相对论平均判别器损失。
4. 更新生成器的参数以减小相对论平均生成器损失。
5. 更新判别器的参数以减小相对论平均判别器损失。
6. 重复步骤1-5，直到达到预定的训练轮数或生成样本达到所需的质量。
## 优点
改善了生成样本的质量和训练的稳定性。通过引入相对论平均策略，生成器和判别器能够更准确地估计样本的真实性，并促使它们相互逼近，提高生成样本的质量。这种模型在图像生成、文本生成和其他生成任务中都有应用。


# WGAN-GP
WGAN-GP通过引入梯度惩罚解决了WGAN中的限制问题。梯度惩罚是通过在判别器的输出和真实样本之间的采样点上计算梯度范数的平均值，并将其与预定义的惩罚因子相乘来实现的。这个惩罚项鼓励判别器在整个输入空间上保持平滑的梯度，从而使判别器满足Lipschitz连续性的要求。

具体来说，在WGAN-GP中，生成器和判别器的训练过程如下：

1. 从真实数据和生成器生成的样本中采样。
2. 在采样点上计算判别器的输出。
3. 计算判别器的梯度惩罚，并将其添加到判别器的损失函数中。
4. 更新判别器的参数以最小化损失函数。
5. 对生成器进行更新，最大化判别器对生成样本的输出。

WGAN-GP相对于传统的GAN具有几个优点。首先，它提供了更稳定的训练过程，减少了训练中的模式崩溃和模式衍生问题。其次，通过梯度惩罚，WGAN-GP避免了对判别器权重的剪裁或正则化，使得模型的训练更简单和可靠。最后，WGAN-GP在生成器和判别器之间提供了更准确的梯度信号，从而改善了生成样本的质量和多样性。