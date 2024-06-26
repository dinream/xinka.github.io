多视图混合密码猜测
# 摘要

1. 集合多种模型进行密码猜测（也称为混合密码猜测），从而更好地捕获真实破解者的破解能力
2. 问题提出：为什么整合模型会有效果，如何整合模型还没有研究，
3. 本文从多视图学习的概念中汲取灵感：
	1. 将各种密码猜测模型生成的猜测列表视为数据的多个视图。
	2. 通过对这些密码猜测列表的分析发现了混合密码猜测模型能增强破解能力的原因：
		1. 集成更多元的视图可以覆盖更广泛的异构密码特征。
		2. 并提供更详细的有效密码信息分布。
4. 提出了一种新的密码猜测框架：GuessFuse 
	1. 采用多视图子集提取模块和分段分割选择模块，从多个猜测列表中准确提取和重组有效密码。
5. 在 六个大型数据集中证明了 GuessFuse 的有效性。
	1. 通过组合两个（或五个）猜测列表，GuessFuse 在 107 个猜测中比其最重要的同行平均高出 11.00% ∼ 59.62%（或 4.70% ∼ 17.66%）

# 引言
1. 密码使用频繁
2. 密码破解器的性能是密码强度估计器的一个设计标准。
3. 密码猜测分类：数据驱动和基于规则。当前主要是数据驱动方法效果更好。
	1. PCFG [6] 对具有简单结构的密码（例如，passsword123）产生更好的猜测性能
	2. 马尔可夫模型 [22] 对于具有上下文相关特征的密码（例如，1qaz2wsx）更有效。
4. 但是现实世界的破解专家能力大于单一的模型能力。
5. 混合密码猜测的研究在初级阶段。
	1. [16]通过并行应用多个密码猜测模型，设计了一个 Minauto 指标作为混合密码猜测的上限。
	2. 一些研究[29]-[33]试图在结构层面整合特定模型的优点（例如，分别在密码结构层面和字符串层面应用PCFG模型和马尔可夫模型进行密码猜测[29]） 。
	3.  另一方面，一些研究[32]、[34]实验性地组合来自不同密码猜测模型的多个猜测列表，以实现更准确的密码猜测。
6. 然而，上述方法要么集成了两种特定的密码猜测模型，要么简单地组合猜测列表，而不是从机制上解决这个问题。未能有效利用多种异构密码猜测模型的优势。
7.  更重要的是，据我们所知，为什么集成多个密码猜测模型可以增强破解能力尚未得到满意的答案。
8. 我们将各种密码猜测模型生成的猜测列表视为数据的多个视图。 通过对多个猜测列表组合的深入分析，我们揭示了混合密码猜测能够增强破解能力的关键原因。
9.  同时我们还发现多个视图之间的子集中的密码也遵循Zipf定律。

基于这些关键发现，我们提出了一种基于多视图学习的混合密码猜测框架，名为 GuessFuse。 GuessFuse 可根据以下工作流程从巨大的密码空间中实现准确的密码猜测结果。
1. GuessFuse 首先使用单独的密码猜测模型（例如 PCFG 和马尔可夫模型）生成多个密码猜测列表。 
2. 然后，使用多视图子集提取方法提取多个猜测列表之间的相交和互补子集。
3. 接下来，GuessFuse 根据幂律将子集分割成密码段。 
4. 大量的实验表明，GuessFuse 的性能优于同类产品，并且能够有效地整合多种密码猜测模型的优点。
5. 我们还将 GuessFuse 应用于 PSM 进行更广泛的比较分析。 我们发现 Minauto指标低估了某些密码的强度，从而降低了密码设置的可用性。 使用 GuessFuse 可以解决这个问题。
# 准备工作

## 相关研究
1. 密码破解方式概述
2. 密码强度指标 minauto ， 但Minauto指标只是理论上的上限，并行使用多个密码猜测模型比使用单个模型需要多数倍的猜测次数，与实际猜测场景的要求不一致。
3. 混合密码猜测模型提出但是处于起步阶段。 根据集成对象的划分，现有的混合密码猜测方法可以分为模型架构级别的集成和猜测列表级别的集成。
	1. 模型架构级别：
		1. 2018 年，Zhang 等人。 [29]提出了一种称为 SPSR 的混合密码猜测模型。 SPSR将PCFG模型[6]应用于密码结构层，将马尔可夫链模型[22]应用于密码字符串层。 
		2. 同年，他们还提出了SPRNN模型[30]，该模型结合了结构划分和双向长短期记忆递归神经网络（BiLSTM）。 
		3. 与 SPRNN 相反，Xia 等人。 [31]提出了另一种混合密码猜测模型，称为 PL。 PL采用PCFG模型[6]进行密码字符串层解构，并采用LSTM模型[15]进行结构层建模。 
		这些双重方法有效地利用了两种密码猜测模型在不同密码分析粒度层的优势。 然而，这些模型架构级别的集成需要有针对性的定制，限制了它们集成更广泛的密码猜测模型的能力。
	2. 猜测列表级别：
		1. 2021 年，王等人。 [32]平均组合多个密码猜测列表并对输出进行重复数据删除。 他们发现，将密码猜测模型与明显不同的密码生成策略（例如RNN模型[39]和PCFG模型[6]）相结合可以有效提高破解成功率。 
		2. 2022 年，帕里什等人。 [34]对训练集中的密码进行去重，并按密码频率降序排序，生成猜测列表。 他们将其定义为“身份猜测者”。 他们发现，将模型生成的多个猜测列表与 Identity Guesser 相结合，可以显着提高破解成功率。 
		不过，这两项研究仅在猜测清单层面进行了初步尝试。 同等地组合猜测列表并不能准确地从每个猜测模型中提取有效部分。
		3. 2022 年，Han 等人。 [33]介绍了一种称为 hyPassGu 的混合猜测框架。 hyPassGu 通过限制每个模型生成目标类型的密码并分别确定猜测次数，利用 PCFG 和马尔可夫模型的优势。 尽管声称 hyPassGu 可以应用于 PCFG 和马尔可夫之外的其他模型，但它需要先了解模型的架构及其目标密码类型。 因此，它不能直接应用于其他模型。 另外，hyPassGu根据密码的结构特征将密码粗略地分为两类，导致有效密码大量流失。 因此，hyPassGu的破解成功率低于单个模型，仅略优于限制特定类型密码生成的模型。
4. 关键问题仍然没有令人满意的答案：
	（1）什么有助于提高混合密码猜测的破解能力？ 
	（2）如何有效发挥多种不同模型的优势？ 本文重点解决这些问题。

## 密码猜测场景

**数据驱动的密码猜测模型 + 拖网猜测 + 混合密码猜测**
1. 攻击者首先根据泄露的数据集构建密码猜测模型；
2. 然后使用模型生成猜测；
3. 最后尝试使用猜测破解所有目标密码。 

虽然基于规则的密码猜测工具[20]、[21]也可用于混合密码猜测，但这些工具仍然依赖于目标数据集分布的先验知识来增强密码猜测的有效性。

注意：
拖网场景可以进一步分为站内场景和跨站场景。 虽然跨站密码猜测场景较为真实，但攻击者一般以掌握目标数据的部分分布信息为前提来猜测密码。
因此，为了更好地描述混合密码猜测的概念，我们将站点内拖网场景中的密码猜测任务形式化。
**形式化**：一些符号表示，某个猜测密码成功破解了目标系统的多少个密码

## 多视角学习
多视图学习的概念与混合密码猜测的概念非常吻合。 多视图学习引入了一个函数来对特定视图进行建模，并联合优化所有函数以利用相同输入数据的冗余视图，从而提高学习性能[40]。 
在更高层次上，多视图学习构建多个单一视图并评估它们的表现，然后设计功能来组合这些视图以改善学习成果。
同样，混合密码猜测旨在通过利用从不同角度分析数据的多个模型来提高密码猜测的有效性。 因此，多视图学习可以用于解决在混合密码猜测中集成各种密码猜测模型的优势的挑战。

徐等人。 [41]提出多视图学习基于两个关键原则有效地利用多种视图：
1. 共识和互补。 共识原则旨在最大限度地达成不同观点之间的一致。 
2. 互补原则指出，每种观点都可能包含其他观点所不存在的独特知识。 通过有效利用多种观点的共识和互补性，可以实现对数据的全面、准确的描述。 
在本文中，我们基于这些原理探索混合密码猜测。


## 数据集
使用包含 5400 万个纯文本密码的 6 个数据集（参见表 I）。 
其中，三个数据集来自英文网站，三个数据集来自中文网站，涵盖六种不同的服务类型。 
数据源的多样性有助于减少我们分析中的偏差。 由于我们所有的数据集都可以在互联网上公开获取，因此这项工作的结果是可重复的。
![[Pasted image 20240418173807.png]]

### 数据清洗
原始数据集包含异常情况，例如未解密的哈希字符串、不可能由用户生成的过长密码以及包含不符合标准密码策略的不可打印字符的密码。 因此，我们首先对数据集进行数据清洗。 我们删除包含 95 个可打印 ASCII 字符之外的字符的密码，并删除长度 > 30 的密码。这种数据清理策略在现有的密码猜测文献中也很流行 [24]、[26]、[42]。

### 道德考虑
尽管我们使用的数据集是公开的，并广泛用于密码猜测研究 [8]、[27]、[32]–[34]，但这些数据集包含敏感的个人信息。 因此，我们仅分析密码数据的分布特征，并报告汇总的统计信息。 我们不会将任何数据用于学术研究以外的目的，因此不会增加受影响个人的风险。

## 密码猜测模型
采用了四种领先的密码猜测模型（例如 Markov、PCFG、FLA 和 RFGuess）。

为了减轻其他因素对分析结果的影响，我们重点关注站内密码猜测场景。 这种情况被认为是理想的，因为训练集和测试集具有相同的数据分布，确保了我们分析的一致性。

我们从数据集中随机抽取大小为 $10^6$ 的子集作为测试集，而其余数据用作训练集。 我们发现结果对于基于此测试集大小的分析来说足够稳定。 这一现象与论文[9]、[43]、[44]一致。 用于生成猜测列表的每个模型的设置如下：
 1）PCFG。 我们使用了 PCFG 模型 [6] 的 4.0 版，该模型可以在 GitHub 网站上找到。 该模型的语法包括六个段类别：字母A、数字D、其他字符O、键盘D、特殊字符串X和年份Y。
 2）Markov。 我们选择 4 阶马尔可夫模型，并采用[26]中使用的拉普拉斯平滑和结束符号正则化来生成猜测。 
 3）FLA。 我们利用了 GitHub 网站上提供的 FLA 开源代码2，并遵循[15]中指定的推荐参数。 我们训练了一个由三个 LSTM 层组成的模型，每层有 128 个单元和两个密集层，总共 20 个 epoch。 
 4）RFGuess。 参考[24]，我们训练了一个有 30 棵决策树的随机森林。 其叶子节点的最小数量为10，特征的最大比例为80%，其余为scikit-learn框架的默认值[45]。



# 分析

 本节通过多视角分析，依次解决以下问题：
	（1）混合密码猜测能够提高破解能力的关键原因是什么？
	（2）使用猜测列表进​​行混合密码猜测可以获得什么好处？
## 混合密码猜测的本质
### 数据集比较
我们通过比较每个模型生成的猜测列表来展示密码猜测模型之间的差异
![[Pasted image 20240418204958.png]]
1. PCFG 模型生成的猜测列表包括诸如 “dearbook123456” 和 “DEARBOOK” 之类的密码，这表明偏向于生成包含常见短语和简单结构的密码。 
2. Markov 模型生成的猜测列表包含 “woshili” 和 “1989123” 等密码，反映了基于密码内字符相关性进行猜测的偏见。
3. RFGuess 模型的猜测列表包括诸如 “1234567” 和 “1234567}” 之类的密码，表明了将常见短语与各种特征相结合的趋势。 
4. FLA 模型与马尔可夫模型类似，会生成一个包含 “45665456” 和 “qwqwqwqww” 等密码的猜测列表，显示出其对密码中顺序上下文一致性的偏见。 然而，FLA 模型生成的猜测列表也与马尔可夫模型不同。
5. 中心区域还有“dearbook”、“12345678”等密码。 它表明，尽管每个模型都有独特的偏见，但他们已经就某些密码达成了共识。 这些密码可能很常用，并且很容易被各种模型预测到。

### 为什么需要混合
猜测列表中的相似点和差异强调，即使基于相同的数据集，现有的密码猜测模型也可以生成包含不同密码特征的猜测列表。

本质上，密码猜测模型是根据用户设置易受攻击密码的行为而设计的。通过准确识别和匹配这些弱密码的特征，模型就可以生成破戒律很高的猜测列表。然而，由于用户密码设置行为的异构多样性[42]。即使像 PCFG 这样的已经涵盖了各种弱密码特征的模型（参见第II-E节），也无法在有限的猜测次数内完全覆盖所有特征 。 利用多种具有不同偏差的密码猜测模型的组合，可以更全面地识别和匹配密码特征，从而提高密码猜测的性能。

### 怎么混合
1. 在模型的结构层面上利用多个异构密码猜测模型是相当具有挑战性的，并且不适用于新提出的密码猜测模型（例如RFGuess），这背离了使用多个异构模型的最初想法。
2. 密码猜测模型最终会生成猜测列表。 鉴于多视图学习对于混合密码猜测的天然适用性（参见第 II-C 节），我们将密码猜测模型生成的猜测列表视为数据的多个视图。
	1.  在多视图学习领域，直观的是，集成多个不同视图来分析和处理数据可以产生更全面的信息，从而提高学习模型的性能。 这个概念也适用于混合密码猜测。 
	2. 我们用一个示意性的解释来描述这一点。 
		![[Pasted image 20240418211929.png]]
	3. 如图3所示，不同的密码猜测模型生成的猜测列表{G1，G2，G3，····，Gl}可以覆盖测试集T的不同部分。假设混合密码猜测方法H可以有效地提取出测试集T的不同部分。 从{G1,G2,G3,····,Gl}中覆盖T的密码，生成优化猜测列表eG。 
	4. 组合的猜测列表越多，测试集的覆盖范围就越大，从而产生更好的破解能力。 
	5. 请注意，图 3 中描绘的示意图可能并不通用，因为猜测列表中的密码通常存在重叠。 然而，组合更多猜测列表可以覆盖更多测试集的因果关系是普遍存在的。 我们将通过下面的分析来证明这一点。

## 多视图集成的优点
依据：
1. 大量实验已经证实，我们所描述的结果在六个数据集中得到一致观察，从而证实了它们的普遍性
2. 以 CSDN 数据集生成的 top $10^6$ 密码猜测列表为例。
3. 
实验：
定量特征分析：
对组合猜测列表的统计特征进行定量比较，以证明多视图集成带来的猜测有效性。
1. 将每个模型生成的猜测列表以相同的大小组合起来，并对它们进行重复数据删除，然后将结果与单个猜测列表进​​行比较。
	例如，在比较大小为 $10^3$ 的猜测列表时，我们结合 PCFG 和马尔可夫模型生成的前 $10^3$ 个猜测，删除任何重复的密码。 输出被定义为两个视图集成。 
	请注意，这种组合策略与 Minauto 指标 [16] 的计算相同，并不反映现实场景 ( Minauto 指标是一个理论上的上限，代表了每个密码被所有密码猜测模型破解所需的最小猜测次数。然而，在实际的密码猜测场景中，使用多个密码猜测模型并行进行猜测需要的次数是单个模型的数倍，这与实际需求不一致)。
	
2. 我们的目的只是证明通过跨多个视图的特征量化来增强密码破解能力的可行性。
	![[Pasted image 20240419151949.png]]
	密码结构 Struct：基于 PCFG 算法来分析猜测列表中的密码结构的数量。 例如，如果猜测列表包含 “dearbook134”、“dearbook309” 和 “123dearbook” 等密码，则它包含两种类型的密码结构： “A8D3” 和 “D3A8”。
	有效密码比例 Effect：有效密码/猜测列表大小。猜测列表中的密码如果能够与测试集中的密码匹配，则认为该密码有效。
	破解成功率 $Min_{auto}$
	唯一密码比例 Uniq：唯一密码/单个单个猜测列表 
	

3. 结果：
	1. Effect 和  $Min_{auto}$ 说明了多视图集成所带来的猜测有效性的提高。
	2. Uniq 和 struct 说明通过集成多个视图实现的多样性的增强。整合更多元的观点可以增加猜测的多样性，覆盖更多的密码特征，从而提高密码猜测的有效性

有效的密码分析：
问题：组合多个猜测列表会产生许多独特的密码，其大小远远超过单个猜测列表的大小。 如果全部用于最后的破解，会导致破解次数消耗过多。
![[Pasted image 20240419155303.png]]
幸运的是，图4（a）表明，当猜测数量增长到大约 $4×10^4$ 时，组合多个猜测列表的有效密码数量开始低于单个猜测列表的大小（an注：从一个数据集中选择 100 比 四个数据集中每个选择 25 个效率高）。
在这种情况下，通过选择这些有效密码中的一部分，即使优化后的猜测列表大小小于单个猜测列表的大小，猜测的成功率仍然可以达到多视图集成的 Minauto 指标。（an注：此时单猜测列表 和 多猜测列表分母增加相同大小时， 单猜测列表的分子增加的多，论文描述的意思是减少了 多猜测列表分母增加的个数，有点偷换概念的意思）


在了解测试集的前提下，我们假设了一种最优的混合密码猜测方法，该方法可以有效地整合多个猜测列表以产生优化的猜测列表。 此混合密码猜测方法生成的优化猜测列表包含每个猜测列表中最有效的密码。 如果猜测的密码在测试集中破解频率最高的密码，则认为该密码具有最高的效率。 例如，在整合大小为103的猜测列表时，我们首先对猜测的密码进行去重，并将其与测试集进行比较，计算每个猜测的密码可以破解的测试密码的数量。 然后，我们按照破解次数降序选择猜测的密码，并将其添加到最佳猜测列表中，直到最佳猜测列表的大小也达到103。我们将最佳猜测列表的破解成功率与最佳猜测列表的破解成功率进行比较。 由单一密码猜测模型生成的猜测列表（见图4（b））。 如图 4（b）所示，当猜测数 < 4 × 104 时，与 PCFG 模型相比，整合两视图的最优猜测列表的破解成功率平均提高了 1.3%，整合三视图的破解成功率平均提高了 1.3%。 2.6%，整合四视图增长了2.8%。 尽管有效密码的数量超过了单个猜测列表的大小，但选择效率最高的部分同等大小的猜测密码仍然可以增强破解能力。 

上述结果表明，从多个视图中准确提取有效密码是进行有效混合密码猜测的关键。


多视图子集分析：
目的：为了找到从多个视图中提取有效密码的有效方法，我们对多个视图之间的子集进行了分析。
手段：对不同模型生成的相同大小的猜测列表进​​行集合运算，以获得相交和互补的子集。
![[Pasted image 20240419165944.png]]


我们分析了集成不同数量的视图时多视图子集的变化（见图 5）。 如图 5 所示，集成两个视图生成三个子集，三个视图集成生成七个子集，四个视图集成生成 15 个子集。 此外，随着集成视图数量的增加，每个子集中有效密码的分布变得更加集中。 相交子集中有效密码的比例（中间红色数值表示）随着集成视图的增加而增加。 在基于CSDN数据集的实验中，融合两个视图时，相交子集中有效密码的比例为51.51%。 当集成三视图时，这一比例增加了 21.0%，而当集成四视图时，这一比例又增加了 3.9%。 这些结果表明，集成多个视图可以提供有关有效密码分发的增量信息。 随着更多视图的整合，增量信息增多，有效密码的分布更加集中。

![[Pasted image 20240419170332.png]]



我们继续分析有效密码在各个子集中的分布。 目前，当我们仅通过集合运算提取猜测列表中的相交和互补子集时，子集中的密码不包括顺序。 因此，我们通过分析不同大小的猜测列表中子集的平均破解成功率来评估子集中有效密码的分布（见图7）。 为了便于表示，我们用图6来表示每个平均破解成功率所代表的子集。 具体来说，我们使用 4 位二进制代码来命名每个子集，其中每个位代表密码在特定模型生成的猜测列表中的出现，从左到右分别为：PCFG、FLA、Markov 和 RFGuess。 例如，“1000”表示该子集中的密码仅出现在PCFG猜测列表中。

![[Pasted image 20240419170602.png]]

如图7所示，相交子集“1111”的整体破解效率明显优于其他子集。 然而，相交子集中的密码的后半部分并不优于其他子集中的密码的前半部分。 例如，相交子集“1111”在101次猜测中的平均破解成功率为3.57×10−6，而互补子集“0010”在101次猜测中的平均破解成功率为2.61×10−5。 有趣的是，子集的平均破解成功率与单个猜测列表的大小呈现近似线性关系。 当我们将 15 条曲线作为一个整体来考虑时，这一点变得更加明显。 这可能符合密码中的 Zipf 定律 [46]。 我们使用PDFZipf模型[46]拟合数据：logfr = logC − s · logr，其中fr和r分别对应于平均破解成功率和单个猜测列表的大小。 如表III所示，C ε [-3.55，-0.85]和s ε [-0.92，-0.63]是常数。 拟合的 RMSE 在 [0.08, 0.24] 范围内。

这种现象有助于在混合密码猜测中利用多种密码猜测模型的优势。 尽管密码分布中的齐普夫定律通常用于解释用户设置密码的行为，但也有研究 [33]、[47] 通过分析密码猜测模型中齐普夫定律的存在来改进密码猜测模型。 在我们的例子中，齐普夫定律表明，猜测密码的效率随着密码在子集中的排名降序而下降。 换句话说，子集中排名较高的密码被认为具有较高的破解效率。







# 密码结构

密码结构（Struct.）的数量是通过对猜测列表中的密码进行分析和统计得出的。具体来说，密码结构是指密码中的字符排列方式，例如"A8D3"或"D3A8"。在文中，作者使用PCFG算法对密码结构进行分析，通过猜测列表中的密码来确定不同类型的密码结构的数量。通过对密码结构的数量进行统计分析，可以评估多视图整合对密码破解能力的提升效果。这些统计特征表明，整合更多不同的视图可以增加猜测的多样性，覆盖更多的密码特征，从而提高口令猜测的有效性。

# Minauto
类似于一个破解成功率。


# 多视图提取模块的工作原理是什么？

多视图提取模块的工作原理是从多个猜测列表中提取交集和补集密码，以生成多视图子集。该模块首先根据输入的猜测列表数量生成多个子集，并为每个子集分配逻辑标签。然后，根据排名，模块按照密码的逻辑规则将密码分别添加到交集子集或补集子集中。最终，该模块输出维护的多视图子集，其中包含了有效密码的信息。这一过程有助于准确地从多个视图中提取和重新组织有效密码。

# 分割选择模块的作用是什么？

分割选择模块的作用是进一步细化每个子集，根据幂律间隔将其分割成多个密码片段。该模块利用幂律分布间隔序列来将输入的子集分割成更细粒度的密码片段。然后，该模块利用验证集来评估这些密码片段的破解效率，具体来说，该模块计算这些密码片段的平均破解成功率。最终，该模块重新组织有效的密码片段，生成最终的优化猜测列表。
