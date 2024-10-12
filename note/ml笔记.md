### ml笔记

- 反向传播算法

![image-20240301133821401](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240301133821401.png)

1. 先根据权重,偏移和初始值求出最终值。
2. 根据最终值计算损失函数。
3. 根据求导的链式法则计算出对网络中各个参数的偏导，沿着反方向和学习率更新各个参数。

- 泛化误差

  在机器学习中，[泛化误差](https://www.zhihu.com/search?q=泛化误差&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A3076482004})（generalization error）是指模型在未见过的新数据上的预测误差。简单来说，泛化误差度量的是模型对新数据的适应性，或者说是模型的预测性能

- torch.where()函数[用法](https://blog.csdn.net/Orientliu96/article/details/104827391)

  ```python
  torch.where(condition, x, y) -> Tensor
  ```

##### FC层与MLP的区别

**MLP**是指多层感知器，它是一种包含多个隐藏层的神经网络。 M**LP**可以通过学习输入数据的层次结构来提取复杂的特征，适用于处理复杂的数据集和任务。 与Dense层不同的是，M**LP**中的每个神经元只与前一层的部分神经元连接，因此可以减少参数的数量，提高模型的泛化能力。 **FC** **FC**是指全连接层，它是一种特殊的Dense层，其中每个神经元都与前一层的所有神经元和后一层的所有神经元连接。 **FC层**可以学习数据的复杂特征，并且具有很高的灵活性，适用于各种不同的任务和数据集。 然而，由于**FC层**中的参数数量较多，训练时需要更多的时间和计算资源。

##### softmax 与 sigmoid

**Softmax**和**Sigmoid**函数是两种常用于神经网络中的激活函数，尤其在分类任务中，它们分别用于不同的场景和任务。

##### 波束搜索

相当于对贪婪算法进行了扩展，有一个名为波束宽度的超参数。示例如下：

![image-20240914160341152](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240914160341152.png)

每次选择都会选中宽度数量的概率最大的分支，同时，会将新分支的概率乘以之前的概率作为新的分支的概率（即考虑之前的概率）

##### 端到端学习

非端到端
传统机器学习的流程往往由多个独立的模块组成，比如在一个典型的自然语言处理（Natural Language Processing）问题中，包括分词、词性标注、句法分析、语义分析等多个独立步骤，每个步骤是一个独立的任务，其结果的好坏会影响到下一步骤，从而影响整个训练的结果，这是非端到端的。

端到端
从输入端到输出端会得到一个预测结果，将预测结果和真实结果进行比较得到误差，将误差反向传播到网络的各个层之中，调整网络的权重和参数直到模型收敛或者达到预期的效果为止，中间所有的操作都包含在神经网络内部，不再分成多个模块处理。由原始数据输入，到结果输出，从输入端到输出端，中间的神经网络自成一体（也可以当做黑盒子看待），这是端到端的。

两者相比，端到端的学习省去了在每一个独立学习任务执行之前所做的数据标注，为样本做标注的代价是昂贵的、易出错的



##### 交叉熵损失函数

![image-20240914153535894](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418113056720.png)

### 1. Sigmoid 函数

Sigmoid 函数是一种**S型曲线**，用于将输入值压缩到 $ (0, 1) $ 的范围内。它的公式是：
$
\sigma(x) = \frac{1}{1 + e^{-x}}
$
其中 $ e $ 是自然常数，$ x $ 是输入值。

#### 特性：
- 输出值在 $ (0, 1) $ 之间，因此可以理解为概率。
- 对于大正数，$ \sigma(x) $ 接近于 1；对于大负数，$ \sigma(x) $ 接近于 0。
- 函数值对 $ x = 0 $ 对称，$\sigma(0) = 0.5$。

#### 使用场景：
- **二分类问题**：Sigmoid 函数常用于**二元分类**任务的输出层，将网络的输出值转换为 0 到 1 之间的概率值。
- **概率模型**：在概率相关的模型中，Sigmoid 函数被用来表示事件发生的概率。

#### 缺点：
- **梯度消失问题**：对于非常大的正数或负数，Sigmoid 的梯度变得非常小，这在深度网络训练中可能会导致梯度消失问题。

### 2. Softmax 函数

Softmax 函数用于将一个向量的任意实数值转换为**概率分布**。它的公式是：
$
\text{softmax}(x_i) = \frac{e^{x_i}}{\sum_{j=1}^{n} e^{x_j}}
$
其中 $ x_i $ 是输入向量的第 $ i $ 个元素，$ n $ 是向量的长度。

#### 特性：
- Softmax 将输入的每个元素转换为 0 到 1 之间的值，且所有元素的和为 1。
- Softmax 突出较大的值，使得较大的输入对应较高的概率。

#### 使用场景：
- **多分类问题**：Softmax 函数通常用于**多分类**任务的输出层，它将网络的输出值转化为每个类别的概率，概率之和为 1。
- **分类问题中的概率分布**：在分类任务中，Softmax 输出可以表示输入属于每个类别的概率。

#### 优点：
- 可以处理多分类任务，输出可以解释为各类的概率分布。

### 对比：
- **输出范围**：Sigmoid 将输入压缩到 $ (0, 1) $，而 Softmax 使得一组输出值之和为 1，生成概率分布。
- **应用场景**：Sigmoid 主要用于二分类问题，而 Softmax 适用于多分类问题。
- **多样性处理**：Sigmoid 处理单个输出，而 Softmax 处理一组输出。

总结：Sigmoid 适合用于二分类问题或某些概率模型的内部节点，而 Softmax 则更常用于多分类问题的输出层，提供一个概率分布。

## GAN

- 我将对抗网络理解成：用判别器这个神经网络取代了静态损失函数，从有监督跨越到无监督。经过这种修改，原本对单个网络的优化任务，现在变成对两个神经网络的[双层优化](https://so.csdn.net/so/search?q=双层优化&spm=1001.2101.3001.7020)任务，由此带来的训练不稳定问题我们后面会解决。

![image-20240321160914123](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418112742028.png)

- 了解对抗网络的人类能根据图 5 左的曲线详述这个不稳定的训练过程：
  1. 前期，生成的图片非常假，被判别器一眼识破，因而判别器的损失急剧下降到 0，生成器的损失上升到极大值。
  2. 中期，生成图片的质量逐渐上升，因此生成器损失逐渐回落。同时，判别器的判别难度也不断变高，表现为判别器损失逐渐增加。若此过程可以持续，则模型会逐渐收敛。
  3. 后期，震荡开始（50~500），训练不稳定。生成器与判别器有一方的损失开始波动。同时或者带动另一方的损失波动。如果用肉眼看生成器的输出，则会观察到生成器生成的图片在较差与很差之间波动。（**不收敛 non-convergence**，训练后期模型**持续震荡 oscillate over time**）

- 反演

  在机器学习和深度学习中，"反演"通常指的是从模型的输出反推回输入的过程。在生成对抗网络（GAN）中，反演通常指的是从生成的图像推断出生成该图像所需的潜在表示或向量。

  

#### 扩散模型

###### 数学

![image-20240418112606401](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418112606401.png)

![image-20240418112742028](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240914153535894.png)

![image-20240418113056720](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418113122619.png)

![image-20240418113122619](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240321160914123.png)

![image-20240418113217841](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418113339313.png)



![image-20240418113339313](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418113645612.png)

![image-20240418113454711](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418113217841.png)

![image-20240418113645612](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418113454711.png)

![image-20240418113726937](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418114330299.png)

"argmax"是用来表示使某个函数取得最大值的输入变量的取值

![image-20240418114330299](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418115446763.png)

极大似然估计：

需要事先假设数据服从正态分布，然后从数据中采样一些离散的数据，极大似然估计就是通过这组离散的数据来估计所有的数据服从的正态分布的参数（也就是服从的是什么正态分布）[换句话说就是在什么正态分布下采样得到这组数据的概率最高]

求解极大似然估计时会将似然函数取对数，这样比较好求，但是极大对数似然很难求出解析解，所以就有了下面的ELBO，用来求近似的数值解。

通过求ELBO的最大值，近似求极大似然的最大值。

![image-20240418114942014](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418113726937.png)

![image-20240418115446763](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418114942014.png)











###### DDPM

![image-20240418115804085](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418120015929.png)



![image-20240418120015929](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418132544567.png)

![image-20240418115843257](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418115804085.png)

去噪过程也是一个马尔可夫链，$x_{t-1}时刻的图像只与x_t$时刻的图像有关，且每一步$x_{t-1}$(step)的分布的均值和方差也只由后一时刻$x_{t}$决定。

图像的每一个像素的分布的均值和方差都不相同（并且方差不会影响结果，所以只需要处理均值）

所以网络(U-net)的输入是整个图像，输出是图像每个像素位置的均值和方差（方差不一定有），输出和输入的尺寸是一致的。

![image-20240418130717373](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240419155529729.png)

$q(x_{t-1}|x_t,x_0)$经过推导可以得到下面的公式，除了均值之外，其余都是已知量，因此只需模型预测均值即可。

![image-20240418132544567](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240419155744410.png)



- 理解：

  ![image-20240419155529729](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418130717373.png)

  输入：img ， eps(噪音)[已知]，t

  输出：eps‘ （unet预测的噪音）

  通过eps和eps' 做loss调整unet的参数

![image-20240419155744410](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240418115843257.png)

输入：z_t, t

输出：预测的eps

通过预测的eps和z_t得到z_t-1，重复这个过程，得到最终的z_0

###### DDIM

- 由于DDPM加噪基于马尔科夫链过程，那么在去噪过程过程也必须基于走这个过程，导致step数很多。
- DDIM的训练过程和DDPM一样，则可以利用起DDPM的权重，代码也可重用。而只要重新写一个sample的代码，就可以享受到采样step减少的好处。
- DDIM的采样过程是个确定的过程。

DDIM 的特点：DDIM 和 DDPM 很类似，训练的目标函数也是相同的，所以能直接用 DDPM 训练好的模型，修改一下采样过程就能用了

- 反向过程使用非马尔科夫链过程：DDPM 的前向和反向的每一步都是马尔科夫链的过程，也就是当前时刻的结果$x_{t-1}$只依靠前一时刻的值$x_t$ ，但 DDIM 在- 反向使用的是非马尔科夫链过程，也就是当前时刻同时依靠$x_t$和$x_0$，能够使用更少的步数来生成图片，提高采样的效率（10~50倍加速）
- 有更好的一致性：只要初始值一样，使用不同步长生成的样本最后的高层特征也是相似度的，生成过程是确定的
- 可以使用插值：由于 DDIM 的生成结果有很好的一致性，所以能获得有意义的语义插值
  

###### classifier guidance

未引入classifier guidance时：

1. 生成的图像保真度不高
2. 生成的图像不可控（即多样性高）

作用：

1. 提升扩散模型的保真度 （提高FID值）
2. classifier guidance，将DDPM转换为条件DDPM

特点：

1. 本质上是一种采样方法，有点类似于ddim，并不改变ddpm的训练过程，只改变ddpm的采样方式。即$p(x) -> p(x|y)$
2. 不需要重新训练扩散模型，可以直接使用预训练模型。但是需要额外的训练一个噪声图像分类器，并且在采样时需要额外的梯度引导。使用分类模型的loss作为辅助损失约束。

Classifier-Free Guidance 的想法是这样的：同时训练无条件生成模型和条件生成模型（实际上这俩是一个模型，只是训练时有概率输入是有条件的（0.7），有概率是无条件的（0.3）），在推理时，同时 forward 带输入条件的生成和无条件的生成，然后把俩结果进行线性组合外推，得到最终的条件生成结果。

最终使无条件的生成趋近于有条件的生成

- reverse diffusion 过程

  使用的是unet结构，因为输入的$x_T$和最终生成的$x_0$的尺寸是一样的。

- time embedding

  和Terfromer中的位置编码一样，也是用正弦编码。

  使用时可以有多种方式加入到unet中（直接加或拼接起来等等）

  因为unet的参数是全局共享的，而reverse过程是一步一步预测的，为了使不同阶段预测的有所不同（刚开始预测低频信息，如轮廓，最后预测高频信息，如细节），引入time编码，用来提醒模型现在走到哪一步了

  

![image-20240419161340156](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240419161340156.png)



$L = \lambda_{clip} L_{clip} + \lambda_{pose}L_{pose}+\lambda_{head}L_{head}+\lambda_{im}L_{im}+\lambda_{reg}L_{reg}$