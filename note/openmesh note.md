## openmesh note

- 在OpenMesh中，应该使用 `Vec3d` 类型来执行这些向量运算(如点乘，叉乘)，而不是 `Normal` 类型。

在OpenMesh中，将 `Normal` 类型转换为 `Vec3f` 或 `Vec3d` 类型可以通过以下方式完成：

1. **Normal 转换为 Vec3f**： 如果您的 `Normal` 类型是 `Vec3f`，则可以直接进行类型转换，因为它们的底层数据结构是相同的。

   ```c++
   cppCopy codeOpenMesh::Normal normal;
   OpenMesh::Vec3f vec3f(normal[0], normal[1], normal[2]);
   ```

2. **Normal 转换为 Vec3d**： 如果您的 `Normal` 类型是 `Vec3f`，您需要将其分量的类型转换为双精度浮点数（`double`）：

   ```c++
   cppCopy codeOpenMesh::Normal normal;
   OpenMesh::Vec3d vec3d(static_cast<double>(normal[0]), static_cast<double>(normal[1]), static_cast<double>(normal[2]))
   ```

- 有一个点查询三个点（同一个三角形）

  ```c++
  for (auto v_it = mesh.vertices_begin(); v_it != mesh.vertices_end(); ++ v_it)
      {
          auto vhandle = (*v_it);
          auto p_v = mesh.point(vhandle);
          int count = 0;
          for (auto voh_it = mesh.voh_iter(vhandle); voh_it.is_valid(); ++ voh_it)
          {
          auto next_voh = mesh.next_halfedge_handle(*voh_it);
          auto to_voh = mesh.to_vertex_handle(*voh_it), to_next_voh = mesh.to_vertex_handle(next_voh);
              //其余两个点
          auto p_v_2 = mesh.point(to_voh), p_v_3 = mesh.point(to_next_voh);
          auto v12 = p_v_2 - p_v, v13 = p_v_3 - p_v;
          auto v21 = p_v - p_v_2, v23 = p_v_3 - p_v_2;
          auto v31 = p_v - p_v_3, v32 = p_v_2 - p_v_3;
              //两个边的夹角
          auto angle_2_cos = OpenMesh::dot(v21, v23);
          auto angle_2_sin = OpenMesh::norm(OpenMesh::cross(v21, v23));
          auto angle_2 = atan2(angle_2_sin, angle_2_cos);
          auto angle_3_cos = OpenMesh::dot(v31, v32);
          auto angle_3_sin = OpenMesh::norm(OpenMesh::cross(v31, v32));
          double angle_3 = atan2(angle_3_sin, angle_3_cos);
              }
              
          }
  ```

- 计算每个半边顶点的cot权重

  ```c++
  // cot laplacian matrix
  std::vector<double> cots;
  cots.resize(pre.n_halfedges(), 0.);
  
  for (auto he_it = pre.halfedges_begin(); he_it != pre.halfedges_end(); he_it++)
  {
  	auto he = (*he_it);
  	if (pre.is_boundary(he))
  	{
  		continue;
  	}
  	auto v0 = pre.point(pre.from_vertex_handle(he));
  	auto v1 = pre.point(pre.to_vertex_handle(he));
  	auto v2 = pre.point(pre.from_vertex_handle(pre.next_halfedge_handle(he)));
  
  	auto e0 = v0 - v2;
  	auto e1 = v1 - v2;
  	double cotangle = dot(e0, e1) / cross(e0, e1).norm();
  	cots[he.idx()] = cotangle;
  }
  ```

- 构建laplacian matrix

  ```c++
  for (int i = 0; i < n_nums; i++)
  {
  	if (focus.size() > 0)
  	{
  		trivec.emplace_back(i, i, 1.);
  		continue;
  	}
  	auto vh = pre.vertex_handle(i);
  	double weight_sum = 0.;
  	for (auto voh_it = pre.voh_iter(vh); voh_it.is_valid(); voh_it++)
  	{
  		auto heh = (*voh_it);
  		double weight = cots[(*voh_it).idx() + cots[(*voh_it).opp().idx()]];
  		weight_sum += weight;
  		trivec.emplace_back(vh.idx(), (*voh_it).idx(), -weight);
  	}	
  	trivec.emplace_back(vh.idx(), vh.idx(), weight_sum);
  }
  ```

  

