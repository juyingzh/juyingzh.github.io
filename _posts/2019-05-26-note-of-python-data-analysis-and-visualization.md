---
layout: post
title: Python数据分析与展示学习笔记
categories: [学习笔记, 数据分析]
tags: [Python, 数据分析, 数据展示]
---


* Content
{:toc}

2019年4月25日，在中国大学MOOC平台上开始学习北京理工大学的网络公开课：[Python数据分析与展示](https://www.icourse163.org/course/BIT-1001870002)。本课程是“Python网络爬虫与数据分析”课程的下半部分。

我们正步入一个数据或许比软件更重要的新时代。运用数据是精准刻画事物、呈现发展规律的主要手段，分析数据展示规律，把思想变得更精细。

数据分析与展示就是理解一组数据所表达的含义。课程讲解利用Python语言进行数据表示、统计和数据的技术方法。其中涉及三个优秀的第三方库：NumPy，用于N维数据的表达及科学计算；Matplotlib，用于绘制坐标系、散点图、极坐标图等直观展示数据趋势和特点；Pandas，应运其Series和DataFrame数据类型进行数据分析和处理。



## 数据分析之表示

### NumPy库入门

#### 数据的维度

数据的维度是一组数据的组织形式。

一维数据由对等关系的有序或无序数据构成，采用线性方式组织，对应列表、数组和集合等概念。其中，列表和数组都是一组数据的有序结构，但列表的数据类型可以不同，数组的数据类型是相同的。Python语言没有数组类型，使用数组类型需要引入Numpy库。

二维数据由多个一维数据构成，是一维数据的组合形式。多维数据由一维或二维数据在新维度上扩展形成。Python语言中使用二维或多维列表表示。

维数据仅利用最基本的二元关系展示数据间的复杂结构。Python语言中使用字典或其他数据表示格式（JSON、XML和YAML格式）。

#### Numpy的N维数组对象

[NumPy](https://www.numpy.org/) 是一个开源的Python科学计算基础库。NumPy是SciPy、Pandas等数据处理或科学计算库的基础。Numpy库约定的引入方式为：

```
importnumpy as np
```
Numpy库的核心是其强大的多维数组对象：ndarray，他由两部分构成：实际的数据和描述这些数据的元数据（数据维度、数据类型等）。ndarray数组一般要求所有元素类型相同（同质），数组下标从0开始。

ndarray数组的两个基本概念：轴(axis)表示数据的维度，秩(rank)表示轴的数量。

ndarray数组对象的基本属性包括：

  * .ndim  秩，即轴的数量或维度的数量
  * .shape  ndarray对象的尺度，对于矩阵，n行m列
  * .size  ndarray对象元素的个数，相当于.shape中`n*m`的值
  * .dtype  ndarray对象的元素类型
  * .itemsize  ndarray对象中每个元素的大小，以字节为单位

ndarray数组元素的主要数据类型包括：

  * bool 布尔类型，True或False
  * intc 与C语言中的int类型一致，一般是int32或int64
  * intp 用于索引的整数，与C语言中ssize_t一致，int32或int64
  * int8 字节长度的整数，取值：[‐128, 127]
  * int16 16位长度的整数，取值：[‐32768, 32767]
  * int32 32位长度的整数，取值：[‐2^31, 2^31‐1]
  * int64 64位长度的整数，取值：[‐2^63, 2^63‐1]
  * uint8 8位无符号整数，取值：[0, 255]
  * uint16 16位无符号整数，取值：[0, 65535]
  * uint32 32位无符号整数，取值：[0, 2^32‐1]
  * uint64 32位无符号整数，取值：[0, 2^64‐1]
  * float16 16位半精度浮点数：1位符号位，5位指数，10位尾数，`(符号)尾数*10^指数`
  * float32 32位半精度浮点数：1位符号位，8位指数，23位尾数
  * float64 64位半精度浮点数：1位符号位，11位指数，52位尾数
  * complex64 复数类型，实部和虚部都是32位浮点数，实部(.real) + j虚部(.imag)
  * complex128 复数类型，实部和虚部都是64位浮点数

ndarray数组可以由非同质对象构成，其元素为对象类型（实际上也是同质的），对象元素无法有效发挥NumPy优势，尽量避免使用。

#### ndarray数组的创建和变换

ndarray数组的创建方法有：

  * 从Python中的列表、元组等类型创建ndarray数组
  * 使用NumPy中函数创建ndarray数组，如：arange, ones, zeros等
  * 从字节流（raw bytes）中创建ndarray数组
  * 从文件中读取特定格式，创建ndarray数组

NumPy中创建数组的函数主要有：

  * np.arange(n) 类似range()函数，返回ndarray类型，元素从0到n‐1
  * np.ones(shape) 根据shape生成一个全1数组，shape是元组类型
  * np.zeros(shape) 根据shape生成一个全0数组，shape是元组类型
  * np.full(shape,val) 根据shape生成一个数组，每个元素值都是val
  * np.eye(n) 创建一个正方的`n*n`单位矩阵，对角线为1，其余为0
  * np.ones_like(a) 根据数组a的形状生成一个全1数组
  * np.zeros_like(a) 根据数组a的形状生成一个全0数组
  * np.full_like(a,val)根据数组a的形状生成一个数组，每个元素值都是val
  * np.linspace() 根据起止数据等间距地填充数据，形成数组
  * np.concatenate() 将两个或多个数组合并成一个新的数组

ndarray数组的维度变换方法有：

  * .reshape(shape) 不改变数组元素，返回一个shape形状的数组，原数组不变
  * .resize(shape) 与.reshape()功能一致，但修改原数组
  * .swapaxes(ax1,ax2)将数组n个维度中两个维度进行调换
  * .flatten() 对数组进行降维，返回折叠后的一维数组，原数组不变

ndarray数组的类型变换：

    new_ndarray = ndarray.astype(new_type)  # 方法一定会创建新的数组（原始数据的一个拷贝），即使两个类型一致

ndarray数组向列表的转换：

    ls = ndarray.tolist()

#### ndarray数组的操作

数组的操作主要包括数组的索引和切片。索引是指获取数组中特定位置元素的过程，切片是指获取数组元素子集的过程。

一维数组的索引和切片与Python列表相似，使用[[start], end, [step]]。

多维数组的索引使用[i, j, k]，其中i, j, k分别代表各维度的索引。

多维数组的切片是在每个维度上切片，使用`:`表示选取所以元素。

#### ndarray数组的运算

数组与标量之间的运算等价与标量作用于数组的每一个元素。

NumPy的一元函数主要是对ndarray中的数据执行元素级运算的函数，主要有：

  * np.abs(x), np.fabs(x) 计算数组各元素的绝对值，其中fabs()不支持复数
  * np.sqrt(x) 计算数组各元素的平方根
  * np.square(x) 计算数组各元素的平方
  * np.log(x), np.log10(x), np.log2(x) 计算数组各元素的自然对数、10底对数和2底对数
  * np.ceil(x), np.floor(x) 计算数组各元素的ceiling值或floor值
  * np.rint(x) 计算数组各元素的四舍五入值
  * np.modf(x) 将数组各元素的小数和整数部分以两个独立数组形式返回
  * np.cos(x), np.cosh(x), np.sin(x), np.sinh(x), np.tan(x), np.tanh(x)  计算数组各元素的普通型和双曲型三角函数
  * np.exp(x) 计算数组各元素的指数值
  * np.sign(x) 计算数组各元素的符号值，1(+), 0, ‐1(‐)

NumPy的一些元素级运算的二元函数：

  * `+ ‐ * / **` 两个数组各元素进行对应运算
  * np.maximum(x,y), np.fmax(), np.minimum(x,y), np.fmin() 元素级的最大值/最小值计算，其中fmax()忽略空值（NaN），maximum()传递空值
  * np.mod(x,y) 元素级的模运算
  * np.copysign(x,y) 将数组y中各元素值的符号赋值给数组x对应元素
  * `> < >= <= == !=` 算术比较，产生布尔型数组

### NumPy数据存取与函数

#### 数据的CVS文件存取

CSV (Comma‐Separated Value, 逗号分隔值)是一种常见的文件格式，用来存储批量数据。

保存为CSV文件：`np.savetxt(fname, array, fmt='%.18e', delimiter=None)`，其中
  * fname : 文件名或文件句柄，如果文件名以.gz为扩展名，则自动使用gzip压缩格式
  * array : 存入文件的数组
  * fmt :  写入文件的格式，例如：%d %.2f %.18e
  * delimiter : 分割字符串，默认是任何空格

读取CSV文件：np.loadtxt(fname, dtype=np.float, delimiter=None，unpack=False)
  * frame : 文件、文件名或产生器，可以是.gz或.bz2的压缩文件
  * dtype : 数据类型，可选
  * delimiter : 分割字符串，默认是任何空格
  * unpack  : 如果True，读入属性将分别写入不同变量

#### 多维数据的存取

对多维数据的存储，使用数组对象方法：a.tofile(fid, sep='', format='%s')
  * fid  :  文件、字符串
  * sep :  数据分割字符串，如果是空串，写入文件为二进制
  * format : 写入数据的格式

对多维数据的读取，使用数组对象方法：np.fromfile(file, dtype=float, count=‐1, sep='')
  * file  : 文件、字符串
  * dtype : 读取的数据类型
  * count  : 读入元素个数，‐1表示读入整个文件
  * sep : 数据分割字符串，如果是空串，写入文件为二进制

a.tofile()和np.fromfile()需要配合使用，读取文件时需要知道存入文件时数组的维度和元素类型。可以通过元数据文件来存储额外信息。

#### Numpy便捷的数据存取方法

数据存储：np.save(file, array)存储一个数组，或 np.savez(file, arrays)以非压缩格式在一个文件中存储多个数组
  * file :  文件名，以.npy为扩展名，或扩展名为.npz
  * array  :  数组变量
数据读取：np.load(file)
  * file : 文件名，以.npy为扩展名，或扩展名为.npz

读取.npz文件返回一个NpzFile对象，类似一个字典对象，使用.files属性获取其数组列表。

### NumPy随机数生成函数

NumPy随机数子库提供的主要随机数函数：
  * rand(d0,d1,..,dn) 根据d0‐dn创建随机数数组，浮点数，[0,1)，均匀分布
  * randn(d0,d1,..,dn) 根据d0‐dn创建随机数数组，标准正态分布
  * randint(low[,high,shape]) 根据shape创建随机整数或整数数组，范围是[low, high)
  * seed(s) 随机数种子，s是给定的种子值
  * shuffle(x) 根据数组x的第1轴进行随机排列，改变数组x
  * permutation(x) 根据数组x的第1轴产生一个新的乱序数组，不改变数组x
  * choice(a[,size,replace,p])  从一维数组a中以概率p抽取元素，形成size形状新数组，replace表示是否可以重用元素，默认为False
  * uniform(low,high,size) 产生具有均匀分布的数组,low起始值,high结束值,size形状
  * normal(loc,scale,size) 产生具有正态分布的数组,loc均值,scale标准差,size形状
  * poisson(lam,size) 产生具有泊松分布的数组,lam随机事件发生率,size形状

### NumPy的统计函数

NumPy提供的主要统计函数：
  * sum(a, axis=None) 根据给定轴axis计算数组a相关元素之和，axis整数或元组
  * mean(a, axis=None) 根据给定轴axis计算数组a相关元素的期望，axis整数或元组
  * average(a,axis=None,weights=None) 根据给定轴axis计算数组a相关元素的加权平均值
  * std(a, axis=None) 根据给定轴axis计算数组a相关元素的标准差
  * var(a, axis=None) 根据给定轴axis计算数组a相关元素的方差
  * min(a)，max(a) 计算数组a中元素的最小值、最大值
  * argmin(a)，argmax(a) 计算数组a中元素最小值、最大值的降一维后下标
  * unravel_index(index, shape) 根据shape将一维下标index转换成多维下标
  * ptp(a) 计算数组a中元素最大值与最小值的差
  * median(a) 计算数组a中元素的中位数（中值）

axis=None 是统计函数的标配参数，如果不指定，默认对所有元素统计

### NumPy的梯度函数

梯度函数：np.gradient(f) 计算数组f中元素的梯度，当f为多维时，返回每个维度梯度。

梯度为连续值之间的变化率，即斜率。如连续三个X坐标值：a, b, c，则b的梯度是：(c‐a)/2

## 数据分析之展示

### Matplotlib库入门

[Matplotlib库](https://matplotlib.org/)是Python优秀的数据可视化第三方库。Matplotlib库由各种可视化类构成，内部结构复杂。受Matlab启发，其matplotlib.pyplot是绘制各类可视化图形的命令子库，相当于快捷方式。约定的引入方式：

    import matplotlib as mpl
    import matplotlib.pyplot as plt

#### 基本绘图函数

绘制曲线函数`plt.plot(x, y, format_string, **kwargs)`：

  * x  : X轴数据，列表或数组，可选
  * y  :Y轴数据，列表或数组
  * format_string: 控制曲线的格式字符串，可选
  * `**kwargs` : 第二组或更多(x,y,format_string)

format_string由标记字符、风格字符和颜色字符组成，`fmt = '[marker][line][color]'`，例如：`'^:k'`表示黑色的点线连接的向上三角标志线。

标记字符有：

  * '.' 点标记
  * ',' 像素标记(极小点)
  * 'o' 实心圈标记
  * 'v' 倒三角标记
  * '^' 上三角标记
  * '>' 右三角标记
  * '<' 左三角标记
  * '1' 下花三角标记
  * '2' 上花三角标记
  * '3' 左花三角标记
  * '4' 右花三角标记
  * 's' 实心方形标记
  * 'p' 实心五角标记
  * `'*'` 星形标记
  * 'h' 竖六边形标记
  * 'H' 横六边形标记
  * '+' 十字标记
  * 'x' x标记
  * 'D' 菱形标记
  * 'd' 瘦菱形标记
  * `'|'` 垂直线标记
  * `'_'` s水平线标记

风格字符有：

  * '‐' 实线
  * '‐‐' 破折线
  * '‐.' 点划线
  * ':' 虚线

颜色字符有：

  * 'b' 蓝色
  * 'g' 绿色
  * 'r' 红色 
  * 'c' 青绿色cyan 
  * 'm' 洋红色magenta
  * 'y' 黄色
  * 'k' 黑色
  * 'w' 白色

如果格式字符串只使用颜色字符，可以使用颜色全称或八进制颜色值，如'green'或'#008000'，使用[0,1]小数表示灰度值，如'0.8'。

plt.plot()只有一个输入列表或数组时，参数被当作Y轴，X轴以索引自动生成；当有两个以上参数时，按照X轴和Y轴顺序绘制数据点。

图形保存函数plt.savefig()将输出图形存储为文件，默认PNG格式，可以通过dpi修改输出质量

#### pyplot的中文显示

pyplot并不默认支持中文显示，需要显示中文时，有两种方法。

第一种是全局方法，即通过matplotlib.RcParams类的一个实例matplotlib.rcParams修改配置文件matplotlibrc，来定制各种属性（rc参数）。如：

    mpl.rcParams['font.family']='SimHei'
    mpl.rcParams['font.size']=10

rcParams的字体属性主要有：

  * 'font.family' 用于显示字体的名字
  * 'font.style' 字体风格，正常'normal'或斜体'italic'
  * 'font.size' 字体大小，整数字号或者'large'、'x‐small'

主要的中文字体有：

  * 'SimHei' 中文黑体
  * 'Kaiti' 中文楷体
  * 'LiSu' 中文隶书
  * 'FangSong' 中文仿宋
  * 'YouYuan' 中文幼圆
  * 'STSong' 华文宋体

第二种是局部方法，即在有中文输出的地方，增加一个属性：fontproperties，如：

    plt.title("衰减曲线", fontproperties='SimHei', fontsize=10)

#### pyplot的文本显示

pyplot主要的文本显示函数有：

  * plt.xlabel() 对X轴增加文本标签
  * plt.ylabel() 对Y轴增加文本标签
  * plt.title() 对图形整体增加文本标签
  * plt.text() 在任意位置增加文本
  * plt.annotate() 在图形中增加带箭头的注解

pyplot的文本显示函数支持Latex数学公式，在字符串中使用`$...$`引入数学公式，应使用Raw字符串。

#### pyplot的子绘图区域

创建绘图区域函数plt.subplot(nrows, ncols, plot_number)在全局绘图区域中创建一个分区体系，并定位到一个子绘图区域。

要创建复杂子绘图区域使用subplot2grid()函数：

    plt.subplot2grid(GridSpec, CurSpec, colspan=1, rowspan=1)

其理念是：设定网格，选中网格，确定选中行列区域数量，编号从0开始。如：

    plt.subplot2grid((3,3), (1,0), colspan=2)

使用GridSpec类和subplot()函数也可以实现同样的效果。如：

```python
import matplotlib.gridspec as gridspec
gs=gridspec.GridSpec(3,3)
ax=plt.subplot(gs[1,:-1])
```

#### pyplot基础图表函数

pyplot主要的图表函数有：

  * plt.plot(x,y,fmt,…) 绘制一个坐标图
  * plt.boxplot(data,notch,position) 绘制一个箱形图
  * plt.bar(left,height,width,bottom) 绘制一个条形图
  * plt.barh(width,bottom,left,height) 绘制一个横向条形图
  * plt.polar(theta, r) 绘制极坐标图
  * plt.pie(data, explode) 绘制饼图
  * plt.psd(x,NFFT=256,pad_to,Fs) 绘制功率谱密度图
  * plt.specgram(x,NFFT=256,pad_to,F) 绘制谱图
  * plt.cohere(x,y,NFFT=256,Fs) 绘制X‐Y的相关性函数
  * plt.scatter(x,y) 绘制散点图，其中，x和y长度相同
  * plt.step(x,y,where) 绘制步阶图
  * plt.hist(x,bins,normed) 绘制直方图
  * lt.contour(X,Y,Z,N) 绘制等值图
  * plt.vlines() 绘制垂直图
  * plt.stem(x,y,linefmt,markerfmt) 绘制柴火图
  * plt.plot_date() 绘制数据日期


## 数据分析之概要

### Pandas库入门

[Pandas库](https://pandas.pydata.org/)是一个开源的Python第三方库，提供高性能易用数据结构化和分析工具。Pandas基于NumPy实现，常与NumPy和Matplotlib一同使用。其约定的引入方式为：

    import pandas as pd

Pandas在NumPy的基础数据类型上提供两个基本的数据类型：Series, DataFrame。与NumPy不同的是，Pandas的数据结构更关注数据的应用表达而非数据的结构表达，强调数据与索引间的关系，而不是数据的维度。

#### Series数据类型

Series类型由一组数据及与之相关的数据索引组成。Series类型可以由如下类型创建：

  * Python列表，index与列表元素个数一致
  * 标量值，index表达Series类型的尺寸
  * Python字典，键值对中的“键”是索引，index从字典中进行选择操作
  * ndarray，索引和数据都可以通过ndarray类型创建
  * 其他函数，如range()等

Series类型包括index和values两部分。创建Series对象时会创建一个自动索引，可以添加自定义索引，两个索引并存。Series对象和索引都可以有一个名字，存储在属性.name中。

Series类型的操作类似ndarray类型：

  * 索引方法相同，采用[]
  * NumPy中运算和操作可用于Series类型
  * 可以通过自定义索引的列表进行切片
  * 可以通过自动索引进行切片，如果存在自定义索引，则一同被切片

Series类型的操作类似Python字典类型：

  * 通过自定义索引访问
  * 保留字in操作
  * 使用.get()方法

Series类型在运算中会自动对齐不同索引的数据。

#### DataFrame数据类型

DataFrame类型是一个二维的带标签数据结构，其列可以有不同的数据类型，且共用相同的索引。其行（axis=0）标签称为索引（index），列（axis=1）标签称为列（columns）。DataFrame类型可以看作一个数据表，或Series对象字典。

DataFrame类型可以由如下类型创建：

  * 二维ndarray对象
  * 由一维ndarray、列表、字典、元组或Series构成的字典
  * Series类型
  * 其他的DataFrame类型

#### Pandas的数据操作

Pandas的数据操作就是数据标签进行操作，使用labels和axis指定或使用index、columns指定，缺省的操作一般对行（axis=0）进行。

使用obj.reindex()能够改变或重排Series和DataFrame索引，其主要的参数如下：

  * index, columns 新的行列自定义索引
  * fill_value 重新索引中，用于填充缺失位置的值
  * method 填充方法, ffill向前填充，bfill向后填充
  * limit 最大填充量
  * copy 默认True，生成新的对象，False时，新旧相等不复制

Series和DataFrame的索引是Index对象，Index对象是不可修改类型。索引对象的常用方法有：

  * .append(idx) 连接另一个Index对象，产生新的Index对象
  * .diff(idx) 计算差集，产生新的Index对象
  * .intersection(idx) 计算交集
  * .union(idx) 计算并集
  * .delete(loc) 删除loc位置处的元素
  * .insert(loc,item) 在loc位置增加一个元素item

使用obj.drop()能够删除Series和DataFrame指定行或列索引。缺省为index。

#### Pandas的数据运算

Pandas数据的算术运算的一般规则如下：

  * 算术运算根据行列索引，补齐后运算，运算默认产生浮点数
  * 补齐时缺项填充NaN (空值)
  * 二维和一维、一维和零维间为广播运算
  * 采用`+ ‐ * /`符号进行的二元运算产生新的对象

使用对象方法进行运算可以使用fill_value参数指定填充值：

  * `.add(d, **argws)` 类型间加法运算，可选参数
  * `.sub(d, **argws)` 类型间减法运算，可选参数
  * `.mul(d, **argws)` 类型间乘法运算，可选参数
  * `.div(d, **argws)` 类型间除法运算，可选参数

Pandas数据的比较运算的一般规则如下：

  * 比较运算只能比较相同索引的元素，不进行补齐
  * 二维和一维、一维和零维间为广播运算
  * 采用`> < >= <= == !=`等符号进行的二元运算产生布尔对象

### Pandas数据特征分析

一组数据表达一个或多个含义，从数据提取特征的过程过程包括：

  * 基本统计（含排序）
  * 分布/累计统计
  * 数据特征：相关性、周期性等
  * 数据挖掘（形成知识）

#### 数据的排序

对索引进行排序：.sort_index((axis=0, ascending=True)方法在指定轴上根据索引进行排序，默认升序。

对数据进行排序：DataFrame.sort_values(by, axis=0, ascending=True)方法在指定轴上根据数值进行排序，默认升序。NaN统一放到排序末尾

  * by: axis轴上的某个索引或索引列表，Series对象没有by参数。

#### 数据的基本统计分析

基本的统计分析函数（适用于Series和DataFrame类型）：

  * .sum() 计算数据的总和，默认按0轴计算，下同
  * .count() 统计非NaN值的数量
  * .mean() .median()  计算数据的算术平均值、算术中位数
  * .var()  .std() 计算数据的方差、标准差
  * .min() .max() 计算数据的最小值、最大值
  * .describe() 针对0轴（各列）的统计汇总

基本的统计分析函数（适用于Series类型）：
  * .argmin()  .argmax() 计算数据最大值、最小值所在位置的索引位置（自动索引）
  * .idxmin()  .idxmax() 计算数据最大值、最小值所在位置的索引（自定义索引）

#### 数据的累计统计分析

常用的累计统计函数（适用于Series和DataFrame类型）：

  * .cumsum() 依次给出前1、2、…、n个数的和
  * .cumprod() 依次给出前1、2、…、n个数的积
  * .cummax() 依次给出前1、2、…、n个数的最大值
  * .cummin() 依次给出前1、2、…、n个数的最小值

常用的滚动统计函数，也叫窗口统计函数（适用于Series和DataFrame类型）：  

  * .rolling(w).sum() 依次计算相邻w个元素的和
  * .rolling(w).mean() 依次计算相邻w个元素的算术平均值
  * .rolling(w).var() 依次计算相邻w个元素的方差
  * .rolling(w).std() 依次计算相邻w个元素的标准差
  * .rolling(w).min() .max()依次计算相邻w个元素的最小值和最大值

#### 数据的相关分析

两个事物，表示为X和Y，它们之间的相关性有：

  * X增大，Y增大，则两个变量正相关
  * X增大，Y减小，则两个变量负相关
  * X增大，Y无视，则两个变量不相关

这种相关性可以通过协方差判断：

$$
cov(X,Y)=\frac{\sum_{i=1}^n (X_i-\bar{X})(Y_i-\bar{Y})}{n-1}
$$

  * 协方差>0, X和Y正相关
  * 协方差<0, X和Y负相关
  * 协方差=0, X和Y独立无关

更精确的相关性分析使用Pearson相关系数：

$$
r=\frac{\sum_{i=1}^n (x_i-\bar{x})(y_i-\bar{y})}{\sqrt{\sum_{i=1}^n (x_i-\bar{x})^2}\sqrt{\sum_{i=1}^n (y_i-\bar{y})^2}} \\
r \in [-1,1]
$$

  * 0.8‐1.0 极强相关
  * 0.6‐0.8 强相关
  * 0.4‐0.6 中等程度相关
  * 0.2‐0.4 弱相关
  * 0.0‐0.2 极弱相关或无相关

Pandas常用的相关性分析方法有（适用于Series和DataFrame类型）：

  * .cov() 计算协方差矩阵
  * .corr() 计算相关系数矩阵, Pearson、Spearman、Kendall等系数

## 总结

2019年5月26日，结束了“Python数据分析与展示”课程的学习。

课程主要学习了数据表示、分析和展示的基本方法，其中Numpy库，是各种科学技术和数据分析的基础，其基本的数据结构是ndarray数组；Matplotlib库，是一个功能强大的数据可视化库，提供类似Matlab的交互式命令界面，同时也提供了灵活的面向对象的接口；Pandas库，是一个高性能的处理和分析结构化数据的库，两种基本的数据类型Series和DataFrame是其基础。