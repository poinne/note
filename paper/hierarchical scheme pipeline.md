hierarchical scheme pipeline

先简化，再插值回来。

简化过程中信息不就丢失了吗，如果只是按照拓扑关系加点，那顶点的局部特征不就没有了吗，怎么保证无扭曲



$for t = T, ..., 1 do
    \epsilon \sim \mathcal{N}(0, I) \text{ if } t > 1 \text{ else } \epsilon = 0
    x_{t-1} = \frac{1}{\sqrt{\alpha_t}} \left( x_t - \frac{\beta_t}{\sqrt{1 - \bar{\alpha}_t}} \epsilon_\theta(x_t, t) \right) + \sigma_t z$



$for i = 1, ..., K do
    g_t = \nabla_{x_t} L(x_t)
    \hat{\epsilon}_\theta(x_t) \leftarrow \epsilon_\theta(x_t) - \rho_t g_t
    x_t \leftarrow \sqrt{\bar{\alpha}_t} \left( x_t - \sqrt{1 - \bar{\alpha}_t} \hat{\epsilon}_\theta(x_t) \right) / \sqrt{\bar{\alpha}_t} + \sqrt{1 - \bar{\alpha}_t} \hat{\epsilon}_\theta(x_t)$