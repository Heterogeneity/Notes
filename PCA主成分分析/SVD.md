## 参考：

[知乎的SVD证明](https://zhuanlan.zhihu.com/p/399547902)

## 具体截图和证明过程分析：

![image](https://github.com/Heterogeneity/Notes/assets/102458836/4e00a0ae-97bd-4cde-a835-c7718dc7d5fc)

![image](https://github.com/Heterogeneity/Notes/assets/102458836/c42554b8-c413-4304-a98f-048255a9041b)

- 如果$`1 \leq i = j \leq r`$，由于$`v_i`$为标准化的向量，故自身内积等于1，结果为$` \lambda_i `$

- 如果$`i \neq j`$，由于$`v_i，v_j`$为标准化的正交向量，故内积等于0，结果为0

- 如果$`r < i = j`$，由于$`\lambda_i`$在$`i > r`$时等于0，故结果为0

从这里推导出，$`Av_i`$在$`i > r`$时为0向量，这是因为$`i > r`$时$`A_i`$的自身内积为0，$`1 \leq i \leq r`$时$`v_i`$相互正交

$`u_i`$的形式为$`m`$行1列，这是由$`A_{m*n}v_{i_{n*1}}`$决定的，因此是$`m`$维空间的向量，可以进行基扩充。

![image](https://github.com/Heterogeneity/Notes/assets/102458836/c97218ec-43ee-41d2-842c-581b3fe6824c)

![image](https://github.com/Heterogeneity/Notes/assets/102458836/dd6976bb-1ef8-43a8-aaee-203be53a3ba3)

## 理解：

将$`V`$看作原始空间的标准正交基，$`M`$看作一种变换，$`U`$为变换后空间的标准正交基，而$`D`$描述了变换后的标准正交基和原始标准正交基的scaling程度，公式为$`MV = UD`$

由于$`V`$是正交矩阵，其逆等于转置，因此右乘$`V`$的转置，得到$`M = UDV^T`$，即SVD分解公式。

## 求法：

与证明过程类似，先求出$`M^TM`$的特征值$`\lambda_i`$和标准正交特征向量$`v_i`$，将$`v_i`$组合到一起作为矩阵$`V_{n*n}`$，将非0的$`\lambda_i`$求根号并添加0组合为对角矩阵$`D_{m*n}`$，
按$`u_i = \frac{1}{\sqrt{\lambda_i}}Mv_i`$求出前r个标准正交向量，根据情况使用标准正交基扩充，获得$`u_{r+1}`$到$`u_m`$，将这些标准正交基组合为矩阵$`U_{m*m}`$

最后得到$`M = UDV^T`$

在实际应用中，有许多加速SVD的算法实现，如[Fast SVD for large-scale matrices](https://www.researchgate.net/publication/228820785_Fast_SVD_for_large-scale_matrices)
