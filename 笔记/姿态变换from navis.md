
# 矩阵的乘法就是复合变换

在平面空间中，我们以向量   $\vec{i}=(1,0)$   $\vec{j}=(0,1)$ 为基底就可以表示出来这个平面内所有的向量，对于平面内任一向量可以表示为 $\vec{v}=m\vec{i}+n\vec{j}$  ，$\vec{i}\ \ \ \vec{j}$  向量在变换之后用一个2x2的矩阵去表示

$$\begin{bmatrix} a & b \\ c & d \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} = \begin{bmatrix} ax + by \\ cx + dy \end{bmatrix}$$

在未变换前的平面内有一个 $(x，y)$  向量，经过一次变换之后，之前的 $\vec{i}$ 变到了位于原平面的 $(a,c)$ 的位置， $\vec{j}$ 变到了位于原平面 $(b,d)$ 的位置。变换后的向量与变换后的基底之前的线性关系仍保持不变，那么就可以通过矩阵的乘法去表示出来变换后的向量在原平面内的位置。
那么上述式子就是可以写成

$$\begin{bmatrix} a & b \\ c & d \end{bmatrix} \begin{bmatrix} x \\ y \end{bmatrix} = x \begin{bmatrix} a \\c \end{bmatrix} + y\begin{bmatrix} b \\ d \end {bmatrix} = \begin{bmatrix} ax + by \\ cx + dy \end{bmatrix}$$

这样就完成了一次变换，如果我们想要进行多次变换

$$\overbrace{\begin{bmatrix} a_2 & b_2 \\ c_2 & d_2 \end{bmatrix}}^{A_2} \overbrace{\begin{bmatrix} a_1 & b_1 \\ c_1 & d_1 \end{bmatrix}}^{A_1} \begin{bmatrix} x \\ y \end{bmatrix} = \begin{bmatrix} ax + by \\ cx + dy \end{bmatrix}$$

 那么我们就可以理解成$A_1$ 里面的两个列向量在$A_2$ 矩阵的一次变换后形成的两个新基底，就可以表示出来经过两次变换之后的新向量在原坐标系下的位置，这也就是结合律可以在矩阵的乘法里面用到的原因

但是在矩阵乘法里面绝对不能用交换律，变换的顺序一旦改变，就会导致变换之后的基底发生变化


# 行列式

行列式计算出来的值就是变换之后的两个基底形成的平行四边形的面积与变换前的平行四边形面积的比值
$$A = \frac{\text{变换后单位方块的面积}}{\text{变换前单位方块的面积}}$$

一个3x3的矩阵计算可以展开成
$$
\det \begin{pmatrix} a & b & c \\ d & e & f \\ g & h & i \end{pmatrix} = a \det \begin{pmatrix} e & f \\ h & i \end{pmatrix} - b \det \begin{pmatrix} d & f \\ g & i \end{pmatrix} + c \det \begin{pmatrix} d & e \\ g & h \end{pmatrix}
$$

# 逆矩阵

$$
\begin{aligned}
2x+5y+3z=-3 \\
4x+0y+8z=0 \\
1x+3y+0z=2 \\ \end{aligned} \quad \xrightarrow{\quad} \quad \overbrace{\begin{bmatrix} 2 & 5 & 3 \\ 4 & 0 & 8 \\ 1 & 3 & 0 \end{bmatrix}}^{A} \overbrace{\begin{bmatrix} x \\ y \\ z \end{bmatrix}}^{\vec{x}} = \overbrace{\begin{bmatrix} -3 \\ 0 \\ 2 \end{bmatrix}}^{\vec{v}}
$$


对于一个三元一次方程组，我们转换成矩阵形式之后，就可以得到如上所示的形式。在对矩阵定义之后，我们得到了 $A \  \vec{x} = \vec{v}$  ,按照上面讨论过线性代数的本质就是复合变化，我们就可以认为在原平面内有一个向量  $\vec{x}$  在经过一次 A 的变换之后，变成了向量 $\vec{v}$ 。 如果我们想要去求解 $\vec{x}$ 向量，应该怎么实现呢？
所以我们定义，一个矩阵的逆就是做一次反变换，这个可以实现反变换的矩阵就是逆矩阵 $A^{-1}$

$$
\begin{aligned} 
A \  \vec{x} = \vec{v}\\
A^{-1} A \  \vec{x} =A^{-1} \vec{v}\\
\vec{x} =A^{-1} \vec{v}\\
\end{aligned}
$$

# 非方阵矩阵 

对于一个非方阵矩阵，比如一个3x2的矩阵

$$
\begin{bmatrix} 2 & 5  \\ 4 & 0  \\ 1 & 3   \end{bmatrix}
$$

其中有两列，视作两个三维基向量，我们可以认为是在一个空间下的两个基底，如果这时候我们再给他输入两个系数，就可以得到在三维空间下的坐标

$$\begin{bmatrix} 2 & 5  \\ 4 & 0  \\ 1 & 3   \end{bmatrix} \begin{bmatrix} a \\ b \end{bmatrix} = \begin{bmatrix} x \\ y \\ z  \end{bmatrix}$$
 
 同样的，对于一个2x3的矩阵，输入空间是在一个三维空间中，这时候我们输入系数，就可以得到经过变换后的在二维空间里面的输出
 
 所以在几何上$m \times n$ 矩阵就可以实现把一个向量从 $n$ 维空间变换到 $m$ 维空间。


# 基变换

根据之前的陈述，我们在有两个坐标系的情况下，可以表示出一个 $\vec{v}$  向量在第一个坐标系下的位置，在知道了两个坐标系之间的变换方式之后，也可以表示出变换之后的  $\vec{v}$  向量在第一个坐标系下面的表示矩阵，可是这种变换都是基于我们的坐标系去描述
如果一个向量 $\vec{v}$ 本身是第二个坐标系下的坐标，我们需要描述的也是在第二个坐标系下再变换后的向量在第二个坐标系下面的位置，这听起来似乎和我们之前讨论的没什么区别，但下面的式子计算为什么看起来这么麻烦呢？

$$
\begin{bmatrix} 2 & -1\\ 1 & 1
\end{bmatrix}^{-1}
\begin{bmatrix} 0 & -1 \\ 1 & 0
\end{bmatrix}
\begin{bmatrix} 2 & -1 \\ 1 & 1
\end{bmatrix}
\vec{v} 
$$
我们将前三个式子可以写成 $A^{-1} M A$  A是在第一个坐标系下描述的第二个坐标系的基底，M矩阵是在第一个坐标系下去描述的一种变换（上式为旋转90°），我们再乘以  $\vec{v}$  ，在经过这个式子转化之后，我们就可以实现，利用在第一个坐标系下的一种变换法则，对第二个坐标系下的坐标进行了转换。
这样有什么好处呢？比如上述的第二个坐标系下他本身的基底就不是一个直角坐标系，是在第一个坐标系（直角坐标系）下的（2，1）和（-1，1），如果我们还想在第二个坐标系实现旋转90°的操作，就很难直接看出来一个旋转矩阵。相反的是，我们可以很直观的在第一个坐标系里面去找到表示这个操作的方法，那么通过 $B = A^{-1} M A$ ，B矩阵乘第二个坐标系下的一个向量，就可以实现需要的变换了。


# 特征向量与特征值

一个坐标系在变换的过程中，可能会有一些向量的朝向保持不变，只会被拉伸（压缩），不会被旋转，这样的向量就被成为特征向量，拉伸（压缩）的倍数就被称为特征值。

$$\exists \vec{v} \neq \vec{0}, \lambda \text{ 使得 } A\vec{v} = \lambda \vec{v}$$
在3D空间中变换最直观的就是旋转轴
具体的特征值的计算方法我们不再赘述



学习资料：
[旋转，旋转矩阵和欧拉角（内旋、外旋）_欧拉角外旋-CSDN博客](https://blog.csdn.net/m0_51533441/article/details/137715820)
[【中科大RM电控合集】从欧拉角到旋转矩阵_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1vYCSY2EuS/?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=aad5cac6c8b3de7f8becfb0cb02d82c3)
[(14 条消息) 深入理解矩阵乘法：左乘与右乘 - 知乎](https://zhuanlan.zhihu.com/p/1977050517332043366)
[坐标变换中，矩阵左乘和右乘的区别-CSDN博客](https://blog.csdn.net/u011208918/article/details/131791369)


看到这里相信你已经有了一点线性代数的基础，那么用变换的眼光去看待矩阵乘法的方法你也肯定知道了，那么接下来就让我们更进一步吧！

先把视角放在我们最熟悉的二维坐标系下：

# 二维旋转

## 基本的旋转

我们有一个二维平面下的向量 $\vec{p} = (x,y)$ ，我们将这个向量**逆时针**旋转 $\theta$ 角，新位置怎么表示？
先回到我们高中时候就学习过的知识
设原点到点的距离为 $r$，初始角度为 $\phi$，则
$x = r\cos\phi,\ y = r\sin\phi$。转完后角度变成 $\phi + \theta$：

$$
x' = r\cos(\phi+\theta) = r\cos\phi\cos\theta - r\sin\phi\sin\theta = x\cos\theta - y\sin\theta
$$
$$
y' = r\sin(\phi+\theta) = r\sin\phi\cos\theta + r\cos\phi\sin\theta = x\sin\theta + y\cos\theta
$$

如果我们用矩阵的放法写出来就是

$$
\begin{pmatrix} x' \\ y' \end{pmatrix}
=
\underbrace{\begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}}_{R(\theta)}
\begin{pmatrix} x \\ y \end{pmatrix}
$$
我们就把 $R(\theta)$ 叫做一个旋转矩阵，用来表示一个向量在二维坐标系下逆时针旋转 $\theta$ 角的变换，而且这个变换矩阵，只与 $\theta$ 角有关，与旋转的向量本身并没有任何关系。

## 旋转后新基下的坐标表示

我们之前对坐标系不变，只有向量的变换或剪切已经很熟悉了，在3b1b的课程最后几节，我们开始接触有关基变换的内容，如果我们的向量在变换之中是不会变化的，而是坐标系(基)在变化，情况又会变成什么样子呢？
对于二维世界坐标系下的一个向量 ​, 将坐标系旋转​得到新的坐标系A, 设该向量在新坐标系下的坐标为 $^A\vec{p}$ ​, $R(\alpha)$ 是从旧坐标系到新坐标系下的变换矩阵， 则
$$
R(\alpha) \ {^A\vec{p}} = \vec{p}
$$
证明：
因为我们本身的向量是没有变化的，变换的是坐标系；我们先把这变换简化理解成一种简单的旋转，暂时没有剪切；那么如果坐标系相对一个方向旋转了 $\theta$ 角，那么就可以认为是坐标系不变，向量相对坐标系旋转了  $-\theta$ 角(反向)，则有：
$$
^A\overrightarrow{p} = R(-\alpha) \overrightarrow{p}
$$
我们给两边同时乘$R(\alpha)$ 
$$
R(\alpha) \ {^A\overrightarrow{p}} = R(\alpha) \ R(-\alpha) \ \overrightarrow{p} \\
$$
$$
R(\alpha) \ {^A\overrightarrow{p}} = \overrightarrow{p}
$$
得证；
所以我们就可以用新基矩阵(在旧坐标系下的描述)，乘以新坐标系下的坐标，就可以转回原来的矩阵。

# 三维世界坐标系下的旋转


> [!note]+ 三维空间下, z轴/Yaw旋转矩阵, y轴/Pitch旋转矩阵, x轴/Roll旋转矩阵分别为
> $$
> Rz(\alpha) = Yaw(\alpha) =
> \begin{pmatrix}
> \cos(\alpha) & -\sin(\alpha) & 0 \\
> \sin(\alpha) & \cos(\alpha) & 0 \\
> 0 & 0 & 1 \\
> \end{pmatrix}
> $$
> $$
> Ry(\beta) = Pitch(\beta) =
> \begin{pmatrix}
> \cos(\beta) & 0 & \sin(\beta) \\
> 0 & 1 & 0 \\
> -\sin(\beta) & 0 & \cos(\beta) \\
>  \end{pmatrix}
> $$
> $$
> Rx(\theta) = Roll(\theta) = \begin{pmatrix}
> 1 & 0 & 0 \\
> 0 & \cos(\theta) & -\sin(\theta) \\
> 0 & \sin(\theta) & \cos(\theta) \\
>  \end{pmatrix}
> $$

证明：
声明：以下的旋转是默认为按正方向(逆时针)去旋转的
1.**绕z轴旋转$\alpha$ (yaw)** 
我们先来看绕z轴的旋转，当绕着z轴旋转的时候，本质上只有x和y在旋转，这就是我们之前讨论的二维平面的旋转，根据我们之前的讨论：
$$
\begin{pmatrix}x'\\y'\end{pmatrix} = \begin{pmatrix}\cos\alpha & -\sin\alpha\\ \sin\alpha & \cos\alpha\end{pmatrix}\begin{pmatrix}x\\y\end{pmatrix}
$$
我们加上不变的z分量，就可以拓展到三维中：
$$
\begin{pmatrix}x'\\y'\\z'\end{pmatrix} = \begin{pmatrix}
\cos\alpha & -\sin\alpha & 0\\
\sin\alpha & \cos\alpha & 0\\
0 & 0 & 1
\end{pmatrix}\begin{pmatrix}x\\y\\z\end{pmatrix}
$$
就可以得到上述的$R_z(\alpha)$

2.**绕x轴旋转 $\theta$ (roll)**
同理，绕x轴旋转的时候，x分量是不会变化的，本质上只有y和z在旋转，同上述推导可得：
$$
\begin{pmatrix}x'\\y'\\z'\end{pmatrix} = \begin{pmatrix}
1 & 0 & 0\\
0 & \cos\theta & -\sin\theta\\
0 & \sin\theta & \cos\theta
\end{pmatrix}\begin{pmatrix}x\\y\\z\end{pmatrix}
$$
3.**绕y轴旋转 $\beta$ (pitch)**
但是在绕y轴旋转的时候，我们按照右手准则，旋转的平面实际上是(z，x)平面，是从z轴转向x轴的，我们仍可以按照之前的二位平面逻辑去思考讨论
我们这时候看到的是一个(z，x)平面，假设最初的向量和x轴正向的夹角是 $\phi$ ，那么绕y轴旋转一个$\beta$角之后，我们现在相对于x轴正向的角度是 ( $\phi  - \beta$ ) 
$$
x' = r\cos(\phi-\beta) = r\cos\phi\cos\beta + r\sin\phi\sin\beta = x\cos\beta + z\sin\beta
$$
$$
z' = r\sin(\phi-\beta) = r\sin\phi\cos\beta - r\cos\phi\sin\beta = -x\sin\beta + z\cos\beta
$$

所以我们在这个时候描述这个二维平面的旋转的时候应该是
$$
\begin{pmatrix}
x'\\z'\end{pmatrix} = 
\begin{pmatrix}\cos\beta & \sin\beta\\ -\sin\beta & \cos\beta\end{pmatrix}\begin{pmatrix} x\\z
\end{pmatrix}
$$
交换顺序，再加上y分量，得到矩阵形式就是：
$$
\begin{pmatrix}x'\\y'\\z'\end{pmatrix} = 
\begin{pmatrix}
\cos\beta & 0 & \sin\beta\\
0 & 1 & 0\\
-\sin\beta & 0 & \cos\beta
\end{pmatrix}
\begin{pmatrix}x\\y\\z\end{pmatrix}
$$

## 左乘和右乘

在3b1b里面我们已经接触过了左乘在变换里面的应用，但是还没有接触过右乘，那么右乘在变换之中有什么区别呢？
我们了解过，如果对于一个坐标系需要进行多次变换 $R_1 \  R_2 \  R_3...$  按照之前的学习，那么在最后得到的旋转矩阵就是 $R=R_3R_2R_1$ 在左边连续相乘，但这种多次变换用一个矩阵表示是有条件的，这种乘法是用于在每一次旋转互不干扰的情况下进行的，但是如果每一次变换都是在之前的变换基础上进行的，那么又会是什么样子？

假设我们要进行两次变换，第一次变换是 $R_1$ 是在原先的坐标系上进行变换；之后再进行 $R_2$ 变换，在 $R_1$ 变换之后的坐标系下进行变换。那我们的处理就是将 $R_2$ 转换成在最初坐标系下的一种变换就可以；在最终的变换呈现就是 $(R_1 R_2 R_1^{-1}) R_1 = R_1 R_2$ 

所以我们现在就可以了解到：左乘和右乘的在变换里面的使用是根据变换的之间的关系决定的
- 如果多次变换之间没有任何关系，那么多次变换得到的总变换矩阵就是 $R=R_3R_2R_1$ 
- 如果多次变换之间是有顺序的，那就在复合之间不断右乘  $R=R_1 R_2 R_3$ 

## 坐标系

在机器人学的研究中，我们通常要考虑在三维空间里一个物体的特性，而非常重要的两个特性就是：**位置和姿态**；我们会研究如何使用数学方法去表示和计算这些参量。在接下来这一部分，我们将要讨论位置和姿态的描述以及包含这两个描述的统一体：**位姿**。

 我们建立一个对于所有刚体都可以拿来参考的坐标系，叫**世界坐标系**；通过这个坐标系，我们就饿可以用一个3×1的位置矢量去给坐标系里面的任何点定位

$$
\begin{aligned} \vec{P} = \begin{bmatrix} P_x \\ P_y \\ P_z \end{bmatrix} = {}^A P_{B\,org} = \text{origin of } \{B\} \text{ represented in } \{A\} \\ \text{这是B点在世界坐标系\{A\}下的向量p的描述} \end{aligned}
$$

世界坐标系已经解决了我们想知道目标物体位置问题，为了表示一个刚体自身的姿态，我们以物体上某点作为原点，建立物体坐标系，同样是一个三维坐标轴，
## 欧拉角

欧拉角就是一种用来描述刚体或者坐标系在一个固定坐标系中的姿态，通过使用三个坐标轴的旋转分量，来描述旋转。欧拉角根据旋转轴的不同，以及旋转顺序的不同，分为不同的类别。这里我们主要介绍内旋和外旋的区别。首先要区分好两个坐标系，一个是全局坐标系（世界坐标系），另一个是局部坐标系（物理坐标系），在旋转过程中，一个是变化的，一个是不变的；内旋是围绕局部坐标系进行，外旋是按照全局坐标系进行。
## 内旋 （旋转轴/自身轴）

三维空间下, 将一物体按照z-y-x顺序绕局部坐标系旋转角度, 最终的旋转矩阵为
我们在三次旋转过程中，每一次旋转都是上一次旋转之后的局部坐标系去进行下一次旋转，在这个旋转里，我们局部坐标系下的向量（刚体），是会跟着局部坐标系一起旋转的。
所以我们的目标是计算旋转之后的刚体在全局坐标系下的位姿，刚体在局部坐标系的位姿是保持不变的，只是不同时刻局部坐标系位姿不同。
$$
Rypr(\alpha, \beta, \theta) = Yaw(\alpha) Pitch(\beta) Roll(\theta) = Rz​(\alpha)Ry​(
\beta)Rx​(\theta)
$$

## 外旋 （固定轴）

外旋与内旋相对，是围绕着固定的全局坐标系进行的，与局部坐标系无关，因此可以看做是对向量（刚体）的旋转，三维空间下, 将一物体按照z-y-x顺序绕世界坐标系旋转​角度, 最终的旋转矩阵为
$$
Rzyx(\alpha, \beta, \theta) =   Rx(\theta) Ry(\beta) Rz(\alpha)
$$


到这里你会发现内旋和外旋的顺序是不一样的，根据左乘与右乘在变换里的区别就能想到
- 内旋是绕着自身的轴去旋转，每一次变换都是在之前的变换上进行，所以是右乘，那么按照z-y-x的顺序旋转出来的旋转矩阵就是 $R = R_z R_y R_x$  
- 外旋是绕着固定轴去旋转，每一次变换都与之前的变换无关，所以就是左乘，那么按照z-y-x的顺序旋转出来的旋转矩阵就是 $R = R_x R_y R_z$ 

所以即使看似是相同的变换，在内旋和外旋下，就会导致不一样的结果；
例：我们有一个向量 $\vec{p} = (1,0,0)$ 我们对这个向量进行绕Z-Y-X的顺序旋转90°，但是用内旋和外旋的方法去旋转。
旋转矩阵：
$$
R_z(90°) = \begin{pmatrix}0&-1&0\\1&0&0\\0&0&1\end{pmatrix}\ 
R_y(90°) = \begin{pmatrix}0&0&1\\0&1&0\\-1&0&0\end{pmatrix}\ 
R_x(90°) = \begin{pmatrix}1&0&0\\0&0&-1\\0&1&0\end{pmatrix}
$$
外旋：
$$R_{\text{外}} = R_x(90°) \cdot R_y(90°) \cdot R_z(90°)= 
\begin{pmatrix}
0 & 0 & 1\\
0 & -1 & 0\\
1 & 0 & 0
\end{pmatrix}
$$
$$
\vec{p}_{\text{外}} = R_{\text{外}} \begin{pmatrix}1\\0\\0\end{pmatrix}
= \begin{pmatrix}0\\0\\1\end{pmatrix}
$$
内旋：
$$R_{\text{内}} = R_z(90°) \cdot R_y(90°) \cdot R_x(90°)= \begin{pmatrix}0&0&1\\0&1&0\\-1&0&0\end{pmatrix}$$
$$\vec{p}_{\text{内}} = R_{\text{内}} \begin{pmatrix}1\\0\\0\end{pmatrix}
= \begin{pmatrix}0\\1\\0\end{pmatrix}$$

## 旋转矩阵的性质

旋转矩阵 R 满足：
$$
R^T R = I \quad \Longleftrightarrow \quad R^{-1} = R^T
\qquad\qquad
\det(R) = +1
$$
因为旋转矩阵在前面讨论情况下，旋转前后基底之间的关系不变，三列都是两两垂直的单位向量；任意两列的点积为0，每列和自己点积为1，满足正交矩阵的性质，也就满足了上述性质。
正交矩阵的行列式为±1，但旋转矩阵的行列式必定为1，这种旋转代表的是一种纯旋转（保定向）的变换；但是如果一个行列式为-1，就代表的是一种镜像反射（反定向）的变化，经过这种变化之后的坐标系会与原来的坐标系手性相反。

# 移动

## 齐次坐标

对于空间里面的一个刚体，我们对他的操作不知有旋转，还有平移，那么对于一个点来说，变换的表达式就应该是：
$$
\mathbf{v}' = R\mathbf{v} + \mathbf{t}
$$
 但旋转是满足线性变换的，并且要求原点是不能动的，但平移不是，对原点的位置也有破坏
 所以我们就把原来的3×3旋转矩阵升一个维度，变成4×4的矩阵
$$
T = \begin{bmatrix} R & \mathbf{t} \\ \mathbf{0} & 1 \end{bmatrix}
= \begin{bmatrix} r_{11} & r_{12} & r_{13} & t_x \\ r_{21} & r_{22} & r_{23} & t_y \\ r_{31} & r_{32} & r_{33} & t_z \\ 0 & 0 & 0 & 1 \end{bmatrix} = \begin{bmatrix} R\mathbf{x} + \mathbf{t} \\ 1 \end{bmatrix}
$$

把这样用一个矩阵去表示出来旋转和平移的矩阵叫做变换矩阵，但是矩阵具体计算出来的形式和顺序有关，先旋转再平移和先平移再旋转表示出来的矩阵是否一致呢？
我们通过接下来的这个例子来看一下：

在世界坐标系下面有一个点 $\vec{p} = (1,0,0)$ ，有绕z轴旋转90°，沿x轴正方向移动2个单位长度

$$
\text{旋转矩阵} \ R = \begin{pmatrix}
0 & -1 & 0\\
1 & 0 & 0\\
0 & 0 & 1
\end{pmatrix} \ \ \text{平移向量}t = \begin{pmatrix} 2 \\ 0 \\ 0 \end{pmatrix}
$$
> [!note]+ 先旋转再平移：$p' = Rp + t$ 
> 变换矩阵：
> $$
> T_1 = \begin{pmatrix}
> R & t \\
> 0 & 1\end{pmatrix}
> = \begin{pmatrix}
> 0 & -1 & 0 & 2\\
> 1 & 0 & 0 & 0\\
> 0 & 0 & 1 & 0\\
> 0 & 0 & 0 & 1
> \end{pmatrix} 
> $$
> 最后的结果就是：$T_1 \begin{pmatrix}1\\0\\0\\1\end{pmatrix} = \begin{pmatrix}2\\1\\0\\1\end{pmatrix}$


> [!note]+ 先平移再旋转：$p' = R(p + t)$
> 变换矩阵：
> $$
> T_2 = \begin{pmatrix}
> R & R t \\
> 0 & 1
> \end{pmatrix}
> = \begin{pmatrix}
> 0 & -1 & 0 & 0\\
> 1 & 0 & 0 & 2\\
> 0 & 0 & 1 & 0\\
> 0 & 0 & 0 & 1
> \end{pmatrix}
> $$
> 最终的结果：$T_2 \begin{pmatrix}1\\0\\0\\1\end{pmatrix} = \begin{pmatrix}0\\3\\0\\1\end{pmatrix}$

通过此来看，变换矩阵会根据实际情况里旋转和平移的顺序有不同的计算结果

## 齐次坐标的缩放因子 w

齐次坐标就是用N+1维来代表N维坐标的点或向量，也可说用齐次坐标来表示笛卡尔坐标
例如对于一个直角坐标系点坐标（x,y），我们在末尾加上一个变量w，一个点(X,Y)在齐次坐标里面变成了（x,y,w），真实的点坐标其实是  $X = \frac{x}{w} \  Y = \frac{y}{w}$ 
我们在一个坐标系里表示一个点很容易，但是如何表示一个无穷远的点？这时候缩放因子就起作用了，当我们把缩放因子置0时，所表示的真实点就变成了无穷远的点，更方便的是，我们可以通过这种方式去把（x，y）表示成一个点或者向量：
- 如果是一个点，加入缩放因子就表示成（x，y，1）
- 如果是一个向量，加入缩放因子就表示成（x，y，0）
所以可以观察我们之前的变换矩阵，就可以论证
注：你可能发现，4×4的变换矩阵，按照之前的理解来说，四列都应该是向量才对，但是为什么右下角偏偏要多个1？具体的原因通过计算可以直接论证，这里不再证明，只说结论：右下角的1正是其次变化的必然要求，因为我们需要保证变换前后齐次因子的权重是不变的，右下角的1正好可以保证这一点，点变换之后还是点，向量变换之后还是向量。

# 四元数

学习资料：
[四元数的可视化_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1SW411y7W1/?vd_source=aad5cac6c8b3de7f8becfb0cb02d82c3)
[(6 条消息) 四元数：从复数推广到三维旋转的数学本质 - 知乎](https://zhuanlan.zhihu.com/p/1980626025026179223)

通过3b1b的视频，可以对四元数有一个很初步的了解，思维和之前理解旋转很像；
一个标准形式的四元数由一个实部和三个虚部组成，完整表达三维旋转的角度和轴向信息：
$$
q = q_0 + q_1 i + q_2 j + q_3 k
$$
针对单位四元数，$\|\boldsymbol{q}\| = \sqrt{q_0^2 + q_1^2 + q_2^2 + q_3^2} = 1$ ，其与旋转角度的关系是：$q_0 = cos(\frac{\theta}{2})$ ，用此来表示旋转
实部的 $q_0$ 无方向属性，对应角度信息量
- 当$q_0$近似1的时候，反解出来的 $\theta$ 近似于0，相当于微幅旋转
- 当$q_0$近似0的时候，反解出来的 $\theta$ 近似于180，相当于大幅旋转
虚部 $q_1 i + q_2 j + q_3 k$ 对应旋转的轴方向信息
旋转轴 $u = (u_x,u_y,u_z)$ 和旋转角 $\theta$ 可以转换成四元数
$$
\mathbf{q} = q = \cos\frac{\theta}{2} + \sin\frac{\theta}{2} \, (u_x i + u_y j + u_z k) =  \begin{bmatrix} \cos(\theta/2) \\ u_x \sin(\theta/2) \\ u_y \sin(\theta/2) \\ u_z \sin(\theta/2) \end{bmatrix}
$$
对于一个纯四元数，即 $q = 0 + q_1 i + q_2 j + q_3 k$ 用来表示空间里的一个点或者向量
如果我们有一个四元数Q去表示一个旋转，那么最终旋转得到的结果表示成
$$
p' = Q \ p \ Q^{-1}
$$
四元数要左乘点，右边要补上Q的逆，原因可在网上了解，这里不做讨论


> [!note]+ 我们通过一个例子来理解：
> 我们有一个向量 $\vec{v} = (1, 0, 0)$  ，要绕z轴旋转90°（右手系）
> 第一步：构造单位四元数
> 绕单位轴 $\mathbf{u} = (0,0,1)$ 转 $\theta = 90^\circ$ 的单位四元数为：
> $$q = \cos\frac{\theta}{2} + \sin\frac{\theta}{2} \, (0i + 0j + 1k)
> = \cos 45^\circ + \sin 45^\circ \, k
> = \frac{\sqrt{2}}{2} + \frac{\sqrt{2}}{2} k$$
> 它的共轭：
> $$q^{-1} = \frac{\sqrt{2}}{2} - \frac{\sqrt{2}}{2} k$$
> 
> 第二步：将向量表示为纯四元数
> 
> $$p = 0 + 1i + 0j + 0k = i$$
> 
> 
> 第三步：执行双乘 $p' = q \, p \, q^{-1}$
> 先计算 $q p$：
> $$q p = \left(\frac{\sqrt{2}}{2} + \frac{\sqrt{2}}{2}k\right) i
> = \frac{\sqrt{2}}{2} i + \frac{\sqrt{2}}{2} k i$$
> 
> 根据四元数乘法规则：$k i = j$，所以
> 
> $$q p = \frac{\sqrt{2}}{2} i + \frac{\sqrt{2}}{2} j$$
> 
> 再右乘 $q^{-1}$：
> 
> $$p' = (q p) \, q^{-1}
> = \left(\frac{\sqrt{2}}{2} i + \frac{\sqrt{2}}{2} j\right) \left(\frac{\sqrt{2}}{2} - \frac{\sqrt{2}}{2} k\right)$$
> 
> 逐项展开,化简计算合并同类相之后：
> $$p' = \left(\frac{1}{2}i - \frac{1}{2}i\right) + \left(\frac{1}{2}j + \frac{1}{2}j\right) = 0i + 1j = j$$
> 
> 纯四元数 $j$ 对应三维向量 $(0, 1, 0)$，即 Y 轴正方向。这正是绕 Z 轴旋转 90° 后 X 轴指向的方向，结果正确。

四元数不是矩阵乘法，旋转轴并不只局限于某个坐标轴了，只需要你有一个确定的轴和角度，都可以完成旋转，只需要根据轴和角度推出四元数即可。

四元数里多次变换在内旋和外旋是相同的
- 外旋：后做的旋转左乘在先做的旋转上，即 $q_{\text{total}} = q_2 \, q_1$
- 内旋：后做的旋转右乘在先做的旋转上，即 $q_{\text{total}} = q_1 \, q_2$


