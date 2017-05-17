# The EM algorithm

有限混合分布为在随机现象上观察到的数据提供了建模和聚类的基于数学的灵活方法。这里我们专注于普通混合模型的使用，它可以用于对连续的数据进行聚类，同时估计潜在的密度函数。这些混合模型可以通过经由EM（期望最大化）算法的最大似然概率来拟合。

## 引言

有限混合模型正在越来越多的被用于各种各样的随机现象的分布建模和数据集合的聚类。这里我们考虑在聚类分析中的应用。

我们让p维向量（y=（y_1,…,y_p）^T）包含p个变量的值，这些值是由n个（独立的）用来聚类的实体中的每一个测量得到的,同时我们让y_j表示与第j（j=1,…,n）个y值，它与第j个实体相关。使用混合方法来聚类时，y_1,…,y_n被假设为由一些不知道比例的π_1,…,π_g组成的有限数字混合体中的观察随机样本，例如g。

Y_j的混合密度如下所示：

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/5.1.png)

其中混合比例π_1,…,π_g加起来等于1，并且组条件密度f_i(y_i; θ_i)由不知道参数（i-1,…,g）的θ_i向量指定。所有不知道参数的向量由下给出：

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/5.2.png)

这里的上标“T”表示的是向量转置。这个方法使用Ψ的估计，并根据组件成员的后验概率的估值将数据进行概率聚类为g个集群。

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/5.3.png)

其中τ_i(y_j)是先验概率，y_j（y_j观测中的真实实体）属于混合体的第i个部分（i=1,…,g;j=1,…,n）。

参数向量Ψ可以使用最大似然概率来估计。Ψ的最大似然概率估计（MLE,maximum likehood estimate）是通过似然方程的一个适当的根得到的。

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/5.4.png)

其中

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/5.5.png)

是Ψ的对数似然函数。符合局部极大值的上式解可以通过期望最大化（EM）算法得到。

对于连续数据的建模，组件条件密度通常被认为属于同一参数族，例如正态。在这种情况下，

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/5.6.png)

其中

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/5.7.png)

表示的是p维多元正太分布，均值向量为μ，协方差矩阵为Σ。
采用诸如正态分布和t密度的椭圆对称分量的混合模型的一个吸引人的特征就是在数据的仿射变换（也就是关于数据的位置、规模和旋转变化的操作）下，隐含集群是不变的。这样聚类的过程就不依赖于例如量纲或者簇的旋转这样不相干的因素。

## 正态混合的最大似然估计

McLachlan和Peel描述了多元正态分量的最大似然估计的EM算法的E-和M-步骤。对于这个问题的EM框架，没有观测到的分量标签z_ij看作“遗失”数据。根据y_i是否属于第i个混合的成分，z_ij（i=1,…,g;j=1,..,n）被当做0或者1来处理。

在EM算法的第（k+1）次迭代，假定Ψ的当前估值为Ψ^k，E-步骤需要获取完整数据的对数概率![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/5.8.png)的期望。由于不可见的z_ij是线性的，这个E步骤会因将z_ij替换为它们的条件期望而受到影响。这个条件期望根据观测数据y_i获得，使用Ψ^(k)表示。也就是说Z_ij使用当前的拟合Ψ^(k)被替换为![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/5.9.png)，它是y_j属于第i个混合分量的后验概率。可以表示为：

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/5.10.png)

在M-步骤,对于第i个分量，更新的混合比例π_i的估计，均值向量μ_i和协方差矩阵Σ_i由下面的式子给出

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/5.11.png)

可以看出M-步骤存在在封闭形式中。 
这些E-和M-步骤会一直轮流进行，直到这些估计参数的变化或者对数概率小于一个特定的阈值为止。

## 簇的数量

通过考虑概率函数，我们可以为g选择一个合适的值。在没有关于数据中存在的簇的数量的任何先前信息的情况下，当g的值增长的时候我们可以观察到对数概率函数的增长。

在任何阶段，选择g=g0还是g=g1,例如g1=g0+1,可以通过进行似然比检验或者使用一些信息化标准，例如BIC（Bayesian information criterion）来决定。不幸的是，规则性条件不适用于似然比检验统计量λ通常的零卡方分布，分布的自由度等于在混合模型中对于g=g1和g=g0的参数数量的差数d。可以通过使用重抽样的方法来处理。或者，我们也可以选择使用BIC，这样就会导致如果-2logλ大于dlog(n)时，就选择g=g1而不是g=g0。
