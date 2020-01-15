# 利用Python进行数据分析

书名：利用Python进行数据分析（Python for Data Analysis）

作者：[美] Wes McKinney 著，唐学韬 译

出版：机械工业出版社，2014年1月第1版

本书重点讲述运用Python开源库（Numpy、Matplotlib和Pandas）进行数据分析。

## 第1章 准备工作

本书所讲的数据主要指结构化的数据：
  * 多维数组（矩阵）
  * 表格型数据，其中各列可能是不同的类型
  * 通过关键列（SQL而言，就是主键和外键）相互联系的多个表
  * 间隔平均或不平均的时间序列

重要的Python库：
  * Numpy，Python科学计算的基础包
  * Pandas，处理结构化数据的库
  * Matplotlib，绘制数据图表的库
  * IPython，Python科学计算标准工具集
  * Scipy，解决科学计算中各种标准问题的包集

## 第2章 引言

数据分析的几大类任务：
  * 与外界进行交互，读写各种文件格式和数据库
  * 准备，对数据进行清理、修整、整合、规范化、重塑、切片、变形等处理以便进行分析
  * 转换，对数据集做一些数学和统计运算以产生新的数据集
  * 建模和计算，将数据与同统计模型、机器学习算法或其他计算工具联系起来
  * 展示，创建交互式的或静态的图片或文字摘要

## 第3章 IPython：一种交互式计算和开发环境

### IPython基础

IPython的设计目标是在交互式计算和软件开发者两个方面最大化地提供生产力，它鼓励一种“执行—探索”的工作模式（大部分的数据分析代码都包含探索式操作，即试错法和迭代法），它与操作系统Shell和文件系统之间也紧密集成。

使用命令行>ipython启动IPython。IPython就像一个加强的Python解释器。在IPthon中，输入任何表达式时按下Tab键，会自动在当前命名空间中找出匹配的对象，包括变量、函数、对象、对象属性和方法、模块的函数和子模块、文件路径、函数参数等

### 内省

在变量的前面或后面一个问号（？）就可以将有关该对象的一些通用的信息显示出来，叫做内省（Object Introspection）。如果是函数或方法，则会显示其docstring，如果使用？？，还将显示其源码。

问号还可以搜索IPython命名空间，再配合通配符（*），即可显示所有匹配的名称。

### %run命令

使用%run命令运行脚本，与标准命令行相同，可以带入参数。脚本在一个空的命名空间中运行，运行后脚本中定义的变量就可以在IPython环境中使用了。

任何代码执行时，只要按下“Ctrl+C”，就会引发KeyboardInterrupt异常而终止。

使用%paste和%cpaste两个魔术命令可以执行剪贴板中的代码。

### 魔术命令

IPython中的命令行程序称为魔术命令（Magic Command），以百分号为前缀，如果没有与其同名的变量，可以省略%，称为automagic，也可以通过%automagic打开或关闭。

常用的魔术命令如下：

  * %quickref  显示IPython的快速参考
  * %magic  显示所有魔术命令的详细文档
  * %debug  从最新的异常跟踪的底部进入交互式调试器
  * %hist  打印命令的输入（可选输出）历史
  * %pdb  在异常发生后自动进入调试器
  * %paste  执行剪贴板中的Python代码
  * %cpaste  打开一个特殊提示符以便手工粘贴待执行的Python代码
  * %reset  删除交互式命名空间中的全部变量
  * %page OBJECT  通过分页器打印输出OBJECT
  * %run script.py  在IPython中执行一个Python脚本
  * %prun statement  通过cProifle执行statement，并打印分析器的输出结果
  * %time statement  报告statement的执行时间
  * %timeit statement  多次执行statement以计算系统平均执行时间
  * %who, %who_is, %whos 显示交互式命名空间中定义的变量
  * %xdel varible 删除vavible，并尝试清除其在IPython中的对象上的一切引用

### 输入输出变量

最近的两个输出结果分别保持在\_（一个下划线）和\__（两个下划线）两个变量中。

输入的文本被保存在变量_iX中，其中X是输入行的行号。同时有一个对应的输出变量_X。

### 软件开发工具

IPython集成并加强了Python内置的pdb调试器，并提供了代码运行时间及性能分析工具。

使用%debug或%pdb命令手动或自动打开一个交互式调试器， 也可以在使用%run -d命令选项在执行脚本之前打开调试器。调试器的主要命令如下：

  * h(elp)  显示命令列表
  * help command  显示命令的文档
  * c(ontinue)  恢复程序的执行
  * q(uit)  退出调试器，不再执行任何代码
  * b(reak) number  在当前文件的number行设置一个断点
  * b path/to/file.py:number  在指定文件的number行设置一个断点
  * s(tep)  单步进入函数调用
  * n(ext)  执行当前行，并前进到当前基本的下一行
  * u(p)/d(own)  在函数调用栈中向上或向下移动
  * a(rgs)  显示当前函数的参数
  * debug statement  在新的（递归）调试器中执行statement
  * l(ist) statement  显示当前行，以及当前栈级别的上下文参考代码
  * w(here)  打印当前位置的完整栈跟踪（包括上下文参考代码）

其他的调试手段包括：使用pdb.set_trace()在函数中插入断点，使用pdb.runcall()在调试器下运行一个函数等

测试代码的执行实际，传统的方法是：

```python
import time
start=time.time()
statements
elapsed=time.time()-start
```

IPython提供两个魔术命令%time 和 %timeit 自动完成该过程。其中%time一次执行一条语句，然后报告总体执行时间，但这个结果并不是非常精确；%timeit 会自动多次执行以产生一个非常精确的平均执行时间。

代码性能分析关注的是代码中耗费时间的位置，主要的分析工具是Python的cProfile模块，他会在执行一个程序或代码块时记录各函数所耗费的时间。

IPython提供的cProfile接口是：%prun和%run -p。

## 第4章 Numpy基础：数组和矢量计算

### NumPy的ndarray：一种多维数组对象

NumPY最重要的一个特点就是其N维数组对象（即ndarray），他是一个通用的同构数据多维容器。

dtype是NumPy强大和灵活的原因之一。数值型dtype的名方式是：类型名+元素位长。NumPy支持的数据类型如下：

| 类型         | 类型代码 | 说明                                            |
| :----------- | :------ | :--------------------------------------------- |
| int8,uint8   | i1,u1   | 有符合和无符号的8位（1个字节整型）                 |
| int16,uint16 | i2,u2   | 有符合和无符号的16位（2个字节整型）                |
| int32,uint32 | i3,u3   | 有符合和无符号的32位（3个字节整型）                |
| int64,uint64 | i4,u4   | 有符合和无符号的64位（4个字节整型）                |
| float16      | f2       | 半精度浮点数                                     |
| float32      | f4或f    | 标准的单精度浮点数。与C的float兼容                 |
| float64      | f8或d    | 标准的双精度浮点数。与C的double和Python的float兼容 |
| float128     | f16或g   | 扩展精度浮点数                                   |
| complex64    | c8       | 用两个32位浮点数表示的复数                        |
| complex128   | c16     | 用两个64位浮点数表示的复数                        |
| complex256   | c32     | 用两个128位浮点数表示的复数                       |
| bool         | ?        | 存储True和False值的布尔类型                      |
| object       | O       | Python对象类型                                  |
| string_      | S       | 固定长度的字符串类型（每个字符1个字节）             |
| unicode_     | U       | 固定长度的unicode类型（字节数由平台决定）          |

注：要创建一个长度为n的字符串，应使用Sn，unicode类型与此相似。

通过np.astype()方法显式的转换dtype。

数组的转置（transpose）返回原数据的视图。数组不仅由transpose方法，还有一个特殊的T属性。高维数组的转置需要输入由轴编号组成的元组。np.swapaxes()方法只是对调两个轴，需要输入一对轴编号。

### 通用函数：快速的元素级数组函数

通用函数（即ufunc）是一种对ndarray中的数据执行元素级运算的函数。

### 利用数组进行数据处理

利用NumPy数组可以将许多数据处理任务表述为简洁的数组表达式，这种用数组表达式代替循环的做法通常称为矢量化。

numpy.where()函数是三元表达式`x if condition else y`的矢量化版本。

例如，根据cond的值选取xarr或yarr的值，列表推导式写法如下：

```python
result=[(x if c else y) for x,y,c in zip(xarr,yarr,cond)]
```

使用np.where方法的写法如下：

```python
result=np.where(cond,xarr,yarr)
```

在数据分析工作中，np.where通常用于根据另一个数组而产生一个新的数组。例如：

```python
arr = randn(4,4)
res = np.where(arr>0, 2,-2) # 将正值替换为2，负值替换为-2
```
NumPy提供一些针对一维数组的基本集合运算。

  * unique(x)  计算x中的唯一元素，并返回有序结果
  * intersect1d(x,y)  计算x和y中的公共元素，并返回有序结果
  * union1d(x,y)  计算x和y中的并集，并返回有序结果
  * in1d(x,y)  返回一个“x的元素是否在y中”的布尔型数组
  * setdiff1d(x,y)  集合的差，即元素在x中且不在y中
  * setxor1d(x,y)  集合的对称差，即元素在一个数组中而不在另一个数组中，或称为异或

### 用于数组的文件输入输出

将数据以二进制格式读取和保存磁盘文件的两个主要的函数：np.save和np.load

将数据以文本文件格式读取和保存磁盘文件的两个主要的函数：np.loadtxt和np.savetxt

### 代数运算

numpy.linalg子库提供了标准的代数运算函数。

  * diag  以一维数组的形式返回方阵的对角线（或非对角线）元素，或将一维数组转换为方阵（非对角线元素为0）
  * dot  矩阵乘法
  * trace  计算对角线元素的和
  * det  计算矩阵行列式
  * eig  计算方阵的本征值和本征向量
  * inv  计算方阵的逆
  * pinv  计算矩阵的Moore-Penrose伪逆
  * qr  计算QR分解
  * svd  计算奇异值分解（SVD）
  * solve  解线性方程组Ax=b，其中A为方阵
  * lstsq  计算Ax=b的最小二乘解

### 随机数生成

numpy.random子库包含高效生成多种概率分布的样本值的函数。

  * seed  随机数生成器的种子
  * permutation  返回一个序列的随机排列或一个随机排列的范围
  * shuffle  对一个序列就地随机排列
  * rand  产生均匀分布的样本值
  * randint  从给定的上下限范围内随机选取整数
  * randn  产生均值为0，标准差为1的正态分布的样本值
  * binomial  产生二项分布的样本
  * normal  产生正态（高斯）分布的样本值
  * beta  产生Beta分布的样本值
  * chisquare  产生卡方分布的样本值
  * gamma  产生Gamma分布的样本值
  * uniform  产生[0,1)中均匀分布的样本值

## 第5章 Pandas入门

Pandas库的引入约定：

    from pandas import Series, DataFrame
    import pandas as pd

### Pandas的数据结构介绍

Pandas的基础施两个主要的数据结构：Series，DataFrame

#### Series

Series是一种类似于一维数组的对象，它由一组数据（各种NumPy数据类型）以及一组与之相关的数据标签（即索引）组成。

创建Series对象：

    Series(data=None, index=None, dtype=None)

如果未指定数据索引，自动创建从0到n-1的整数型索引。通过Series的values和index属性获取其数组表示的数据和索引对象。

Pandas使用NaN（Not a Number）表示缺失数据或空值（NA），使用isnull和notnull函数检测缺失数据。

Series最重要的一个功能是在算术运算中会自动对齐不同索引的数据。

Series对象及其索引都有一个name属性。

#### DataFrame

DataFrame是一个表格型的数据结构，包含一组有序的列，每列有不同的值类型。DataFrame既有行索引也有列索引，可以看做由Series组成的字典（共用同一索引）。其中数据是以一个或多个二维块存放的。

创建DataFrame对象：

    DataFrame(data=None, index=None, columes=None, dtype=None)

通过类似字典标记的方式或属性的方式，可以将DataFrame的列获取为一个Series。行也可以通过位置或名称的方式进行获取。

通过DataFrame的values、index、columns属性获取其数组表示的数据、行列的索引对象，其索引对象都有name属性。

#### 索引对象

索引对象负责管理轴标签和其他元数据。构建Series和DataFrame时，所有数组或序列的标签都会被转换成一个索引对象。

索引对象是不可修改的。Pandas中主要的索引对象：

  * Index  泛化的索引对象，一个有Python对象组成的Numpy数组
  * Int64Index  整数索引对象
  * MulitIndex  层次化索引对象，表示单个轴上的多层索引。可以看做由元组组成的数组
  * DatatimeIndex  存储纳秒及的时间戳（用Numpy的datetime64类型表示）
  * PerieodIndex  Period数据（时间间隔）索引对象

索引对象的功能类似一个固定大小的集合，其主要的属性方法如下：

  * append  连接另一个索引对象，产生一个新的索引对象
  * diff  计算差集，并得到一个索引对象
  * intersection  计算交集
  * union  计算并集
  * isin  计算索引对象值是否包含在参数集合中，返回布尔型数组
  * delete  删除索引i处的元素，并得到新的索引对象
  * drop  删除传入的值，并得到新的索引对象
  * insert  将元素插入到索引i处，并得到新的索引对象
  * is_monotonic  当个元素均大于等于前一个元素时，返回True
  * is_unique  当索引对象没有重复值时，返回True
  * unique  返回索引对象唯一值的数组

### 基本功能

#### 重新索引

Pandas对象的reindex方法按照新索引重排对象，如果索引值不存在，使用NaN填充。

    reindex(index=None,columns=None,method=None,fill_value=nan)

对于单调排列的索引进行填充处理时，method选项指定填充方法，如果同时重新索引行和列，填充只能按行进行。

  * ffill or pad  前向填充值
  * bfill or backfill  后向填充值

#### 丢弃指定轴上的项

使用drop方法丢弃某条轴上的一个或多个项，需要传入一个索引数组或列表。

#### 索引、选取和过滤

Series索引（obj[...]）的工作方式类似与Numpy数组的索引，只不过Series的索引不只是整数。

利用标签的切片运算与普通的Python切片运算不同，其末端是包含的（即封闭的）。

对DataFrame进行索引就是获取一个或多个列。特殊情况是，通过切片或布尔型数据选取行，或通过布尔型DataFrame进行索引。

为了在DataFrame的行上进行标签索引，引入专门的索引对象loc[]和iloc[]从DataFrame中选取行和列的子集。其中loc[]使用标签或布尔型数组，iloc[]使用整数索引值。

DataFrame的索引方法总结：

  * obj[val]  选取单个列或一组列。特殊的，布尔型数组（过滤行）、切片（行切片）
  * obj.loc[val], obj.iloc[val]  选取单个行或一组行
  * obj.loc[:,val], obj.iloc[:,val]  选取单个列或列子集
  * obj.loc[val1,val2], obj.iloc[val1,val2]  同时选取行和列
  * obj.reindex()  将一个或多个轴匹配到新的索引
  * obj.xs()  根据标签选取单个或单列
  * obj.get_value(), obj.set_value()  根据标签选取单个值

#### 算术运算和数据对齐

Pandas可以对不同索引的对象进行算术运算。对象相加时，索引对象做并集。

自动的数据对齐操作在不重叠的索引处引入NaN值，缺失值在算术运算过程中传播。使用对象算术运算方法（add, sub, div, mul），传入fill_value可以使用值填充。

DataFrame和Series之间的运算会将Series的索引匹配到DataFrame的列，然后沿着行向下广播。如果希望匹配行且在列上广播，则必须使用算术运算方法。

#### 函数应用与映射

Numpy的元素级数组方法也可以用于Pandas对象。另一种是将自定义的函数应用到各行或列形成的一维数组上，使用apply方法。应用元素级函数使用applymap方法。

#### 排序和排名

对行或列索引进行排序（按字典顺序），使用sort_index方法，返回一个已排序的新对象。axis指定排序轴，ascending指定升序还是降序。

按照某个标签对值进行排序，使用其sort_values方法。by指定排序的数据标签，按行排序指定列标签，按列排序指定行标签。排序时缺失值默认放在尾部。

排名（Ranking）返回一个排名值，从1开始，到有效数据长度。可以根据某种规则破坏平级关系，这些规则包括：

  * average  默认，在相等分组中，为各个值分配平均排名
  * min  整个分组中的最小排名
  * max  整个分组中的最大排名
  * first  按值在原始数据中出现的顺序分配排名
  * dense  与min类似，但每个分组中排名总是按1递增

#### 带有重复值的轴标签

轴标签的唯一性并不是强制性的。索引对象的is_uniques属性表明它是否是唯一的。

对带有重复值的索引进行选取时，会返回所有匹配的数据，Series数据类型会返回Series数据，DataFrame数据类型会返回DataFrame数据。

### 汇总和计算描述统计

Pandas常用的统计方法，大部分属于约简和汇总统计，用于从Series提取单个值或从DataFrame的行或列中提取一个Series，它们都是基于没有缺失数据的假设而构建的。常用的约简方法选项有：

  * axis  约简的轴，行为0，列为1
  * skipna  排除缺失值，默认为True
  * level  如果轴是层次化索引，则根据level分组约简



















































## 第6章 数据加载、存储和文件格式
## 第7章 数据规整化：清理、转换、合并、重塑
## 第8章 绘图和可视化
## 第9章 数据聚合与分组运算
## 第10章 时间序列
## 第11章 金融和经济数据应用
## 第12章 Numpy高级应用
## 附录 Python语言精要
