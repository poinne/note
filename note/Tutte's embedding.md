### Tutte's embedding 参数化

Tutte理论：

对于一个三角形网格曲面，如果把边界固定到一个凸多边形上，且网格内部点是它的一邻域的凸组合的话，那么就可以通过求解一个线性方程组，得到双射的参数化结果。

![image-20240509154940712](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509160225521.png)

**对于边界顶点**

<img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509154940712.png" alt="image-20240509160225521" style="zoom:67%;" />

**对于内部顶点**

<img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240329164921735.png" alt="image-20240509160247432" style="zoom:67%;" />

### 实现

1. **读入一个网格**

2. **找到网格的边界（有序）**

   先确定两个有前后关系的边界顶点，然后依次求出其余边界顶点

3. **将网格的边界放到一个凸多边形上（比如说一个圆，N个边界点，圆分为n等分）**

   根据边界顶点数将2pi进行平分，然后对每个顶点依次求cos和sin，得到对应的二维坐标。

4. **然后所有的内部点都是其一邻域的凸组合，可以得到稀疏线性方程组，AX=B(B边界点为1，内部点为0)**

   创建稀疏矩阵后，可以使用三元组（x,y,value)来向矩阵放入元素。

   因为使用的是Uniform Laplacian Matrix,因此各个元素值如下：

   ![image-20240329164921735](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509160247432.png)

   为了固定边界点，设置边界点值为1，内部点对角线位置为度，有边为-1，无边为0.

   ![image-20240329165509826](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240329165509826.png)