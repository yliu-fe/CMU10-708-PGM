
第一次作业中包含两个编程工作，分别是：
- 联合树算法的简单算例`junction_tree.py`
- Baum-Welch算法`baum_welch.py`


# Junction Tree

联合树算法算例是Question 3.3的唯一内容，承接前面的Question 3.2，都在同一张贝叶斯网络下进行。原始贝叶斯网络结构如下所示：

![image.png](https://s2.loli.net/2023/11/01/6MEPtObLUwTm8ak.png)

显然，亲缘化后的无向图如下所示：

![image.png](https://s2.loli.net/2023/11/01/at3S5JV9EHFvPZB.png)

给出的算例是这样的：

![image.png](https://s2.loli.net/2023/11/01/C5WoYFdKvwUcnVu.png)

这张表以`numpy.array`的形式写成了`factors()`函数：

```python
def factors(x):
    assert len(x) == 11
    phi = dict()
    phi["a"] = np.array([x[0], 1 - x[0]])
    phi["ab"] = np.array([[x[1], 1 - x[1]], [x[2], 1 - x[2]]])
    phi["ae"] = np.array([[x[3], 1 - x[3]], [x[4], 1 - x[4]]])
    phi["bc"] = np.array([[x[5], 1 - x[5]], [x[6], 1 - x[6]]])
    phi["ced"] = np.array(
        [[[x[7], 1 - x[7]], [x[8], 1 - x[8]]], [[x[9], 1 - x[9]], [x[10], 1 - x[10]]]]
    )
    return phi
```

$x_0$到$x_{10}$的取值在`junction.py`的最后给出了。具体要求参见assignment对应的pdf文件。

## 1. initial clique potentials
### 明确工作内容

首先完成初始的团势函数，对于亲缘化后的无向图进行三角化，显然有：

![image.png](https://s2.loli.net/2023/11/01/V4UQrl7CAwG8xYR.png)

很明显，这里面有三个团：$\{ABE,BCE,CDE\}$，而无向图的概率分布就是这三个团的势函数乘积：

$$
p(a,b,c,d,e) = \psi(abe)\psi(bce)\psi(cde)
$$

这里我们需要做的是给出这三个$\psi$。他们之间是有公共节点的，因此每个团的势函数并非是团内的条件概率分布的直接乘积，要照顾到其他团是不是已经计算了某个点的条件概率：

$$
\begin{align*}
\psi(abe) &= p(a) p(b\mid a) p(e \mid a)\\
\psi(bce) &= p(c\mid b)\\
\psi(cde) &= p(d \mid c,e)
\end{align*}
$$

其中，对于$\psi(bce)$，团$BCE$中的点$b,c$的条件分布已经包含在了$\psi(abe)$中，因此无需再次讨论；对于$\psi(cde)$，点$c,e$的条件分布分别由$\psi(abe), \psi(bce)$给出，无需讨论。

现在我们明确了所需的工作。
### 代码

框架代码：

```python
def initial_clique_potentials(phi):
    psi = dict()
    # TODO ...
    return psi
```

显然，我们需要像`factors()`函数一样，把$\psi$写成一个dictionary。

首先从最简单的$\psi(cde)$开始，因为它就是$P(d\mid c,e)$，而这个条件概率完整地被算例给出：

```python
psi["ced"] = phi["ced"]
```

类似的，$\psi(bce)$就是$p(c\mid b)$，但并不能这么简单的写进去，因为这个概率实际上应该是$p(c \mid b,e)$，但是通过贝叶斯网络的定义可知，在已知父节点$B$，而未知$D$的情况下，$C$与其非子结点$E$条件独立：$C \perp E \mid B$。但是，为了严格的符合团势函数的要求，即包含团中所有节点的势函数（概率分布），因此我们要写成三维矩阵——尽管代表$e$的第三维并不起作用：

```python
bce_tmp = np.zeros(2,2,2)
bce_tmp[:,:,0] = phi["bc"]
bce_tmp[:,:,1] = phi["bc"]
psi["bce"] = bce_tmp
```

最麻烦的是$\psi(abe)$，因为它是三个条件概率的乘积。我们先从$p(a)p(b\mid a)$来看看这是一个什么样的运算：

> [!团势函数的运算]
> 从$p(a)p(b\mid a)$的简单情况开始。根据算例，$p(a)$是一个列向量，未不失一般性，我们规定这种概率分布的矩阵/向量的第一个元素代表变量为0，第二个元素代表变量为1（时的概率）：
> 
> $$
> \begin{equation}
> 	p(a) = \left[\begin{array}{c}
> 		x_0\\
> 		1-x_0
> 	\end{array}\right], \quad 
> 	p(b\mid a) = \left[\begin{array}{cc}
> 		x_1 & 1- x_1\\
> 		x_2 & 1- x_2
> 	\end{array}\right]\nonumber
> \end{equation}
> $$
> 
> 那么，$\psi(a,b) = p(a)p(b\mid a)$应该表示为：
> $$
> 
> \begin{equation}
> 	\psi(a,b) = \left[\begin{array}{cc}
> 		x_0 x_1 & x_0(1- x_1)\\
> 		(1-x_0) x_2 & (1-x_0)(1- x_2)
> 	\end{array}\right]
> \end{equation}
> $$

相当于$p(a)$向量中的每个元素，分别乘到了$p(b\mid a)$的对应行中，这样的运算可以用`numpy.einsum()`函数进行：

```python
ab_tmp = np.einsum("a,ab->ab", phi["a"], phi["ab"])
```

> 关于`einsum`的具体用法，参见：
> - 知乎专栏： [如何理解和使用Numpy.einsum](https://zhuanlan.zhihu.com/p/27739282)
> - numpy文档：[numpy.einsum](https://numpy.org/doc/stable/reference/generated/numpy.einsum.html)

进一步，把$\psi(abe) = p(a)p(b\mid a)p(e\mid a)$可以写为：

```python
psi["abe"] = np.einsum("ab,ae->abe", ab_tmp, phi["ae"])
```

得到的`psi["abe"]`是一个三维的`np.array`，维度依次为`a,b,e`的取值。

总结一下成品代码：

```python
def initial_clique_potentials(phi):
    psi = dict()
    
    ab_tmp = np.einsum("a,ab->ab", phi["a"], phi["ab"])
    psi["abe"] = np.einsum("ab,ae->abe", ab_tmp, phi["ae"])
    bce_tmp = np.zeros(2,2,2)
    bce_tmp[:,:,0] = phi["bc"]
    bce_tmp[:,:,1] = phi["bc"]
    psi["bce"] = bce_tmp
    psi["ced"] = phi["ced"]
    return psi
```

## 2. messages
接下来完成`message(psi)`函数。

### 明确工作内容

