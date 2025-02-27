title: 【翻译】TextRank:对文本排序
tags: 
  - 算法
  - kaggle
categories: 
  - 算法
  - NLP
comments: true
date: 2017-04-26 22:12:58
---

> 本文翻译自：[http://web.eecs.umich.edu/~mihalcea/papers/mihalcea.emnlp04.pdf](http://web.eecs.umich.edu/~mihalcea/papers/mihalcea.emnlp04.pdf)

## 摘要

本篇论文中，我们将介绍TextRank算法，这是一种针对于文本处理的基于图的排序模型算法，并且展示这个模型是如何能够被成功的应用在自然语言应用中。特别的，我们提出了两个创新的无监督方法用于关键词和句子提取，并且展示我们得到的测试结果与先前公布的基准结果的对比。

## 1 介绍

基于图的排序算法，像Kleinberg的HITS算法（Kleinberg，1999）或Google的PageRank算法（Brin和Page，1998）已成功应用于引文分析，社交网络和万维网链接结构分析。可以说，这些算法通过提供一种依靠Web架构师的集体知识而不是网页的单独内容分析的网页排名机制,可以作为网页搜索技术领域触发的范式转换的关键要素。简而言之，基于图的排序算法是通过考虑从整个图形递归计算的全局信息，而不是仅依赖于局部顶点特定信息来决定图中的顶点的重要性的一种方式。

对从自然语言文档中提取的词汇或语义图，使用类似的思路，可以得出一种基于图形的排名模型，这种模型可以应用于各种自然语言处理的应用程序中，其中从整个文本中获取的知识用于得出本地 排名/选择 的决策。 这种面向文本的排序方法，可以应用于关键短语的自动提取，摘要提取，以及词义消歧的任务（Mihalcea et等等，2004）。

在这篇论文中，我们会介绍基于从自然语言文本中提取的图形，来介绍TextRank图的排序模型。我们调查并评估了TextRank对于由无监督关键词和句子提取，这两种语言处理任务的应用，并展示使用TextRank获得的结果与在这些领域开发的最先进的系统进行比较。

## 2 TextRank模型

基于图的排序算法本质上是基于从整个图形递归绘制的全局信息，来决定图中顶点的重要性的一种方式。基于图表的排名模式实现的基本思想是“投票”或“推荐”。当一个顶点链接到另一个顶点时，它主要是在为另一个顶点投票。顶点投射的投票数越多，顶点的重要性就越高。此外，顶点投票的重要性决定了投票本身的重要性，而这一信息也被排名模型考虑在内。因此，与顶点相关联的分数，取决于为其投的票，以及投射这些投票的顶点的分数来确定。

正式的，令集合$G=(V,E)$是具有顶点$V$和边$E$集合的有向图，其中$E$是$V×V$的子集。对于给定的顶点$V\_i$，令$In(V\_i)$是指向它的顶点集（前辈），并且令$Out(V\_i)$是顶点$V\_i$指向（后继）的顶点集合。顶点$V\_i$的分数定义如下（Brin和Page，1998）：

![](/img/17_04_26/001.png)

其中$d$是0和1之间的阻尼因子，它作用于将从给定顶点跳转到图中的另一个随机顶点的概率集成到模型中。在网页浏览的上下文中，这种基于图表的排名算法实现了“随机冲浪者模型”，其中用户以概率$d$随机点击链接，并以概率$1-d$跳转到一个全新的页面。因子$d$通常设置为0.85（Brin和Page，1998），这也是我们在具体实现中所使用的值。

从图中分配给每个节点的任意值开始，迭代计算直到达到低于给定的阈值。运行算法之后，得到一个与每个顶点相关联的分数，这表示图中顶点的“重要性”。请注意，TextRank运行到完成后获得的最终值，不受初始值的选择的影响，初始值的选择只会影响到收敛的迭代次数。

重要的是要注意，尽管本文中描述的TextRank应用程序依赖于从Google的PageRank（Brin和Page，1998）导出的算法，但是其他基于图表的排序算法，例如 HITS（Kleinberg，1999）或位置函数（Herings等，2001）也可以轻松地整合到TextRank模型中（Mihalcea，2004）。

### 2.1 无向图

虽然传统上应用于有向图，但是也可以将基于递归图的排序算法应用于无向图，在这种情况下，顶点的出度（out-degree）等于顶点的入度（in-degree）。对于松散连接的图形，随着边缘数量与顶点数量成比例，无向图趋向于具有更多的逐渐收敛曲线。

图1绘制了具有250个顶点和250个边缘的随机生成图的收敛曲线，收敛阈值为0.0001。随着图形的连通性增加（即较大数量的边缘），通常在较少迭代之后实现收敛，并且有向和无向图的收敛曲线实际上是重叠的。

![](/img/17_04_26/002.png)

图1：基于图的收敛曲线排名：有向/无向，加权/未加权图，250个顶点，250个边。

### 2.2 权重图

在网页浏览的上下文中，页面中包含多个或部分链接到另一个页面是不寻常的，因此，基于图的原始PageRank定义是假设未加权的图。

然而，在我们的模型中的图是由自然语言文本构造的，并且也可以由包括从文本中提取的单元（顶点）之间的多个或部分链接构造。因此，可以在模型中指示并将两个顶点$V\_i$和$V\_j$之间的连接的“强度”作为加到连接两个顶点的对应边缘的权重$w\_{ij}$来指示并合并到该模型中。因此，我们引入了一个新的基于图的排名公式，在计算与图中顶点相关的分数时考虑了边权重。请注意，可以定义类似的公式来整合顶点权重。

![](/img/17_04_26/003.png)

图1绘制了2.1节中相同样本图的收敛曲线，其中对边进行了0到10的随机加权。尽管与未加权相比，最终顶点得分（因此排名）显着不同，但是对于加权和未加权图，收敛次数和收敛曲线的形状几乎相同。

### 2.3 文本作为图

为了使基于图的排序算法能够应用于自然语言文本，我们必须构建一个表示文本的图形，并将具有有意义关系的单词或其他文本实体进行互连。 根据手头的应用，各种尺寸和特征的文本单位可以作为图中的顶点添加，例如。 单词，搭配，整个句子或其他。 类似地，它是指示用于绘制任何两个这样的顶点之间的连接的关系的类型，例如。 词汇或语义关系，语境重叠等。

无论添加到图形中的元素的类型和特征如何，将基于图的排序算法应用于自然语言文本包括以下主要步骤：

- 1.识别最佳定义手头任务的文本单位，并将其作为顶点添加到图形中。











