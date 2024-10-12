步骤

1. **读入网格模型**

2. **用二维数组存放网格的边信息**

   因为网格模型的点都很多，因此不能直接使用`[][]`来定义二维数组。可以使用vector

3. **使用dijkstra查询两点的最短距离**

   因为是查询指定两点之间的最短距离，因此不必求出所有点之间的最短距离。

保存两点之间的最短路径，然后通过修改路径上顶点的位置来展示，效果如下：

<img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240402154107970.png" alt="image-20240402154107970" style="zoom:50%;" />

[dijkstra](https://blog.csdn.net/qq_74156152/article/details/134481475)

main 传递参数：

```c++
int main(int args, char *argv[])  
// argv[0] 为可执行文件名称
  int v0 = std::stoi(argv[1]);

  int v1 = std::stoi(argv[2]);
```

```bash
./HW1 1 3
```

请求法向：

```c++
 mesh.request_face_normals();
mesh.request_vertex_normals();
mesh.update_normals();
```

[]()