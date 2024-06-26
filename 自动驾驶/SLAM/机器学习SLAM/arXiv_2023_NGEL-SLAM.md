# 零、概念和问题

## 概念

##### 1、隐式神经表示

Implicit Neural Representation ，是一种利用神经网络学习场景或对象的表示的方法。

与显示表示的区别

- 相对于显示表示，隐式表示并不直接输出场景或对象的具体几何形状或颜色信息，而是通过学习一个隐含函数来生成所需的输出。

显示表示举例：能直接看到三维模型

- point cloud
- [Mesh](https://blog.csdn.net/Zsh00058555/article/details/114644574)
- voxel

隐式神经表示举例：

- 典型的例子是NeRF(可见反射表面无关表征)，其中神经网络学习了一个函数，该函数接受三维空间坐标作为输入，并输出对应点的颜色和密度信息。
- 这些隐式函数通常是深度神经网络，如多层感知器（MLP）或卷积神经网络（CNN）。

##### 2、NeRF

[NeRF：用深度学习完成3D渲染任务的蹿红 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/390848839)

一种用于三维场景重建的计算机图形学技术。

工作原理

- 训练一个神经网络(多层MLP)，该网络接受从不同视角观察的图像作为输入，然后输出场景中每个点的颜色和密度。
  - 将三维场景用MLP表示，前向的网络计算就和人眼或者相机观察三维场景的过程一致，当整个计算过程都可微的时候，通过**渲染图片**的监督，便可以对MLP进行优化，“学”出三维场景的“隐式”参数。

- 这些输出随后可以用于生成新的图像，从而使得神经网络能够生成逼真的三维场景。

![img](https://raw.githubusercontent.com/letMeEmoForAWhile/typoraImage/main/img/v2-8540750b44b1da49ccd1fe38915324a0_720w.webp)

##### 3、全局一致性

指建立的地图与实际环境中的**拓扑结构**和**几何结构**相一致的性质。

包括：

1. **环境拓扑结构的一致性**：反映出环境中房间、走廊、门等关键结构的布局，而不是将它们错置或者漏掉。
2. **几何结构的一致性**：地图中的物体形状和相对位置应该与实际环境一致。
3. **尺度的一致性**：地图的尺度应该与实际环境相符。

##### 4、占用 occupancy

通常指的是一个场景中的三维空间中是否被实体物体占用或被填充。

具体来说，在三维重建中，占用场景通常用占用格子(occupancy grid)或体素(voxel)表示。如果某个格子或体素被占用，表示该位置存在物体

##### 5、Uncertainty

为了识别占用率没有被良好训练的像素。

NeRF（可见反射表面无关表征）是一种用于从二维图像重建三维场景的技术，它使用神经网络来学习场景中每个点的颜色和密度。然而，由于训练数据的有限性和噪声，NeRF可能无法完全捕捉场景的所有细节，导致在渲染新视角时出现不确定性或模糊。

NeRF中的不确定性（uncertainty）是指NeRF对场景的表示的可信度或可靠性的度量。不确定性可以分为两种类型：

- **模型不确定性（model uncertainty）**：这是指由于神经网络的结构或参数的不确定性导致的不确定性。模型不确定性反映了NeRF对场景的理解程度，以及它能否泛化到新的视角。模型不确定性可以通过使用集成方法（ensemble）或贝叶斯神经网络（Bayesian neural network）等技术来估计或降低。
- **数据不确定性（data uncertainty）**：这是指由于训练数据的噪声或不完整性导致的不确定性。数据不确定性反映了NeRF对场景的观察程度，以及它能否重建未观察到的区域。数据不确定性可以通过使用不确定性估计（uncertainty estimation）或主动学习（active learning）等技术来估计或降低。

[NeRF中的不确定性的量化和降低对于提高NeRF的性能和可解释性非常重要。一些最新的研究工作](https://arxiv.org/abs/2209.08546)[1](https://arxiv.org/abs/2209.08546)[2](https://arxiv.org/abs/2209.08718)[3](https://arxiv.org/abs/2109.02123)[已经探索了在NeRF中引入和利用不确定性的方法，取得了一些进展。](https://arxiv.org/abs/2209.08546)[1](https://arxiv.org/abs/2209.08546)[2](https://arxiv.org/abs/2209.08718)[3](https://arxiv.org/abs/2109.02123)分别是以下三篇论文的编号：

- [ActiveNeRF: Learning where to See with Uncertainty Estimation](https://arxiv.org/abs/2209.08546)[1](https://arxiv.org/abs/2209.08546)
- [Density-aware NeRF Ensembles: Quantifying Predictive Uncertainty in Neural Radiance Fields](https://arxiv.org/abs/2209.08546)[2](https://arxiv.org/abs/2209.08718)
- [Stochastic Neural Radiance Fields: Quantifying Uncertainty in Neural Radiance Fields](https://arxiv.org/abs/2209.08546)[3](https://arxiv.org/abs/2109.02123)

## 问题

##### 1、动态局部建图进程中，建图模块是怎么用关键帧训练局部地图的

##### 2、NeRF在SLAM中具体是怎么作用的，输入和输出分别是什么

输入：

- 场景的连续图像。

输出：

- 场景的隐式表示。

举例：

- iMAP使用单目相机获取场景的连续图像，并使用视觉里程计技术估计相邻场景之间的相对位姿。
- 然后，将相邻图像的位姿估计结果作为NeRF的输入，生成相邻图像之间的3D场景。
- 最后，使用优化算法对相邻图像之间的位姿进行优化，从而实现对场景的实时重建和定位。

# 一、背景

论文题目：NGEL-SLAM: Neural Implicit Representation-based Global Consistent Low-Latency SLAM System

传统的SLAM系统，如ORB-SLAM三个系列

- 优点
  - 低延迟，高精度定位
  - 并采用环路检测来确保全局一致性
- 缺点
  - 只能构建稀疏的点地图，缺乏稠密的几何和纹理信息

基于神经隐式表示的SLAM系统，如iMAP和NICE-SLAM

- 优点
  - 拥有高保真的场景重建
- 缺点
  - 基于神经表示的定位缺乏对环路闭合的支持，导致在大场景中由于缺乏全局一致性而性能不佳。
  - 即使环路闭合被集成到他们的系统中，==例如通过用传统SLAM系统替换定位，针对更新后的姿态重新训练整个地图需要大量时间。==
  - 此外，他们制图网格的慢速收敛进一步阻碍了满足低延时要求。

##### 本文提出的系统：NGEL-SLAM

- 将传统SLAM系统ORB-SLAM3的定位精度与隐式神经表示提取稠密网格和生成高保真图像的能力相结合。

##### 主要贡献：

- 准确的跟踪和绘图。
- 全局一致性。集合了闭环检测来确保全局一致性。
- 低延时
- 在各种数据集上进行实验，展示了与基线相比具有竞争力的性能。

# 二、系统架构

![image-20240102152914073](https://raw.githubusercontent.com/letMeEmoForAWhile/typoraImage/main/img/image-20240102152914073.png)

包含两个主要模块：

- 跟踪模块
- 建图模块

也可进一步分为三个进程：<!--三个进程并行-->

- 跟踪：

  - 输入：RGB-D图像流
  - 实时跟踪相机位姿

- 动态局部建图

  - 输入：关键帧
  - 跟踪模块：执行**局部BA**
  - 建图模块：训练对应的局部地图

- 闭环检测

  当闭环被检测到时

  - 使用全局BA优化相机位姿
  - 更新场景表示

## A、跟踪和建图模块

跟踪模块：基于ORB-SLAM3

建图模块：使用多个**隐式神经映射(implicit neural map)**来表示场景

## B、动态局部建图

当一个帧被确定为关键帧时

- 跟踪模块：
  - 使用**局部BA**来优化**相关的关键帧**，并将姿态和新的关键帧提供给建图模块。
- 建图模块：
  - 首先，执行局部地图选择，通过评估**共识关系**来确定新关键帧属于现有的局部地图。可以防止建图模块生成冗余的局部地图。
  - 如果关键帧不属于现有任何的局部地图，则会初始化一个新的局部地图并相对于当前关键帧进行定位，当前关键帧被称为定位帧。
  - 在局部地图确定后，==建图模块用关键帧训练局部地图==，这些关键帧的姿态在局部BA中进行了优化。

## C、闭环检测

动机：

- 确保全局一致性
- 纠正累积误差

当闭环被检测到时

- 跟踪模块：
  - 执行全局BA。<!--全局BA会导致先前预测的相机姿势立即发生变化，通常称为轨迹跳跃-->
    - 为了解决轨迹跳跃问题，基于单体积的隐式神经方法需要对场景表示进行重新训练，并更新大多数先前训练的参数，非常耗时。
- 建图模块
  - 在我们的系统中，使用多个局部地图来表示整个场景。并且**场景表示**具有两个阶段的优化。
    - 第一阶段，建图模块通过执行**子地图调整(Sub-map Adjustment)**,通过==变换具有锚点关键帧姿态的子图==来更新场景表示。
    - 第二阶段，建图模块对先前的局部地图进行微调以纠正误差。
  - 第一阶段是纠正局部地图之间的误差的实时调整，实验结果表明，该阶段纠正了场景中的很大一部分错误。第二阶段消除了局部地图中的小误差，进一步提高了场景表示的准确性。

## D、基于不确定性的图像渲染

我们们的图像包含多个子地图，因此在从给定视点渲染图像时需要考虑两种情况。

- 当==视锥体==与子地图完全相交时，允许我们使用该特定子地图渲染图像。
- 视锥体位于不同子地图的边界处时，无法使用单个子地图生成完整的图像。这时，==采用基于最低不确定性的图像逐像素融合==。

# 三、地图表示和训练

## A、基于八叉树的隐式神经表示

动机：

- 基于体素网格的NeRF架构中，大部分空间未被占用，使用密集的网格结构会浪费内存。

原理：

- 使用基于稀疏八叉树的网格。八叉树只在空间被占用的地方生生长。

![image-20240103151132163](https://raw.githubusercontent.com/letMeEmoForAWhile/typoraImage/main/img/image-20240103151132163.png)

如何获得一个点p的特征

- 首先，当我们查询一个点p的时候，p在级别$i$中的特征$z_p^{(i)}$可以通过==三线性插值==获得。
- 然后，我们将不同级别的特征相加，得到$z_p$作为$p$的特征

在该方法中，我们使用了两个小型MLP解码器，分别用于获得占用率$o_p$和颜色$c_p$

## B、体积渲染

动机：

- 优化A中的场景表示框架

方法：

- 使用NeRF中提出的可微分体积渲染

给定相机的固有参数和当前相机姿态，我们从相机中心$o$沿着==像素的归一化视图方向==$v$投射光线$r$

##### 深度和颜色

我们沿着光线采样N个点，定义为$x_i = o + d_i v$ , 其中$d_i$是点$x_i$的深度。

这些点预测出来的占用率和颜色分别表示为$o_i$和$c_i$，由此计算深度$\hat{D}$和颜色$\hat{C}$
$$
\hat{D}(r) = \sum ^N _{i = 1} T_i \alpha_i d_i
$$

$$
\hat{C}(r)= \sum ^ N _{i=1} T_i \alpha_i c_i
$$

 其中

- $\alpha_i = o_i$
- $T_i = \prod^i _{j=1} (1 - o_j)$

==分别代表采样点i沿着射线r的透射率和α值==

##### 基于体素的采样

动机：

- 为了充分利用八叉树结构，我们采用了基于体素的采样策略

具体方法：

- 给定射线$r$,我们查询与它相交的体素，并在这些体素内沿着射线采样点。
- ==由于八叉树只在点被占据的地方生长，因此采样的点要么都靠近对象曲面，要么都在对象内部。==
- 每个体素采样固定数量$N_{point}$个点。因此，对于与$N_{voxel}$个体素相交的射线$r$,我们采样$N = N_{point} × N_{voxel}$个点

##### 优化

动机：

- 优化三、A中的**场景表示特征**和**解码器**

方法：

- 在当前帧选择**M**个像素，并使用**光度损失(photometric loss)**和**几何损失(geometry loss)**来训练场景表示

- 光度损失是渲染的彩色图像和真实彩色图像之间的均方误差(MSE)损失
  $$
  \mathcal{L}_p = \frac{1}{M} \sum^M _{i=1} |\hat{C}-C_{gt}|_2
  $$

- 几何损失是渲染深度和地面真值深度之间的简单$\mathcal{L}_1$损失
  $$
  \mathcal{L}_g = \frac{1}{M} \sum^M _{i=1}|\hat{D}- D_{gt}|
  $$

- 通过最小化下列损失函数，同时优化特征$z$和解码器参数$\theta$
  $$
  \min_{z,\theta} \lambda_p + \mathcal{L}_p + \mathcal{L}_g
  $$
  

##### 不确定性

由于==占用==遵循伯努利分布，点p的占用值$o_p$代表一个点被占用的概率，它的方差可以被计算为 $Var = o_p(1-o_p)$

一个射线的方差可以被计算为
$$
\sigma_o^2(r) = \frac{1}{N} \sum^N_{i = 1}o_i (1-o_i)
$$

# 四、实验

数据集：

- 分析跟踪和建图能力
  - Replica
  - ScanNet
  - TUM RGB-D
- 消融实验
  - NICE-SLAM提供的公寓数据集

基准：三个开源的RGB-D隐式神经网络SLAM

- iMAP
- NICE-SLAM
- ESLAM

TUM数据集上的评估

![image-20240104100614956](https://raw.githubusercontent.com/letMeEmoForAWhile/typoraImage/main/img/image-20240104100614956.png)

大规模场景ScanNet

- 定量实验

![image-20240104100728896](https://raw.githubusercontent.com/letMeEmoForAWhile/typoraImage/main/img/image-20240104100728896.png)

- 定性实验

  ![image-20240104100849342](https://raw.githubusercontent.com/letMeEmoForAWhile/typoraImage/main/img/image-20240104100849342.png)

- 提取的网格如下：

![image-20240104101308073](https://raw.githubusercontent.com/letMeEmoForAWhile/typoraImage/main/img/image-20240104101308073.png)

运行时间和存储

![image-20240104101549337](https://raw.githubusercontent.com/letMeEmoForAWhile/typoraImage/main/img/image-20240104101549337.png)

建图速度更快，建图所需的帧更少(只用关键帧)

# 五、iMAP

步骤

- iMAP使用单目相机获取场景的连续图像，并使用视觉里程计技术估计相邻图像之间的相对位姿。

- 然后，将相邻图像的位姿估计结果作为NeRF的输入，生成相邻图像之间的3D场景。

- 最后，使用优化算法对相邻图像之间的位姿进行优化，从而实现对场景的实时重建和定位。iMAP中的优化算法是基于非线性最小二乘法的优化算法。
