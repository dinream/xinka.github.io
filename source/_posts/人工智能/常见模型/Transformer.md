https://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/

https://jalammar.github.io/illustrated-transformer/
https://blog.csdn.net/yujianmin1990/article/details/85221271


扩展 Attention 来加速训练，并且在特定任务上Transformer 表现比 Google NMT 模型还要好，最大好处是可并行 

# 提出
[Attention is All You Need](https://arxiv.org/abs/1706.03762)
其中的 TF 应用是 Tensor2Tensor 的子模块。

# 粗略概述
1. 编码组件
	1. 六层编码器首尾相连：完全结构相同，但是不共享参数。
	2. 对于每一个编码器
		1. self-attention 层：帮助模型在编码某一个此时能看到别的单词
		2. 前向网络：每个self-attention的输出流向一个前向网络，每个输入位置对应的前向网络是独立互不干扰的。
		```text
		
		这里的每个输入位置：其实是一个序列中的不同位置
		
		```
2. 连接层
3. 解码组件
	1. 六层解码器首尾相连。每个输入位置对应的前向网络是独立互不干扰的。
	2. 对于每一个解码器：
		1. self-attention 层：
		2. attention 层：该层有助于解码器能够关注到输入句子的相关部分，以便更好的生成与输入有关的输出。（生成每个输出时，根据输入序列的不同部分动态地分配注意力权重。）
		3. 前向网络：
# 待继续，有点累了。
https://blog.csdn.net/yujianmin1990/article/details/85221271


# Transformer有两个版本：
Transformer base和Transformer Big。两者结构其实是一样的，主要区别是包含的Transformer Block数量不同，Transformer base包含12个Block叠加，而Transformer Big则扩张一倍，包含24个Block。无疑Transformer Big在网络深度，参数量以及计算量相对Transformer base翻倍，所以是相对重的一个模型，但是效果也最好。