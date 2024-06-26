# 一、背景：

解决分线性分类问题，将样本映射到更高维的空间，使样本在更高维的空间内线性可分。

用一个具体文本分类的例子来看看这种向高维空间映射从而分类的方法如何运作，想象一下，我们文本分类问题的原始空间是1000维的（即每个要被分类的文档被表示为一个1000维的向量），在这个维度上问题是线性不可分的。现在我们有一个2000维空间里的线性函数：
$$
f(x’)=<w’,x’>+b
$$
式中的 w’和x’都是2000维的向量，只不过w’是定值，而x’是变量（严格说来这个函数是2001维的）。现在我们的输入，是一个1000维的向量x，分类的过程是先把x变换为2000维的向量x’，然后求这个变换后的向量x’与向量w’的内积，再把这个内积的值和b相加，就得到了结果，看结果大于阈值还是小于阈值就得到了分类结果。



# 二、定义：

![image-20210731155017902](https://raw.githubusercontent.com/letMeEmoForAWhile/typoraImage/main/img/image-20210731155017902.png)

- 定义f为similar函数（这里使用高斯核函数）

![image-20210731155106050](https://raw.githubusercontent.com/letMeEmoForAWhile/typoraImage/main/img/image-20210731155106050.png)

若样本点x与标记点L1距离接近，则f1≈1，若距离很远，则f1≈0

- 假设四个权重θ0~θ1分别为 -0.5，1，1，0 并当θ0+θ1f1+θ2f2+θ3f3≥0时 预测1 ，其他情况预测为0

计算得样本点x靠近L1、L2时预测为1，靠近L3时预测为。画出决策边界（红圈部分）。

# 三、核函数的应用：

问题：如何得到标记点L1、L2、L3……

直接将**所有的**training examples 作为标记点

![image-20210731161615706](https://raw.githubusercontent.com/letMeEmoForAWhile/typoraImage/main/img/image-20210731161615706.png)

给定了训练样本（x1，y1）,(x2,y2)……（xm，ym），选择所有的训练样本作为标记点。

当我们要预测xi（xi包含在x1~xm之中）的类别时，计算xi与每一个标记点的similarity函数值（fi表示xi与自己的similarity函数，所以一定为1），再加上固定值f0=1，组成向量fi（上述图片红色圈出来的部分）。fi（fi是m+1维的）就是用来描述训练样本的特征向量

# 四、核函数举例

### 1、径向基函数——Radial Basis Function（RBF）

通常定义为空间中任一点x到某一中心xc之间欧氏距离的单调函数 ,可记作 k(||x-xc||)。最常用的径向基函数是**高斯核函数**，形式为 
$$
k(||x-xc||)=e^{- ||x-xc||^2/2*σ^2 }
$$
