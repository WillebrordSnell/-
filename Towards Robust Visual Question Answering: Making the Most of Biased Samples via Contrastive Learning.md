# Towards Robust Visual Question Answering: Making the Most of Biased Samples via Contrastive Learning
    原文链接： https://arxiv.org/abs/2210.04692

## Abstract
  作者针对VQA中language priors问题选用`对比学习`这一方案构建了模型 MMBS

## Motivation
  作者认为VQA这一类的task严重依赖language priors(也就是说对图片的依赖较小)，会导致在进行回答时，只依赖language priors而没有根据imamge信息进行回答。同时，作者认为现在许多针对language priors的方法都是通过牺牲ID上的性能以提高OOD上的表现。
  
  language-prior 和 trade-off 的问题出在 biased sample上。前者是导致over-reliance的原因，后者是在想方设法降低biased sample的重要性。因此如果模型能够从biased sample里学习到该task的intrinsic information便能同时减轻上述两种问题。

## Contributions
  * 提出一种对比学习的方法有效解决了VQA中 language prior 和 ID-OOD performance trade-off 的问题
  * 提出一种能够区分biased和unbiased样本的算法，并应用在对比学习中使用
  * 实验结果表示该论文能在VQA-CP v2数据集上(language-bias sensitive)上兼容多种VQA backbones并且能够取得compeitive performance

## Method
### Positive Sample Construction
  作者提出两种方式构建positive sample
 * Shuffling：随机地讲问题类别词和插入到句中其他位置，作者认为这种方法能够提高建立问题类别和答案关系的难度，以避免language priors
 * Removal：直接删除问题类别词，作者认为这样能够直接消除问题类别和答案之间的关系
 
 作者提出4种应用方式：S,R,B,SR
 * S：使用Shuffling
 * R：使用Removal
 * B：两种方式都使用
 * SR：在yes/no问题上使用Removal，其他问题上用Shuffling(非yes/or问题上包含着一些和答案相关的信息因此用Shuffling更为合适)

### Unbiased Sample Selection
作者蒋unbiased(or OOD) sample定义为训练集中对每个问题类别里答案分布出现频率较少的样本
