# UTC: A Unified Transformer with Inter-Task Contrastive Learning for Visual Dialog
原文链接： https://arxiv.org/abs/2205.00423

## Abstract
作者发现关于visual dialog中现存的方法对排序和生成答案这两个任务都是分开来考虑，或者是分别通过两个模型隐式地捕获一种微弱的relation。在这篇文章中，作者提出了一种基于UTC框架并使用对比学习的模型，该模型能够联合generative和discriminative两种task。考虑到先前学习范式的限制，本文提出两种对比学习的loss(context contrastive loss 和 answer contrastive loss)，作者认为这两种loss利用了dialog中的上下文和目标答案作为anchor points以便从不同的方面提供representation learning signals，因此这两种和loss使得discriminative和generative两种task起到互补的作用。

## Introduction
### visual dialog
一般来说，visual dialog中有两种setting，一种是通过discriminative decoder讲候选答案进行排序，另一种是通过generative decoder去生成目标答案。相较于VQA,visual dialog不仅能够从关于图片
的角度上提问，也要agent充分地从之前的question和answer中得到一些线索。  
如下图所示，作者认为目前大多数的visual dialog model都是通过设计不同的attention机制从而在分别地在训练answer ranking和generating两个task时去捕获一些interaction。最近也有通过训练一整个网络时使用两种decoder去捕获(weakly)generative和discriminative之间的关系。 
![image](https://user-images.githubusercontent.com/33151771/197530481-649282bc-8b3a-48e7-9e08-a1d73fd7d159.png)
作者认为设计这种联合模型(能够对答案进行排序也能够生成目标答案)的task仍有两个挑战
* 怎样完整地将discriminative中answer candidates里丰富的语义信息传递给answer generation 
作者认为discriminative setting集中于dialog context和answer的alignment，而目前大多数方式都是两步式的pipeline：对于给定的dialog context，answer candidates首先从corresponding answer set中随机选择，每个answer candidate再与对应的dialog context匹配以决策出该候选答案是否是target answer。这种将dialog每轮单独处理，并且没有区分dialog context representation，这就引出了第二个挑战
* 如何捕获一个dialog中其他轮次和其他dialog中所有轮次的context关系
此外，dialog context在diacerminative setting中增强了answer information，然而在generatiive setting中并没有，并且现存的方式很难在generative setting训练过程中利用到enhanced dialog context representations(因为他们对两个task都是单独处理的)

## Contributions

  
## Method

