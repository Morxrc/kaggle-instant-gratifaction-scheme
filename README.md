# kaggle instant gratifaction scheme
 本方案在私榜可以达到**0.97598**的分数(同rank1 相同分数)

****

## 1.比赛简介:

[instant gratifation比赛链接](https://www.kaggle.com/c/instant-gratification/overview/description) 

数据：人造数据

metric: AUC

kaggle kernel[链接](https://www.kaggle.com/meistermorxrc/0-97598-in-private-score?scriptVersionId=16004236)

![得分:](https://github.com/Morxrc/kaggle-instant-gratifaction-scheme/blob/master/result.png)



## 2.比赛中的关键点：

偷懒借用[包大人](https://zhuanlan.zhihu.com/p/70102466)在赛后总结中将这个比赛看作一个满分100分的考试，考核关键点如下：

1. 第一条是最简单的，如何通过eda和模型反馈发现magic特征。**（考察 EDA、数据分析）**分值80分
2. 第二条是难一点的，如何通过magic特征，筛选样本和特征，进一步提升模型。（**考察 样本、特征筛选）** 分值10分
3. 第三步是最难的，如何根据数据分布，选择最合适的模型。**（考察 模型选择）** 分值10分
4. 第四步附加题。结果选择**（考察同分布估计和运气以及实力）** 附加题 5分



## 3.本方案summary:

​	**1.**机遇三种偏为冷门，但是对此数据分布及其适合的模型进行建模。即：基于**GMM**（高斯混合模型），**QDA**（二次判别分析），**NUSVC**对数据建模。


​	**2.**将**cv**问题中即为常见，但是在传统数据挖掘中并不曾广泛使用的**psuedo labeling** （伪标签）技术。运用到其中，极大地提高了模型的拟合能力。


​	**3.**由于**AUC**指标本质上是一种排序指标，普通的融合方法并不会起太大作用，在本次比赛上我们原创了一种新的融合方式，基于双阈值搜索的**Blend**方法，此方法可以极大地提高最终模型的泛化能力，基于此方法的结果可以拿到**1st**的比赛成绩**(种种原因并未提交)**。



注:很多选手完成了4个步骤然后将剩下的交给了运气，因为除了QDA以外的其他模型,表现都显得十分"差劲"。因此在这个比赛中，几乎没有人去尝试融合,Trust the luck，我们选取了NUSVC做为辅助模型进行融合,是基于在结果为双边或者多边分布的情况下，一部分是中间地带，一部分是两边，对于两边的样本**误伤（当你把1的样本预测成0，或者0的样本预测为1都是一种误伤）**的概率相对较低，而中间地带其实是灰色地带，在这里误伤的主要来源，对于模型来说，这些样本是他无法确定的，所以才大多数落在了这里面；那么有什么办法把这里的样本弄清楚？大家都清楚的一点是QDA跟NUSVC融合的效果不在那么好，因为NUSVC无论线下还是线上还是比较差的，而且即使你给NUSVC很低的融合比例，线上也永远不是提升的；这是因为这两边都是QDA非常精准的地带了，再带入新的模型融合只有干扰可言，不会有任何收益；所以这里参考的思路是，**中间地带概率融合**；



