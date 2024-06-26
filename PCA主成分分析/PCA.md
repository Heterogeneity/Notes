## 理解：

PCA (principal component analysis) 主要是给数据**降维**用的一种方法。所谓**降维**即减少数据的特征维度，同时能够在降维的同时尽量保持原始维度下的信息，这样样本矩阵的大小减小，既达到节省计算量的目的，又能保证降维后数据的有效性。

现考虑区分红酒、啤酒和白酒三种酒类，每种酒类的特征假设有**颜色**、**味道**以及**是否为液体**三个维度。显然，单看**颜色**这一维度，三种酒可以被很好地区分开，如果给出一个连续的色谱，三种酒类的颜色在色谱上的位置应该相距较远，其颜色的量化值应该差距较大，换句话说，在**颜色**这一轴上，样本的投影值**方差**较大。

如果在**是否为液体**这一维度上，我们是不能明显分开三种酒类的。在这一维度上，假设我们用"1"代表"是"，"0"代表"否"，那么所有的酒样本都会落在"1"这一点上，换句话说，在**是否为液体**这一轴上，样本投影值的**方差**为0，因为显然酒都是液体。

通过上例我们可以看出，样本不同维度的特征对区分样本的帮助不尽相同。在实际应用中，我们可以抛弃那些对区分样本帮助不大的特征维度，保留那些能够明显区分样本的特征维度，这样既减少计算量，又保留了尽可能多的信息。在区分三种酒的例子中，我们可以抛弃掉**是否为液体**这一维度，这样，对降维后的数据进行分类和对原始维度的数据进行分类的结果应该是差不多的。

如何分辨哪些特征维度对区分样本的帮助较大或较小？或者说，哪些特征维度的提供的信息更多或更少？在区分三种酒的例子中，我们发现，样本在提供更多信息的特征维度上的投影值的方差更大，而特征维度提供的信息越少，样本在该维度上的投影值方差就越小，对于区分样本毫无帮助的特征维度，投影值的方差为0。

对于一组有m个特征的样本集，我们想将特征维度由m降到n($`n < m`$)，要如何做？找出提供信息最多的n个特征保留即可，这一做法相当于在m维空间中计算样本在每个坐标轴上的投影值的方差，然后按照方差大小降序排列这m个坐标轴，保留前n个轴即可，也就是保留这n维特征。但是这样做有一个坏处，如果每个坐标轴，或者说每个特征维度，提供的信息相差不多，我们这种粗暴的降维方式就会大量损失原始样本集的信息。

为了解决这一问题，我们希望在原始m维空间中，找出一组**新的m维正交基向量**（或者说一组新的坐标轴），使得原始样本在新的基向量表示下能够达到这样的效果：第一基向量方向上的样本投影值方差是样本在整个空间中任意方向上的投影值方差最大的（换句话说，在整个空间中，找出样本在其方向上投影值方差最大的那个向量作为第一基向量），第二基向量方向上的样本投影值方差是样本在整个空间中与第一基向量正交方向上的投影值方差最大的……以此类推。其目的是为了尽量将原始信息保留在前面的几个基向量上，这样后面的基向量提供的信息就会变少，我们的问题就得以解决。

我们这样理解，原始特征表示的总信息设为$`\theta`$，这是一个固定值（我们降维过程中要保证$`\theta`$尽量减少的小一些，即信息损失小一些），对于m个基，每个基$`v_i`$能表示的信息设为$`\theta_i`$，它们组合起来表示的信息为$`\theta`$。之前我们遇到的问题是，$`\theta_i`$之间相差不大，导致删去一些维度会发生大量信息损失，即$`\theta`$减少。而我们的解决方法是，先求能够满足$`max{\theta_1}`$的基，这样$`\theta_1`$增大了，其他的$`\theta_i`$自然就减少了，相当于尽量将原始信息保留在第一基向量上，接着求在$`\theta - \theta_1`$下能够满足$`max{\theta_2}`$的基，以此类推。其中，$`\theta - \theta_1`$表示在与第一基向量正交的方向中找第二基向量，这样理解，$`\theta - \theta_1`$表示第一基向量以外的基向量表示的信息与第一基向量的信息**无关**，这在向量上表现为**正交**，这也是为什么我们要找的基向量是**正交**的原因。

最终，我们得到了新的m个基向量。在原始基向量表示下，我们称每个基向量方向为一个**特征**，而在新的基向量下，我们称每个新基向量方向为一个**主成分**，即**principal component**，原始样本不再由原始的特征表示，而由主成分表示，事实上，每个主成分都是原始特征的线性组合。在主成分表示下，我们做之前的降维操作，就可以尽可能少地减小降维导致的信息损失，这就是PCA的思想。

## 数学推导：

包含m个样本的样本矩阵为$`A_{m*n}`$。首先要去中心化，即每个样本要减去平均值$`x_i - \overline{x}`$，这一步是为了后续算方差时方便。要求第一基向量，设其方向上的单位向量为$`v`$，现要求样本在其方向上的投影值方差$`S^2`$，其中，样本在$`v`$方向上对应的投影值为$`v^{T}A^T`$，而它对应的方差$`S^2 = \frac{1}{m - 1} * (v^{T}A^T) (v^{T}A^T)^T = \frac{1}{m - 1} * (Av)^T (Av)`$，约束条件为$`v^{T}v = 1`$，即$`v`$是单位向量。

由$`S^2 = \frac{1}{m - 1} * (Av)^T (Av) = \frac{1}{m - 1} * v^{T}A^T Av`$，注意到$`\frac{1}{m - 1} * A^T A`$是关于样本特征的协方差矩阵，设为$`C_{n*n}`$，因此$`S^2 = v^{T}C_{n*n}v`$

用lagrange乘子法求解该问题：

$`F(v) = v^{T}C_{n*n}v + \lambda(1 - v^{T}v)`$。对v求偏导令其等于0得到$`C_{n*n}v = \lambda v`$，这里用到矩阵求导公式$`\frac{\partial{x^{T}Ax}}{\partial x} = Ax + A^Tx`$和$`\frac{\partial{x^{T}x}}{\partial x} = 2x`$。

我们发现最优解$`v`$满足$`C_{n*n}v = \lambda v`$，即$`v`$是矩阵$`C_{n*n}`$的特征向量。进一步来看，将该等式带入式$`S^2 = v^{T}C_{n*n}v`$，有$`S^2 = v^{T} \lambda v`$，又由于$`v`$是单位向量，其内积为1，故$`S^2 = \lambda`$，事实上，如果要求后续第k基向量，只需要添加约束条件使前面求得的基向量与$`v_k`$内积为0，再用lagrange乘子法求解即可。这样我们就从数学上知道，样本投影值方差最大的那个标准基向量方向即是协方差矩阵最大特征值对应的特征向量的方向，而如果去掉这一维度，其他维度上的样本投影值方差最大的那个标准基向量方向即是协方差矩阵第二大特征值对应的特征向量的方向，以此类推。

## 实际操作：

由协方差矩阵定义可知，这是一个**实对称**矩阵，因此必然对应n个正交的单位特征向量（大多数情况下协方差矩阵满秩正定），我们就使用这n个特征向量作为新的基底，表示为矩阵$`V_{n*n}`$，将原本的样本点坐标转换到新基底表示的坐标。

我们使用$`B_{m*n}`$表示新的样本矩阵，$` V_{n*n} B_{m*n}^T = A_{m*n}^T`$，其几何意义是，**在V基底下坐标为B的点，实际对应笛卡尔坐标系下坐标为A的点（要求基底、样本点都是按列存储）**。左乘V矩阵的逆（这里由于V矩阵正定，因此V的逆即V的转置）有$`B_{m*n} = (V_{n*n}^T A_{m*n}^T)^T`$。

如果协方差矩阵不满秩，则选取前面大于0的特征值，用其对应的标准化特征向量组成一个正交矩阵，按照上述方法求解新的样本矩阵即可。

事实上，若我们要将数据从n维降到p维，只需要选取前p个大于0的特征值，用其对应的特征向量组成正交矩阵，接着求解新的样本矩阵即可。

选取前多少个特征值合适？假设协方差矩阵共有n个大于0的特征值，我们引入指标“累计贡献率”，其计算公式为$`\frac{\sum\limits_{i=1}^{p} \lambda_i}{\sum\limits_{i=1}^{n} \lambda_i}`$。一般来说，累计贡献率超过80%-90%就足够了。

在实际应用时，我们常对C使用SVD或EVD求解V。

## 判断特异点：

在新的样本矩阵中，我们希望判断出哪些数据点是原始样本集中的离群点，即样本x远离数据分布的平均水平。删去这些点可以提高PCA的分析精度。我们可以通过指标“点对主成分方差的贡献率（CTR）”来衡量哪些点是特异点。对第h个主成分$`y_h`$，点x对其方差的贡献公式为$`CTR_h=\frac{y_h^2(i)}{n \lambda_h}=\frac{y_h^2(i)}{\sum\limits_{k=1}^n y_h^2(k)}`$。经验上，若某一样本点对轴h的CTR值大于$`\frac{1}{3}`$，即认为该点是特异点，需要除去。

## 样本点在主平面图上表现的质量：

在由第一主成分和第二主成分构成的主平面上，如果该主平面的累计贡献率很高，且有两个投影点的距离很接近，是否可以认为在原数据中，这两个样本点的距离也很接近？

答案是不一定，因为如果两个原始样本点离主平面很远，即本身就没有在平面上得到很好的表示，那么投影后会出现虚假的近似现象。

如何衡量样本点在主平面图上表现的质量？只需要求原始样本点和主平面之间的$`cos\theta`$值即可，具体计算公式为$`\frac{|\hat{x}|}{|x|}`$，其中x是原始样本点，$`\hat{x}`$是映射后的样本点。

## 参考：
[如何通俗易懂地讲解什么是 PCA（主成分分析）？ - 石溪的回答 - 知乎](https://www.zhihu.com/question/41120789/answer/1304023183)

[主成分分析（PCA）的原理和简单推导](https://www.bilibili.com/video/BV1X54y1R7g7/?spm_id_from=333.337.search-card.all.click&vd_source=bd9b0c20658086a80ddce73476f5c881)

[如何通俗易懂地讲解什么是 PCA（主成分分析）？ - 马同学的回答 - 知乎](https://www.zhihu.com/question/41120789/answer/481966094)

[30分钟学会PCA主成分分析 - 一个有毅力的吃货的文章 - 知乎](https://zhuanlan.zhihu.com/p/149902727)

[统计学笔记 | 主成分分析 - 玩指针的张同学的文章 - 知乎](https://zhuanlan.zhihu.com/p/430296241)

## 补充：

要求主成分之间垂直（即正交），是基于以下原因：

- 信息的独立性：正交的坐标轴确保了不同主成分携带的信息是独立的。如果坐标轴不垂直，那么投影后的数据在不同轴上的值可能会相互影响，导致信息重叠。

- 简化解释：正交性质简化了对主成分的解释。每个主成分可以单独考虑，而不需要担心与其他主成分的交互作用。

- 避免信息的损失：如果主成分不是正交的，那么在投影过程中可能会损失一些信息，因为某些特征可能在多个方向上被重复考虑或忽视。

如果主成分不是垂直的，那么：

- 多重共线性：这可能导致多重共线性问题，即一个主成分的变化可能同时影响多个其他成分。

- 数据解释困难：解释非正交的主成分将变得更加困难，因为它们之间的关系不是简单的独立或线性组合。

因此，为了保持数据的完整性和解释性，PCA要求新坐标系的主成分是垂直的。这样，每个主成分都可以作为一个独立的变量来考虑，使得数据分析和降维更加有效。

## 相关知识：

ICA(独立成分分析)

t-SNE（摆动自组织嵌入）
