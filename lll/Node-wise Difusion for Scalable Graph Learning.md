# Node-wise Diffusion for Scalable Graph Learning

[toc]

## 符号

### P3.1

花体字：集合

大写：矩阵

小写：向量

G = (V, E, X) 花体v：节点集合  花体E：边集合 大写x：特征矩阵    （G是自环连通图

$N_u$ :u的邻居节点集合

$d_u$：u的度

A[u,v]：A矩阵第u行第v列的值

A：G的邻接矩阵

D：图里每个点的度对应的一个对角矩阵（虽然感觉该整成向量

X：G的特征矩阵

### P3.2

$\pi$:代表一种状态，pi这个向量对应的是一个土豆放到这个图里面无限传播后留在向量对应的每个点的概率   

$\pi_{(v)}$ :无限次传播后到v这个点的概率是v的度/2m

$ℓ_u$：节点u能接受的从其它节点扩散来的最大的跳数	

m:边数

${p^ℓ[u,v]-\pi(v)\over \pi(v)}\le\tau$ :p[u,v]的ℓ次幂

$\tau$：描述ℓ的，来自邻居的信息能在节点u聚集

P：转移矩阵,P[u,v]是节点u转移到节点v的概率（diffusion matrix也是它

$\lambda 123...$:是P的特征值

Theorem里头的$\lambda$是P第二大的特征值（除了1以外最大的那个特征值

### P3.2证明

For ease of exposition, node � ∈ V also indicates its index

$e_u$:u方向上是1其它都是0的向量（横

$1_n$:n大的全是1的向量

$\widetilde{P}=D^{1/2}PD^{-1/2} = D^{-1/2}AD^{-1/2}$ $因为这个转移矩阵是P = D^{-1}A$

$u_i^T :\widetilde{P}$的第i个特征值对应的特征向量  的转置 （竖

$u_i$：横	

$\pi $：横向量，

$\pi (v)$： = dv/2m，第v个节点对应的$\pi $值，应该是表示某一时刻到传播到节点v这里的概率或者权重

### P3.3

$\omega$：参数omega在第3.3节中的目的是控制通用扩散函数的扩展趋势

$d_G$：G的平均度数，也反映图的密度   （图G=（V,E），其中V是图G的顶点集合，E是图G的边集。假设图G中的边数目为m，即|E|=m，顶点数目|V|=n，图的边密度p=2m/n(n-1)。

X：特征矩阵，是每个节点的特征对应的矩阵，不是想的那个特征

$H_t$：随时间，特征变化转移后的X

$\lambda : = $1-$\Delta_\lambda$ ,

三张图函数的纵坐标：f ($\omega $, ℓ)，是上面的Ht每一项的系数

C：归一化常数$C = $

$\rho$：模型主动输入的参数，(参数rho的目的是控制通用扩散函数的扩展趋势。通用扩散函数是一种用于实现图上节点扩散的函数，它能够在长距离上平滑扩展，在短距离上迅速减小，并在特定跳数处达到峰值。参数rho用于调节通用扩散函数的扩展趋势，通过改变rho的取值，可以实现平滑扩展、指数式扩展（如Personalized PageRank）或峰值扩展（如Heat Kernel PageRank）的效果。)

### P3.4

$\tau $：目标集，从节点集V的子集

Γ （gamma）:=I[$\Tau$,·] ,T是目标集中节点对应的index，（Γ 不是方阵     Γ[i, u] = 1 if T [i] = u and Γ[i, u] = 0 otherwise.

L:max{$ℓ_u $ : ∀u ∈ T }, 输入$\tau $找出目标集中每个节点的ℓ，然后L是这些ℓ中最大的

U：目标集里面每个u对应的ℓ（每个u对应的ℓ通过给定的$\tau $算出来）通过函数U算出来的值组成的对角阵,反映了第ℓ跳的邻居节点在节点特征传播中的权重（所有第ℓ跳的邻居节点的普遍权重）

figure1里面的函数：x轴是跳数，y轴是权重（传播权重是指在特征传播过程中，用于衡量邻居节点对目标节点的影响程度的值。如果在跳数为3时函数具有较高的值，这意味着节点在跳数为3时从邻居节点接收到了大量的信息。

ZT:矩阵ZT代表了节点特征的聚合结果

### P4.1

$\phi$：权重函数$\phi (ℓ,u,v) = U(\omega,\rho,ℓ)P^ℓ[u,v]$ ，用来量化节点v对u的重要程度

rw：

$\epsilon$：阈值，如果对于u的邻接点v，有$\phi (ℓ,u,v)$ > $\epsilon$,那么v是u的$\epsilon$-importance neighbor

$\theta$：产生RWs的次数



## 问题

-----为什么$u_i$模长为1-----概率

------${p^ℓ[u,v]-\pi(v)\over \pi(v)}\le\tau$ :p[u,v]的ℓ次幂----------是P的ℓ次幂的[u,v]好像

$\Delta_\lambda$ 谱隙是什么阿？

目标集干嘛用的

阈值用来干嘛的

first-K selection为什么能减轻RWs访问到的不重要节点的影响阿？

phi函数能量化v对u的重要程度，为什么还需要NIGCN（那个伪码）搞出来的重要程度tv

## P3

P的作用：P[u,v]反映节点u转移到节点v的概率，在匹配红臭，黄香这种东西的时候给个转移的概率，跳出局部最优（不懂阿啊啊啊啊



给了个$\tau$来反映从其它节点接受信息的最大跳数ℓ的限制，（Defnition3.1：对于某个节点u，对于任意节点v都满足那个式子，所以每个u对应一个ℓ

证明了下ℓ的最大的表达式

介绍了下普遍的diffusion 模型 HKPR 什么热方程什么的

然后这个方程不好，它搞了个自己的GHD出来

GHD可以通过调节参数$\rho$为1，GHD退化成HKPR，$\rho$为0则是PPR.(这种类型的函数曲线在图1中是最理想的，因为它能够展现出平滑、指数或峰值扩展的趋势，这取决于参数的不同组合。这种函数曲线能够满足不同图形的需求，并且能够在长距离上平滑扩展，在短间隔上迅速减小，并在特定跳数处达到峰值。因此，这种函数曲线能够更好地适应不同图形的特点和需求。

 and computes the corresponding �-distance as �. Then, NDM accumulatesthe weights of neighbors within � ranges for each node � ∈ T, recorded as ZT. Note that U[�, �] = 0 if ℓ > �. Finally, representation ZT is calculated by multiplying the feature matrix X

1. NDM首先找出目标集中节点的最大的度，算出L
2. NDM计算目标集T中每个节点u的L范围内的邻居节点的权值
3. ZT不是方阵，是T*n大小的，T是目标集的大小，ZT代表了节点特征的聚合结果



## OPTIMIZATION IN NODE REPRESENTATION LEARNING

### Instantiation of NDM

#### 重要邻居的识别和选择

definition 4.1:为了量化节点u的邻居节点v对u的重要程度，引入了权重函数$\phi (ℓ,u,v) = U(\omega,\rho,ℓ)P^ℓ[u,v]$ 

自己的看法：虽然有函数phi来量化节点的重要程度，但是也不能一个一个的遍历，所以使用RWs来随机游走，检测哪些节点是重要节点

使用随机游走生成的RWs来访问目标节点，并且在权重估计中使用了RWs的长度和次数。通过随机游走，可以捕捉到高概率下的重要邻居节点。然而，随机游走也会选择一些不重要的邻居节点。

We observe that 10.6% neighbors contribute to 99% weights, and the rest 89.4% neighbors share the left 1% weights, as shown in Figure 2

RWs识别到的很多节点都是不重要的节点，文中提出了一个first-K selection方法，来减轻不重要邻居节点对模型的影响，就是只保留前K个RWs访问到的邻居节点。

最后一段文章又说，RWs会产生不重要节点，first-K selection在重要邻居节点不够K个的时候仍会选出K个，各有弊端，在互相补偿后得到了更好的效果（伪码就是RWs的过程中取前K个）





通过引理2产生theta，进行theta次RWs，如果一个节点v在第ℓ跳的时候被访问到，用U函数算出的对于节点u的ℓ跳的权重并除以theta，然后加到tv（v节点对应的对于u的权重）并在重要邻居集合S中加上v

#### 4.2

好像就分析了下复杂度

