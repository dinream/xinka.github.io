Mixture  of Experts 
https://baoyu.io/translations/llm/mixture-of-experts-explained
https://www.aixinzhijie.com/article/6825966
深入理解混合专家模型
1. 相较于密集型模型，预训练速度更快
2. 拥有比同等参数更快的推理速度
3. 对显存要求高，因为需要将所有的专家模型都加载到内存中。
4. 虽然在微调方面存在挑战，有光明前景https://arxiv.org/pdf/2305.14705.pdf
# 概念
## 引入
在有限的计算资源下，相较于用更多步骤训练一个小型模型，训练一个大型模型即便步骤更少效果通常更好。
MoEs 让模型以远低于传统密集模型的计算成本进行预训练，这意味着你可以在相同的计算预算下显著扩大模型或数据集的规模。

## 原理

### 构成
1. 稀疏 MoE 层：代替了传统的密集型前馈网络（FFN）层。包含若干“专家”，每个专家都是一个独立的神经网络。实际上，这些专家通常是FFN，但它们也可以是更复杂的网络，，甚至可以是 MoE 本身，形成一个层级结构的 MoE。
2. 一个门控网络或路由器：用于决定那些 Token 分配给哪个专家。例如，在下图中，“More”这个 Token 被分配给第二个专家![[Pasted image 20240310201128.png]]
	1. 一个 token 可以分配给多个专家，如何高效的将 Token 分配给合适的专家，是使用 MoE 技术时需要考虑的关键问题之一。
	2. 这个路由器由一系列可学习的参数构成。它与模型的其他部分一起进行训练。
总结：在 Transformer 中，我们将每一个 FFN（前馈网络）层替换为 MoE 层，由一个门控层和若干”专家“组成。
# 挑战
1. 训练：在预训练阶段的计算效率极高，但在微调时往往难以适应新场景，容易造成过拟合现象。
2. 推理：尽管 MoE 模型可能包含大量参数，但是在推理过程中只有部分参数被使用，（所以它的推理速度远快于参数相同的模型）但是所有参数都需加载到内存中，因此对内存的需求相当大。
# 历史
1. 1991 提出
2. 2010~2015 两个不同的领域推动了 MoE 发展
	1. 专家作为主键
	2. 条件计算
3. 引入稀疏性概念在 NLP 领域 快速发展（本文重点），在计算机视觉等也有探索。
## 稀疏性
稀疏性基于条件计算的概念，不同于密集型模型中所有参数对所有输入都有效，稀疏性让我们能只激活系统的部分区域。
> 条件计算（即网络的某些部分仅针对特定样本激活）的概念使得在不增加计算量的情况下扩大模型规模成为可能，从而在每层 MoE 中使用了数千名专家。
(密集型模型 + 稀疏性 ==> 稀疏模型)

## 问题
在 MoE 中，当数据通过活跃的专家时，实际的批量大小会减小。例如，如果我们的批量输入包含 10 个 Token，**可能有五个 Token 由一个专家处理，另外五个 Token 分别由五个不同的专家处理，这导致批量大小不均匀，资源利用率低下**。下文中的 [优化 MoE 性能](https://baoyu.io/translations/llm/mixture-of-experts-explained#making-moes-go-brrr) 一节将讨论更多挑战及其解决方案。
## 解决
那我们该如何解决这些问题呢？通过一个学习型的门控网络 (G)，决定将输入的哪些部分分配给哪些专家 (E)：

在这种设置中，所有专家都参与处理所有输入——这是一种加权乘法过程。但如果 G 的值为 0 呢？这种情况下，就无需计算相应专家的操作，从而节约了计算资源。那么，典型的门控函数是什么样的呢？在传统设置中，我们通常使用一个简单的网络配合 softmax 函数。这个网络会学习如何选择最合适的专家处理输入。

为了让门控学习如何路由到不同的专家，需要路由到一个以上的专家，因此至少需要选择两个专家。[Switch Transformers](https://baoyu.io/translations/llm/mixture-of-experts-explained#switch-transformers) 章节将重新审视这一决策。

我们为什么要加入噪声？这是为了实现负载均衡！

## 为多专家系统 MoEs 负载均衡 tokens
如果所有的 tokens 都被发送到少数几个受欢迎的专家，这将导致训练效率低下。在标准的多专家系统训练中，门控网络倾向于主要激活相同的几位专家。这会形成自我加强的循环，因为得到优先训练的专家会被更频繁地选择。为了减轻这种情况，引入了一种**辅助损失**来鼓励平等对待所有专家。这种损失确保所有专家获得大致相同数量的训练样本。后续章节还将探讨“专家容量”的概念，这涉及到一个专家能处理的 tokens 数量上限。在 `transformers` 中，这种辅助损失可以通过 `aux_loss` 参数来调节。
## MoEs 和 Transformers
Transformers 模型展示了一个明显的趋势：增加参数的数量可以显著提高性能。Google 的 [GShard](https://arxiv.org/abs/2006.16668) 项目正是在这方面进行了深入探索，试图将 Transformers 模型扩展到超过 6000 亿个参数。
## 专家在学习中角色和专长
解码器的专家倾向于特定的 Token 组或基础概念。例如可能形成专门处理标点符号或转悠名词的专家，而解码器的专家则在专业化方面表现的较为平均。
## 增加专家数量对预训练的影响
增加更多的专家可以提高样本效率和加速训练过程，但增益逐渐减少（特别是在达到 256 或 512 个专家后），并且在推理过程中需要更多的 VRAM。在大规模应用中研究的 Switch Transformers 的特性，在小规模应用中也得到了验证，即便是每层只有 2、4 或 8 个专家
## 微调 MoE 技术
Mixtral 软件已经在 transformers 4.36.0 版本中得到支持，您可以通过运行 `pip install "transformers==4.36.0 --upgrade"` 命令进行安装。
密集型模型和稀疏型模型在过拟合上表现出明显不同的特点。稀疏型模型更易于过拟合，因此我们可以尝试在专家系统内部应用更强的正则化手段，例如不同层次的 dropout 率——对密集层和稀疏层分别设置不同的 dropout 率。

在微调过程中，一个关键的决策是是否采用辅助损失。ST-MoE 的研究人员尝试关闭辅助损失，并发现即使高达 11% 的 Token 被丢弃，模型的质量也几乎不受影响。这表明 Token 丢弃可能是一种有效的防止过拟合的正则化策略。

另一个尝试是冻结所有非专家层的权重，结果如预期那样导致了性能大幅下降，因为 MoE 层占据了网络的大部分。相反，仅冻结 MoE 层的参数几乎能达到更新所有参数的效果。这种方法可以加速微调过程，同时减少内存使用。
通过仅冻结 MoE 层，我们不仅能加快训练速度，还能保持模型的质量。这些发现同样源于 ST-MoE 的研究论文。 

## 何时选择稀疏 MoEs 和稠密模型？

在多机器、高吞吐量的场景中，专家系统是非常有效的。如果预训练的计算预算有限，那么稀疏模型将是更佳的选择。对于 VRAM 较少、吞吐量低的情况，稠密模型则更为合适。

**注意：** 我们不能直接比较稀疏和稠密模型之间的参数数量，因为这两种模型代表的是完全不同的概念。

1. 稀疏模型：
    - 定义：稀疏模型是一种模型设计策略，其中模型结构被设计为具有少量的参数或专家（Experts），每个专家对应于模型中的一个子模型。
    - 特点：
        - 模型结构简单，仅包含少量的专家。s
        - 每个专家专注于处理特定的输入模式或任务。
        - 专家之间在参数空间上是相互独立的。
        - 在推理阶段，只有少量的专家被激活并参与预测，以提高计算效率。
    - 优点：
        - 可以处理复杂的输入模式，并对不同的任务进行专门化处理。
        - 可以节省模型的参数量和计算资源。
    - 缺点：
        - 需要设计合适的选择机制和激活策略，以确保在不同输入情况下激活合适的专家。
        - 对于一些特定的输入模式，可能需要较大的专家数量才能达到较好的性能。
2. 稠密模型：
    - 定义：稠密模型是一种模型设计策略，其中模型结构被设计为具有更多的参数和层级，以便能够更全面地学习输入数据的特征和表示。
    - 特点：
        - 模型结构相对更复杂，包含多个参数较多的层。
        - 在训练过程中，模型可以通过反向传播算法端到端地学习输入数据的特征。
        - 通常用于处理输入模式和任务较为均衡的情况。
    - 优点：
        - 可以捕获输入数据中更丰富的特征，并具有更强的表达能力。
        - 可以适应不同的输入模式和任务，并在训练过程中自动学习特征的权重和表示。
    - 缺点：
        - 参数较多，需要更多的计算资源和内存空间。
        - 在处理特定输入模式和任务时，可能存在过拟合的风险。

![[Pasted image 20240311104630.png]]


# 最新进展
- [The Sparsely-Gated Mixture-of-Experts Layer (2017)](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1701.06538)  
    [稀疏门控专家混合层（2017）](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1701.06538)
- [GShard: Scaling Giant Models with Conditional Computation and Automatic Sharding (2020)](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2006.16668)  
    [GShard：通过条件计算和自动分片扩展巨型模型（2020）](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2006.16668)
- [MegaBlocks: Efficient Sparse Training with Mixture-of-Experts (2022)](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2211.15841)  
    [MegaBlocks：高效稀疏训练与专家混合（2022）](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2211.15841)
- [Mixture-of-Experts Meets Instruction Tuning (2023)](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2305.14705)  
    [混合专家遇见指令调整（2023）](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2305.14705)

[万字长文详解 MoE - 超越ChatGPT的开源混合专家模型 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/674162664)



[1. Diederik P. Kingma, Jimmy Ba. “Adam: A Method for Stochastic Optimization.” International Conference on Learning Representations(2014).](https://www.semanticscholar.org/paper/a6cb366736791bcccc5c8639de5a8f9636bf87e8) 

[2. Ashish Vaswani, Noam M. Shazeer et al. “Attention is All you Need.” Neural Information Processing Systems(2017).](https://www.semanticscholar.org/paper/204e3073870fae3d05bcbc2f6a8e263d9b72e776) 

[3. Jimmy Ba, J. Kiros et al. “Layer Normalization.” arXiv.org(2016).](https://www.semanticscholar.org/paper/97fb4e3d45bb098e27e0071448b6152217bd35a5) 

[4. Rongjie Yi, Liwei Guo et al. “EdgeMoE: Fast On-Device Inference of MoE-based Large Language Models.” arXiv.org (2023).](https://doi.org/10.48550/arXiv.2308.14352) 

[5. Alexander Hauptmann. “From Syntax to Meaning in Natural Language Processing.” AAAI Conference on Artificial Intelligence (1991).](https://www.semanticscholar.org/paper/23894a64bcd7db9007c90fd201264d113e67b6a7) 

[6. Chenguang Zhu, Yichong Xu et al. “Knowledge-Augmented Methods for Natural Language Processing.” Annual Meeting of the Association for Computational Linguistics (2023).](https://doi.org/10.1145/3539597.3572720) 

[7. Dastan Hussen Maulud, Subhi R. M. Zeebaree et al. “State of Art for Semantic Analysis of Natural Language Processing.” Qubahan Academic Journal (2021).](https://doi.org/10.48161/QAJ.V1N2A44) 

[8. Neha Yadav. “Applications Associated With Morphological Analysis And Generation In Natural Language Processing.” (2017). 284-286.](https://www.semanticscholar.org/paper/a1730ffaf9701f9bc66962fe3823f4c4404bccdd)

