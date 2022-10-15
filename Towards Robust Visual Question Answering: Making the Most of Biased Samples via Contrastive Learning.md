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
### Main Results
![image](https://user-images.githubusercontent.com/33151771/195863648-6506d75d-2576-4a21-bc66-24a234a678a1.png)
* Performance based on different VQA models：本文提出的方法能够在baseline的基础上得到较大的提升(1.66 ~10.60 absolute accuracy improvement)。由于YES/NO问题更容易受language bias的影响，所以对于一般的模型，MMBS在YES/NO问题上的表现提升较大(22.73 ~29.28)。在ID上,baseline在大多数debiasing methods都牺牲VQA v2的性能的情况下也能通过MMBS进行些许的提升。尤其是LMH和LMH+MMBS在VQA v2上5.52有的提升，作者认为这是因为最大化地利用biased samples 能够有效地减轻在ID上性能的下降
![image](https://user-images.githubusercontent.com/33151771/195873110-220686e8-6136-423a-8402-5f71ccff984b.png)

* Comparison with ensemble-based SOTAs: 从上表中基于UpDn的backbone我们可以观察到：1):在VQA-CP v2上提升最大的LPF在VQA v2中降低得很多，这种现象说明确实是有在克服language priors的能力和记忆样本知识的能力(the ability to memorize the knowledge of in-distribution samples)做取舍的,CF-VQA在一定程度上减轻了这种现象但是它在VQA-CP v2上的表现明显不如本文提出的方法。2)：LMH+MMBS在VQA-CP v2上取得了最好的表现并且能够复现在VQA v2上的正确率，甚至远超了其他的工作。这说明这种"取舍"问题通过本文提出的方法能够得到有效地解决。3):此前的工作(例如CF-VQA和LPF)能偶在longuage biased更有可能存在的YES/NO问题上取得高正确率，相较之下，本文的方法在YES/NO问题上取得相对较好的成绩的同时能在更有挑战性的非YES/NO问题上大大优于它们。

* Comparison with data-balancing SOTAs：从下表我们可以观察到1)：大多数的bata-balancing方式都会影响ID上的表现，作者认为这是由于balanced training priors和biased test priors的不一致导致的。2)：相较于另一种对比学习模型(LMH+CSS+CL：通过data-balancing方式高金LMH+CSS，在VQA-CP v2上提升了0.23，并且牺牲了在VQA v2上的正确率)，本文提出的MMVS即使变化了VQA backbones也不会影响ID的表现。3)：本文的在以SAR为baseline上(SAR+MMBS)带来了巨大的提升，甚至能够与MUTANT(有最好的性能)的性能有一较之力，并且不需要额外的人工去标注其他大量的数据
* ![image](https://user-images.githubusercontent.com/33151771/195971501-db4af426-98f3-448b-afff-7e75a8dc507e.png)

