

- AMGAN[2]使用类激活图和两个鉴别器来识别真假和属性。fashion -AttGAN[24]在AttGAN[9]的基础上进行了改进，通过包含额外的优化目标来更好地适应时尚领域。VPTNet[18]旨在通过将属性编辑作为形状-外观编辑的两阶段过程来处理较大的形状变化。这些作品都采用了GAN框架，因此支持编辑的属性范围相对较少

- 

### GAN Inversion

<<<<<<< HEAD
![rrr](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423155305172.png)

![image-20240423155425269](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423155801920.png)

![image-20240423155650955](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423155425269.png)

![image-20240423155801920](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423155650955.png)

![image-20240423155957898](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423155957898.png)

![image-20240423160114953](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423160231482.png)

![image-20240423160200201](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423160114953.png)

![image-20240423160231482](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423160200201.png)

![image-20240423160355298](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423160355298.png)

上图中(c)就是基于学习的方法，(d)就是基于优化的方法

![image-20240423160906373](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423160906373.png)
=======
![image-20240423155305172](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423155305172.png)

![image-20240423155425269](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423155801920.png)

![image-20240423155650955](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423155425269.png)

![image-20240423155801920](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423155650955.png)

![image-20240423155957898](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423155957898.png)

![image-20240423160114953](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423160231482.png)

![image-20240423160200201](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423160114953.png)

![image-20240423160231482](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423160200201.png)

![image-20240423160355298](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423160355298.png)

上图中(c)就是基于学习的方法，(d)就是基于优化的方法

![image-20240423160906373](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240423160906373.png)
>>>>>>> b90bd3d (实验室提交)



纷纷



正常情况下是GAN的生成器是随机选择一个噪音z，然后生成一幅图像x'，如果这个GAN的生成器已经训练好了，那么就会生成一个和所给条件尽可能相似的图像x'，此时x' 近似于原始图像x，那么就可以看作$x=G(z)$，GAN Inversion则是将图像x映射回潜在表达z’，进而得到由预训练的生成器G合成的图像x‘，其中x'与真实的x要足够接近。

$z^* = arg min_zL(G(z,\theta),x)$，其中L是图像或特征空间中的距离度量

综上就是：把给定图像映射到预训练 GAN 模型的 latent space，以便生成器可以从 latent code 重建图像。  （通过改变 latent code，我们可以在保留生成图像一些属性的同时操纵另一些属性。）

总结：以前是通过噪声找图像，现在是通过图像找噪声；





无论 GAN inversion 方法如何，一个重要的设计选择是将图像嵌入到哪个 latent space。 一个好的 latent space 应该是（各种语义属性相互）分离的并且易于嵌入。这种 latent space 中的 latent code 具有以下两个属性：它忠实地、逼真地重建输入图像，并有助于下游图像编辑任务。

