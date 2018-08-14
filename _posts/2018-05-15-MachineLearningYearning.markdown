---
layout:     post
title:      "Machine Learning Yearning,Andrew NG"
subtitle:   "Machine Learning Yearning,Technical Strategy for AI Engineers,In the Era of Deep Learning. Andrew NG"
date:       2018-05-15 21:00:00
author:     "石头人m"
header-img: "img/post-bg-wali.jpg"
header-mask: 0.3
catalog:    true
tags:
    - 机器学习
    - 深度学习
---

Machine Learning Yearning,Technical Strategy for AI Engineers,In the Era of Deep Learning. Andrew NG.

Machine Learning Yearning is a deeplearning.ai project.

http://www.mlyearning.org/

### 1 Why Machine Learning Strategy
机器学习（machine learning）是无数重要应用的基础，其包含网络搜索、垃圾邮件检测、语音识别以及产品推荐等内容。假如你和你的团队正在研发一项机器学习应用，并且想要取得较为快速的进展，本书的一些内容将会有所帮助。

![](/img/mldl/Ng-MLY/01-01.jpeg)

### 2 How to use this book to help your team
完成本书的阅读后，你将对于“如何在机器学习项目中设定一个技术方向”有着深层次的了解.

优先级的稍加改变会对团队的生产力产生巨大的影响，我希望你能通过帮助团队进行一些有效的改变，从而成为团队里的超级英雄！

![](/img/mldl/Ng-MLY/02-01.jpeg)

### 3 Prerequisites and Notation
监督学习（supervised learning）是指使用已标记（labeled）的训练样本  来学习一个从  映射到  的函数。监督学习算法主要包括线性回归（linear regression）、对数几率回归（logistic regression，又译作逻辑回归）和神经网络（neural network）。虽然机器学习的形式有许多种，但当前具备实用价值的大部分机器学习算法都来自于监督学习。

### 4 Scale drives machine learning progress
不少关于深度学习（神经网络）的想法已经存在了数十年，而这些想法为什么现在才流行起来了呢？

有两个主要因素推动着近期的发展：
- 数据可用性（data availability）
- 计算规模（computational scale）

具体而言，即使你积累了更多的数据，但应用在类似于对数几率回归（logistic regression）这样的旧学习算法上，其性能表现（performance）也将趋于”平稳“。这意味着算法的学习曲线将”变得平缓“，即使提供更多的数据，算法的性能也将停止提升。

![](/img/mldl/Ng-MLY/04-01.jpeg)
旧的学习算法似乎并不知道要如何来处理如今这个规模量级的数据。

如果你在相同的监督学习任务上选择训练出一个小型的神经网络（neutral network, NN），则可能会获得较好的性能表现：

![](/img/mldl/Ng-MLY/04-02.jpeg)
该图显示了在小数据集上应用神经网络的效果会更好，但这种效果与将神经网络应用在大数据集时不太一致。 在小数据集条件下，传统算法是否会表现得更好，取决于人们如何进行特征工程。例如，假设你只有 20 个训练样本，那么使用对数几率回归还是神经网络可能无关紧要；此时人为的特征工程将比让算法进行选择产生更大的影响。但如果你有 100 万个样本数据，我会赞同使用神经网络。

这里的”小型神经网络“指的是只含有少量的隐藏元/层数/参数的神经网络。但如果你训练的神经网络规模越来越大，反而能够获得更好的表现：

![](/img/mldl/Ng-MLY/04-03.jpeg)
因此，为了获得最佳的性能表现，你可以这样做：

(i) 训练大型的神经网络，效果如同上图的绿色曲线；

(ii) 拥有海量的数据。

在算法训练时，许多其它的细节也同等重要，例如神经网络的架构。但目前来说，提升算法性能的更加可靠的方法仍然是训练更大的网络以及获取更多的数据。

完成 (i) 和 (ii) 的过程异常复杂，本书将对其中的细节作进一步的讨论。我们将从传统学习算法与神经网络中都起作用的通用策略入手，循序渐进地讲解至最前沿的构建深度学习系统的策略。


## Setting up development and test sets

### 5 Your development and test sets
在大数据时代来临前，机器学习中的普遍做法是使用 70% / 30% 的比例来随机划分出训练集和测试集。这种做法的确可行，但在越来越多的实际应用中，训练数据集的分布与人们最终所关心的分布情况往往不同，此时执意要采取这样的划分则是一个坏主意。

我们通常认为：

- 训练集（training set）用于运行你的学习算法。

- 开发集（development set）用于调整参数，选择特征，以及对学习算法作出其它决定。有时也称为留出交叉验证集（hold-out cross validation set）。

- 测试集（test set）用于评估算法的性能，但不会据此决定使用什么学习算法或参数。

在定义了开发集（development set）和测试集（test set）后，你的团队将可以尝试许多的想法，比如调整学习算法的参数来探索出哪些参数的效果最好。开发集和测试集能够帮助你的团队快速检测算法性能。

换而言之，开发集和测试集的使命就是引导你的团队对机器学习系统做出最重要的改变。

所以你应当这样处理：

    合理地设置开发集和测试集，使之近似模拟可能的实际数据情况，并处理得到一个好的结果。

这就要求你主观地进行判断，应该投入多少来确定一个理想的开发集和测试集，但请不要假定你的训练集分布和测试集分布是一致的。尽可能地选出能够反映你对最终性能期望的测试样本，而不是使用那些在训练阶段已有的数据，这将避免不必要的麻烦。

### 6 Your dev and test sets should come from the same distribution
一旦定义了开发集和测试集，你的团队将专注于提高开发集的性能表现，这就要求开发集能够体现任务的核心：使算法在所有地区都表现优异，而不仅仅是其中的两个。

开发集和测试集的分布不同还将导致第二个问题：你的团队开发的系统可能在开发集上表现良好，但在测试集上却表现不佳。我目睹过这样的事情发生，这将让人十分沮丧并且浪费大量的时间，因此希望你不要重蹈覆辙。

举个例子，假设你的团队开发了一套能在开发集上运行性能良好，却在测试集上效果不佳的系统。如果开发集和测试集分布相同，那么你就会非常清楚地知道问题在哪：在开发集上过拟合了（overfit）。解决方案显然就是获得更多的开发集数据。

但是如果开发集和测试集来自不同的分布，解决方案就不那么明确了。此时可能存在以下一种或者多种情况：
1. 在开发集上过拟合了。
2. 测试集比开发集更难进行预测，尽管算法做得足够好了，却很难有进一步的改进空间。
3. 测试集不一定更难预测，但与开发集性质不同（分布不同）。因此在开发集上表现良好的算法不一定在测试集上也能够表现良好。如果是这种情况，大量改进开发集性能的工作将会是徒劳的。

构建机器学习应用已是一件难事，而开发集和测试集分布的不匹配又会引入额外的不确定性，即提高算法在开发集上的性能表现，是否也会提升其在测试集的性能表现？在这样的情况下很难弄去清楚哪些工作是有效的，哪些工作是在浪费时间，从而会影响到工作的优先级确定。

在处理第三方基准测试（benchmark）问题时，提供方可能已经指定了服从不同分布的开发集和测试集数据。与数据分布一致的情况相比，此时运气带来的性能影响将超过你所使用的技巧带来的影响。找到能够在某个分布上进行训练，并能够推广到另一个分布的学习算法，是一个重要的研究课题。但如果你想要在特定的机器学习应用上取得进展，而不是搞研究，我建议你尝试选择服从相同分布的开发集和测试集数据，这会让你的团队更有效率。

### 7 How large do the dev/test sets need to be?
开发集的规模应该大到足以区分出你所尝试的不同算法间的性能差异。例如，如果分类器 A 的准确率为 90.0% ，而分类器 B 的准确率为 90.1% ，那么仅有 100 个样本的开发集将无法检测出这 0.1% 的差异。相比我所遇到的机器学习问题，一个样本容量为 100 的开发集的规模是非常小的。通常来说，开发集的规模应该在 1,000 到 10,000 个样本数据之间，而当开发集样本容量为 10,000 时，你将很有可能检测到 0.1% 的性能提升。

从理论上说，还可以检测算法的变化是否会在开发集上造成统计学意义上的显著差异。 然而在实践中，大多数团队并不会为此而烦恼（除非他们正在发表学术研究论文），而且我在检测过程中并没有发现多少有效的统计显著性检验。

像广告服务、网络搜索和产品推荐等较为成熟且关键的应用领域，我曾见过一些团队非常积极地去改进算法性能，哪怕仅有 0.01% 的提升，因为这将直接影响到公司的利润。在这种情况下，开发集规模可能远超过 10,000 个样本，从而有利于检测到那些不易察觉的效果提升。

那么测试集的大小又该如何确定呢？它的规模应该大到使你能够对整体系统的性能进行一个高度可信的评估。一种常见的启发式策略是将 30% 的数据用作测试集，这适用于数据量规模一般的情况（比如 100 至 10,000 个样本）。但是在大数据时代，我们所面临的机器学习问题的样本数量有时会超过 10 个亿，即使开发集和测试集中样本的绝对数量一直在增长，可总体上分配给开发集和测试集的数据比例正在不断降低。可以看出，我们并不需要远超过评估算法性能所需的开发集和测试集规模，即开发集和测试集的规模并不是越大越好。

### 8 Establish a single-number evaluation metric for your team to optimize
所谓的单值评估指标（single-number evaluation metric）有很多，分类准确率就是其中的一种：你在开发集（或测试集）上运行分类器后，它将返回单个的数据值，代表着被正确分类的样本比例。根据这个指标，如果分类器 A 的准确率为 97％，而分类器 B 的准确率为 90%，那么我们可以认为分类器 A 更优秀。

相比之下，查准率（Precision，又译作精度）和查全率（Recall，又译作召回率）均不是单值评估指标，因为它给出了两个值来对你的分类器进行评估。多值评估指标将使算法之间的优劣比较变得更加困难。

你的团队在进行开发时往往会尝试许多的算法架构、模型参数、特征选择，或者是其它的想法。使用单值评估指标（如准确率）可以让你将所有的模型根据在此指标上的表现进行排序，从而快速确定哪一个模型的性能表现最好。

如果你认为查准率和查全率很关键，可以参考其他人的做法，将这两个值合并为一个值来表示。例如取二者的平均值，或者你可以计算 “F1分数（F1 score）” ，这是一种经过修正的平均值计算方法，比进行简单取平均的效果会好一些。

如果你想了解更多关于 F1 分数的信息，可以参考  https://en.wikipedia.org/wiki/F1_score

因此，当你在多个分类器之间进行抉择时，使用单值评估指标将帮助你更快速地作出决定。它能给出一个清楚的分类器性能排名，从而帮助明确团队后续的处理方向。

最后补充一个例子，假设你在“美国”、“印度”、“中国”和“其它地区”，这四个关键市场跟踪你的猫分类器的准确率，并且获得了四个指标。通过对这四个指标取平均值或进行加权平均，你将得到一个单值指标。取平均值或者加权平均值是将多个指标合并为一个指标的最常用方法之一。

### 9 Optimizing and satisficing metrics
下面将提到组合多个评估指标的另一种方法。
假设你既关心学习算法的准确率（accuracy），又在意其运行时间（running time），请从下面的三个分类器中进行选择：

	Classifier  Accuracy  Running time
	
	A          90%        80ms
	
	B          92%        95ms
	
	C          95%        1500ms

将准确率和与运行时间放入单个公式计算后可以导出单个的指标，这似乎不太自然，例如：

	Accuracy - 0.5 * RunningTime

你可以有一种替代的方案：首先定义一个“可接受的”运行时间，一般低于 100ms 。接着在限定的运行时间范围内最大化分类器的准确率。此处的运行时间是一个“满意度指标” —— 你的分类器必须在这个指标上表现得“足够好”，这儿指的是它应该至多需要 100ms，而准确度是一个“优化指标”。

如果考虑 N 项不同的标准，比如模型的二进制文件大小（这对移动端 app 尤为重要，因为用户不想下载体积很大的 app）、运行时间和准确率。你或许会考虑设置 N-1 个“满意度”指标，即要求它们满足一定的值，下一步才是定义一个“优化”指标。例如为二进制文件的大小和运行时间分别设定可接受的阈值，并尝试根据这些限制来优化准确率指标。

最后再举一个例子，假设你正在设计一个硬件设备，该设备可以根据用户设置的特定“唤醒词”来唤醒系统，类似于Amazon Echo 监听词为 “Alexa”，苹果（Apple） Siri 监听词为 “Hey Siri”，安卓（Android） 监听词为 “Okay Google”，以及百度（Baidu）应用监听 “Hello Baidu.” 我们关心的是假正例率（false positive rate）—— 用户没有说出唤醒词，系统却被唤醒了，以及假反例率（false negative rate）——用户说出了唤醒词，系统却没能正确被唤醒。这个系统的一个较为合理的优化对象是最小化假反例率（优化指标），同时受到每24小时不超过一次误报的约束（满意度指标）。

一旦你的团队的评估指标保持一致并进行优化，他们将能够取得更快的进展。

### 10 Having a dev set and metric speeds up iterations
我们很难事先就知道哪种方法最适合解决面临的新问题，即使是一个经验丰富的机器学习研究员，他通常也需要在发现令人满意的方案前尝试不同的想法。在建立一个机器学习系统时，我往往会这样：

- 1.尝试一些关于系统构建的想法（idea）。

- 2.使用代码（code）实现想法。

- 3.根据实验（experiment）结果判断想法是否行得通。（第一个想到的点子一般都行不通！）在此基础上学习总结，从而产生新的想法，并保持这一迭代过程。

![](/img/mldl/Ng-MLY/10-01.jpeg)
上图展示的是前面所提到的迭代过程，循环得越快，你也将进展得越快。此时拥有开发集、测试集和度量指标的重要性便得以体现了：每当你有了一个新想法，在开发集上评估其性能可以帮你判断当前的方向是否正确。

假如你没有一个特定的开发集和度量指标，则需要在每次开发新的分类器时把它整合到 app 中，并且体验几个小时来了解分类器的性能是否有所改进。这相当耗费时间！另外，如果你的团队将分类器的准确率从 95.0％ 提高到 95.1％，这 0.1% 的提升可能很难被检测出来。但是积少成多，通过不断积累这 0.1％ 的改进，你的系统将取得很大的改进。拥有开发集和度量指标，可以使你更快地检测出哪些想法给系统带来了小（或大）的提升 ，从而快速确定要继续研究或者是要放弃的方向。

### 11 When to change dev/test sets and metrics
开展一个新项目时，我会尽快选好开发集和测试集，因为这可以帮团队制定一个明确的目标。

我通常会要求我的团队在不到一周（一般不会更长）的时间内给出一个初始的开发集、测试集和度量指标，提出一个不太完美的方案并迅速采取行动 ，比花过多时间去思考要好很多。但是一周的时间要求并不适用于成熟的应用程序，譬如垃圾邮件过滤。我也见到过一些团队在已经成熟的系统上花费数月的时间来获得更好的开发集和测试集。

如果你渐渐发现初始的开发集、测试集和度量指标设置与期望目标有一定差距，快速想方法去改进它们。例如你的开发集与度量指标在排序时将分类器 A 排在 B 的前面，然而你的团队认为分类器 B 在实际产品上的表现更加优异，这个时候就需要考虑修改开发集和测试集，或者是你的评估指标了。

在上面的例子里，有三个主要原因可能导致分类器 A 的评分较低：

- 1.你需要处理的实际数据的分布和开发集/测试集数据的分布情况不同。

- 2.你在开发集上过拟合了。

- 3.该指标所度量的不是项目应当优化的目标。

在项目中改变开发集、测试集或者度量指标是很常见的。一个初始的开发集、测试集和度量指标能够帮助团队进行快速迭代，当你发现它们对团队的导向不正确时，不要担心，你只需要对其进行修改并确保团队了解新的方向是什么。

### 12 Takeaways: Setting up development and test sets

- 选择作为开发集和测试集的数据，应当与你预期在将来获取并良好处理的数据有着相同的分布，但不需要和训练集数据的分布一致。
- 开发集和测试集的分布应当尽可能一致。
- 为你的团队选择一个单值评估指标进行优化。需要考虑多项目标时，不妨将它们整合到一个表达式里（比如对多个误差指标取平均），或者定义满意度指标和优化指标。
- 机器学习是一个高速迭代的过程：在最终令人满意的方案出现前，你可能要尝试很多想法。
- 拥有开发集、测试集和单值评估指标可以帮你快速评估一个算法，从而加速迭代过程。
- 当你探索一个全新的应用时，尽可能在一周内建立你的开发集、测试集和指标，而在成熟的应用上则可以花费更长的时间。
- 传统的 70% / 30% 训练集/测试集划分对大规模数据并不适用，实际上开发集和测试集的比例会远低于 30%。
- 开发集的规模应当大到能够检测出算法精度的细微改变，但也不用太大；测试集的规模应该大到能够使你对系统的最终性能作出一个充分的估计。
- 当开发集和评估指标不再能给团队一个正确的导向时，就尽快修改它们：(i) 如果你在开发集上过拟合，则获取更多的开发集数据。(ii) 如果开发集和测试集的数据分布和实际关注的数据分布不同，则获取新的开发集和测试集。 (iii) 如果评估指标不能够对最重要的任务目标进行度量，则需要修改评估指标。

### 13 Build your first system quickly, then iterate
### 14 Error analysis: Look at dev set examples to evaluate ideas
### 15 Evaluating multiple ideas in parallel during error analysis
### 16 Cleaning up mislabeled dev and test set examples
### 17 If you have a large dev set, split it into two subsets, only one of which you look at
### 18 How big should the Eyeball and Blackbox dev sets be?
### 19 Takeaways: Basic error analysis
### 20 Bias and Variance: The two big sources of error
### 21 Examples of Bias and Variance
### 22 Comparing to the optimal error rate
### 23 Addressing Bias and Variance
### 24 Bias vs. Variance tradeoff
### 25 Techniques for reducing avoidable bias
### 26 Techniques for reducing Variance
### 27 Error analysis on the training set
### 28 Diagnosing bias and variance: Learning curves
### 29 Plotting training error
### 30 Interpreting learning curves: High bias
### 31 Interpreting learning curves: Other cases
### 32 Plotting learning curves
### 33 Why we compare to human-level performance
### 34 How to define human-level performance
### 35 Surpassing human-level performance
### 36 Why train and test on different distributions
### 37 Whether to use all your data
### 38 Whether to include inconsistent data
### 39 Weighting data
### 40 Generalizing from the training set to the dev set
### 41 Addressing Bias, and Variance, and Data Mismatch
### 42 Addressing data mismatch
### 43 Artificial data synthesis
### 44 The Optimization Verification test
### 45 General form of Optimization Verification test
### 46 Reinforcement learning example
### 47 The rise of end-to-end learning
### 48 More end-to-end learning examples
### 49 Pros and cons of end-to-end learning
### 50 Learned sub-components
### 51 Directly learning rich outputs
### 52 Error Analysis by Parts
### 53 Beyond supervised learning: What’s next?
### 54 Building a superhero team - Get your teammates to read this
### 55 Big picture
### 56 Credits