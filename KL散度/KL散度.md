# KL散度

![image](https://github.com/Heterogeneity/Notes/assets/102458836/31e8e78f-3a43-4fee-9167-66ab471f5daa)

KL散度（Kullback-Leibler divergence），可以以称作相对熵（relative entropy）或信息散度（information divergence）。

KL散度的理论意义在于度量两个概率分布之间的差异程度，当KL散度越大的时候，说明两者的差异程度越大；而当KL散度小的时候，则说明两者的差异程度小。如果两者相同的话，则该KL散度应该为0。

# 例子

若有概率分布p和q，用q去近似p，则KL散度可以表示为

![image](https://github.com/Heterogeneity/Notes/assets/102458836/9a65efe7-381a-4402-ac56-61aef1d633a9)

可以看出，KL散度具有非负性且不对称，也就是说用q近似p的KL散度和用p近似q的KL散度的值不一定相同。

若pq都是离散型随机变量的分布，则KL散度为

![image](https://github.com/Heterogeneity/Notes/assets/102458836/262ca928-d39b-4eda-b784-f63431237605)

展开上式，有

![image](https://github.com/Heterogeneity/Notes/assets/102458836/77e418a3-ad07-4083-8b57-a19d0be1f522)

可以看到，q近似p的KL散度其实是q近似p的交叉熵减去p的熵。

在信息论中，熵代表着信息量，即理论平均最小编码长度，用q近似p得到的最小编码长度一定比p本身的熵小，二者差异即KL散度。

所以KL散度在信息论中还可以称为相对熵（relative entropy）。
