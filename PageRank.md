# PageRank

# 总览

PageRank是由Sergey Brin和Larry Page在1998年4月的第七届国际全球广域网会议（WWW7）中提出的。它是一个使用互联网上的超链接的搜索排序算法。基于这个算法，他们构建了Google搜素引擎，并且取得了巨大的成功。现在，每个搜索引擎都有自己的基于超链接的排序算法。

PageRank提供了一个网页的静态排行，也就是说PageRank为每个页面离线计算了一个值，并且它不依赖于搜索查询。这个算法利用了Web的民主特性，将页面的大量链接结构作为每个页面的质量指标。PageRank的精髓就是通过把从x页面到Y页面的一个链接，作为x页面给y页面的投票。然而，PageRank不仅仅只是单纯的统计票数，或者一个页面收到的链接数。它也分析投票的页面。如果投出票的页面本身是“重要”的，那么它会使得它投票的对象也是重要的。这就是在社交网络中的等级声望的思想。

## 算法

现在我们介绍PageRank公式。首先我们先陈述一些Web页面中的主要内容。

页面i的入链接(in-link)：从其他页面指向页面i的超链接。通常而言，不考虑从同一个站点指来的链接。

页面i的出链接(out-link)：从页面i指向其他页面的超链接。一般而言，不考虑指向同一个站点的链接。

下面这些基于等级声望的思想可以用来导出PageRank算法:
1. 从一个页面指向另一个页面的超链接是一种隐藏的权力的转移。因此，页面i收到的入链接越多，页面i的声望就越高。
2. 指向页面i的页面也有他们自己的声望分数。拥有较高声望的页面的投票比拥有较少声望的页面的投票更重要。简而言之，被重要的页面指向的页面也是重要的。

根据社交网络中的声望等级，页面i的重要性（这里是i的PageRank分数）是由所有指向i的页面的PageRank分数加起来所决定的。因为一个页面可能指向其他的很多页面，它的声望分数应该由它指向的页面所共享。

为了使用公式表示上述的思想，我们把Web作为一个有向图G=（V,E）这里的V是顶点或节点的集合，例如所有页面的集合，同时E是图中有向边的集合，例如超链接。这里设所有页面的总数为n（如n=|V|）。第i个页面的PageRank分数（由P(i)表示）定义如下

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/6.1.png)

这里的Oj是页面j的出链接的数量。数学上而言，我们有n个未知数的n个线性方程系统。我们可以用一个矩阵来代表所有的等式。设P是PageRank值的n维列限量，例如：P=（P(1),P(2),…,P(n)）T。

设A是图的邻接矩阵，

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/6.2.png)

我们可以重写n个等式的系统为

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/6.3.png)

这是特征系统的特征值方程，这里P的解是特征值为1的特征向量。由于这是一个循环的定义，我们可以使用一个迭代的算法来解决它。事实证明如果满足某些条件，1
就是最大的特征值，并且PageRank向量P就是主特征向量。Power iteration(幂迭代)就是一个用来寻找P的有名的数学技巧。

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/6.4.png)

然而问题在于由于Web图不满足条件，上式也不太合适。事实上上式也可以由马尔科夫链得到。然后就可以应用马尔科夫链里的一些理论结果。在扩张Web图使得它满足条件之后，就能得到下面的等式：

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/6.5.png)

这里的e是全为1的列向量。这样我们就得到了每个页面i的PageRank公式：

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/6.6.png)

上式等于在原始PageRank论文中给出的公式：

![](https://github.com/ankang1993/data-mining-algorithms/blob/master/figure/6.7.png)

参数d被称为阻尼因子，可以将它的值设为0和1之间。另一篇论文中使用的是d=0.85。

可以使用幂迭代的方法来计算Web页面的PageRank值，这样会得到特征值为1的主特征向量。这个算法非常简单，如图4所示。我们可以从任意指定的PageRank的初始值开始。如果PageRank值变化不大或是达到收敛，那么迭代就结束了。在图4中，如果残余向量的1-范式小于预定的阈值e，那么迭代就结束了。

由于在Web搜索中我们只对网页的排名感兴趣，所以这个算法最终是否收敛我们并不关心。这样就可以使用更少的迭代。在另一篇论文中，一个拥有3.22亿链接的数据库在经过了52次迭代之后就达到了可以接受的效果。

## 关于PageRank的进一步参考

自从在论文中提出了PageRank算法，研究者已经对模型提出了很多的改进、替代模型、提高计算、增加时间维度等。Liu、Langville和Meyer的书中包含了一些PageRank的深度分析和其他几个基于链接的算法。
