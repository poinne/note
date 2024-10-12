![image-20240905200903457](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240905200903457.png)

添加额外的loss（arap）来使模型理解几何形变，保持局部刚性



不同模型的关键点匹配

JSM（加入和两个模型都较相似的模型作为中间模型）



![image-20240905203442004](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240905204236258.png)





第二位：

![image-20240905204236258](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240905204504191.png)



借助图神经网络在点云上定义拉普拉斯算子



![image-20240905204504191](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240905204737265.png)

![image-20240905204628239](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240905203442004.png)

laplace算子在网格上的定义：

![image-20240905204737265](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240905204628239.png)



点云没有拓扑关系，如何计算laplace？

1. 通过重建、三角化来恢复拓扑（之前的方法），再计算laplace

   



通过k-means生成拓扑

![image-20240905205800981](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240905210048299.png)





![image-20240905210048299](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240905205800981.png)

构建一个网络，来学习laplace算子的行为：

将laplace算子当作一个黑盒，一个向量通过这个黑盒得到一个对应的输出，

构建多组向量，分别应用到真实的laplace算子和生成的laplace上。

![image-20240905210345792](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240905211419508.png)

![image-20240905211419508](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240905211442275.png)

![image-20240905211442275](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240905210345792.png)