GCN与GAT都是将邻居顶点的特征聚合到中心顶点上（一种aggregate运算），利用graph上的local stationary学习新的顶点特征表达。不同的是GCN利用了拉普拉斯矩阵，GAT利用attention系数。一定程度上而言，GAT会更强，因为 顶点特征之间的相关性被更好地融入到模型中。

## GCN

GNN到引入卷积,引入卷积的算法可以分为谱方法和非谱方法，谱方法基于图的谱表征，通过图拉普拉斯算子的特征分解，在傅里叶域中定义的卷积运算需要进行密集的矩阵计算和非局部空间的滤波。GCN可以有效的对节点的一阶邻域进行处理，从而避免复杂的矩阵运算。但是GCN依赖于图的结构信息，这就导致了在特定图结构上，训练得到的模型不可以直接被使用到其他图结构上。非谱方法直接在图上进行卷积（不是在图谱上）。这种方法的挑战是如何找到一个或者定义一个运算来处理可变大小的邻域并保证参数共享机制。

## GAT

GAT模型可以有效的适用于基于图的归纳学习问题和转导学习的问题。

### GAT本质上有两种运算方式

- global graph attention

  不依赖于图的结构，对于inductive任务无压力，但是丢掉了图的结构，等于自废武功，此外面临高昂的运算成本。

- mask graph attention
​
29
#### 为什么GAT适用于inductive任务？
30
​
31
GAT中重要的学习参数是 ![[公式]](https://www.zhihu.com/equation?tex=W) 与 ![[公式]](https://www.zhihu.com/equation?tex=a%28%5Ccdot%29) ，因为上述的逐顶点运算方式，这两个参数仅与顶点特征相关，与图的结构毫无关系。所以测试任务中改变图的结构，对于GAT影响并不大，只需要改变 ![[公式]](https://www.zhihu.com/equation?tex=%5Cmathcal%7BN%7D_i) ，重新计算即可。
32
​
33
与此相反的是，GCN是一种全图的计算方式，一次计算就更新全图的节点特征。学习的参数很大程度与图结构相关，这使得GCN在inductive任务上遇到困境。
  注意力机制的运算只在邻居顶点上进行。

### GAT的步骤

和所有的attention mechanism一样，GAT的运算也分为两步：

- 计算注意力系数（attention coefficient）

- 加权求和（aggregate)
