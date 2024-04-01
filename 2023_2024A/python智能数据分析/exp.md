## 实验流程设计（数据集的选择和分析，分步骤描述实验过程）

### 数据集的选择

选择2022 年高教社杯全国大学生数学建模竞赛C题古代玻璃制品的成分分析与鉴别的数据集，因为该次数模竞赛题目本身就要求对数据集进行聚类/分类/回归等操作。

### 数据集的预处理

数据集给出了各种玻璃制品相应的主要成分所占比例（比例为0表示未检测到该成分）。这些数据的特点是成分性，即各成分比例的累加和应为 100%，但因检测手段等原因可能导致其成分比例的累加和非 100%的情况。本题中将成分比例累加和介于 85%~105%之间的数据视为有效数据。

所以预处理应该将成分比例不在85%~105%之间的数据剔除，如图中红框所示，有的检测成分明显异常，大于105%，就应在数据集预处理阶段剔除：

![P1](D:\Typora\notes\2023_2024A\python智能数据分析\pics\P1.png) 

此处选择整行删除。

### 对玻璃制品的化学物质含量求均值和方差

由于有高钾和铅钡两种不同类型的玻璃，所以首先按玻璃类型对数据集进行划分。之后对表格第3列之后的每一列进行求均值和方差的操作。

### 对未知类型的玻璃制品进行种类预测

对于另一个数据集，其中有8个未知类型的玻璃制品，使用sklearn库中的5种常用机器学习模型：RandomForest，svm，GaussianNB，tree，MLPClassifier对这8个制品进行分类。

具体操作概括如下：

1. 取已知玻璃类型表格中的玻璃制品的各种化学物质含量的数据为x1

2. 取已知玻璃类型表格中的玻璃制品的的类型数据为y1

3. 拆分训练集和测试集

4. ```python
   x_train, x_test, y_train, y_test = train_test_split(x1, y1, test_size=0.3, random_state=12)
   ```

5. 训练模型

6. ```python
   Randomforest_model = RandomForestClassifier()
   Randomforest_model.fit(x_train, y_train)
   ```

   

7. 取未知玻璃类型表格中的玻璃制品的各种化学物质含量的数据为x2

8. 对x2数据进行类型预测

9. ```python
   print("随机森林分类预测：{}\n\n\n".format(Randomforest_model.predict(x2)))
   ```



### 打印出训练模型的评估质量效果

调用如下函数打印出模型的评估报告

```python
print(classification_report(Randomforest_model.predict(x_test), y_test))
```

### 对训练的结果进行绘图展示

使用seaborn对使用模型预测出的8个未知类型的玻璃制品的类型结果进行柱状图绘制。

## 算法和代码编写

### 数据集的预处理

读入原始数据集data_unpreprocessed.xlsx，跳过表头，对数据集每行的第3列起的数据进行求和，判断是否各种化学成分含量之和在85%和105%之间，不满足则删去该行。最后存放预处理后的数据到data_preprocessed.xlsx中。

data_unpreprocessed.xlsx：

![P3](pics\P3.png) 

代码：

```python
import pandas as pd

# 读取 Excel 文件
df = pd.read_excel('data_unpreprocessed.xlsx')
# 逐行遍历 DataFrame，判断从第三列开始的数据之和是否满足条件，如果不满足，则删除该行
for index, row in df.iterrows():
    if index == 0: # 跳过表头
        continue
    if (row.iloc[2:].sum() > 105) or (row.iloc[2:].sum() < 85):
        df = df.drop(index)

df.to_excel('data_preprocessed.xlsx', index=False)
```

### 对数据集按照玻璃制品类型进行划分

按高钾、铅钡两种类型对数据集进行划分并分别存入新的表格中。

```python
import pandas as pd

# 读取 Excel 文件
df = pd.read_excel('data_preprocessed.xlsx')

# 根据类型列筛选出符合条件的行
high_potassium = df[df['类型'] == '高钾']
lead_barium = df[df['类型'] == '铅钡']

# 将符合条件的行分别保存到两个新的 Excel 文件中
high_potassium.to_excel('data_high_potassium.xlsx', index=False)
lead_barium.to_excel('data_lead_barium.xlsx', index=False)

```

### 对玻璃制品的化学物质含量求均值和方差

读入原始数据集data_unpreprocessed.xlsx，从第3列开始对每一列的数据求均值和方差。

```python
import pandas as pd

# 读取 Excel 文件
df = pd.read_excel('data_high_potassium.xlsx')

print("高钾：")
# 从第三列开始对每一列的数据求均值和方差
for column in df.columns[2:]:
    mean_value = df[column].mean()
    var_value = df[column].var()
    print(f"'{column}': 均值 = {round(mean_value, 2)},方差 = {round(var_value, 2)}")
# 从第三列开始对每一列的数据求均值和方差
mean_values = df.iloc[:, 2:].mean()
variance_values = df.iloc[:, 2:].var()

# 将均值和方差添加到数据框中作为新的行
df = df.append(mean_values, ignore_index=True)
df = df.append(variance_values, ignore_index=True)

df.to_excel('data_high_potassium_with_mean_and_variance.xlsx', index=False)
print("\n\n\n")

# 铅钡
df = pd.read_excel('data_lead_barium.xlsx')
print("铅钡：")

for column in df.columns[2:]:
    mean_value = df[column].mean()
    var_value = df[column].var()
    print(f"'{column}': 均值 = {round(mean_value, 2)},方差 = {round(var_value, 2)}")

mean_values = df.iloc[:, 2:].mean()
variance_values = df.iloc[:, 2:].var()

df = df.append(mean_values, ignore_index=True)
df = df.append(variance_values, ignore_index=True)

df.to_excel('data_lead_barium_with_mean_and_variance.xlsx', index=False)
```

### 对未知类型的玻璃制品进行种类预测并打印训练结果

调用sklearn库中函数进行机器学习分类，并打印结果。

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.metrics import classification_report
from sklearn.ensemble import RandomForestClassifier
from sklearn import svm
from sklearn.naive_bayes import GaussianNB
from sklearn import tree
from sklearn.neural_network import MLPClassifier

# x 用以预测的数据
x_headers = ['是否风化', '二氧化硅(SiO2)', '氧化钠(Na2O)', '氧化钾(K2O)', '氧化钙(CaO)', '氧化镁(MgO)', '氧化铝(Al2O3)', '氧化铁(Fe2O3)',
             '氧化铜(CuO)', '氧化铅(PbO)', '氧化钡(BaO)', '五氧化二磷(P2O5)', '氧化锶(SrO)', '氧化锡(SnO2)', '二氧化硫(SO2)']
# y 预测的目标
y_header = ['类型']
# 读入数据data1
data1 = pd.read_excel('data_preprocessed.xlsx')

x1 = data1[x_headers]
y1 = data1[y_header]

# 拆分训练集和测试集
x_train, x_test, y_train, y_test = train_test_split(x1, y1, test_size=0.3, random_state=12)

# 读入数据data2
data2 = pd.read_excel('data_forPredict.xlsx')

x2 = data2[x_headers]

# Randomforest分类

Randomforest_model = RandomForestClassifier()

Randomforest_model.fit(x_train, y_train)
# 分类效果报告
print("Randomforest分类报告：")
print(classification_report(Randomforest_model.predict(x_test), y_test))

print("随机森林分类预测：{}\n\n\n".format(Randomforest_model.predict(x2)))

# SVC 支持向量机分类

SVC_model = svm.SVC()
SVC_model.fit(x_train, y_train)
# 分类效果报告
print("SVC 支持向量机分类报告：")
print(classification_report(SVC_model.predict(x_test), y_test))

print("SVM分类预测：{}\n\n\n".format(SVC_model.predict(x2)))

# 朴素贝叶斯分类

GaussianNB_model = GaussianNB()
GaussianNB_model.fit(x_train, y_train)
# 分类效果报告
print("朴素贝叶斯分类报告：")
print(classification_report(GaussianNB_model.predict(x_test), y_test))

print("朴素贝叶斯分类预测：{}\n\n\n".format(GaussianNB_model.predict(x2)))

# 决策树分类

tree_model = tree.DecisionTreeClassifier()
tree_model.fit(x_train, y_train)
# 分类效果报告
print("决策树分类报告：")
print(classification_report(tree_model.predict(x_test), y_test))

print("决策树分类预测：{}\n\n\n".format(tree_model.predict(x2)))

# BP神经网络分类
BP_model = MLPClassifier(solver='sgd', activation='relu', max_iter=500, alpha=1e-3, hidden_layer_sizes=(32, 32),
                   random_state=1)
BP_model.fit(x_train, y_train)
# 分类效果报告
print("BP神经网络分类报告：")
print(classification_report(BP_model.predict(x_test), y_test))

print("BP神经网络分类预测：{}\n\n\n".format(BP_model.predict(x2)))
```

### 对训练的结果进行绘图展示

使用seaborn绘制结果柱状图：

```python
import seaborn as sns
import pandas as pd
from matplotlib import pyplot as plt

data = pd.read_excel('data_result.xlsx')
data.head()
rc = {'font.sans-serif': 'SimHei',
      'axes.unicode_minus': False}
sns.set(context='notebook', style='ticks', rc=rc)
sns.barplot(x="类型", y="数量", data=data)
plt.show()
```

## 实验结果

### 数据集预处理

如图，各种化学成分之和不在85%和105%的行已被删去（比如之前有的化学成分含量为999，已经被预处理过滤了）。

![P4](pics\P4.png) 

### 对玻璃制品的化学物质含量求均值和方差

如图，打印出的高钾类玻璃制品的化学物质含量的均值和方差如下：

![P5](pics\P5.png) 

铅钡类玻璃制品的化学物质含量的均值和方差如下：

![P5](pics\P6.png) 

可以看出两种玻璃制品普遍二氧化硅含量高，相对来说高钾类氧化钾和氧化钙含量较高，而铅钡类氧化铅和氧化钡含量较高。

### 对未知类型的玻璃制品进行种类预测并打印训练结果

原未知类型的玻璃制品的化学成分含量如图：

![P5](pics\P7.png) 

用随机森林模型训练后打印出分类报告和预测结果如下：

![P5](pics\P8.png) 

#### precision

$precision = TP / ( TP + FP )$

precision(精确率)是精确性的指标，表示被分类器正确分为正例的个数(TP)占被分类器分为正例的样本(TP+FP)的比重。

此处高钾和铅钡precision = 1.00说明高钾类和铅钡类被分类器分为正例的样本全部正确，说明精准性高。

#### recall

$recall = TP / ( TP + FN ） = TP / P$

recall(召回率)是覆盖面的度量，也就是被分类器正确分为正例的个数(TP)占原始数据中全部正例(TP+FN)的比重。

此处recall = 1.00说明模型分类准确。

#### f1-score

$\frac{1}{F_1} = \frac{1}{2}\cdot(\frac{1}{P}+\frac{1}{R})$   	(P=precisioin,R=recall)

当P和R都很高的时候，F1才会高，F1的取值范围是0到1，此处F1为1说明模型分类准确。

#### support

支持度，是指原始的真实数据中属于该类的个数

#### accuracy

**accuracy**

  准确率，指正确分类（不管是正确分为P还是N）的比率

 $ accuray = （TP + TN) / (TP + FP + TN + FN ) = (TP + TN) / (P+N)$

### 其它模型分类结果

其它模型分类结果与随机森林模型报告同理：



SVC 支持向量机分类报告：

![P5](pics\P9.png) 

朴素贝叶斯分类报告：

![P5](pics\P10.png) 

决策树分类报告：

![P5](pics\P11.png) 

BP神经网络分类报告

![P5](pics\P12.png) 



容易得出8种未知种类的玻璃制品预测为：

['高钾' '铅钡' '铅钡' '铅钡' '铅钡' '高钾' '高钾' '铅钡']

### 绘图结果

用seaborn绘制结果如下：

预测高钾类型的玻璃制品为3个，铅钡类型的玻璃制品为5个。

![P5](pics\P17.png) 

## 实验分析和总结

结果分析：

1. 通过计算已知类型的玻璃制品的化学成分含量的均值和方差，可以看出两种玻璃制品普遍二氧化硅含量高，相对来说高钾类氧化钾和氧化钙含量较高，而铅钡类氧化铅和氧化钡含量较高。
2. 使用sklearn库中的5种常用机器学习模型：RandomForest，svm，GaussianNB，tree，MLPClassifier对这8个制品进行分类，其中只有朴素贝叶斯分类的recall和f1-score指数没有达到1，其余全部达到了1，说明模型预测准确性高。预测结果为['高钾' '铅钡' '铅钡' '铅钡' '铅钡' '高钾' '高钾' '铅钡']。

问题总结：

​	   实验初期不知道如何分析调用classification_report()得出的分析报告，通过复习老师上课内容和网上查阅资料掌握了precision，recall，f1-score   support，accuracy等数据对应的意义。

未来展望：

​	   希望在未来的学习过程中熟练掌握sklearn库中更多的内容。









