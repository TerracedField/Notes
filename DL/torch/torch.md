[toc]

## tensor

### cat合并

```
(tensor([[ 0.4755,  0.1765,  0.2200, -0.3758,  0.0590, -0.1009, -0.2270,  0.3249],
         [ 0.4501,  0.1816,  0.3269, -0.3423,  0.1115,  0.0378, -0.2091,  0.1768],
         [ 0.3281,  0.0851,  0.2253, -0.4032,  0.1696, -0.0475, -0.1658,  0.3093]],
        grad_fn=<AddmmBackward0>),
 tensor([[-0.2055,  0.0579, -0.2525,  0.0588],
         [-0.1978,  0.0563, -0.2532,  0.0682],
         [-0.1921,  0.0464, -0.2399,  0.0697]], grad_fn=<AddmmBackward0>))
```

```python
torch.cat((net1(X1), net2(X2)), dim=1)
```

```
tensor([[-0.0395,  0.2852, -0.1403,  0.1256, -0.2409, -0.4229,  0.2372,  0.2719,
          0.0626, -0.1902,  0.0975, -0.1365],
        [-0.1373,  0.2139,  0.0534,  0.0018, -0.2146, -0.2590,  0.2603,  0.4758,
          0.0848, -0.2055,  0.1218, -0.1293],
        [-0.0796,  0.2327, -0.0647,  0.1997, -0.1470, -0.3759,  0.2663,  0.3674,
          0.0888, -0.1786,  0.1104, -0.1245]], grad_fn=<CatBackward0>)
```

按第一维合并

### 维度变换

数字对应原来的维度位置

```python
# 原来 E_i.shape:(N,C,H,W)
E_i = E_i.permute(2, 3, 1, 0) # 换成(H,W,C,N)
```

交换0,1两个维度的数据


```python
P = P.transpose(0, 1)
```

### 去除维度

去除倒数第一维度

```python
P = P.squeeze(-1)
```

去除倒数第一，二维度

```python
P = P.squeeze(-1,2)
```

### 合并维度

```python
# 假设E_i已经定义，形状为[3, 64, 128, 256]
E_i = torch.randn(3, 64, 128, 256)

# 使用torch.view合并后两个维度
E_i_reshaped = E_i.view(3, 64, -1)  # -1表示自动计算该维度的大小
```

或者reshape合并中间两维度：

```python
x = torch.reshape(x, (x.shape[0], -1, x.shape[3]))
```

### 增加维度

假设：

```
Q_i.shape: torch.Size([3, 64, 32768])
```

对于提供的张量`Q_i`的形状`torch.Size([3, 64, 32768])`，如果想在第一个维度前添加一个新的维度，可以使用以下代码：

```python
Q_i = Q_i.unsqueeze(0)
```

这将在索引0的位置添加一个新的维度，结果形状将是`torch.Size([1, 3, 64, 32768])`。

如果想在最后一个维度后添加一个新的维度，可以使用：

```python
Q_i = Q_i.unsqueeze(-1)
```

这将在最后一个维度后添加一个新的维度，结果形状将是`torch.Size([3, 64, 32768, 1])`。

### rand

在PyTorch中，`torch.rand`和`torch.randn`是两个用于生成随机数的函数，它们的主要区别在于生成的随机数分布不同：

1. **torch.rand**：

	- 这个函数用于生成均匀分布的随机数。

	- 它生成的随机数在[0, 1)区间内，即从0（包括）到1（不包括）之间的随机浮点数。

	- 用法示例：

		```python
		import torch
		tensor = torch.rand(size)  # size可以是整数，也可以是整数的元组
		```

		这将生成一个形状为`size`的张量，其中的元素是从[0, 1)均匀分布的随机数。

2. **torch.randn**：

	- 这个函数用于生成标准正态分布（均值为0，标准差为1）的随机数。

	- 它生成的随机数遵循正态分布，即大部分数值集中在0附近，分布的形状呈钟形曲线。

	- 用法示例：

		```python
		import torch
		tensor = torch.randn(size)  # size同样可以是整数，也可以是整数的元组
		```

		这将生成一个形状为`size`的张量，其中的元素是标准正态分布的随机数。

根据你的应用场景，你可以选择使用`torch.rand`或`torch.randn`。例如，在初始化神经网络的权重时，通常使用`torch.randn`来模拟权重的初始分布，因为正态分布可以帮助梯度下降算法更快地收敛。而在需要均匀分布的随机数时，比如在某些采样算法中，可能会使用`torch.rand`。

### rearrange

`rearrange` 函数是 `einops` 库中的一个非常强大的函数，它用于重新排列多维数组的形状和维度。这个函数的一般形式是：

```python
rearrange(input, pattern, **kwargs)
```

- `input`：要重新排列的输入数组。
- `pattern`：一个字符串，指定了输入数组的维度如何被重新排列。
- `**kwargs`：一些额外的参数，用于替换模式字符串中的变量。

在你提供的代码中，`rearrange` 函数被用来将图像的二维空间特征映射（即图像的像素）转换为一系列一维的图像块（patches），这些图像块可以被送入Transformer模型中进行处理。

具体来说，`rearrange` 函数的调用如下：

```python
img_patches = rearrange(x,
                        'b c (patch_x x) (patch_y y) -> b (x y) (patch_x patch_y c)',
                        patch_x=self.patch_size, patch_y=self.patch_size)
```

这里的参数解释如下：

- `x`：输入数组，通常是图像的张量，形状为 `(batch_size, channels, height, width)`。
- `'b c (patch_x x) (patch_y y)'`：这是输入模式字符串，其中：
  - `b` 代表批次大小（batch size）。
  - `c` 代表通道数（channels）。
  - `(patch_x x)` 和 `(patch_y y)` 表示将高度和宽度分别分割成 `patch_x` 和 `patch_y` 大小的块，`x` 和 `y` 是这些块的数量。
- `-> b (x y) (patch_x patch_y c)`：这是输出模式字符串，表示重新排列后的形状：
  - `b` 仍然是批次大小。
  - `(x y)` 表示将分割后的块重新排列成一个新的二维形状，`x` 和 `y` 分别是块在高度和宽度上的数量。
  - `(patch_x patch_y c)` 表示每个块的形状，`patch_x` 和 `patch_y` 是块的空间维度，`c` 是通道数。
- `patch_x=self.patch_size` 和 `patch_y=self.patch_size`：这些是额外的参数，用于替换模式字符串中的 `patch_x` 和 `patch_y` 变量，它们表示每个块的空间尺寸。

总的来说，这行代码的作用是将输入图像分割成 `patch_size x patch_size` 的小块，并将这些块重新排列成一个一维的序列，每个块包含 `patch_size * patch_size * channels` 个元素，这样就可以将这些块作为序列输入到Transformer模型中。





## 自动求导

```python
def f(a):
    b = a * 2
    while b.norm() < 1000:
        print("\n",b.norm())
        b = b * 2
    if b.sum() > 0:
        c = b
        print("C==b\n",c)
    else:
        c = 100 * b
        print("c=100b\n",c)
    return c

a = torch.randn(size=(3,1), requires_grad=True)
print(a.shape)
print(a)
d = f(a)
d.backward() #<====== run time error if a is vector or matrix RuntimeError: grad can be implicitly created only for scalar outputs
d.sum().backward() #<===== this way it will work
print(d)
```

尝试对`d`调用`.backward()`，如果`a`是向量或矩阵，这将引发运行时错误：`RuntimeError: grad can be implicitly created only for scalar outputs`。翻译为：运行时错误：梯度只能为标量输出隐式创建。通过调用`d.sum().backward()`，可以解决这个问题，因为这样会创建一个标量输出，允许梯度被隐式创建。



因为是sum，所以实际上是x1+x2+x3(size=(3,1))，所以求得的对x的梯度

### 例子

```python
%matplotlib inline
import matplotlib.pylab as plt
from matplotlib.ticker import FuncFormatter, MultipleLocator
import numpy as np
import torch

f,ax=plt.subplots(1)

x = np.linspace(-3np.pi, 3np.pi, 100)
x1= torch.tensor(x, requires_grad=True)
y1= torch.sin(x1)
y1.sum().backward()

ax.plot(x,np.sin(x),label=‘sin(x)’)
ax.plot(x,x1.grad,label=“gradient of sin(x)”)
ax.legend(loc=‘upper center’, shadow=True)

ax.xaxis.set_major_formatter(FuncFormatter(
lambda val,pos: ‘{:.0g}$\pi$’.format(val/np.pi) if val !=0 else ‘0’
))
ax.xaxis.set_major_locator(MultipleLocator(base=np.pi))

plt.show()
```

x1.grad为cos是因为y1是sum的反向传播，相当于对每个x1里面的x1i累加的分量求导

## nn

### module

```python
import torch
from torch import nn
from torch.nn import functional as F

net = nn.Sequential(nn.Linear(20, 256), nn.ReLU(), nn.Linear(256, 10)) # 权重偏置默认随机

X = torch.rand(2, 20) # 1个大小为2*20的矩阵作为输入
print(X)
net(X)
```

在这个例子中，我们通过实例化`nn.Sequential`来构建我们的模型，层的执行顺序是作为参数传递的。简而言之，(**`nn.Sequential`定义了一种特殊的`Module`**)，即在PyTorch中表示一个块的类，它维护了一个由`Module`组成的有序列表。注意，两个全连接层都是`Linear`类的实例，`Linear`类本身就是`Module`的子类。另外，到目前为止，我们一直在通过`net(X)`调用我们的模型来获得模型的输出。这实际上是`net.__call__(X)`的简写。这个前向传播函数非常简单：它将列表中的每个块连接在一起，将每个块的输出作为下一个块的输入。



```python
class MLP(nn.Module):
    # 用模型参数声明层。这里，我们声明两个全连接的层
    def __init__(self):
        # 调用MLP的父类Module的构造函数来执行必要的初始化。
        # 这样，在类实例化时也可以指定其他函数参数，例如模型参数params（稍后将介绍）
        super().__init__()
        self.hidden = nn.Linear(20, 256)  # 隐藏层
        self.out = nn.Linear(256, 10)  # 输出层

    # 定义模型的前向传播，即如何根据输入X返回所需的模型输出
    def forward(self, X):
        # 注意，这里我们使用ReLU的函数版本，其在nn.functional模块中定义。
        return self.out(F.relu(self.hidden(X)))
```

我们首先看一下前向传播函数，它以`X`作为输入， 计算带有激活函数的隐藏表示，并输出其未规范化的输出值。 在这个`MLP`实现中，两个层都是实例变量。 要了解这为什么是合理的，可以想象实例化两个多层感知机（`net1`和`net2`）， 并根据不同的数据对它们进行训练。 当然，我们希望它们学到两种不同的模型。

接着我们[**实例化多层感知机的层，然后在每次调用前向传播函数时调用这些层**]。 注意一些关键细节： 首先，我们定制的`__init__`函数通过`super().__init__()` 调用父类的`__init__`函数， 省去了重复编写模版代码的痛苦。 然后，我们实例化两个全连接层， 分别为`self.hidden`和`self.out`。 注意，除非我们实现一个新的运算符， 否则我们不必担心反向传播函数或参数初始化， 系统将自动生成这些。



### Sequential

现在我们可以更仔细地看看`Sequential`类是如何工作的， 回想一下`Sequential`的设计是为了把其他模块串起来。 为了构建我们自己的简化的`MySequential`， 我们只需要定义两个关键函数：

1. 一种将块逐个追加到列表中的函数；
2. 一种前向传播函数，用于将输入按追加块的顺序传递给块组成的“链条”。

下面的`MySequential`类提供了与默认`Sequential`类相同的功能。

```python
class MySequential(nn.Module):
    def __init__(self, *args):
        super().__init__()
        for idx, module in enumerate(args):
            # 这里，module是Module子类的一个实例。我们把它保存在'Module'类的成员
            # 变量_modules中。_module的类型是OrderedDict
            self._modules[str(idx)] = module

    def forward(self, X):
        # OrderedDict保证了按照成员添加的顺序遍历它们
        for block in self._modules.values():
            X = block(X)
        return X
```

`__init__`函数将每个模块逐个添加到有序字典`_modules`中。 读者可能会好奇为什么每个`Module`都有一个`_modules`属性？ 以及为什么我们使用它而不是自己定义一个Python列表？ 简而言之，`_modules`的主要优点是： 在模块的参数初始化过程中， 系统知道在`_modules`字典中查找需要初始化参数的子块。

当`MySequential`的前向传播函数被调用时， 每个添加的块都按照它们被添加的顺序执行。 现在可以使用我们的`MySequential`类重新实现多层感知机。

```python
net = MySequential(nn.Linear(20, 256), nn.ReLU(), nn.Linear(256, 10))
net(X)
```

### forward

`Sequential`类使模型构造变得简单， 允许我们组合新的架构，而不必定义自己的类。 然而，并不是所有的架构都是简单的顺序架构。 当需要更强的灵活性时，我们需要定义自己的块。 例如，我们可能希望在前向传播函数中执行Python的控制流。 此外，我们可能希望执行任意的数学运算， 而不是简单地依赖预定义的神经网络层。

到目前为止， 我们网络中的所有操作都对网络的激活值及网络的参数起作用。 然而，有时我们可能希望合并既不是上一层的结果也不是可更新参数的项， 我们称之为*常数参数*（constant parameter）。 例如，我们需要一个计算函数 𝑓(𝑥,𝑤)=𝑐⋅𝑤⊤𝑥的层， 其中𝑥是输入， 𝑤是参数， 𝑐是某个在优化过程中没有更新的指定常量。 因此我们实现了一个`FixedHiddenMLP`类，如下所示：

```python
class FixedHiddenMLP(nn.Module):
    def __init__(self):
        super().__init__()
        # 不计算梯度的随机权重参数。因此其在训练期间保持不变
        self.rand_weight = torch.rand((20, 20), requires_grad=False)
        self.linear = nn.Linear(20, 20)

    def forward(self, X):
        X = self.linear(X)
        # 使用创建的常量参数以及relu和mm函数
        X = F.relu(torch.mm(X, self.rand_weight) + 1)
        # 复用全连接层。这相当于两个全连接层共享参数
        X = self.linear(X)
        # 控制流
        while X.abs().sum() > 1:
            X /= 2
        return X.sum()
```

在这个`FixedHiddenMLP`模型中，我们实现了一个隐藏层， 其权重（`self.rand_weight`）在实例化时被随机初始化，之后为常量。 这个权重不是一个模型参数，因此它永远不会被反向传播更新。 然后，神经网络将这个固定层的输出通过一个全连接层。

注意，在返回输出之前，模型做了一些不寻常的事情： 它运行了一个while循环，在𝐿1范数大于1的条件下， 将输出向量除以2，直到它满足条件为止。 最后，模型返回了`X`中所有项的和。 注意，此操作可能不会常用于在任何实际任务中， 我们只展示如何将任意代码集成到神经网络计算的流程中。

### 混合

我们可以[**混合搭配各种组合块的方法**]。 在下面的例子中，我们以一些想到的方法嵌套块。



```python
class NestMLP(nn.Module):
    def __init__(self):
        super().__init__()
        self.net = nn.Sequential(nn.Linear(20, 64), nn.ReLU(),
                                 nn.Linear(64, 32), nn.ReLU())
        self.linear = nn.Linear(32, 16)

    def forward(self, X):
        return self.linear(self.net(X))

chimera = nn.Sequential(NestMLP(), nn.Linear(16, 20), FixedHiddenMLP())
chimera(X)
```

### 添加块

```python
def block1():
    return nn.Sequential(nn.Linear(4, 8), nn.ReLU(),
                         nn.Linear(8, 4), nn.ReLU())

def block2():
    net = nn.Sequential()
    for i in range(4):
        # 在这里嵌套
        net.add_module(f'block {i}', block1())
    return net

rgnet = nn.Sequential(block2(), nn.Linear(4, 1))
rgnet(X)
```

print得到：

```
Sequential(
  (0): Sequential(
    (block 0): Sequential(
      (0): Linear(in_features=4, out_features=8, bias=True)
      (1): ReLU()
      (2): Linear(in_features=8, out_features=4, bias=True)
      (3): ReLU()
    )
    (block 1): Sequential(
      (0): Linear(in_features=4, out_features=8, bias=True)
      (1): ReLU()
      (2): Linear(in_features=8, out_features=4, bias=True)
      (3): ReLU()
    )
    (block 2): Sequential(
      (0): Linear(in_features=4, out_features=8, bias=True)
      (1): ReLU()
      (2): Linear(in_features=8, out_features=4, bias=True)
      (3): ReLU()
    )
    (block 3): Sequential(
      (0): Linear(in_features=4, out_features=8, bias=True)
      (1): ReLU()
      (2): Linear(in_features=8, out_features=4, bias=True)
      (3): ReLU()
    )
  )
  (1): Linear(in_features=4, out_features=1, bias=True)
)
```







## 参数管理

### 访问参数

```python
import torch
from torch import nn

net = nn.Sequential(nn.Linear(4, 8), nn.ReLU(), nn.Linear(8, 1))
X = torch.rand(size=(2, 4))
net(X)
```

我们从已有模型中访问参数。 当通过`Sequential`类定义模型时， 我们可以通过索引来访问模型的任意层。 这就像模型是一个列表一样，每层的参数都在其属性中。 如下所示，我们可以检查第二个全连接层的参数。

```python
print(net[2].state_dict())
```

```
OrderedDict([('weight', tensor([[ 0.2184,  0.1997, -0.2909,  0.1927, -0.0060,  0.3408, -0.2632,  0.1543]])), ('bias', tensor([0.3183]))])
```

输出的结果告诉我们一些重要的事情： 首先，这个全连接层包含两个参数，分别是该层的权重和偏置。 两者都存储为单精度浮点数（float32）。 注意，参数名称允许唯一标识每个参数，即使在包含数百个层的网络中也是如此。



注意，每个参数都表示为参数类的一个实例。 要对参数执行任何操作，首先我们需要访问底层的数值。 有几种方法可以做到这一点。有些比较简单，而另一些则比较通用。 下面的代码从第二个全连接层（即第三个神经网络层）提取偏置， 提取后返回的是一个参数类实例，并进一步访问该参数的值。

```python
print(type(net[2].bias))
print(net[2].bias)
print(net[2].bias.data)
```

```
<class 'torch.nn.parameter.Parameter'>
Parameter containing:
tensor([0.3183], requires_grad=True)
tensor([0.3183])
```



### 初始化参数

让我们首先调用内置的初始化器。 下面的代码将所有权重参数初始化为标准差为0.01的高斯随机变量， 且将偏置参数设置为0。

```python
def init_normal(m):
    if type(m) == nn.Linear:
        nn.init.normal_(m.weight, mean=0, std=0.01)
        nn.init.zeros_(m.bias)
net.apply(init_normal)
net[0].weight.data[0], net[0].bias.data[0]
```

我们还可以[**对某些块应用不同的初始化方法**]。 例如，下面我们使用Xavier初始化方法初始化第一个神经网络层， 然后将第三个神经网络层初始化为常量值42。

```python
def init_xavier(m):
    if type(m) == nn.Linear:
        nn.init.xavier_uniform_(m.weight)
def init_42(m):
    if type(m) == nn.Linear:
        nn.init.constant_(m.weight, 42)

net[0].apply(init_xavier)
net[2].apply(init_42)
print(net[0].weight.data[0])
print(net[2].weight.data)
```

### 自定义层

我们可以使用内置函数来创建参数，这些函数提供一些基本的管理功能。 比如管理访问、初始化、共享、保存和加载模型参数。 这样做的好处之一是：我们不需要为每个自定义层编写自定义的序列化程序。

现在，让我们实现自定义版本的全连接层。 回想一下，该层需要两个参数，一个用于表示权重，另一个用于表示偏置项。 在此实现中，我们使用修正线性单元作为激活函数。 该层需要输入参数：`in_units`和`units`，分别表示输入数和输出数。

```python
class MyLinear(nn.Module):
    def __init__(self, in_units, units):
        super().__init__()
        self.weight = nn.Parameter(torch.randn(in_units, units))
        self.bias = nn.Parameter(torch.randn(units,))
    def forward(self, X):
        linear = torch.matmul(X, self.weight.data) + self.bias.data
        return F.relu(linear)
```

用nn.Parameter使得w和b保存梯度

## 加载、保存中间结果

### tensor

```python
import torch
from torch import nn
from torch.nn import functional as F

x1 = torch.arange(4)
torch.save(x1, 'x-file') # 保存，同目录下存一个文件
x2 = torch.load('x-file') # 加载，同目录下加载x-file文件
```

### dict

```python
mydict = {'x': x, 'y': y}
torch.save(mydict, 'mydict')
mydict2 = torch.load('mydict')
```

### 加载、保存模型



```python
class MLP(nn.Module):
    def __init__(self):
        super().__init__()
        self.hidden = nn.Linear(20, 256)
        self.output = nn.Linear(256, 10)

    def forward(self, x):
        return self.output(F.relu(self.hidden(x)))

net = MLP()
X = torch.randn(size=(2, 20))
Y = net(X)
```

接下来，我们[**将模型的参数存储在一个叫做“mlp.params”的文件中。**]

```python
torch.save(net.state_dict(), 'mlp.params')
```

为了恢复模型，我们[**实例化了原始多层感知机模型的一个备份。**] 这里我们不需要随机初始化模型参数，而是(**直接读取文件中存储的参数。**)

```python
clone = MLP()
clone.load_state_dict(torch.load('mlp.params'))
clone.eval()
```

## expand

扩展wei'du

```python
# 扩展cls_token以匹配batch_size，并保持其他维度不变
# expand需要指定扩展的维度，所以这里每个dim都需要用-1来表示
cls_token = self.cls_token.expand(x.shape[0], -1, -1)
```

## 使用GPU

### 查看参数所在设备

```python
X1 = torch.rand(3, 64)
X2 = torch.rand(3, 64)
X1.device
```

```
device(type='cpu')
```



###  将模型放在GPU上

李沐预设了一个GPU的函数

```python
def try_gpu(i=0):  #@save
    """如果存在，则返回gpu(i)，否则返回cpu()"""
    if torch.cuda.device_count() >= i + 1:
        return torch.device(f'cuda:{i}')
    return torch.device('cpu')

def try_all_gpus():  #@save
    """返回所有可用的GPU，如果没有GPU，则返回[cpu(),]"""
    devices = [torch.device(f'cuda:{i}')
             for i in range(torch.cuda.device_count())]
    return devices if devices else [torch.device('cpu')]
```

将模型放在GPU上

```python
net = nn.Sequential(nn.Linear(3, 1))
net = net.to(device=try_gpu())
```

我的电脑本地只有一个GPU，等价于：

```python
net = nn.Sequential(nn.Linear(3, 1))
net = net.to(device='cuda:0')
```

CPU、GPU使用区别


```python
import time
import torch

def try_gpu(i=0):  #@save
    """如果存在，则返回gpu(i)，否则返回cpu()"""
    if torch.cuda.device_count() >= i + 1:
        return torch.device(f'cuda:{i}')
    return torch.device('cpu')

startTime1=time.time()
for i in range(100):
    A = torch.ones(500,500)
    B = torch.ones(500,500)
    C = torch.matmul(A,B)
endTime1=time.time()

startTime2=time.time()
for i in range(100):
    A = torch.ones(500,500,device=try_gpu())
    B = torch.ones(500,500,device=try_gpu())
    C = torch.matmul(A,B)
endTime2=time.time()

print('cpu计算总时长:', round((endTime1 - startTime1)*1000, 2),'ms')
print('gpu计算总时长:', round((endTime2 - startTime2)*1000, 2),'ms')
```





## 矩阵乘法

A.shape =（b,m,n)；B.shape = (b,n,k)
numpy.matmul(A,B) 结果shape为(b,m,k)

两个Tensor维度要求：

- "2维以上"的尺寸必须完全对应相等；
- "2维"具有实际意义的单位，只要满足矩阵相乘的尺寸规律即可。



第一个参数的最后一个维度和第二个参数的倒数第二个维度的尺寸（即维度大小）必须相同







## 广播求和

```python
#@save
class AdditiveAttention(nn.Module):
    """加性注意力"""
    def __init__(self, key_size, query_size, num_hiddens, dropout, **kwargs):
        super(AdditiveAttention, self).__init__(**kwargs)
        self.W_k = nn.Linear(key_size, num_hiddens, bias=False)
        self.W_q = nn.Linear(query_size, num_hiddens, bias=False)
        self.w_v = nn.Linear(num_hiddens, 1, bias=False)
        self.dropout = nn.Dropout(dropout)

    def forward(self, queries, keys, values, valid_lens):
        queries, keys = self.W_q(queries), self.W_k(keys)
        # 在维度扩展后，
        # queries的形状：(batch_size，查询的个数，1，num_hidden)
        # key的形状：(batch_size，1，“键－值”对的个数，num_hiddens)
        # 使用广播方式进行求和
        features = queries.unsqueeze(2) + keys.unsqueeze(1)
        features = torch.tanh(features)
        # self.w_v仅有一个输出，因此从形状中移除最后那个维度。
        # scores的形状：(batch_size，查询的个数，“键-值”对的个数)
        scores = self.w_v(features).squeeze(-1)
        self.attention_weights = masked_softmax(scores, valid_lens)
        # values的形状：(batch_size，“键－值”对的个数，值的维度)
        return torch.bmm(self.dropout(self.attention_weights), values)
```



如对一个1\*3的向量和一个3\*1的向量求和，如1\*3的是[1, 2, 3]，3\*1的是[4, 5, 6]^T。那么变成，然后相加

```
[[1, 2 ,3], 
 [1, 2 ,3], 
 [1, 2 ,3]]
 
 [[4, 4 ,4], 
  [5, 5 ,5], 
  [6, 6 ,6]]
```

## Dataset类

`Dataset`类是PyTorch中的一个核心组件，用于定义数据集的结构和行为。它是一个抽象基类，提供了数据加载和预处理的基本框架，使得用户可以自定义数据集的处理方式。以下是`Dataset`类的一些关键特性和使用方法：

### 基本结构

`Dataset`类定义了三个主要的方法，所有子类都需要根据具体数据集实现这些方法：

1. **`__init__`方法**：
   - 构造函数，用于初始化数据集对象。通常在这里设置数据路径、加载数据、预处理参数等。

2. **`__getitem__`方法**：
   - 索引方法，接受一个索引值`idx`，并返回该索引对应的数据项（通常是一个样本及其标签）。
   - 这个方法是数据加载的核心，它定义了如何根据索引加载和处理单个数据样本。

3. **`__len__`方法**：
   - 返回数据集中的样本总数。
   - 这个方法用于确定数据集的大小，通常在创建数据加载器（`DataLoader`）时使用。

### 使用场景

`Dataset`类通常用于以下场景：

- **数据加载**：从文件系统、数据库或其他来源加载原始数据。
- **数据预处理**：对原始数据进行必要的转换和标准化，以适应模型训练的需要。
- **数据增强**：通过旋转、缩放、裁剪等方法增加数据的多样性。
- **批处理**：将多个样本组合成一个批次，以便于并行处理。

### 继承和实现

当创建自定义数据集时，需要继承`Dataset`类，并实现上述三个方法。例如：

```python
from torch.utils.data import Dataset

class CustomDataset(Dataset):
    def __init__(self, data_paths, transform=None):
        self.data_paths = data_paths
        self.transform = transform

    def __getitem__(self, index):
        data = load_data(self.data_paths[index])
        if self.transform:
            data = self.transform(data)
        return data

    def __len__(self):
        return len(self.data_paths)
```

### 与DataLoader结合使用

`Dataset`对象通常与`DataLoader`类结合使用，`DataLoader`负责将数据集封装成批次，并提供多线程加载、打乱数据顺序等功能。例如：

```python
from torch.utils.data import DataLoader

dataset = CustomDataset(data_paths, transform=some_transform)
data_loader = DataLoader(dataset, batch_size=32, shuffle=True, num_workers=4)

for batch in data_loader:
    # 处理每个批次的数据
    pass
```

通过这种方式，`Dataset`类为PyTorch用户提供了一个灵活且强大的方式来处理和加载数据，使得数据准备工作更加高效和系统化。



## DataLoader

`DataLoader`是PyTorch中的一个重要组件，用于封装数据集并提供批量加载数据的功能。它继承自Python的迭代器（Iterator），使得数据可以以批次（batch）的形式高效地加载和预处理。以下是`DataLoader`的一些关键特性和使用方法：

### 基本特性

1. **批量加载**：
   - 将数据集分成多个批次，每个批次包含多个样本，这样可以利用GPU的并行计算能力，提高训练效率。

2. **多线程/多进程加载**：
   - 通过设置`num_workers`参数，可以实现数据的多线程/多进程加载，从而减少数据加载时间。

3. **数据打乱**：
   - 通过设置`shuffle`参数，可以在每个epoch开始时打乱数据，有助于模型训练的泛化能力。

4. **数据预处理**：
   - 可以与`Dataset`类结合使用，对每个样本进行预处理。

5. **自定义批次处理**：
   - 通过`collate_fn`参数，可以自定义如何将多个样本合并成一个批次。

### 使用方法

以下是如何使用`DataLoader`的基本步骤：

1. **创建自定义的`Dataset`类**：
   - 如之前所述，继承`Dataset`类并实现`__getitem__`和`__len__`方法。

2. **实例化`DataLoader`**：
   - 将自定义的`Dataset`实例传递给`DataLoader`，并设置相关参数。

3. **迭代`DataLoader`**：
   - 在训练循环中迭代`DataLoader`，获取每个批次的数据。

### 示例代码

```python
from torch.utils.data import DataLoader, Dataset

# 假设CustomDataset是之前定义的自定义数据集类
dataset = CustomDataset(data_paths, transform=some_transform)

# 实例化DataLoader
data_loader = DataLoader(dataset, batch_size=32, shuffle=True, num_workers=4)

# 迭代DataLoader
for batch in data_loader:
    # 处理每个批次的数据
    pass
```

### 参数详解

- `dataset`：要加载的数据集，必须是`Dataset`对象。
- `batch_size`：每个批次的样本数。
- `shuffle`：是否在每个epoch开始时打乱数据。
- `num_workers`：用于数据加载的子进程数。设置为大于0的值可以加快数据加载速度，特别是在处理大规模数据集时。
- `collate_fn`：一个函数，用于定义如何将多个样本合并成一个批次。如果设置为`None`，则使用默认的合并方式。

### 自定义`collate_fn`

在某些情况下，可能需要自定义如何将多个样本合并成一个批次。例如，当数据集中包含不同大小的图像时，可能需要在`collate_fn`中进行特殊的处理：

```python
def my_collate_fn(batch):
    images, labels = zip(*batch)
    images = torch.stack(images, dim=0)
    labels = torch.tensor(labels)
    return images, labels

data_loader = DataLoader(dataset, batch_size=32, collate_fn=my_collate_fn)
```

## transform

在 PyTorch 的 `torchvision.transforms` 模块中，有一个现成的 `Compose` 类，它用于将多个图像变换操作组合成一个操作序列。这个类允许将多个变换包装成一个单独的变换。

以下是 `torchvision.transforms.Compose` 的基本用法：

```python
import torchvision.transforms as transforms

# 定义一系列变换操作
transform_list = [
    transforms.Resize(256),
    transforms.CenterCrop(224),
    transforms.ToTensor(),
    transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])
]

# 使用Compose将它们组合成一个变换
composed_transforms = transforms.Compose(transform_list)

# 现在你可以像使用一个普通变换一样使用它
# 假设img是你的PIL图像
img_transformed = composed_transforms(img)
```

在这个例子中，`Compose` 接受一个包含多个变换的列表，并且当你调用 `composed_transforms` 对象时，它会按照列表中的顺序依次应用这些变换。

在你提供的代码中，`Compose` 类也是用来实现同样的功能，但是它是自定义实现的。自定义的 `Compose` 类可以应用于同时需要对图像和目标（例如掩码或标签）进行变换的场景。在 PyTorch 的官方实现中，`Compose` 类通常只处理图像变换，而不涉及目标变换。自定义的 `Compose` 类通过接受图像和目标作为一对输入，并确保每个变换都同时应用于图像和目标，从而扩展了这一功能。

自定义 `Compose` 类的实现如下：

```python
class Compose(object):
    def __init__(self, transforms):
        self.transforms = transforms

    def __call__(self, image, target):
        for t in self.transforms:
            image, target = t(image, target)
        return image, target
```

在这个自定义实现中，`__call__` 方法接受一对图像和目标，然后逐个应用组合中的每个变换，最后返回变换后的图像和目标。这种方式特别适用于需要对图像及其对应的标注或掩码同时进行相同预处理的场景。



## call

在Python中，`__call__` 是一个特殊的方法，也称为魔术方法或双下方法。当它在类中被定义时，它允许一个类的实例表现得像一个函数，即可以通过直接调用实例来执行`__call__`方法。这通常用于实现函数式编程的某些特性，或者使得对象可以像函数那样被调用。

在PyTorch的`transforms`模块中，`__call__`方法被广泛使用，以实现图像预处理和增强操作的链式调用。具体来说，`__call__`方法在以下场景中发挥作用：

1. **使对象可调用**：
   - 当你有一个类的实例，并且希望像调用函数那样使用它时，你可以在类中定义`__call__`方法。这样，你就可以直接调用实例，而不是显式地调用其方法。

2. **参数传递**：
   - 在`transforms`中，`__call__`方法允许你传递图像和目标（如掩码或标注）作为参数，并在内部执行一系列预处理操作。

3. **链式操作**：
   - 通过在`__call__`方法中处理图像和目标，并将处理后的图像和目标返回，你可以将多个预处理操作链接在一起。这使得数据预处理流程变得简洁和高效。

### 示例

以下是一个简单的示例，说明如何在自定义类中使用`__call__`方法：

```python
class Greeter:
    def __init__(self, name):
        self.name = name

    def __call__(self):
        print(f"Hello, {self.name}!")

# 创建Greeter实例
greeter = Greeter("Kimi")

# 像调用函数一样调用实例
greeter()  # 输出: Hello, Kimi!
```

在这个示例中，`Greeter`类有一个`__call__`方法，使得其实例可以像函数一样被调用。当你调用`greeter()`时，实际上是在调用`greeter`实例的`__call__`方法。

### 在PyTorch中的使用

在PyTorch的`transforms`模块中，`__call__`方法的使用方式如下：

```python
class RandomHorizontalFlip:
    def __init__(self, flip_prob):
        self.flip_prob = flip_prob

    def __call__(self, image, target):
        if random.random() < self.flip_prob:
            image = F.hflip(image)
            target = F.hflip(target)
        return image, target

# 创建RandomHorizontalFlip实例
flip_transform = RandomHorizontalFlip(0.5)

# 调用实例进行图像翻转
image, target = flip_transform(image, target)
```

在这个例子中，`RandomHorizontalFlip`类的实例可以像函数一样被调用，接受图像和目标作为参数，并根据定义的概率进行水平翻转。这种方式使得预处理操作的组合变得非常灵活和方便。
