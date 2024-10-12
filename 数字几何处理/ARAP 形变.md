### ARAP 形变

对一个网格模型进行形变时尽可能保持刚性。

用户设置个别离散点的新位置来表达他所想要的形变，就能自动根据所需保持的形体信息来计算出剩余离散点应有的位置。

尽可能刚性，即保持边不变，因此变换后的边与变换前的边之间的变换可以通过一个旋转矩阵来表示。

可以得到目标函数：

![image-20240513225936009](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240513225936009.png)

$p'_i - p'_j$表示变换后的顶点i的一邻域的边。$p_i - p_j$表示变换前顶点i的一邻域的边。

（能量是离散到了edge上，但求和时还是用了顶点来组织，这样便于分析和理解）

优化这个能量函数也就是去最小化变形前后triangle mesh上每一对edge的长度之差的加权平方和，这等同于希望尽量使所有的edge都只经历刚性变换。又由于三角形具有稳定性，即三条边长度确定时形状确定，故尽量去保持每条边的长度也就是在尽量地保持局部的形状。

因为函数中存在两个变量，即变换后的边和对应的旋转矩阵，因此可以使用local-global法进行优化。

**假设当$p'_i - p'_j$已经确定：**

![image-20240513230420196](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240513230448488.png)

改写为矩阵形式：

![image-20240513230448488](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240513230420196.png)

此时只有$R_i$为变量，只需优化中间的项。

![image-20240513230823256](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240513230834895.png)

![image-20240513230834895](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240513230823256.png)

即可求出每个顶点对应的最优旋转矩阵。

**假设$R_i$已经确定：**

此时目标函数是一个二次能量函数，求解该函数方法为：

对函数求导，并令导数等于零，可以得到极小值点对应的变形后的顶点位置。这是因为二次能量函数通常是一个凸函数，其导数等于零的点对应着函数的全局最小值或者局部最小值。

![image-20240513230420196](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240513230448488.png)

对$p'_i$进行求导

![image-20240513232147126](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240513232256072.png)

则

![image-20240513232256072](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240513232147126.png)



上面等式左侧为b，求解$Ax = b$，A为Laplace矩阵，因为Laplace刻画了局部性质，用Laplace进行全局求解可以确保得到的解连续，光滑。

综上：

![image-20240513232129064](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240513232129064.png)

![image-20240513230919343](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240513230919343.png)