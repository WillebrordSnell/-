# SimCSE: Simple Contrastive Learning of Sentence Embeddings
    原文链接： (https://arxiv.org/abs/2104.08821)



## Abstract
  作者通过contrastive learning框架在sentence embedding上取得了SOTA的结果。作者通过对比学习模块预测输入的句子本身，同时以dropout作为"扰动"(可以看作是一种数据增强)，作者发现这种简单的方式效果出奇得好，并且移除dropout的时候会出现representation collapse。之后，作者提出一种监督学习方式：从natural language inference datasets中通过contrastive learning框架，将"entailment"对作为positives，将"contradiction"对作为negatives。并在STS task上评估SimCSE

## Motivation
 * alignment：计算示例对embedding后的距离
 * uniformity：测量embedding的"均匀"分布程度
## Contributions

  
## Method

