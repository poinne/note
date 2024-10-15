# Pytorch

### transforms 结构及用法

主要是一个transforms.py文件，可以看作是一个工具箱，将特定格式的图片通过工具箱中的工具（class，如totensor、resize等）进行变换，得到想要的结果。

<img src="C:\Users\17839\Desktop\mLearn\31.png" style="zoom:50%;" />

使用transforms时要注意输入和输出，看官方文档 ’Ctrl + mouse1‘，关注方法需要什么参数，通过print()、print(type())查看返回值是什么类型（如果文档没有说明的话）

##### nn.BatchNorm2d

`nn.BatchNorm2d`，即二维批量归一化（Batch Normalization），是深度学习中常用的一种技术，用于加速神经网络的训练并提高其性能。它的作用包括以下几个方面：

1. **加速训练收敛速度**：Batch Normalization 可以加速神经网络的训练，因为它有助于解决梯度消失和梯度爆炸的问题。这意味着神经网络可以更快地收敛到最优解。
2. **减小权重初始化的依赖**：在没有 Batch Normalization 的情况下，初始化神经网络的权重通常需要非常小心，以防止梯度消失或梯度爆炸。Batch Normalization 可以减轻对初始化的依赖，因此更容易训练。
3. **正则化效果**：Batch Normalization 在 mini-batch 上对每个特征进行归一化，因此它有轻微的正则化效果，可以降低过拟合的风险。
4. **稳定性**：Batch Normalization 有助于使神经网络对输入数据分布的变化更加稳定，这使得神经网络对于不同尺度、平移和旋转的输入更加鲁棒。
5. **允许较大的学习率**：由于 Batch Normalization 稳定了网络的训练，通常可以使用更大的学习率，从而加速训练过程。
6. **允许深层网络**：Batch Normalization 有助于训练深层神经网络，因为它减轻了梯度消失问题，使得更深的网络层可以有效地传播梯度。

Batch Normalization 的主要思想是对每个 mini-batch 的输入特征进行归一化，使其均值接近零，标准差接近一。这通过缩放和平移操作实现，这些操作的参数是可以学习的。这样，神经网络的每一层都可以自适应地调整输入的分布，使其更利于训练。