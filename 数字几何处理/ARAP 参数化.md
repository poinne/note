### ARAP 参数化

1. **普通方法参数化到UV平面上**：
   - 首先，使用常规的方法将三维曲面映射到UV平面上。这可能涉及到一些常用的参数化技术，比如Tutte's embedding等，将曲面的每个顶点映射到二维平面上的对应位置。
2. **Local操作**：
   - 在这一步中，将每个三角形拆分为独立的面。
   - 对于每个独立的三角形面，固定全局变换矩阵J，求出在当前全局变换下，每个三角形可以采用的最佳局部变换矩阵L。这里的局部变换指的是将三角形从UV平面映射回曲面上的变换。
3. **Global操作**：
   - 在这一步中，固定局部变换矩阵L。
   - 找到最适合当前局部变换的全局变换矩阵J。这意味着找到一组全局变换参数，使得整个曲面的所有局部变换都能得到最优的结果。
4. **迭代**：
   - 将步骤2和步骤3多次迭代，直到结果趋于稳定为止。这意味着在每次迭代中，根据当前的全局和局部变换，不断优化参数化结果，直到达到满意的效果为止。

![image-20240509142749889](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509115419204.png)

ARAP参数化算法尽可能的保证三角形没有扭曲地映射在二维平面上。

因为需要映射过程中尽可能保持三角形没有扭曲，所有发生变换的矩阵为旋转矩阵。我们可以通过限制变换矩阵，得到相应参数化结果。

假设三维模型上的一个三角形t经过保边映射后表示为$x_T={x^0_t, x^1_t, x^2_t}$,相应的参数化后的三角形表示为 $u_T={u^0_t, u^1_t, u^2_t}$。这两个三角形之间的关系可以用一个2×2的雅可比（Jobabian）矩阵$J_t(u)$表示。

用$L_t$来表示受到限制的变换矩阵，也就是旋转变换矩阵，可以定义如下的能量函数来表示可能的变换$J_t$和目标变换$L_t$之间的差别：
$$
E(u,L) = \sum^T_{t=1}{A_t||J_t(u)-L_t||^2_F}
$$
其中$A_t$是三角形的面积。能量函数也可以重构为：
![image-20240509115419204](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509133112833.png)

此时方程中有两个未知量，$u、x$，u表示当前网格映射到二维平面上的坐标。x表示当前网格中对每个三角形进行全等变换（isometric）到平面上后的坐标。（每个三角形单独变，不需要考虑彼此之间的坐标）(每个三角形的变换是在局部坐标系下完成的？)

在每个3D的三角形上创建一个局部坐标系，那么对于该三角形来说，三角形的3D坐标$(x,y,z)$可以转化为局部坐标系上的2D坐标$(X,Y)$。

<img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509131142009.png" alt="image-20240509131142009" style="zoom:50%;" />

此时，局部坐标系上的2D坐标$(X,Y)$与参数化后的坐标$(u,v)$之间的雅可比矩阵（切映射）为：

<img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509135147610.png" alt="image-20240509133112833" style="zoom:67%;" />

综上：

<img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509134550758.png" alt="image-20240509134550758" style="zoom:67%;" />

使用local/global方法来优化这个目标方程。

**local**

在u和x之间存在对应的jacobi变换矩阵来描述两种坐标之间的变换。固定uv，求出$J_t$:

<img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509135147610.png" alt="image-20240509133112833" style="zoom:67%;" />

将得到的jacobi矩阵进行带符号svd分解：
$$
J_t = U\Sigma V^T
$$
此时，受到限制的旋转矩阵$L_t$:
$$
L_t = UV^T
$$
**global**

确定$L_t$时，$E(u,L)$是一个二次多项式，可以重构为：

<img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509143251436.png" alt="image-20240509135147610"/>

可以通过求导：

<img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509145443678.png" alt="image-20240509143251436" style="zoom:67%;" />

利用最小二乘求解。



### 实现

1. 计算并存储每个3D三角形面的局部坐标系

   ![image-20240509145443678](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509145859412.png)

   ![image-20240509145859412](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509142749889.png)

   ```c++
   Eigen::MatrixXd localcoord;
       localcoord.resize(f_num, 6);
   
       for (int i = 0; i < f_num; i ++)
       {
           auto f_h = mesh.face_handle(i);
           auto n = mesh.normal(f_h);
           auto fv_it = mesh.fv_iter(f_h);
           auto v0 = mesh.point(*fv_it);
           ++ fv_it;
           auto v1 = mesh.point(*fv_it);
           ++ fv_it;
           auto v2 = mesh.point(*fv_it);
   
           auto e = v1 - v0;
           auto x_ = e.normalized();
           auto y_ = cross(n, x_);
           auto e1 = v2 - v0;
   		//  0:x0, 1:y0, 2:x1, 3:y1, 4:x2, 5:y2
           // 下标0这个点作为坐标系的原点，因此为(0, 0)
           // 下标1这个点作为x轴的方向，因为x坐标为点0到点1这条边的长度，y坐标为0
           // 面法向作为z轴，则y轴为x轴与z轴叉乘的方向，下标2这个点的坐标为这个点到原点的这条边分别与x、y轴点乘的结果。
           localcoord.row(i) << 0, 0, e.norm(), 0, dot(e1, x_), dot(e1, y_);
       }
   ```

2. 参数化曲面，得到初始的uv坐标

   这一步可以使用任意方法进行曲面参数化，对结果没有影响。

3. 构建laplacian maxtrix，用于在后续步骤中更新uv（该矩阵保持不变）

   ```c++
   std::vector<double> cots(mesh.n_halfedges());
       std::vector<Eigen::Triplet<double>> trivec;
       for (auto he_it = mesh.halfedges_begin(); he_it != mesh.halfedges_end(); ++ he_it)
       {
           if (mesh.is_boundary(*he_it))
               continue;
           auto v0 = mesh.point(mesh.from_vertex_handle(*he_it));
           auto v1 = mesh.point(mesh.to_vertex_handle(*he_it));
           auto v2 = mesh.point(mesh.to_vertex_handle(mesh.next_halfedge_handle(*he_it)));
   
           auto e0 = v1 - v0;
           auto e1 = v2 - v0;
           double cotangle = dot(e0, e1) / cross(e0, e1).norm();
           cots[(*he_it).idx()] = cotangle;
           int vid0 = mesh.from_vertex_handle(*he_it).idx();
           int vid1 = mesh.to_vertex_handle(*he_it).idx();
           std::cout << vid0 <<std::endl;
           trivec.emplace_back(vid0, vid0, cotangle);
           trivec.emplace_back(vid1, vid1, cotangle);
           trivec.emplace_back(vid0, vid1, -cotangle);
           trivec.emplace_back(vid1, vid0, -cotangle);
       }
       Eigen::SparseMatrix<double> smat;
       smat.resize(v_num, v_num);
       smat.setFromTriplets(trivec.begin(), trivec.end());
   
       Eigen::SparseLU<Eigen::SparseMatrix<double>> solver;
       solver.compute(smat);
   ```

   

4. local：通过参数化uv和局部xy来计算$J_t$，通过SVD分解得到$L_t$

   ```c++
   for (int i = 0; i < f_num; i ++)
           {
               auto f_h = mesh.face_handle(i);
               auto fv_it = mesh.fv_iter(f_h);
               auto i0 = (*fv_it).idx();
               ++ fv_it;
               auto i1 = (*fv_it).idx();
               ++ fv_it;
               auto i2 = (*fv_it).idx();
               // compute Jacobian
               Eigen::Matrix2d P, S, J;
               P << uv(i1, 0) - uv(i0, 0), uv(i2, 0) - uv(i0, 0), uv(i1, 1) - uv(i0, 1), uv(i2, 1) - uv(i0, 1);
               S << localcoord(i, 2) - localcoord(i, 0), localcoord(i, 4) - localcoord(i, 0),
                       localcoord(i, 3) - localcoord(i, 1), localcoord(i, 5) - localcoord(i, 1);
               J = P * S.inverse();
               // svd
               // 同时计算 UV
               Eigen::JacobiSVD<Eigen::Matrix2d> svd(J, Eigen::ComputeFullU | Eigen::ComputeFullV);
               Eigen::Matrix2d U = svd.matrixU();
               Eigen::Matrix2d V = svd.matrixV();
               Eigen::Matrix2d R = U * V.transpose();
   
               if (R.determinant() < 0)
               {
                   U(0, 1) = -U(0, 1);
                   U(1, 1) = -U(1, 1);
                   R = U * V.transpose();
               }
               Lts[i] = R;
               
           }
   ```

   <img src="https://raw.githubusercontent.com/poinne/md-pic/main/image-20240509134550758.png" alt="image-20240509134550758" style="zoom:67%;" />

   

5. global：通过$L_t$和laplacian矩阵，得出新的uv坐标。

   ```c++
   Eigen::VectorXd bu, bv;
           bu.setZero(v_num);
           bv.setZero(v_num);
           for (int i = 0; i < f_num; i ++)
           {
               auto f_h = mesh.face_handle(i);
               auto fv_it = mesh.fv_iter(f_h);
               auto i0 = (*fv_it).idx();
               ++ fv_it;
               auto i1 = (*fv_it).idx();
               ++ fv_it;
               auto i2 = (*fv_it).idx();
               
               auto he2 = mesh.halfedge_handle(f_h);
               auto he0 = mesh.next_halfedge_handle(he2);
               auto he1 = mesh.next_halfedge_handle(he0);
               Eigen::Vector2d e0, e1, e2;
               //v1 - v0
               e0 << localcoord(i, 2), localcoord(i, 3);
               //v2 - v1
               e1 << localcoord(i, 4) - localcoord(i, 2), localcoord(i, 5) - localcoord(i, 3);
               //v0 - v2
               e2 << -localcoord(i, 4), -localcoord(i, 5);
               Eigen::Vector2d b0 = cots[he0.idx()] * Lts[i] * e0;
               bu[i0] -= b0[0];
               bv[i0] -= b0[1];
   
               bu[i1] += b0[0];
               bv[i1] += b0[1];
               Eigen::Vector2d b1 = cots[he1.idx()] * Lts[i] * e1;
               bu[i1] -= b1[0];
               bv[i1] -= b1[1];
   
               bu[i2] += b1[0];
               bv[i2] += b1[1];
   
               Eigen::Vector2d b2 = cots[he2.idx()] * Lts[i] * e2;
               bu[i2] -= b2[0];
               bv[i2] -= b2[1];
   
               bu[i0] += b2[0];
               bv[i0] += b2[1];
               
           }
           uv.col(0) = solver.solve(bu);
           uv.col(1) = solver.solve(bv);
   ```

### 问题

- global步骤bu和bv的操作
- A矩阵含义