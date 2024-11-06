#### Topology-controllable Implicit Surface Reconstruction Based on Persistent Homology



使用隐式B样条来拟合3D点云

输入为带有法向的采样的点云坐标。

构建以点云为中心的立方体，计算立方体的离散符号距离场：

![image-20241105201813461](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241105201813461.png)

每个顶点的正负通过加权的有符号投影距离计算。最终，测量数据D（三维立方复形）表示为：

![image-20241105202248547](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241105202248547.png)

同时引入偏移点和适当的符号估计修正方法，改善由稀疏点云生成的签名距离场的质量。（略）

定义函数$f(x,y,z)$，其均匀网格域为$\mathcal{D}$:

![image-20241105202749276](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241105202749276.png)

定义$f$ 为三变量张量积B样条函数：

![image-20241105202828403](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241105202828403.png)

用来拟合三维立方复形D，对于该函数隐式表示的曲面，通过PIA迭代，更新控制参数$C_{ijk}$，得到用B样条函数表示的近似符号距离函数。

优化目标为：

![image-20241105202914332](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241105202914332.png)



对于拓扑控制：

使用拟合点云数据的B样条来进行下水平集过滤和同调群计算来构建k-PD。

（初始 B-样条是指在使用拓扑可控优化之前，通过拟合点云数据得到的第一个近似距离场。这个近似距离场通常使用 MLS 方法或其他插值方法生成，并使用 B-样条函数进行表示。）

![image-20241105215024640](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241105215024640.png)

计算每一维的PD的特征，根据持续时间长短按从长到短排序。长的视为主要特征，靠近对角线的视为噪声。设计拓扑目标函数，以使长的更长，短的更短。

定义拓扑逆映射将PD映射回立方复形：

![image-20241105215606831](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241105215606831.png)

通过该函数可以让拓扑目标函数可以通过微分链式法则来控制B样条的控制参数。

拓扑目标函数可以定义如下：

前一项是主要拓扑特征，后一项是噪声拓扑特征。

![image-20241105215746620](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241105215746620.png)

![image-20241105221210466](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241105221210466.png)



拓扑目标函数可视化：

![image-20241105221154174](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241105221154174.png)

最后的目标函数为：

![image-20241105215903635](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241105215903635.png)

流程图如下：

![image-20241105215929178](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241105215929178.png)





#### SDFConnect: Neural Implicit Surface Reconstruction of a Sparse Point Cloud  with Topological Constraints[2024 CVRP]

同样是点云重建，在Neural-Pull framework 的基础上，结合上篇论文中提出的拓扑损失，进行重建结果的约束。

不同在于，上篇拓扑损失是用优化的方式，定义了一个可微的拓扑逆映射函数，将k-PD值映射回点。这篇直接通过自动微分来做。

![image-20241105223706597](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241105223706597.png)