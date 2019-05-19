---
layout: post
title: Python数据分析与展示学习笔记
categories: [学习笔记, 数据分析]
tags: [Python, 数据分析, 数据展示]
---


* Content
{:toc}

2019年4月25日，在中国大学MOOC平台上开始学习北京理工大学的网络公开课：[Python数据分析与展示](https://www.icourse163.org/course/BIT-1001870002)。

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
  * .size  ndarray对象元素的个数，相当于.shape中n*m的值
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
  * float16 16位半精度浮点数：1位符号位，5位指数，10位尾数，(符号)尾数*10^指数
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
  * np.eye(n) 创建一个正方的n*n单位矩阵，对角线为1，其余为0
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












#### 
#### 

### NumPy数据存取与函数

单元3：实例1：图像的手绘效果
03
## 数据分析之展示


### Matplotlib库入门
#### 
#### 
#### 
#### 
### Matplotlib基础绘图函数示例
#### 
#### 
#### 
#### 


单元6：实例2：引力波的绘制
04
## 数据分析之概要

### Pandas库入门
#### 
#### 
#### 
#### 
### Pandas数据特征分析
#### 
#### 
#### 
#### 
  
## 总结

2019年5月2日，利用五一休假时间，结束了“Python网络爬虫与信息提取”课程的学习。

课程主要学习了网络爬虫获取信息的基本过程和方法、学习了Requests库和BeautifulSoup库，以及Scrapy爬虫框架。学习了实现网络爬虫的两种技术路径：一个是Requests + BeautifulSoup方法，适用于小型的网络爬虫项目（网页级）；另一个是Scrapy爬虫框架，适用于中型的网络爬虫项目（网站级）。