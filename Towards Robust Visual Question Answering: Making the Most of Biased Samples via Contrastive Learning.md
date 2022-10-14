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
作者将unbiased(or OOD) sample定义为训练集中对每个问题类别里答案分布出现频率较少的样本。因此unbiased sample中就不大可能包含影响模型鲁棒性的不可信的关系，并且positive samples中一些意料之外的噪声也许会对unbiased sample的学习带来消极的影响。所以基于上述两点原因，作者并没有对unbiased samples建立positive samples。并且作者提出了一个新的算法来过滤掉这些unbiased sample，该算法包含以下三步：
* (i) calculating the answer frequencies;
* (ii) determining the unbiased answer proportion;
* (iii) selecting the unbiased samples.

## Experiments
### Datasets and Evaluation
作者认为此前很多工作在VQA v2和VQA-CP v2上表现不一并且VQA-CP v2数据集上更能显示出模型的鲁棒性。同时，作者认为有许多工作是在VQA v2上牺牲性能以换取在VQA-CP v2的表现，而一个鲁棒性强的VQA模型应该在两个数据集上都有较好的表现。因此作者对各个模型都计算在两个数据集上ID,OOD的表现
### Baselines and Implementations
作者基于三种朴素(plain VQA models)的VQA模型(BAN,UpDn,LXMERT)和两种消融(debiasing methods)方式(LMH,SAR)测试MMBS，同时作者还
###Main Results
![image](https://user-images.githubusercontent.com/33151771/195863648-6506d75d-2576-4a21-bc66-24a234a678a1.png)
* Performance based on different VQA models：本文提出的方法能够在baseline的基础上得到较大的提升(1.66 ~10.60 absolute accuracy improvement)。由于YES/NO问题更容易受language bias的影响，所以对于一般的模型，MMBS在YES/NO问题上的表现提升较大(22.73 ~29.28)。在ID上,baseline在大多数debiasing methods都牺牲VQA v2的性能的情况下也能通过MMBS进行些许的提升
* 
* Comparison with ensemble-based SOTAs:
