1. 
2. 如何将learning与sdf联系起来？
3. 如何用learning计算sdf?
4. 这些论文是在欧式空间计算的，我需要在流形空间计算



**Deep Eikonal Solvers：**

Eikonal 方程用于描述在给定空间中从源点到其他点的最短路径的性质，特别是在计算测地距离时。

快速 Eikonal 求解器（Fast Eikonal Solvers）是一类用于高效计算 Eikonal 方程解的方法，主要应用于计算距离场和最短路径问题。

**快速行进法**是最著名的快速Eikonal求解器之一：

Fast marching method：

可以计算每个网格顶点相对于某个**源点（或初始曲线）**的距离函数值（测地距离）。

快速Eikonal求解器主要包括两个部分：

- **局部数值求解器**：用于在某个点近似计算距离函数。它根据该点周围的已知距离值，提供该点的距离近似值。
- **排序/更新方法**：决定下一个要计算的点。这个方法会选择合适的点来进一步近似距离函数。



论文的目的就是训练一个模型作为局部数值求解器，传统的局部数值求解器是通过公式和数学推导计算点的距离近似，而新的方法则使用**神经网络**来代替公式。

神经网络经过训练后，能够对不同的几何结构和采样条件下的局部距离进行更高精度的估计。





**DGM: A deep learning algorithm for solving partial differential equations**

DGM算法的核心思想是通过神经网络近似求解高维偏微分方程。

使用神经网络近似偏微分方程中的函数，

具体做法是使用随机梯度下降（SGD）方法在没有构造grid的情况下逼近PDE的解。这个方法的关键步骤是随机生成空间和时间点，在这些点上计算误差并通过梯度下降更新神经网络的参数。

神经网络可以看做对某个函数的近似，我们使用神经网络来近似偏微分方程的解，并根据该近似的解与原方程的匹配程度定义[损失函数](https://zhida.zhihu.com/search?content_id=170428981&content_type=Article&match_order=1&q=损失函数&zhida_source=entity)，通过训练神经网络，优化损失函数，我们求得神经网络的参数，神经网络所表达的函数得以确定后，我们就得到了原方程的近似解。在DGM的方法中，损失函数采用的是基于强解（Strong Solution）的构造，即假定求得的解满足原方程所要求的光滑性质。

使用神经网格近似抛物线偏微分方程：

![image-20241002200646818](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241002200646818.png)

![image-20241002200742345](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241003104138176.png)

![image-20241002200757196](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241002200742345.png)

![image-20241003104120734](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241003104159248.png)

![image-20241003104138176](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241002200757196.png)

![image-20241003104159248](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241002204652083.png)





:

这篇文章使用深度神经网络来得到偏微分方程的数值解，应该是一种强解，主要思想就是用神经网络来拟合偏微分方程的解函数。文章后面也使用了二阶的方法来加速求解。

但是训练好后只能拟合一个偏微分方程的解



**Weak Adversarial Networks for High-dimensional Partial Differential Equations**

利用GAN的思想求偏微分方程的弱解。

在没有强解的情况下，WAN方法能对弱解有更好的近似，而基于强解的方法有更大的误差。而当原方程有强解的时候，所有的方法效果相差不大。





![image-20241002204652083](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241003140508088.png)



在对于线性偏微分方程，通常可以使用更直接的方法来求解，比如：

1. **分离变量法**：将偏微分方程分解为几个单变量的方程，进而求解。
2. **特征线法**：用于一阶偏微分方程，通过构造特征线来简化问题。
3. **傅里叶变换或拉普拉斯变换**：对于特定边界条件的线性偏微分方程，可以通过变换方法求解。
4. **有限差分法或有限元法**：这些数值方法也适用于线性偏微分方程，通常比牛顿法更为高效。

尽管牛顿法可以用于线性偏微分方程，尤其是在将其转化为线性代数方程组时，但由于其特性和要求的计算量，使用专门针对线性问题的方法会更为方便和高效。因此，牛顿法在处理线性偏微分方程时并不常用。

牛顿法可以用于求解偏微分方程（PDEs），尤其是在涉及到非线性偏微分方程的情况下。牛顿法主要是用来寻找方程的根，而许多偏微分方程可以通过将其转化为一个非线性方程的形式来应用牛顿法。

在具体应用中，牛顿法通常结合有限差分法、有限元法或谱方法等数值方法，以离散化偏微分方程。这种方法的基本思路是：

1. **离散化**：使用适当的数值方法将偏微分方程转化为离散形式，通常得到一个非线性代数方程组。
2. **构造雅可比矩阵**：对非线性方程组求解时，需要计算雅可比矩阵，该矩阵包含了方程组关于未知数的偏导数。
3. **迭代求解**：通过牛顿迭代公式，不断更新未知数的值，直到收敛到所需的精度。

牛顿法的优点在于其收敛速度快，尤其是在接近解时。然而，它也有一些缺点，比如对初始猜测的敏感性以及在高维情况下计算雅可比矩阵的开销较大。

总的来说，牛顿法在求解某些类型的偏微分方程时是有效的，尤其是在处理非线性问题时。

![image-20241003140508088](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241010101700805.png)





**截断误差**



由实际问题建立起来的数学模型，在很多情况下要得到准确解是困难的，通常要用数值方法求它的近似解，例如常把无限的计算过程用有限的计算过程代替，这种模型的准确解和由数值方法求出的近似解之间的误差称为截断误差。因为截断误差是数值计算方法固有的，因此又称方法误差。





##### 论文1：PHYSICS-AWARE DIFFERENCE GRAPH NETWORKS FOR SPARSELY-OBSERVED DYNAMICS

该论文虽然是用learning方式解定义在mesh上的动态的PDE，但是用的还是数据驱动的方式。

论文是这样做的：

1. **作者图网络（GN）以及有限差分，定义了一个模块：**

   **通过将传统离散微分几何中对梯度和拉普拉斯的定义的修改，将权改为可学习的权重，定义了在网络中求梯度和拉普拉斯的方法。**

   【提出空间差异层（SDL），由一组可学习的参数组成，用于定义可学习的差分算子作为梯度和拉普拉斯算子的一种形式，以充分利用邻近信息。】（论文翻译）

   1. 传统的方法：

      ![image-20241010101451480](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241010101519598.png)

      ![image-20241010101519598](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241003104120734.png)

      定义在三角网格上的微分算子：

      ![image-20241010101700805](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241010101810590.png)

      

      作者修改后的定义：

      ![image-20241010101726379](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241010101451480.png)

      

      可学习的权重通过GN的k-hop子图得到：

      相当于高阶差分方程。

      ![image-20241010101810590](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241010101726379.png)

      （虽然权重使用可k-hop子图，但是在进行差分时还是用的一邻域？）

   ​	在得到mesh上各顶点的信号的梯度和拉普拉斯后，将它们与当前信号$f(t)$进行连接，得到差分图（different graph)(有限差分形成的图),差分图包括描述空间变化的所有信息。

   此时，每个节点的特征包括（定义在节点上的信号值，经过调制的梯度，经过调制的拉普拉斯）

   **虽然得到了差分图（包括梯度和拉普拉斯和信号值），但是作者并没有直接用差分图进行信号值的更新和计算，而是将它作为一个特征向量，输入进RGN中，进行信号值的更新。**

   所以作者设计这个模块，求图上顶点和边的梯度和拉普拉斯的目的并不是为了进行PDE的计算，而是将之作为一种特征来指导网络参数的更新。= =

2. 将第一步得到的差分图输入进RGN中（这里的GRN可以是任意的循环图网络），进行信号值的更新。



**训练过程：**

对于要求解的动态PDE，根据在连续背景下的公式，结合有限差分，得到其对应的离散情况下的PDE公式，然后采样得到数据集。

数据集中数据为$(x,y),t_i,f(t_i)$，即对于每个空间中的采样点(x,y)，都有一系列的时间步长的时间和对应时间下的信号值。

训练过程为有监督训练，损失函数为预测值与真值的MSE。



##### 论文2：Learning high-order geometric flow based on the level set method

这篇文章提出了一个新的框架，来求解高阶几何流。然后用提出的框架求解两个GPDE，并用实验验证了该框架在不同的模型上的效果。

作者将求解PDE问题看作PDE约束的优化问题，结合PINN和LSM，求解高阶的GPDE（几何偏微分方程）

【(该框架就是PINN + LSM)，整体的思想和做法和PINN一样，不同点在于：

1. PINN求解的一般都是简单且低阶的PDE，该框架求解的是复杂且高阶的几何流(GPDE)
2. PINN对数据的需求比较少，只需要初边值即可，该框架则是通过数据驱动。

该框架的算法步骤如下：

![image-20241014123255074](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241014123255074.png)

其中，梯度下降的公式为：

![image-20241014123452603](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241014123452603.png)

需要减去两项，分别是要初边值以及各个约束项。



###### High-order quasi Xuguo flow

根据相关论文和LSM，可得，需要求解的高阶GPDE为：

![image-20241014130213151](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241014130213151.png)

结合LBO公式：（以平均曲率为例）

![image-20241014130256131](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241014130256131.png)

可得：

![image-20241014130316462](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241014130316462.png)

最终可以得到：

![image-20241014130330326](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241014130330326.png)

对于每个$e_i$，第一项为已经预计算出的值，即ground true，第二项为模型预测出的值。

要求解的损失函数为：

![image-20241014130351418](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241014130351418.png)

求解过程参考上面的算法1。

对于这个损失函数，我不理解$e_1\sim e_7$这几项的意义，从公式上看，是将复杂的GPDE进行一步一步的分解，分步计算最终的PGDE，$e_1\sim e_7$ 则是每一步产生的误差。

但是前面的一项是怎么来的？代码写的和论文也并不相同，代码中同时对两个要处理的GPDE进行求loss，并且有多项减号前后是相同的。


###### High-order surface diffusion flow of Cahn–Hilliard model

使用LSM和k的高阶LBO来近似表面扩散流，以平滑PDE系统。

引入 Heaviside function：

![image-20241014141347598](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241014141347598.png)

new Cahn-Hilliard model reads as:

![image-20241014141437791](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241014141437791.png)



##### 论文3：Deep Level Sets for Salient Object Detection

这篇论文用一系列卷积层初始化显著图，然后在超像素级别对其进行细化。 水平集损失函数用于帮助学习二进制分割图。

模型的架构如下：

![image-20241014145556029](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241014145556029.png)

首先将图片输出VGG16进行显著性目标检测， 得到检测结果的二值图，将二值图的值水平移动到[-0.5,0.5]，作为初始的水平集，然后通过GSF对结果进行进一步的处理，最终借助Heaviside function，将水平集图转换为最终的显著性图。

**训练过程及输入输出**

首先训练VGG15个epoch，输入为224\*224的图像，通过BCE进行有监督训练，输出为代表显著性的56\*56二值图。
（水平集初始化）然后使用水平集方法，通过水平集loss对VGG进行无监督的训练（微调），水平集的更新借助自动微分，通过作者得出的对水平集的导数进行。输入为初始化后的水平集图，输出为优化后的水平集图。
最后将GSF层加入网络，进行最后的微调。输入为缩放到224\*224的水平集图和通过gslicr生成的有400~500个超像素的图，输出为经过过滤后的水平集图。



###### 水平集方法

水平集方法在这篇文章中的用法为：通过损失函数来帮助VGG学习二进制分割图。

水平集在模型中的表示方式为：VGG输出图像上每个像素的标量值经过水平位移到[-0.5,0.5]得到，并作为初始水平集。

作者通过定义水平集的损失函数，来约束和更新图像上的水平集：

![image-20241015094034617](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241015094034617.png)

Length(C):

将对曲线的积分转换为对曲面的积分。

![image-20241015094229615](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241015094229615.png)



最后两项强制显著性图在显著区域内部和外部均一致。常数$c_1、c_2$表示内部和外部的显著性均值。

![image-20241015103127712](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241015103127712.png)

Heaviside 函数及其导数：

用于将最终的水平集转换为二值图（显著图）。【并不是只进行一次，而是每次训练都会参与】

![image-20241015094255258](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241015094255258.png)

H函数会在$z = 0$处发生突变，且其导数只在0处有值，其余位置为0，会在优化过程中陷入局部极小值。作者使用改进的H函数来解决这个问题：

![image-20241015094805744](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241015094805744.png)



Loss对水平集$\phi$的导数为：

![image-20241015103318058](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241015103318058.png)

可以借助自动微分求梯度，进行计算。

问题1：这里是自动微分完成，还是编写代码完成？



##### 论文4：DevelSet: Deep Neural Level Set for Instant Mask Optimization

该文章目的是进行掩码优化

模型由两部分组成，第一部分名为DSN，由ViT作为主干网络，并通过两个分支网络同时优化两个损失函数，第二部分名为DSO，通过CG法对DSN输出的水平集图在GPU上进行优化，得到最终的掩码图。

![image-20241015160037204](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241015160037204.png)

训练过程：

1. 作者使用TSDF来作为水平集。
2. 作者没有使用Fast Matching 来生成初始TSDF，而是自己开发了一种基于 CUDA 并行性特性的 TSDF 算法。将输入的掩码图转换为初始水平集图。（当应用于神经网络创建的复杂掩模时，CUDA_TSDF可以减少98%以上的TSDF计算时间）
3. 将通过CUDA TSDF算法生成的初始水平集图作为输入，输入DSN。
4. DSN有两个输出分支，分别用于输出$m_{\theta}$ 矩阵（加权矩阵， 用于DSO优化时曲率使用）和准优化的水平集图 $\phi_{0,\theta}$。
5. DSO以 $\phi_{0,\theta}$ 作为输入，通过CG（共轭梯度法），结合CFL条件在GPU上对水平集图进行优化。其中，DSN输出的$m_{\theta}$用于水平集函数中速度的计算。



###### DSO（DevelSet-Optimizer）

DSO 的损失由两部分组成：

![image-20241015170536387](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241015170536387.png)

第一项：

（ILT校正损失旨在最小化输出标称图像和输入目标之间基于像素的差异）

![image-20241015171555029](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241015171555029.png)

第二项：

（最小化 PVBand 的面积，我们期望最小/最大工艺条件下最里面/最外面的晶圆尽可能接近目标图像）

![image-20241015171603753](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241015171603753.png)

此时速度为：

![image-20241015171644587](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241015171644587.png)



水平集的运动方程为：

![image-20241015171851147](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241015171851147.png)





引入曲率项来控制曲线形状，为了增加曲线形状的平滑度，同时保持一些尖锐的边缘，曲率权重在不同位置应该不同，这个问题通过添加加权矩阵 $m_0$ 来解决。

最终水平集的演化方程为：

![image-20241016101953850](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241016101953850.png)

通过将对梯度和曲率的求解集成到cuda中，可以极大加快优化的速度。具体算法如下：

由于每个像素的计算都是独立的，因此可以多线程并行执行。

![image-20241016105548024](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241016105548024.png)

优化DSO 的算法过程如下：

![image-20241016165937104](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241016165937104.png)

因为这是涉及到光刻的优化过程，所以和一般的优化过程有一些不同，关键不同在于：

$Z、M、\phi$ 这三个变量，$Z$ 表示光刻图案，显示当前优化的效果。$M$ 表示光刻图案对应的掩膜，$\phi$ 为水平集函数。

一般的优化过程，直接根据当前水平集来计算速度，然后更新得到新的水平集。

但是这个优化过程在计算速度时需要计算两部分：

- 每个像素上水平集的曲率[直接计算水平集的梯度和曲率即可]【内部项】
- 损失函数的导数。需要先将水平集转换为掩膜（将[-x,y]范围的矩阵转换为二值图），然后根据掩膜来计算梯度。[通过自动微分进行]【外部项】

掩膜会影响光刻图案，水平集又用来更新掩膜，所以在每次迭代中，水平集更新后，需要根据当前的水平集来更新掩膜，进而更新光刻图案。

![image-20241016171719302](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241016171719302.png)

###### DSN（DevelSet-Net）

DSO是优化的主体，DSN相当于一个预处理，用于生成准优化水平集和曲率的加权矩阵。

用于加速DSO的优化（减少优化次数）

DSN通过多分支的输出，同时优化两个损失函数：

![image-20241016110845754](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241016110845754.png)

- 水平集分支预测

  将预测值和GT值的MSE作为目标函数。LSM的GT值是在DSN训练前，使用DSO迭代生成的。

  （DSO也可以直接使用DSN的输入作为输入，只不过训练时间更久，且可能陷入局部最优，使用DSN的输出作为输出，有更好的初始解，可以避免局部最优，且迭代次数减少90%）

![image-20241016111056018](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241016111056018.png)

- 调制分支预测

  调制分支需要学习如何在曲率变化敏感的区域做出最合适的调整，以保持边界形状的平滑性和复杂度控制。

  通过给水平集$\phi$ 的 GT值加上一组小的偏移，来检测曲率敏感区域。

  （对比不同的偏移距离，以及TSDF的变化情况，可以判断曲率的敏感度）

  （对曲率比较敏感的区域一般是比较锐利或是拐角，在优化过程中需要避免这些区域过度光滑）

  ![image-20241016130324317](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241016130324317.png)

  $H(\phi)$ 是 Heaviside 函数

  使用AHF 避免在$\phi = 0$ 时的不连续性：

  ![image-20241016131654286](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241016131654286.png) 

  偏移量 h是从[−20,20] 的均匀分布中采样的。

  因此，该分支的目标函数为：

  ![image-20241016130603409](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241016130603409.png)

  其中：

  ![image-20241016161716753](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241016161716753.png)

  分支的输出为：$H_{\varepsilon }(\phi_{m,\theta})$

  

**参数选择器**

因为在优化过程中有很多可变的超参数，因此通过这个事先训练的参数选择器对参数组合进行最优的选择。



问题：

1. DSN两个分支的损失函数中的GT值是通过事先训练DSO得到的，但是训练DSO时$m_{\theta}$ 是怎么来的？事先训练DSO时没有使用曲率项？
2. 调制分支，计算损失函数时的$m_{gt}$ 是什么？





##### 小结

论文3和论文4都是借助水平集损失函数来优化掩码，且都提供真值进行有监督学习，虽然用到了水平集，但是只是对于每个输入预测的LS，只会与真值进行一次mse，用于更新模型的参数。没有水平集演变的过程。

论文2有演变的过程，作者提出一个类似PINN的框架，但是是有监督训练，并且求的是复杂高阶的GPDE，通过将高阶的GPDE拆分为多项，分别设为损失函数，进行有监督的迭代训练。



论文2提出的框架问gpt，训练过程中水平集是如何表示和更新的。



##### 论文5：Deep Level Sets: Implicit Surface Representations for 3D Shape Inference

将水平集函数隐式表示的曲面拟合为目标模型的表面。

这篇论文，作者提出了一个基于水平集函数的损失函数：

![image-20241016215317916](https://raw.githubusercontent.com/poinne/md-pic/main/image-20241016215317916.png)

并设计了一个简单的CNN网络，分别使用水平集loss和以前的基于体素的loss，通过输入的多视角图像重建出3d模型，以证明作者提出的水平集loss的优势。

如何表示3d模型上的水平集

训练过程是怎么样的

如何通过多视角图像提取信息并与水平集结合的？