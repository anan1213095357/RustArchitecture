<div align="center">

# 🏗️ WebGL Rust-Style Snapping Engine
**完全还原《Rust》的硬核 3D 建筑吸附引擎（原生 Babylon.js 实现）**

[![Babylon.js](https://img.shields.io/badge/Babylon.js-5.0+-FF7C00?style=for-the-badge&logo=babylon.js&logoColor=white)](https://babylonjs.com/)
[![Algorithm](https://img.shields.io/badge/Algorithm-Rust_Style_Snapping-red?style=for-the-badge)](#)
[![Scale](https://img.shields.io/badge/Scale-Massive_Structures-blue?style=for-the-badge)](#)

</div>

---

## 🚀 核心灵魂：Rust 级别的纯正吸附算法

这个项目的根本目的只有一个：**在 Web 端完美复刻《Rust》那种严丝合缝、随心所欲的插槽式吸附建造（Socket-based Snapping）体验。** 

市面上绝大多数前端 WebGL 建造系统都在使用死板的全局网格（Grid-based）或极易穿模的包围盒判定，导致根本无法搭建复杂的生存类基地。本项目彻底抛弃了这些玩具做法，从底层重构了基于空间矩阵变换的**定向插槽吸附算法**。

### 🧩 1. 定向插槽与空间矩阵算法 (Socket & Matrix Snapping)
算法不再依赖全局坐标系，而是为每一个建筑模块定义了带有方向向量的独立插槽（Sockets）：
*   **精准的法线对齐**：系统实时遍历周围建筑模块，提取预定义的插槽坐标（`pos`）和法线向量（`norm`）。当玩家准星靠近时，算法通过矩阵旋转（`Matrix.RotationY`）强制目标法线相互取反（`.scale(-1)`），实现百分百严丝合缝的边缘贴合。
*   **独立双侧视觉探测 (Dual-sided Detection)**：针对**半高矮墙**、门框、窗户等复杂墙体，系统独创了 `detectPos` 与物理 `pos` 分离的设计。无论墙体多高、多薄，天花板和上层墙体都能精准识别到挂载点，彻底消灭拼接缝隙。
*   **完美兼容三角逻辑**：突破了方形与三角形混合建站的底层数学难题。通过重写基础向量法线（`baseNorm`）与极坐标分布，三角地基与三角天花板能够完美嵌合进方形网格的建筑生态中。

### 🏙️ 2. 突破上限：支持搭建宏大建筑群
基于插槽的局部空间计算带来了极其可怕的扩展性，这意味着你可以用它搭建**摩天大楼或庞大的公会城堡**，而不用担心坐标系崩溃或性能宕机：
*   **无限制的立体延伸**：地基接地基、墙壁接边缘、天花板接墙面、楼梯跨层连接……多达 12 种组件（直梯、L梯、U梯、螺旋梯等）共享同一套吸附逻辑协议，玩家可以无缝向高空和四周无限扩展。
*   **性能零损耗判定**：所有判定均基于局部到世界矩阵（`Vector3.TransformCoordinates`）的轻量级数学运算，绕开了昂贵的物理引擎碰撞查询。预览模型（Ghost Mesh）的生成如丝般顺滑。

### 💥 3. 动态承重与坍塌物理 (Structural Integrity & Decay)
宏大建筑必须受到物理法则的约束。系统内置了类似《Rust》的结构稳定性算法：
*   **BFS 连通性校验**：每次拆除（Destroy）操作后，底层都会触发广度优先搜索（BFS）算法，以地基为根节点实时扫描整栋建筑的结构连通性。
*   **连锁物理坍塌**：一旦某个模块失去与地基的有效连接，系统将瞬间摧毁所有上层悬空建筑，并调用 Cannon.js 物理引擎生成带有冲量（Impulse）的动态碎块，完美还原爆炸抄家时的震撼坍塌效果。

---

## ⚙️ 架构与组件协议

本引擎预设了极高容错率的组件校验协议。不同类型的组件存在严格的吸附优先级与兼容性判定：

| 组件分类 | 吸附协议规则 | 核心代表组件 |
| :--- | :--- | :--- |
| **Foundation (地基)** | 仅吸附同类地基边缘，并提供 Z 轴及 Y 轴基础法线支撑。 | 方形地基、三角地基 |
| **Wall (墙体)** | 强依赖地基或天花板的边缘，同类之间支持垂直叠加（如矮墙堆叠）。 | 标准墙、半高矮墙、窗户墙、门框 |
| **Floor (天花板)** | 必须依附于墙体顶部插槽或同类天花板边缘，是垂直扩展的核心跳板。 | 方形天花板、三角天花板 |
| **Stairs (楼梯)** | 具备复杂的复合插槽，需同时读取底层起点与高层终点的法线数据。 | 直/L/U/螺旋 楼梯 |

---

## 🛠️ 快速上手

纯原生 JavaScript 实现，**零配置，无依赖，无需 Webpack 或 Node.js**。这就是最纯粹的 WebGL 暴力美学。

1. 拉取代码：
   ```bash
   git clone [https://github.com/yourusername/babylon-rust-snapping.git](https://github.com/yourusername/babylon-rust-snapping.git)
