# Towards Robust Visual Question Answering: Making the Most of Biased Samples via Contrastive Learning
## Abstract
  作者针对 VQA中 language priors 的问题 选用 对比学习 这一方案构建了模型 MMBS

## Motivation
  作者认为VQA这一类的task严重依赖language priors(也就是说对图片的依赖较小)，会导致在进行回答时，只依赖language priors而没有根据imamge信息进行回答。同时，作者认为现在许多针对language priors的方法都是通过牺牲ID上的性能以提高OOD上的表现。
  
  language-prior 和 trade-off 的问题出在 biased sample上。前者是导致over-reliance的原因，后者是在想方设法降低biased sample的重要性。因此如果模型能够从biased sample里学习到该task的intrinsic information便能同时减轻上述两种问题。

## Contributions
  * 提出一种对比学习的方法有效解决了VQA中 language prior 和 ID-OOD performance trade-off 的问题
  * 提出一种能够区分biased和unbiased样本的算法，并应用在对比学习中使用
  * 实验结果表示该论文能在VQA-CP v2数据集上(language-bias sensitive)上兼容多种VQA backbones并且能够取得compeitive performance

