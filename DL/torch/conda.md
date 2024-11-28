[toc]

## conda

cmd里面到anaconda prompt

```
activate
```

进来就是base环境

### 删除环境

```
conda remove -n 需要删除的环境名 --all
```

### 创建环境

创建一个python3.7.6的环境，环境名称为wuenda

```
conda create -n wuenda python=3.7.6
```

### 切换环境

如切换到wuenda环境

```
activate wuenda
```

退出wuenda环境

```
deactivate
```

### 列出所有环境

```
conda env list
```

### 回滚

首先列出

```
conda list --revisions
```

然后回滚

```
conda install --revision N
```

其中N是修订号

### 查看包

```
conda list
```

 即可查看anaconda当前环境下的所有包

### 安装库

[使用Anaconda、Pip、Anaconda Navigator安装第三方库 - 玩转大数据 - 博客园 (cnblogs.com)](https://www.cnblogs.com/sx66/p/17824382.html#:~:text=Anaconda Navigator是一个图形界面，可以用于管理Anaconda环境和安装第三方库。 它提供了一个用户友好的界面，可以轻松地安装和管理库。 要安装一个库，我们可以按照以下步骤： 1.打开Anaconda,Navigator。 2.选择Environments选项卡。 3.在左侧的面板中选择要安装库的环境。 4.在右侧的面板中，单击底部的“Install”按钮。 5.在搜索框中输入要安装的库的名称。)

```
conda install 库名
```

或者在anaconda navigator里面对应的环境下的not installed里面搜

### 克隆环境

#### 用conda环境名克隆

假设已有环境名为A，需要生成的环境名为B：

```bash
conda create -n B --clone A
```

理解为如果是conda命令下的包，则直接迁移，pip命令下载的包则要重新下

略慢，有时候克隆不了

#### 用conda环境路径克隆

比如环境路径为`D:\A`，想要创建一个名为B的新环境，可以使用以下命令：

```bash
conda create -n B --clone D:\A
```

理解为直接复制过来



Linux下面先查看环境路径

```bash
conda config --show envs_dirs
```

得到如下：


```
(base) yjy@dell-Precision-5820-Tower-X-Series:~/yjy/envs$ conda config --show envs_dirs
envs_dirs:
  - /mnt/3bb01a96-4d48-4134-9dcc-1e1cdd11daa3/yjy/envs
  - /home/yjy/.conda/envs
  - /usr/local/share/anaconda/envs
```

ls查看当前路径下是否是conda环境

```bash
ls /mnt/3bb01a96-4d48-4134-9dcc-1e1cdd11daa3/yjy/envs/yjy
```

发现是，则：

```bash
conda create -n unet --clone /mnt/3bb01a96-4d48-4134-9dcc-1e1cdd11daa3/yjy/envs/yjy
```





## jupyter

### 切换环境

切换到想要加到jupyter的那个环境，如wuenda

```
conda activate wuenda    # this is the environment for your project and code
conda install ipykernel
conda deactivate
```

然后在jupyter右上角选就是了

### 查用法

```python
import numpy as np
np?
```

1个问号得里面函数

```python
import numpy as np
np??
```

2个问号得源码

## 语法

李沐：chapter_preliminaries

## cuda

```bash
CUDA_VISIBLE_DEVICES=0 python train.py --dataset Synapse --vit_name R50-ViT-B_16
```

`CUDA_VISIBLE_DEVICES=0`是一个环境变量，用于指定哪些GPU对当前运行的进程可见。在深度学习训练中，这允许你控制模型应该在哪个GPU上运行。当你设置`CUDA_VISIBLE_DEVICES=0`时，只有编号为0的GPU（通常称为GPU 0）对当前进程可见。

这个环境变量不是在Python代码内部直接设置的，而是在命令行中设置的，在你运行`train.py`脚本之前。
