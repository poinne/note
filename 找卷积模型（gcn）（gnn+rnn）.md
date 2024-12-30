1. 找卷积模型（gcn）（gnn+rnn）

2. 修改模型
3. 写方法



1. 在网格上初始化水平集，（一样）
2. 计算水平集的离散梯度和拉普拉斯
3. 根据计算的微分算子来求目标函数
4. 计算目标函数的雅可比和近似海塞矩阵，
5. 根据优化方法（L-M法）更新水平集的值
6. 重复2~5，直至达到终止条件。



1. 在网格上初始化水平集
2. 输入水平集值，通过模型卷积预测新的水平集值（预测增量）
3. 计算新水平集的离散梯度和拉普拉斯
4. 根据计算的微分算子来求目标函数
5. 利用自动微分来得到目标函数的梯度，通过反向传播优化模型参数
6. 用预测的结果更新水平集的值
7. 重复2~6，直至达到终止条件。







case1: 生成初始训练数据（生成插值曲线，构造初始水平集）

case2: 训练isogcn，近似梯度

case3: 训练一个简单的模型，看看效果

1. 插值和光滑约束
2. 可视化结果
3. 差分方式修改

case2: 找可行的方法





傅里叶变换就是将非周期性函数看作周期无穷大的函数，利用一组正交基来将各种信号拆分出来

![image-20241115200927403](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241115200927403.png)

**拉普拉斯算子是所有自由度上进行微小变化后所获得的增益**

![image-20241115201327164](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241115201327164.png)

![image-20241115201341878](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241115201341878.png)



两个函数的卷积，其中f可以理解为被卷积的数据，而g可以理解为卷积核。

n表示输出序列的索引。

![image-20241116130658109](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241116130658109.png)

所谓的翻转只是因为你站立的现在是过去的未来 ，因为在上一时刻的输入的影响在当前时刻仍会起作用。



![image-20241116131337853](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241116131337853.png)


$$
F'_j = W_0 \times F_i + \mathcal{A}_{j \in \mathcal{N}(i)}(W_1 \times [sdf_j\parallel interior_{j} \parallel exteriro_{j}])
$$




