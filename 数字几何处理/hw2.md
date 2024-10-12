#### hw2

#### 计算Local Averaging Region

<img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240402114444424.png" alt="image-20240402114444424" style="zoom:50%;" />

设所求顶点为A，对于该顶点周围的1-ring三角形，有两种情况：

`通过计算两条边之间的点积，与0比较来判断该角是否为钝角，若为钝角则记下该角，并将这个三角形标记为钝角三角形。`

1. **是钝角三角形**

   对于钝角三角形，遍历每个角，分配面积给每个顶点。若该角为钝角，则分配三角形面积的 1/2，否则分配 1/4（每个顶点分配三次，因为每个顶点会遍历三遍）

   <img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240402120427888.png" alt="image-20240402115631211" style="zoom:50%;" />

2. **不是钝角三角形**

   若不是钝角，则需要计算三角形的外接圆，如下图所示，每个顶点分配的面积为该顶点相邻的三角形的 1/2

   <img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240402120437511.png" alt="image-20240402115404025" style="zoom:50%;" />

- 使用OpenMesh::dot()计算角度时，是与PI / 2比较，还是直接与0比较？效果上没有差别？

#### gaussCurvature

主要是通过一个公式：

![image-20240402120427888](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240402115631211.png)

![image-20240402120437511](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240402115404025.png)

即每个顶点的曲率为(2pi - 1领域内三角形在该点的角度之和) / LAR（local average region)

通过这个公式做出来的效果如下：（没有使用colorbar)

![image-20240402120632580](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240402121031404.png)

而调用libigl的api得到的效果如下：

<img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240402150257982.png" alt="image-20240402120709107" style="zoom:90%;" />

通过查看api的源码发现这个方法并没有除以LAR，但是尝试不除LAR后效果和之前实现的差不多，只是颜色不同。如下：（因为没有除LAR，且LAR小于1，所以得到的曲率偏小）

![image-20240402121031404](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240402135314443.png)

#### meanCurvature

通过下面的公式计算：

![image-20240402135314443](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240402120709107.png)

![image-20240402150257982](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240402120632580.png)

对于每个顶点，计算1邻域内每个三角形除顶点所在角之外的夹角的tan值。其中$A_{Mixed}$即LAR

![image-20240402145826337](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240402145826337.png)

使用libigl api效果如下：

![image-20240402152813925](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240402152813925.png)

还是libigl实现的更好