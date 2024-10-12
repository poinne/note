#### amgan

添加一个额外的CNN网络提取类激活图（CAMs），【论文提到：由于不可能对每个属性都有一个真实掩码，我们建议使用类激活映射(CAM)】

相比于cfae，amgan使用cams来监督mask map生成，并且对于生成器的loss多了一个感知损失（使用cnn比较生成图和相对应的真实图的高层的feature）



##### sagan（空间注意力gan）

![image-20240303175032947](C:\Users\YS\AppData\Roaming\Typora\typora-user-images\image-20240303175032947.png)

第一项为重建损失，第二项为保证无关区域不被修改