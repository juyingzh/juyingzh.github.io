---
layout: post
title: 利用Python进行数据分析读书笔记
categories: [数据分析]
tags: [Python, 数据分析, 读书笔记]
---

* Content
{:toc}

书名：利用Python进行数据分析，第二版（Python for Data Analysis，2nd Edition）

作者：[美] Wes McKinney 著

出版：O’Reilly Media, Inc. October 2017, Second Edition

本书重点讲述运用Python开源库（Numpy、Matplotlib和Pandas）进行数据分析。之前参考的是第一版的中译本，但由于Pandas库更新很快，第一版很多代码与新版不兼容了，所以重新以第二版为参考学习一下。第二版目前还没有正式中译本出版，参考了一个社区翻译的版本。



和第一版相比以下两个更新重要：所有的Python代码升级为Python 3.6；Pandas更新到了2017最新版。虽然本书的第二版使用了更新的pandas版本，但其中使用的代码仍然有许多提示将要废弃，感觉pandas这个库发展太快了，随着这几年数据处理领域的快速发展，处理工具和处理方法都在发生巨大的变化。

以前参照着帮助文档使用了一段时间pandas，对其中的一些概念和数据处理似懂非懂，这次深入学习后才发现，其中的内容很多。数据分析是一门实践性很强的学问，很多知识必须在实践中才能理解和掌握。掌握了基本概念之后，要在实践中深入学习pandas的知识，还是要学习官方文档，并及时关注开发团队发布的文档更新。

## 第1章 准备工作

本书所讲的重点是Python编程、库，以及用于数据分析的工具，也就是数据分析要用到的Python编程。

本书所讲的数据主要指结构化的数据，这个术语代指了所有通用格式的数据：

  * 多维数组（矩阵）
  * 表格型数据，其中各列可能是不同的类型
  * 通过关键列（SQL而言，就是主键和外键）相互联系的多个表
  * 间隔平均或不平均的时间序列

重要的Python库：
  * Numpy，Python科学计算的基础包
  * Pandas，提供了处理结构化数据的大量数据结构和函数。用得最多的pandas对象
    是DataFrame，它是一个面向列（column-oriented）的二维表结构，另一个是Series，一个一维的标签化数组对象。pandas兼具NumPy高性能的数组计算功能以及电子表格和关系数据库灵活的数据处理功能。
  * Matplotlib，用于绘制图表和其它二维数据可视化的Python库
  * IPython和Jupyter，IPython提供与Python交互式计算的工具，鼓励“执行-探索”的工作流，区别于其它编程软件的“编辑-编译-运行”的工作流。Jupyter是一个更宽泛的多语言交互计算工具，现在支持40种编程语言。IPython现在可以作为Jupyter使用Python的内核（一种编程语言模式）。IPython变成了Jupyter庞大开源项目（一个交互和探索式计算的高效环境）中的一个组件。还可以使用通过Jupyter Notebook，一个支持多种语言的交互式网络代码“笔记本”，来使用IPython。
  * Scipy，一组解决科学计算中各种标准问题的包的集合
  * scikit-learn，Python的通用机器学习工具包
  * statsmodels，一个统计分析包，与scikit-learn比较，statsmodels包含经典统计学和经济计量学的算法。statsmodels更关注于统计推断，提供不确定估计和参数p值。而scikit-learn更注重预测。

数据分析的几大类任务：
  * 与外界进行交互，读写各种文件格式和数据库
  * 准备，对数据进行清理、修整、整合、规范化、重塑、切片、变形等处理以便进行分析
  * 转换，对数据集做一些数学和统计运算以产生新的数据集
  * 建模和计算，将数据与统计模型、机器学习算法或其他计算工具联系起来
  * 展示，创建交互式的或静态的图片或文字摘要

Python社区广泛采取的一些常用模块的命名惯例：

```python
import	numpy as np
import	matplotlib.pyplot as plt
import	pandas	as	pd
import	seaborn	as	sns
import	statsmodels	as	sm
```

在Python软件开发过程中，不建议直接引入类似NumPy这种大型库的全部内容（`from numpy import *`）。

数据规整（Munge/Munging/Wrangling）指的是将非结构化和（或）散乱数据处理为结构化或整洁形式的过程。

我的标准开发环境，IPython加文本编辑器。我通常在编程时，反复在IPython或Jupyter notebooks中测试和调试每条代码。也可以交互式操作数据，和可视化验证数据操作中某一特殊集合。在shell中使用pandas和NumPy也很容易。

## 第2章 Python语法基础，IPython和Jupyter Notebooks

### 2.1 Python解释器

进入Python运行命令行下运行命令：`>Python`，退出解释器运行`>>>exit()`。Python使用命令行参数运行程序：

```python
python hello_world.py
```

从事数据分析和科学计算的人使用IPython，一个强化的Python解释器，或Jupyter notebooks，一个网页代码笔
记本，它原先是IPython的一个子项目。而IPython使用`%run`命令交互执行程序：

```python
%run hello_world.py
```

IPython默认采用序号的格式  `In [2]:`，而非Python标准的 `>>>`提示符不同。

### 2.2 IPython基础

IPython的设计目标是在交互式计算和软件开发者两个方面最大化地提供生产力，它鼓励一种“执行—探索”的工作模式（大部分的数据分析代码都包含探索式操作，即试错法和迭代法），它与操作系统Shell和文件系统之间也紧密集成。

使用命令行`>ipython`启动IPython。IPython就像一个加强的Python解释器。在IPthon中，输入任何表达式时按下Tab键，会自动在当前命名空间中找出匹配的对象，包括变量、函数、对象、对象属性和方法、模块的函数和子模块、文件路径、函数参数等。

Notebook是Jupyter项目的重要组件之一，它是一个代码、文本（有标记或无标记）、数据可视化或其它输出的交互式文档。Jupyter Notebook需要与内核互动，内核是Jupyter与其它编程语言的交互编程协议。Python的Jupyter内核是使用IPython。要启动Jupyter，在命令行中输入`>jupyter notebook`。

#### Tab补全

IPython shell较标准的Python解释器重要的改进之一是具备其它IDE和交互计算分析环境都有的tab补全功能。

在shell中输入表达式，按下T ab，会在命名空间中搜索已输入变量（对象、函数等等），可以补全任何对象的方法和属性，以及模块下的子模块和函数。Tab也可以补全文件路径，函数的关键词参数等。

#### 内省

在变量的前面或后面一个问号（？）就可以将有关该对象的一些通用的信息显示出来，叫做内省（Object Introspection）。如果是函数或方法，则会显示其docstring，如果使用？？，还将显示其源码。

问号还可以搜索IPython命名空间，再配合通配符（*），即可显示所有匹配的名称。例如，以下命令可以获得Numpy命名空间下包含load的所有函数：

```python
np.*load*?
```

#### %run命令

使用%run命令运行脚本，与标准命令行相同，可以带入参数。脚本在一个空的命名空间中运行，运行后脚本中定义的变量就可以在IPython环境中使用了。

在Jupyter notebook中，你也可以使用`%load`，它将脚本导入到一个代码格中。

任何代码执行时，只要按下“Ctrl+C”，就会引发KeyboardInterrupt异常而终止。

使用%paste和%cpaste两个魔术命令可以执行剪贴板中的代码。其中，%paste可以直接运行剪贴板中
的代码，%cpaste则会给出一条提示，你可以粘贴任意多的代码再运行。

#### 魔术命令

IPython中的命令行程序称为魔术命令（Magic Command），以百分号为前缀，如果没有与其同名的变量，可以省略%，称为automagic，也可以通过%automagic打开或关闭。

许多魔术命令有“命令行”选项，可以通过？查看。

一些魔术函数与Python函数很像，它的结果可以赋值给一个变量。例如：

```python
foo = %pwd
```

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
  * %who, %who_ls, %whos 显示交互式命名空间中定义的变量，三者显示的信息级别不同
  * %xdel varible 删除vavible，并尝试清除其在IPython中的对象上的一切引用

#### 集成Matplotlib

IPython在分析计算领域能够流行的原因之一是它非常好的集成了数据可视化和其它用户界面库，比如matplotlib。

%matplotlib魔术函数配置了IPython shell和Jupyter notebook中的matplotlib，否则其创建的图形不会出现在notebook或直到结束shell才会获取session的控制。

```python
%matplotlib  # 在IPython	shell中配置matplotlib，可以创建多个绘图窗口，而不会干扰控制台session
%matplotlib inline  # 在Jupyter notebook中配置matplotlib
```

### 2.3 Python语法基础

Python被认为是强类型化语言，意味着每个对象都有明确的类型（或类），默许转换只会发生在特定的情况下。

你可以用 isinstance()函数检查对象是某个类型的实例：`isinstance(a,int)`。isinstance也可以用类型元组，检查对象的类型是否在元组中。知道对象的类型很重要，最好能让函数可以处理多种类型的输入。

你可能不关心对象的类型，只关心对象是否有某些方法或用途。这通常被称为“鸭子类型”，来自“走起来像鸭子、叫起来像鸭子，那么它就是鸭子”的说法。例如，你可以通过验证一个对象是否遵循迭代协议，判断它是可迭代的。

```python
def	isiterable(obj):
    try:
        iter(obj)
        return	True
    except	TypeError:	#	not	iterable
        return	False
```

总是用这个功能编写可以接受多种输入类型的函数。常见的例子是编写一个函数可以接受任意类型的序列（list、tuple、ndarray）或是迭代器。你可先检验对象是否是列表（或是NUmPy数组），如果不是的话，将其转变成列表：

```python
if	not	isinstance(x,	list)	and	isiterable(x):
    x = list(x)
```

## 第3章 Python的数据结构、函数和文件

### 3.1 数据结构和序列

#### 元组

元组是一个固定长度，不可改变的Python序列对象。

如果将元组赋值给类似元组的变量，Python会试图拆分等号右边的值。使用这个功能，可以很容易地交换变量的名字。

```python
a,b = b,a
```

更高级的元组拆分功能允许从元组的开头“摘取”几个元素，它使用了特殊的语法 `*rest`，这也用在函数中以抓取任意长度列表的位置参数：

```python
values = 1, 2, 3, 4, 5
a, b, *rest = values
```

rest的部分是想要舍弃的部分，rest的名字不重要。作为惯用写法，许多Python程序员会将不需要的变量使用下划线。

#### 列表

与元组对比，列表的长度可变、内容可以被修改。

可以用append在列表末尾添加元素；如果已经定义了一个列表，用extend方法可以追加多个元素。通过加法将列表串联的计算量较大，因为要新建一个列表，并且要复制对象。用extend追加元素，尤其是到一个大列表中，更为可取。

序列的排序，可以用序列方法list.sort将一个列表原地排序（不创建新的对象），也可以使用内置函数sorted产生一个排好序的序列副本。

bisect模块支持二分查找，和向已排序的列表插入值。bisect.bisect可以找到插入值后仍保证排序的位置（右侧），bisect.insort是向这个位置插入值（右侧）。

使用切片的负步进，可以将列表或元组颠倒过来，如：`seq[::-1]`。

一些有用的序列函数：

* enumerate函数，可以返 回 (index, value)元组序列
* sorted函数可以从任意序列返回一个新的排好序的列表，sorted函数可以接受和sort相同的参数
* zip可以将多个列表、元组或其它序列成对组合成一个元组列表，zip的常见用法之一是同时迭代多个序列。
* reversed可以从后向前迭代一个序列，注意 reversed是一个生成器，只有实体化（即列表或for循环）之后才能创建翻转的序列。

给出一个“被压缩的”序列，zip可以被用来解压序列，也可以看作把行的列表转换为列的列表：

```python
pitchers=[('Nolan','Ryan'),('Roger','Clemens'),('Schilling','Curt'),('Bill','Gates')]
first_names,last_names=zip(*pitchers)
[first_names, last_names]
```

#### 字典

字典可能是Python最为重要的数据结构。它更为常见的名字是哈希映射或关联数组。它是键值对的大小可变集合，键和值都是Python对象。

keys和values是字典的键和值的迭代器方法。

用 update方法可以将一个字典与另一个融合。update方法是原地改变字典，因此任何传递给update的键的旧值都会被舍弃。

将两个序列配对组合成字典：`dict(zip(key_list,	value_list))`，因为字典本质上是2元元组的集合，dict可以接受2元元组的列表。

dict的方法get和pop可以取默认值进行返回：`value=some_dict.get(key,default_value)`。如果没有提供默认值，且不存在键，get默认会返回None，pop会抛出一个KeyError异常。

dict的setdefault(key[, default])方法用于设定字典键值，如果键存在则返回值，如果键不存在，则使用默认值插入键值，如果默认值没有提供，则使用None。常见的情况是在字典的值是属于其它集合，如列表。例如，你可以通过首字母，将一个列表中的单词分类：

```python
words=['apple','bat','bar','atom','book']
by_letter={}
for word in words:
    letter=word[0]
    by_letter.setdefault(letter,[]).append(word)
```

collections模块有一个很有用的类defaultdict([default_factory[, ...]])，是dict的子类，default_factory提供了初始值，传递类型或函数以生成每个位置的默认值：

```python
from collections import defaultdict
words=['apple','bat','bar','atom','book']
by_letter = defaultdict(list)
for word in words:
    by_letter[word[0]].append(word)
```

字典的值可以是任意Python对象，而键通常是不可变的标量类型（整数、浮点型、字符串）或元组（元组中的对象必须是不可变的）。这被称为“可哈希性”。可以用hash函数检测一个对象是否是可哈希的（可被用作字典的键）：`hash('string')`

要用列表当做键，一种方法是将列表转化为元组，只要内部元素可以被哈希，它也就可以被哈希。

#### 集合

集合是无序的不可重复的元素的集合。你可以把它当做字典，但是只有键没有值。

可以用两种方式创建集合：通过set函数或使用尖括号set语句。

常用的集合方法：

| 函数                             | 替代语法 | 说明 |
| -------------------------------- | -------- | ---- |
| a.add(x)                         | N/A      | 将元素x添加到集合a |
| a.clear(x)                       | N/A      | 将集合清空 |
| a.remove(x)                      | N/A      | 将元素x从集合a除去 |
| a.pop()                          | N/A      | 从集合a去除任意元素，如果集合为空，则抛出KeyError异常 |
| a.union(b)                       | a \|b | 集合a和b中的所有不重复元素 |
| a.update(b)                      | a \|= b | 设定集合a中的元素为a与b的并集 |
| a.intersection(b)                | a & b | a和b中交叉的元素 |
| a.intersection_update(b)         | a &= b | 设定集合a中的元素为a与b的交集 |
| a.difference(b)                  | a - b | 存在于a但不存在于b的元素 |
| a.difference_update(b)         | a -= b | 设定集合a中的元素为a与b的差 |
| a.symmetric_difference(b)        | a ^ b | 只在a或只在b的元素 |
| a.symmetric_difference_update(b) | a ^= b | 集合a中的元素为只在a或只在b的元素 |
| a.issubset(b) | N/A      | 如果a中的元素全部属于b，则为True |
| a.issuperset(b)                  | N/A      | 如果b中的元素全部属于a，则为True |
| a.isdisjoint(b)                                 |   N/A        | 如果a和b无共同元素，则为True |

所有逻辑集合操作都有另外的原地实现方法，可以直接用结果替代集合的内容。对于大的集合，这么做效率更高。

#### 列表、集合和字典推导式

列表推导式是Python最受喜爱的特性之一。它允许用户方便的从一个集合过滤元素，形成列表，在传递参数的过程中还可以修改元素。字典、集合推导式类似，形式如下：

```python
list_comp=[expr for val in collection if condition]
dict_comp={key-expr:value-expr for value in	collection if condition}
set_comp={expr	for	value in collection if condition}
```

### 3.2 函数

函数是Python中最主要也是最重要的代码组织和复用手段。作为最重要的原则，如果你要重复使用相同或非常类似的代码，就需要写一个函数。通过给函数起一个名字，还可以提高代码的可读性。

函数可以返回多个值，在数据分析和其他科学计算应用中经常用到。如下面的例子：

```python
def f():
    a, b, c= 5, 6, 7
    return a,b,c
a,b,c=f()
```

该函数其实只返回了一个对象，也就是一个元组，最后该元组会被拆包到各个结果变量中。此外，还有一种非常具
有吸引力的多值返回方式，返回字典：

```python
def f():
    a, b, c= 5, 6, 7
    return {'a':a,'b':b,'c':c}
```

由于Python函数都是对象，因此，在其他语言中较难表达的一些设计思想在Python中就要简单很多了。假设我们有一个字符串数组，为了得到一组能用于分析工作的格式统一的字符串，希望对其进行一些数据清理工作并执行一堆转换：去除空白符、删除各种标点符号、正确的大写格式等，将需要在一组给定字符串上执行的所有运算做成一
个列表，这种多函数模式使你能在很高的层次上轻松修改字符串的转换方式，此时的clean_strings也更具可复用性：

```python
import re
states=['  Alabama ','Georgia!','Georgia','georgia','FlOrIda','south carolina##','West virginia?']
def remove_punctuation(value):
    return re.sub(r'[!#?]', '', value)
clean_ops = [str.strip, remove_punctuation, str.title]
def clean_strings(strings, ops):
    result = []
    for value in strings:
        for function in ops:
            value = function(value)
        result.append(value)
    return result
clean_strings(states, clean_ops)
```

lambda函数在数据分析工作中非常方便，因为你会发现很多数据转换函数都以函数作为参数的。直接传入lambda函数比编写完整函数声明要少输入很多字（也更清晰），甚至比将lambda函数赋值给一个变量还要少
输入很多字。

```python
def apply_to_list(some_list, f):
	return [f(x) for x in some_list]
ints = [4, 0, 1, 5, 6]
apply_to_list(ints, lambda x: x * 2)
```

#### 部分参数应用

柯里化（currying）是一个有趣的计算机科学术语，它指的是通过“部分参数应用”（partial	argument	application）从现有函数派生出新函数的技术。例如，假设我们有一个执行两数相加的简单函数：

```python
def	add_numbers(x, y):
	return	x+y
```

通过这个函数，我们可以派生出一个新的只有一个参数的函数add_five，它用于对其参数加5：

```python
add_five=lambda	y:add_numbers(5,y)
```

add_numbers的第二个参数称为“柯里化的”（curried）。内置的functools模块可以用partial函数将此过程简化：

```python
from functools import partial
add_five=partial(add_numbers,5)
```

#### 生成器
能以一种一致的方式对序列进行迭代（比如列表中的对象或文件中的行）是Python的一个重要特点。这是通过一种叫做迭代器协议（iterator	protocol，它是一种使对象可迭代的通用方式）的方式实现的，一个原生的使对象可迭代的方法。

迭代器是一种特殊对象，它可以在诸如for循环之类的上下文中向Python解释器输送对象。大部分能接受列表之类的对象的方法也都可以接受任何可迭代对象。

生成器（generator）是构造新的可迭代对象的一种简单方式。一般的函数执行之后只会返回单个值，而生成器则是以延迟的方式返回一个值序列，即每返回一个值之后暂停，直到下一个值被请求时再继续。要创建一个生成器，只需将函数中的return替换为yeild即可。

```python
def squares(n=10):
    print('Generating squares from 1 to {0}'.format(n ** 2))
    for i in range(1, n + 1):
        yield i**2
gen=squares()
```

另一种更简洁的构造生成器的方法是使用生成器表达式（generator	expression）。这是一种类似于列表、字典、集合推导式的生成器。其创建方式为，把列表推导式两端的方括号改成圆括号：

```python
gen = (x ** 2 for x in range(100))
```

标准库itertools模块中有一组用于许多常见数据算法的生成器。例如，groupby可以接受任何序列和一个函数。它根据函数的返回值对序列中的连续元素进行分组。下面是一个例子：

```python
import itertools
first_letter = lambda x: x[0]
names = ['Alan', 'Adam', 'Wes', 'Will', 'Albert', 'Steven']
for letter, names in itertools.groupby(names, first_letter):
    print(letter, list(names)) # names is a generator
```

一些我经常用到的itertools函数：

| 函数                          | 说明                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| combinations(iterable, k)     | 生成一个由iterable中所有可能的k元元组组成的序列，不考虑顺序（参考另一个函数combinations_with_replacement） |
| permutations(iterable, k)     | 生成一个由iterable中所有可能的k元元组组成的序列，考虑顺序    |
| gourpby(iterable[, keyfunc])  | 为每一个唯一键生成一个(key, sub-iterator)                    |
| product(*iterables, repeat=1) | 生成输入的iterables的笛卡尔积，结果为元组，类似于嵌套for循环 |

### 3.3 文件和操作系统

从文件中取出的行都带有完整的行结束符（EOL），因此你常常会看到下面这样的代码（得到一组没有EOL的行）：

```python
lines=[x.rstrip() for x in open(path)]
```

## 第4章 NumPy基础：数组和矢量计算

NumPy（Numerical Python的简称）是Python数值计算最重要的基础包。大多数提供科学计算的包都是用NumPy的数组作为构建基础。

NumPy的部分功能如下：

* ndarray，一个具有矢量算术运算和复杂广播能力的快速且节省空间的多维数组。
* 用于对整组数据进行快速运算的标准数学函数（无需编写循环）。
* 用于读写磁盘数据的工具以及用于操作内存映射文件的工具。
* 线性代数、随机数生成以及傅里叶变换功能。
* 用于集成由C、C++、Fortran等语言编写的代码的C API。

由于NumPy提供了一个简单易用的C API，因此很容易将数据传递给由低级语言编写的外部库，外部库也能以NumPy数组的形式将数据返回给Python。这个功能使Python成为一种包装C/C++/Fortran历史代码库的选择，并使被包装库拥有一个动态的、易用的接口。

NumPy本身并没有提供多么高级的数据分析功能，理解NumPy数组以及面向数组的计算将有助于你更加高效地使用诸如pandas之类的工具。

对于大部分数据分析应用而言，我最关注的功能主要集中在：

* 用于数据整理和清理、子集构造和过滤、转换等快速的矢量化数组运算。
* 常用的数组算法，如排序、唯一化、集合运算等。
* 高效的描述统计和数据聚合/摘要运算。
* 用于异构数据集的合并/连接运算的数据对齐和关系型数据运算。
* 将条件逻辑表述为数组表达式（而不是带有if-elif-else分支的循环）。
* 数据的分组运算（聚合、转换、函数应用等）。

NumPy之于数值计算特别重要的原因之一，是因为它可以高效处理大数组的数据。这是因为：

* NumPy是在一个连续的内存块中存储数据，独立于其他Python内置对象。NumPy的C语言编写的算法库可以操作内存，而不必进行类型检查或其它前期工作。比起Python的内置序列，NumPy数组使用的内存更少。
* NumPy可以在整个数组上执行复杂的计算，而不需要Python的for循环。

### 4.1 NumPy的ndarray：一种多维数组对象

NumPY最重要的一个特点就是其N维数组对象（即ndarray），他是一个通用的同构数据多维容器，也就是说，其中的所有元素必须是相同类型的。每个数组都有一个shape（一个表示各维度大小的元组）和一个dtype（一个用于说明数组数据类型的对象）。

你可以利用这种数组对整块数据执行一些数学运算，其语法跟标量元素之间的运算一样。

```python
data = np.random.randn(2,3)
data2 = data*10
data.shape, data.dtype
```

#### 创建ndarray

创建数组最简单的办法就是使用array函数。它接受一切序列型的对象（包括其他数组），然后产生一个新的含有传入数据的NumPy数组。

```python
data2 = [[1,2,3,4],[5,6,7,8]]
arr = np.array(data2)
arr.ndim, arr.shape, arr.dtype
```

除np.array之外，还有一些函数也可以新建数组。比如，zeros和ones分别可以创建指定长度或形状的全0或全1数组。empty可以创建一个没有任何具体值的数组。要用这些方法创建多维数组，只需传入一个表示形状的元组即可。

下表列出了一些数组创建函数。由于NumPy关注的是数值计算，因此，如果没有特别指定，数据类型基本都是float64（浮点数）。

| 函数              | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| array             | 将输入数据（列表、元组、数组或其它序列类型）转换为ndarray。要么推断出dtype，要么特别指定dtype。默认直接复制输入数据 |
| asarray           | 将输入转换为ndarray，如果输入本身就是一个ndarray就不进行复制 |
| arange            | 类似于内置的range，但返回的是一个ndarray而不是列表           |
| ones, ones_like   | 根据指定的形状和dtype创建一个全1数组。ones_like以另一个数组为参数，并根据其形状和dtype创建一个全1数组。 |
| zeros, zeros_like | 类似于ones和ones_like，只不过产生的是全0数组而已             |
| empty, empty_like | 创建新数组，只分配内存空间但不填充任何值                     |
| full, full_like   | 用full value中的值，根据指定的形状和dtype创建一个数组。full_like使用另一个数组，用相同的形状和dtype创建一个数组。 |
| eye, identity     | 创建一个正方的N*N单位阵（对角线为1，其余为0）                |

#### ndarray的数据类型
dtype（数据类型）是一个特殊的对象，它含有ndarray将一块内存解释为特定数据类型所需的信息。

dtype是NumPy强大和灵活的原因之一。多数情况下，它们直接映射到相应的机器表示，这使得“读写磁盘上的二进制数据流”以及“集成低级语言代码（如C、Fortran）”等工作变得更加简单。

数值型dtype的命名方式是：类型名+元素位长。NumPy支持的数据类型如下：

| 类型         | 类型代码 | 说明                                            |
| :----------- | :------ | :--------------------------------------------- |
| int8,uint8   | i1,u1   | 有符合和无符号的8位（1个字节）整型                |
| int16,uint16 | i2,u2   | 有符合和无符号的16位（2个字节）整型               |
| int32,uint32 | i3,u3   | 有符合和无符号的32位（4个字节）整型               |
| int64,uint64 | i4,u4   | 有符合和无符号的64位（8个字节）整型               |
| float16      | f2       | 半精度浮点数                                     |
| float32      | f4或f    | 标准的单精度浮点数。与C的float兼容                 |
| float64      | f8或d    | 标准的双精度浮点数。与C的double和Python的float兼容 |
| float128     | f16或g   | 扩展精度浮点数                                   |
| complex64    | c8       | 用两个32位浮点数表示的复数                        |
| complex128   | c16     | 用两个64位浮点数表示的复数                        |
| complex256   | c32     | 用两个128位浮点数表示的复数                       |
| bool         | ?        | 存储True和False值的布尔类型                      |
| object       | O       | Python对象类型                                  |
| string_      | S       | 固定长度的字符串类型（每个字符1个字节）。例如，要创建一个长度为10的字符串，应使用S10 |
| unicode_     | U       | 固定长度的unicode类型（字节数由平台决定）。跟字符串的定义方式一样（如U10） |

注：要创建一个长度为n的字符串，应使用Sn，unicode类型与此相似。

可以通过ndarray的astype方法明确地将一个数组从一个dtype转换成另一个dtype。

#### NumPy数组的运算

数组很重要，因为它使你不用编写循环即可对数据执行批量运算。NumPy用户称其为矢量化（vectorization）。大小相等的数组之间的任何算术运算都会将运算应用到元素级。数组与标量的算术运算会将标量值传播到各个元素。

不同大小的数组之间的运算叫做广播（broadcasting）。

#### 基本的索引和切片

一维数组的索引，从表面上看，跟Python列表的功能差不多。当你将一个标量值赋值给一个切片时（如arr[5:8]=12），该值会自动传播（也就说后面将会讲到的“广播”）到整个选区。

跟列表最重要的区别在于，数组切片是原始数组的视图。这意味着数据不会被复制，视图上的任何修改都会直接反
映到源数组上。由于NumPy的设计目的是处理大数据，假如NumPy坚持要将数据复制来复制去的话会产生何等的性能和内存问题。

“只有冒号”表示选取整个轴。切片[:]会给数组中的所有值赋值，如：`data[:]=1.0`

对于高维度数组，能做的事情更多。在一个二维数组中，各索引位置上的元素不再是标量而是一维数组。因此，可以对各个元素进行递归访问，但这样需要做的事情有点多。你可以传入一个以逗号隔开的索引列表来选取单个元素。也就是说，下面两种方式是等价的：

```python
arr2d[0][2]
arr2d[0, 2]
```

#### 布尔型索引

假设有一个用于存储数据的数组以及一个存储姓名的数组（含有重复项）。假设每个名字都对应data数组中的一行，而我们想要选出对应于名字"Bob"的所有行。跟算术运算一样，数组的比较运算（如==）也是矢量化的。因此，对names和字符串"Bob"的比较运算将会产生一个布尔型数组。这个布尔型数组可用于数组索引：

```python
names = np.array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'])
data = np.random.randn(7, 4)
data[names == 'Bob']
```

布尔型数组的长度必须跟被索引的轴长度一致，如果布尔型数组的长度不对，布尔型索引就会出错。此外，还可以将布尔型数组跟切片、整数（或整数序列）混合使用。

要选择除"Bob"以外的其他值，既可以使用不等于符号（!=），也可以通过~对条件进行否定。下面两种方式是等价的：

```python
data[names != 'Bob']
data[~(names == 'Bob')]
```

选取这三个名字中的两个需要组合应用多个布尔条件，使用&（和）、|（或）之类的布尔算术运算符即可，Python关键字and和or在布尔型数组中无效，要使用&与|。

```python
mask = (names == 'Bob') | (names == 'Will')
data[mask]
```

通过布尔型索引选取数组中的数据，将总是创建数据的副本，即使返回一模一样的数组也是如此。

#### 花式索引
花式索引（Fancy indexing）是一个NumPy术语，它指的是利用整数数组进行索引。

假设我们有一个8×4数组，为了以特定顺序选取行子集，只需传入一个用于指定顺序的整数列表或ndarray即
可：

```python
arr[[4, 3, 0, 6]]
```

一次传入多个索引数组会返回一个一维数组，其中的元素对应各个索引元组：

```python
arr[[1, 5, 7, 2]][:, [0, 3, 1, 2]]
```

无论数组是多少维的，花式索引总是一维的。

花式索引跟切片不一样，它总是将数据复制到新数组中。

#### 数组转置和轴对换
数组的转置（transpose）是重塑的一种特殊形式，它返回的是源数据的视图（不会进行任何复制操作）。数组不仅有transpose方法，还有一个特殊的T属性：

```python
arr = np.arange(15).reshape((3, 5))
arr.T
```

高维数组的转置需要输入由轴编号组成的元组。

```python
arr = np.arange(16).reshape((2, 2, 4))
arr.transpose((1, 0, 2))
```

这里，第一个轴被换成了第二个，第二个轴被换成了第一个，最后一个轴不变。

对于高维数组，T属性产生从高维到低维的完整转置；np.swapaxes()方法只是对调两个轴，需要输入一对轴编号。

### 4.2 通用函数：快速的元素级数组函数

通用函数（即ufunc）是一种对ndarray中的数据执行元素级运算的函数。你可以将其看做简单函数（接受一个或多个标量值，并产生一个或多个标量值）的矢量化包装器。

许多ufunc都是简单的元素级变体，如sqrt和exp，这些都是一元（unary）ufunc。另外一些（如add或maximum）接受2个数组（因此也叫二元（binary）ufunc），并返回一个结果数组。

有些ufunc的确可以返回多个数组。modf就是一个例子，它是Python内置函数divmod的矢量化版本，它会返回浮点数数组的小数和整数部分。

Ufuncs可以接受一个out可选参数，这样就能在数组原地进行操作。如下面的语句将结果写入到原数组中：

```python
np.sqrt(arr, arr)
```

常用的一元ufunc：

| 函数                                              | 说明                                                         |
| ------------------------------------------------- | ------------------------------------------------------------ |
| abs, fabs                                         | 计算整数、浮点数或复数的绝对值。对于非复数值，可以使用更快的fabs |
| sqrt                                              | 计算各元素的平方根x**0.5                                     |
| sqrare                                            | 计算各元素的平方x**2                                         |
| exp                                               | 计算各元素的自然指数e^x                                      |
| log, log10, log2, log1p                           | 分别为自然对数，底数为10和2的对数，（1+x）的自然对数         |
| sign                                              | 计算各元素的正负号：正数为1，零为0，负数为-1                 |
| ceil                                              | 计算各元素的ceiling值：即大于等于该值的最小整数              |
| floor                                             | 计算各元素的floor值：即小于等于该值的最大整数                |
| rint                                              | 将各元素值四舍五入到最接近的整数，保留dtype                  |
| modf                                              | 将数组的小数和整数部分以两个独立的数组的形式返回             |
| isnan                                             | 返回一个表示“哪些值是NaN（这不是一个数字）”的布尔型数组      |
| isfinite, isinf                                   | 分别返回一个表示“哪些元素是有穷（非inf，非NaN）”或“哪些元素是无穷”的布尔型数组 |
| cos, cosh, sin, sinh, tan, tanh                   | 普通型和双曲型三角函数                                       |
| arccos, arccosh, arcsin, arcsinh, arctan, arctanh | 普通型和双曲型反三角函数                                     |
| logical_not                                       | 计算各元素not x的真值，相当于-x                              |

常用的一元ufunc：

| 函数                                                       | 说明                                                         |
| ---------------------------------------------------------- | ------------------------------------------------------------ |
| add                                                        | 将数组中对应的元素相加                                       |
| subtract                                                   | 将数组中对应的元素相减                                       |
| multiply                                                   | 将数组中对应的元素相乘                                       |
| divide, floor_divide                                       | 除法或向下圆整除法（丢弃余数）                               |
| power                                                      | 对第一个数组中的元素，以第二个数组中的相应的元素为指数，计算幂 |
| maximum, fmax                                              | 元素级的最大值计算，fmax将忽略NaN                            |
| minimum, fmin                                              | 元素级的最小值计算，fmin将忽略NaN                            |
| mod                                                        | 元素级的求模计算（除法的余数）                               |
| copysign                                                   | 将第二个数组中的值的符合复制给第一个数组中的值               |
| greater, greater_equal, less, less_equal, equal, not_equal | 执行元素级的比较值运算，产生布尔型数组。相当于中缀运算符>, >=, <, <=, ==, != |
| logical_and, logical_or, logical_xor                       | 执行元素级的真值逻辑运算。相当于中缀运算符&,\|, ^            |

### 4.3 利用数组进行数据处理

利用NumPy数组可以将许多数据处理任务表述为简洁的数组表达式，这种用数组表达式代替循环的做法通常称为矢量化。

例如，想要在一组值（网格型）上计算函数sqrt(x^2+y^2)。np.meshgrid函数接受两个一维数组，并产生两个二维矩阵（对应于两个数组中所有的(x,y)对），现在，对该函数的求值运算就把这两个数组当做两个浮点数那样编写表达式即可：

```python
points = np.arange(-5, 5, 0.01) # 1000 equally spaced points
xs, ys = np.meshgrid(points, points)
z = np.sqrt(xs ** 2 + ys ** 2)
plt.imshow(z, cmap=plt.cm.gray)
plt.colorbar()
plt.title("Image plot of $\sqrt{x^2 + y^2}$ for a grid of values")
plt.draw()
```

#### 将条件逻辑表述为数组运算

numpy.where()函数是三元表达式`x if condition else y`的矢量化版本。

例如，根据cond的值选取xarr或yarr的值，列表推导式写法如下：

```python
result=[(x if c else y) for x,y,c in zip(xarr,yarr,cond)]
```

使用np.where方法的写法如下：

```python
result=np.where(cond,xarr,yarr)
```

在数据分析工作中，np.where通常用于根据另一个数组而产生一个新的数组。例如，有一个由随机数据组成的矩阵，你希望将所有正值替换为2，将所有负值替换为－2；使用np.where，可以将标量和数组结合起来：

```python
arr = np.random.randn(4,4)
res = np.where(arr>0, 2,-2) # 将正值替换为2，负值替换为-2
np.where(arr > 0, 2, arr) # 只将正值替换为2，负值保持不变
```
传递给where的数组大小可以不相等，甚至可以是标量值。

#### 数学和统计方法
可以通过数组上的一组数学函数对整个数组或某个轴向的数据进行统计计算。

sum、mean以及标准差std等聚合计算（aggregation，通常叫做约简（reduction））既可以当做数组的实例方法调用，也可以当做顶级NumPy函数使用。

mean和sum这类的函数可以接受一个axis选项参数，用于计算该轴向上的统计值，最终结果是一个少一维的数组：

```python
arr = np.random.randn(5, 4)
arr.mean(axis=1)
```

其他如cumsum和cumprod之类的方法则不聚合，而是产生一个由中间结果组成的数组。

在多维数组中，累加函数（如cumsum）返回的是同样大小的数组，但是会根据每个低维的切片沿着标记轴计算部分聚类。

基本数组统计方法：

| 方法           | 说明                                                   |
| -------------- | ------------------------------------------------------ |
| sum            | 对数组中的全部或某轴向的元素求和。零长度的数组的sum为0 |
| mean           | 算术平均数。零长度的数组的mean为NaN                    |
| std, var       | 分别为标准差和方差，自由度可调（默认为n）              |
| min, max       | 最小值和最大值                                         |
| argmin, argmax | 分别为最小和最大元素的索引                             |
| cumsum         | 所有元素的累计和                                       |
| cumprod        | 所有元素的累计积                                       |

#### 用于布尔型数组的方法
布尔值会被强制转换为1（True）和0（False）。因此，sum经常被用来对布尔型数组中的True值计数。另外还有两个方法any和all，它们对布尔型数组非常有用。any用于测试数组中是否存在一个或多个True，而all则检查数组中所有值是否都是True。这两个方法也能用于非布尔型数组，所有非0元素将会被当做True。

#### 排序
跟Python内置的列表类型一样，NumPy数组也可以通过sort方法就地排序。顶级方法np.sort返回的是数组的已排序副本，而就地排序则会修改数组本身。

多维数组可以在任何一个轴向上进行排序，只需将轴编号传给sort即可。

计算数组分位数最简单的办法是对其进行排序，然后选取特定位置的值：

```python
large_arr = np.random.randn(1000)
large_arr.sort()
large_arr[int(0.05 * len(large_arr))] # 5% quantile
```

#### 唯一化以及其它的集合逻辑

NumPy提供了一些针对一维ndarray的基本集合运算。

最常用的可能要数np.unique了，它用于找出数组中的唯一值并返回已排序的结果，拿跟np.unique等价的纯Python代码来对比一下：

```python
np.unique(names)
sorted(set(names))
```

另一个函数np.in1d用于测试一个数组中的值在另一个数组中的成员资格，返回一个布尔型数组：

```python
values = np.array([6, 0, 0, 3, 2, 5, 6])
np.in1d(values, [2, 3, 6])
```

NumPy提供一些针对一维数组的基本集合运算。

  * unique(x)  计算x中的唯一元素，并返回有序结果
  * intersect1d(x,y)  计算x和y中的公共元素，并返回有序结果
  * union1d(x,y)  计算x和y中的并集，并返回有序结果
  * in1d(x,y)  返回一个“x的元素是否在y中”的布尔型数组
  * setdiff1d(x,y)  集合的差，即元素在x中且不在y中
  * setxor1d(x,y)  集合的对称差，即元素在一个数组中而不在另一个数组中，或称为异或

### 4.4 用于数组的文件输入输出

NumPy能够读写磁盘上的文本数据或二进制数据。更多的用户会使用pandas或其它工具加载文本或表格数据。

将数据以二进制格式读取和保存磁盘文件的两个主要的函数：np.save和np.load。默认情况下，数组是以未压缩的原始二进制格式保存在扩展名为.npy的文件中的。

```python
arr = np.arange(10)
np.save('some_array', arr)
np.load('some_array.npy')
```

通过np.savez可以将多个数组保存到一个未压缩文件中，将数组以关键字参数的形式传入即可。加载.npz文件时，你会得到一个类似字典的对象，该对象会对各个数组进行延迟加载：

```python
np.savez('array_archive.npz', a=arr, b=arr)
arch = np.load('array_archive.npz')
arch['b']
```

如果要将数据压缩，可以使用numpy .savez_compressed。

将数据以文本文件格式读取和保存磁盘文件的两个主要的函数：np.loadtxt和np.savetxt。

### 4.5 代数运算

线性代数运算（如矩阵乘法、矩阵分解、行列式以及其他方阵数学等）是任何数组库的重要组成部分。不像某些语言（如MA TLAB），通过`*`对两个二维数组相乘得到的是一个元素级的积，而不是一个矩阵点积。因此，NumPy提供了一个用于矩阵乘法的dot函数（既是一个数组方法也是numpy命名空间中的一个函数）。

@符（类似Python	3.5）也可以用作中缀运算符，进行矩阵乘法。

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

### 4.6 伪随机数生成

numpy .random模块对Python内置的random进行了补充，增加了一些用于高效生成多种概率分布的样本值的函数。

这些都是伪随机数，是因为它们都是通过算法基于随机数生成器种子，在确定性的条件下生成的。你可以用NumPy的np.random.seed更改随机数生成种子。

numpy .random的数据生成函数使用了全局的随机种子。要避免全局状态，你可以使用numpy .random.RandomState，创建一个与其它隔离的随机数生成器。

numpy.random子库包含的部分函数：

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

### 4.7 示例：随机漫步

一个简单的随机漫步的例子：从0开始，步长1和－1出现的概率相等。

下面是一个通过内置的random模块以纯Python的方式实现1000步的随机漫步：

```python
import random
position = 0
walk = [position]
steps = 1000
for i in range(steps):
    step = 1 if random.randint(0, 1) else -1
    position += step
    walk.append(position)
plt.figure()
plt.plot(walk[:100])
```

这其实就是随机漫步中各步的累计和，可以用一个数组运算来实现。用np.random模块一次性随机产生1000个“掷硬币”结果（即两个数中任选一个），将其分别设置为1或－1，然后计算累计和：

```python
nsteps = 1000
draws = np.random.randint(0, 2, size=nsteps)
steps = np.where(draws > 0, 1, -1)
walk = steps.cumsum()
```

首次穿越时间，即随机漫步过程中第一次到达某个特定值的时间。假设我们想要知道本次随机漫步需要多久才能距离初始0点至少10步远（任一方向均可）。np.abs(walk)>=10可以得到一个布尔型数组，它表示的是距离是否达到或超过10，而我们想要知道的是第一个10或－10的索引。可以用argmax来解决这个问题，它返回的是该布尔型数组第一个最大值的索引（True就是最大值）。

```python
(np.abs(walk) >= 10).argmax()
```

#### 一次模拟多个随机漫步
如果你希望模拟多个随机漫步过程（比如5000个），只需对上面的代码做一点点修改即可生成所有的随机漫步过程。只要给numpy .random的函数传入一个二元元组就可以产生一个二维数组，然后我们就可以一次性计算5000个随机漫步过程（一行一个）的累计和了：

```python
nwalks = 5000
nsteps = 1000
draws = np.random.randint(0, 2, size=(nwalks, nsteps)) # 0 or 1
steps = np.where(draws > 0, 1, -1)
walks = steps.cumsum(1)
```

计算30或－30的最小穿越时间，因为不是5000个过程都到达了30，不能直接在walks上取绝对值，否则未穿越的随机漫步（行）也会参加统计。可以用any方法来对此进行检查，利用这个布尔型数组选出那些穿越了30（绝对值）的随机漫步（行），并调用argmax在轴1上获取穿越时间：

```python
hits30 = (np.abs(walks) >= 30).any(1) # Walks that hit 30 or -30
crossing_times = (np.abs(walks[hits30]) >= 30).argmax(1)
```

## 第5章 Pandas入门

pandas是基于NumPy数组构建的，特别是基于数组的函数和不使用for循环的数据处理。虽然pandas采用了大量的NumPy编码风格，但二者最大的不同是pandas是专门为处理表格和混杂数据设计的。而NumPy更适合处理统一的数值数组数据。

Pandas库的引入约定：

    import pandas as pd
    from pandas import Series, DataFrame

### 5.1 Pandas的数据结构介绍

Pandas的基础是两个主要的数据结构：Series，DataFrame

#### Series

Series是一种类似于一维数组的对象，它由一组数据（各种NumPy数据类型）以及一组与之相关的数据标签（即索引）组成。

创建Series对象：

    Series(data=None, index=None, dtype=None)

Series的表现形式为：索引在左边，值在右边。如果未指定数据索引，自动创建从0到n-1（N为数据的长度）的整数型索引。通过Series的values和index属性获取其数组表示的数据和索引对象。

通常，所创建的Series带有一个可以对各个数据点进行标记的索引，与普通NumPy数组相比，可以通过索引的方式选取Series中的单个或一组值：

```python
obj = pd.Series([4, 7, -5, 3], index=['d', 'b', 'a', 'c'])
obj2['d'] = 6
```

使用NumPy函数或类似NumPy的运算（如根据布尔型数组进行过滤、标量乘法、应用数学函数等）都会保留索引值的链接。

还可以将Series看成是一个定长的有序字典，因为它是索引值到数据值的一个映射。它可以用在许多原本需要字典参数的函数中。如果数据被存放在一个Python字典中，也可以直接通过这个字典来创建Series。如果只传入一个字典，则结果Series中的索引就是原字典的键（有序排列）。你可以传入排好序的字典的键以改变顺序：

```python
sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
obj1 = pd.Series(sdata)
states = ['California', 'Ohio', 'Oregon', 'Texas']
obj2 = pd.Series(sdata, index=states)
```

sdata中跟states索引相匹配的那3个值会被找出来并放到相应的位置上，但由于"California"所对应的sdata值找不到，所以其结果就为NaN。因为‘Utah’不在states中，它被从结果中除去。

Pandas使用NaN（Not a Number）即“非数字”，表示缺失数据或空值（NA），使用isnull和notnull函数检测缺失数据。

Series最重要的一个功能是在算术运算中会自动对齐不同索引的数据。

Series对象本身及其索引都有一个name属性，该属性跟pandas其他的关键功能关系非常密切。

Series的索引可以通过赋值的方式就地修改。

#### DataFrame

DataFrame是一个表格型的数据结构，包含一组有序的列，每列有不同的值类型（数值、字符串、布尔值等）。DataFrame既有行索引也有列索引，可以看做由Series组成的字典（共用同一索引）。其中数据是以一个或多个二维块存放的（而不是列表、字典或别的一维数据结构）。

创建DataFrame对象：

    DataFrame(data=None, index=None, columes=None, dtype=None)

建DataFrame的办法有很多，最常用的一种是直接传入一个由等长列表或NumPy数组组成的字典，DataFrame会自动加上索引（跟Series一样），且全部列会被有序排列：

```python
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada', 'Nevada'],
        'year': [2000, 2001, 2002, 2001, 2002, 2003],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9, 3.2]}
frame = pd.DataFrame(data)
```

对于特别大的DataFrame，head方法会选取前五行。

如果指定了列序列，则DataFrame的列就会按照指定顺序进行排列。如果传入的列在数据中找不到，就会在结果中产生缺失值。

```python
frame2 = pd.DataFrame(data, columns=['year', 'state', 'pop', 'debt'],
                      index=['one', 'two', 'three', 'four', 'five', 'six'])
frame2['state']
frame2.year
```

通过类似字典标记的方式或属性的方式，可以将DataFrame的列获取为一个Series，返回的Series拥有原DataFrame相同的索引，且其name属性也已经被相应地设置好了。注意：frame[column]适用于任何列的名，但是frame.column只有在列名是一个合理的Python变量名时才适用。

列可以通过赋值的方式进行修改。将列表或数组赋值给某个列时，其长度必须跟DataFrame的长度相匹配。如果赋值的是一个Series，就会精确匹配DataFrame的索引，所有的空位都将被填上缺失值。

为不存在的列赋值会创建出一个新列。关键字del用于删除列。

通过索引方式返回的列只是相应数据的视图而已，并不是副本。因此，对返回的Series所做的任何就地修改全都会反映到源DataFrame上。通过Series的copy方法即可指定复制列。

如果使用嵌套字典创建DataFrame，pandas就会解释为：外层字典的键作为列，内层键则作为行索引。内层字典的键会被合并、排序以形成最终的索引。如果明确指定了索引，则不会这样。

```python
pop = {'Nevada': {2001: 2.4, 2002: 2.9},
       'Ohio'  : {2000: 1.5, 2001: 1.7, 2002: 3.6}}
frame1 = pd.DataFrame(pop)
frame2 = pd.DataFrame(pop, index=[2001, 2002, 2003])
```

你也可以使用类似NumPy数组的方法，对DataFrame进行转置（交换行和列）。

DataFrame构造函数所能接受的各种数据：

* 二维ndarray  数据矩阵，还可以传入行标和列标
* 由数组、列表或元组组成的字典  每个序列会变成DataFrame的一列，所有的序列长度必须相同
* NumPy的结构化/记录数组  类似于“由数组组成的字典”
* 由Series组成的字典  每个Series会成为一列。如果没有显式指定索引，则各Series的索引会被合并成结果的行索引
* 由字典组成的字典  各内层字典会成为一列。键会被合并成结果的行索引，跟“由Series组成的字典”的情况一样
* 字典或Series的列表  各项将会成为DataFrame的一行。字典键或Series索引的并集将会成为DataFrame的列标
* 由列表或元组组成的列表  类似于“二维ndarray”
* 另一个DataFame  该DataFrame的索引将会被沿用，除非显式指定了其他索引
* Numpy的MaskedArray   类似于“二维ndarray”的情况，只是掩码值在结果DataFrame会变成NA/缺失值

通过DataFrame的values、index、columns属性获取其二维ndarray数组表示的数据、行、列的索引对象，其索引对象都有name属性。

#### 索引对象

索引对象负责管理轴标签和其他元数据（比如轴名称等）。构建Series和DataFrame时，所有数组或序列的标签都会被转换成一个索引对象。

索引对象是不可修改的。不可变的特性可以使Index对象在多个数据结构之间安全共享。

除了类似于数组，索引对象的功能也类似一个固定大小的集合，与python的集合不同，pandas的Index可以包含重复的标签。

Pandas中主要的索引对象：

- Index  泛化的索引对象，一个有Python对象组成的Numpy数组
- Int64Index  整数索引对象
- MulitIndex  层次化索引对象，表示单个轴上的多层索引。可以看做由元组组成的数组
- DatatimeIndex  存储纳秒级的时间戳（用Numpy的datetime64类型表示）
- PerieodIndex  Period数据（时间间隔）索引对象

索引对象主要的属性方法如下：

  * append  连接另一个索引对象，产生一个新的索引对象
  * difference  计算差集，并得到一个索引对象
  * intersection  计算交集
  * union  计算并集
  * isin  计算索引对象值是否包含在参数集合中，返回布尔型数组
  * delete  删除索引i处的元素，并得到新的索引对象
  * drop  删除传入的值，并得到新的索引对象
  * insert  将元素插入到索引i处，并得到新的索引对象
  * is_monotonic  当各元素均大于等于前一个元素时，返回True
  * is_unique  当索引对象没有重复值时，返回True
  * unique  返回索引对象唯一值的数组

### 5.2 基本功能

#### 重新索引

Pandas对象的reindex方法按照新索引重排对象，如果索引值不存在，使用NaN填充。

    reindex(index=None,columns=None,method=None,fill_value=nan)

reindex函数的各参数及说明：

* index  用作索引的新序列。既可以是index实例，也可以是其他序列型的Python数据结构。
* method  插值（填充）方式
* fill_value  在重新索引的过程中，需要引入缺失值时使用的替代值
* limit  向前或向后填充时的最大填充量
* tolerence  向前或向后填充时，填充不准确匹配项的最大间距（绝对值距离）
* level  在MulitiIndex的指定级别上匹配简单索引，否则选取其子集
* copy  默认为True，无论如何都复制；如果为False，则新旧相等就不复制

对于时间序列这样的有序数据，重新索引时可能需要做一些插值处理。method选项即可达到此目的，例如，使用ffill可以实现前向值填充：

```python
obj = pd.Series(['blue', 'purple', 'yellow'], index=[0, 2, 4])
obj.reindex(range(6), method='ffill')
```

对DataFrame，reindex可以修改（行）索引和列。只传递一个序列时，会重新索引结果的行，列可以用columns关键字重新索引：

```python
frame = pd.DataFrame(np.arange(9).reshape((3, 3)),
                     index=['a', 'c', 'd'],
                     columns=['Ohio', 'Texas', 'California'])
frame2 = frame.reindex(['a', 'b', 'c', 'd'])
states = ['Texas', 'Utah', 'California']
frame.reindex(columns=states)
```

对于单调排列的索引进行填充处理时，method选项指定填充方法，如果同时重新索引行和列，填充只能按行进行。

  * ffill or pad  前向填充值
  * bfill or backfill  后向填充值

#### 丢弃指定轴上的项

使用drop方法丢弃某条轴上的一个或多个项，需要传入一个索引数组或列表。

由于需要执行一些数据整理和集合逻辑，所以drop方法返回的是一个在指定轴上删除了指定值的新对象。

用标签序列调用drop会从行标签（axis	0）删除值，通过传递axis=1或axis='columns'可以删除列的值：

```python
data = pd.DataFrame(np.arange(16).reshape((4, 4)),
                    index=['Ohio', 'Colorado', 'Utah', 'New York'],
                    columns=['one', 'two', 'three', 'four'])
data.drop(['Colorado', 'Ohio'])
data.drop('two', axis=1)
data.drop(['two', 'four'], axis='columns')
```

#### 索引、选取和过滤

Series索引（obj[...]）的工作方式类似与Numpy数组的索引，只不过Series的索引不只是整数。

利用标签的切片运算与普通的Python切片运算不同，其末端是包含的（即封闭的）。

用一个值或序列对DataFrame进行索引就是获取一个或多个列。特殊情况是：

* 通过切片或布尔型数据选取行；向[	]传递单一的元素或列表，就可选择列。
* 通过布尔型DataFrame进行索引，选择True位置的值，False位置的值为NaN；对其进行赋值，只赋True位置上的值，False位置上的值不变。

```python
data[[True, False, False, False]]  # 选取行
data[2:]  # 选取行
data['one']  # 选取列
data[['one', 'three']]  # 选取列
```

#### 用loc和iloc进行选取

为了在DataFrame的行上进行标签索引，引入专门的索引对象loc[]和iloc[]，可以用类似NumPy的标记，从DataFrame中选取行和列的子集。其中loc[]使用标签或布尔型数组，iloc[]使用整数索引值。

```python
data.loc['Colorado', ['two', 'three']]
data.iloc[2, [3, 0, 1]]
```

这两个索引对象也适用于一个标签或多个标签的切片。

注意：loc和iloc是两个索引对象，重载了`__getitem__`函数，可以使用loc[]和iloc[]方式，分别处理基于标签和整数的索引。ix[]将废弃。

DataFrame的索引方法总结：

* obj[val]  选取单个列或一组列。特殊情况：布尔型数组（过滤行）、切片（行切片），或布尔型DataFrame（根据条件设置值）
* obj.loc[val]  通过标签，选取单个行或一组行
* obj.loc[:, val]  通过标签，选取单个列或列子集
* obj.loc[val1, val2]  通过标签，同时选取行和列
* obj.iloc[where]  通过整数位置，选取单个行或一组行
* obj.iloc[:, where]  通过整数位置，选取单个列或列子集
* obj.iloc[where_i, where_j]  通过整数位置，同时选取行和列
* obj.at[label_i, label_j]  通过行和列标签，选取单一的元素
* obj.iat[i, j]  通过行和列的整数位置，选取单一的元素
* obj.reindex()  将一个或多个轴匹配到新的索引
* obj.xs()  根据标签选取单个或单列

注：obj.get_value(), obj.set_value()两个方法 根据标签选取和设置单个值将废弃，应使用at[]和iat[]运算符

#### 整数索引
pandas对象的整数索引与Python内置的列表和元组的索引语法不同。

```python
ser1 = pd.Series(np.arange(3.))
ser1[-1]  # 出错，没有-1这个索引
ser2 = pd.Series(np.arange(3.), index=['a', 'b', 'c'])
ser2[-1]  # 返回2.0，通过索引'c'访问
```

如果轴索引含有整数，数据选取总会使用标签。为了更准确，应使用loc（标签）或iloc（整数）。

#### 算术运算和数据对齐

Pandas可以对不同索引的对象进行算术运算。对象相加时，如果存在不同的索引对，则结果的索引就是该索引对的并集。

自动的数据对齐操作在不重叠的索引处引入NaN值，缺失值在算术运算过程中传播。

对于DataFrame，对齐操作会同时发生在行和列上。如果DataFrame对象相加，没有共用的列或行标签，结果都会是空。

#### 在算术方法中填充值
在对不同索引的对象进行算术运算时，你可能希望当一个对象中某个轴标签在另一个对象中找不到时填充一个特殊值（比如0）

使用对象算术运算方法（add, sub, div, mul等），传入fill_value可以使用指定值进行填充。

在对Series或DataFrame重新索引时，也可以指定一个填充值。

Series和DataFrame的算术方法包括：add, sub, div, floordiv, mul, power。它们每个都有一个副本，以字母r开
头，它会翻转参数。

#### DataFrame和Series之间的运算

默认情况下，DataFrame和Series之间的运算会将Series的索引匹配到DataFrame的列，然后沿着行向下广播。

如果某个索引值在DataFrame的列或Series的索引中找不到，则参与运算的两个对象就会被重新索引以形成并集。

如果希望匹配行且在列上广播，则必须使用算术运算方法，传入希望匹配的轴。

#### 函数应用与映射

Numpy的元素级数组方法也可以用于Pandas对象。

另一种常见的操作是将自定义的函数应用到各行或列形成的一维数组上，使用apply方法。默认在每列执行，结果是一个Series，使用DataFrame的列作为索引。如果传递axis='columns'到apply，则函数会在每行执行。

传递到apply的函数不是必须返回一个标量，还可以返回由多个值组成的Series。

在DataFrame对象上应用元素级函数使用applymap方法，在Series应用元素级函数使用map方法。

#### 排序和排名

对行或列索引进行排序（按字典顺序），使用sort_index方法，返回一个已排序的新对象。axis指定排序轴，ascending指定升序还是降序。

按照某个标签对值进行排序，使用其sort_values方法。by指定排序的数据标签，按行排序指定列标签，按列排序指定行标签。排序时缺失值默认放在尾部。

排名（Ranking）返回一个排名值，从1开始，到有效数据长度。默认情况下，rank是通过“为各组分配一个平均排名”的方式破坏平级关系的。可以根据某种规则破坏平级关系，这些规则包括：

  * average  默认，在相等分组中，为各个值分配平均排名
  * min  整个分组中的最小排名
  * max  整个分组中的最大排名
  * first  按值在原始数据中出现的顺序分配排名
  * dense  与min类似，但每个分组中排名总是按1递增

#### 带有重复值的轴标签

轴标签的唯一性并不是强制性的。索引对象的is_uniques属性表明它是否是唯一的。

对带有重复值的索引进行选取时，会返回所有匹配的数据，Series数据类型会返回Series数据，DataFrame数据类型会返回DataFrame数据。

### 5.3 汇总和计算描述统计

Pandas常用的统计方法，大部分属于约简和汇总统计，用于从Series提取单个值或从DataFrame的行或列中提取一个Series。跟对应的NumPy数组方法相比，它们都是基于没有缺失数据的假设而构建的。

NA值会自动被排除，除非整个切片都是NA。通过skipna选项可以禁用该功能

常用的约简方法选项有：

  * axis  约简的轴，行为0或index，列为1或column
  * skipna  排除缺失值，默认为True
  * level  如果轴是层次化索引（即MultiIndex），则根据level分组约简

Pandas所有与描述统计相关的方法：

* count  非NA值的数量
* describe  对Series或DataFrame各列计算汇总统计
* min, max  计算最小值和最大值
* argmin, argmax  计算能够获取到最小值和最大值的索引位置（整数）
* idxmin, idxmax  计算能够获取到最小值和最大值的索引
* quantile  计算样本的分位数（0到1）
* sum  值的总和
* mean  值的平均数
* median  值的算术中位数（50%分位数）
* mad  根据平均值计算平均绝对离差
* var  样本值的方差
* std  样本值的标准差
* skew  样本值的偏度（三阶矩）
* kurt  样本值的峰度（四阶矩）
* cumsum  样本值的累计和
* cummin, cummax  样本值的累计最小值和最大值
* cumprod  样本值的的累计积
* diff  计算一阶差分（对时间序列很有用）
* pct_change  计算百分数变化

#### 相关系数与协方差
有些汇总统计（如相关系数和协方差）是通过参数对计算出来的。

Series的corr方法用于计算两个Series中重叠的、非NA的、按索引对齐的值的相关系数。与此类似，cov用于计算协方差。

DataFrame的corr和cov方法将以DataFrame的形式分别返回完整的相关系数或协方差矩阵。

利用DataFrame的corrwith方法，你可以计算其列或行跟另一个Series或DataFrame之间的相关系数。传入一个Series将会返回一个相关系数值Series（针对各列进行计算）；传入一个DataFrame则会计算按列名配对的相关系数。

传入axis='columns'即可按行进行计算。无论如何，在计算相关系数之前，所有的数据项都会按标签对齐。

#### 唯一值、值计数以及成员资格
还有一类方法可以从一维Series的值中抽取信息。

函数unique，可以得到Series中的唯一值数组，返回的唯一值是未排序的，如果需要的话，可以对结果再次进行排序（uniques.sort()）。

函数value_counts用于计算一个Series中各值出现的频率，结果Series是按值频率降序排列的。value_counts还是一个顶级pandas方法，可用于任何数组或序列。

isin用于判断矢量化集合的成员资格，可用于过滤Series中或DataFrame列中数据的子集。

与isin类似的是Index.get_indexer方法，它可以给你一个索引数组，从可能包含重复值的数组到另一个不同值的数组。

```python
to_match = pd.Series(['c', 'a', 'b', 'b', 'c', 'a'])
unique_vals = pd.Series(['c', 'b', 'a'])
pd.Index(unique_vals).get_indexer(to_match)
```

Index.get_indexer方法根据当前索引，提取目标索引的索引器，对当前值应用索引器，使当前值与新索引对齐。函数返回一个整数数组，整数从0到n-1，表示当前索引与目标索引匹配的位置，如果不匹配，则为-1。

```python
indexer = index.get_indexer(new_index)
new_values = cur_values.take(indexer)
```

常用的一些方法：

* isin  计算一个表示“Series各值是否包含于传入的值序列中”的布尔型数组
* get_indexer  计算一个数组中的各值到另一个不同值数组的整数索引
* unique  计算Series中的唯一值数组，按发现的顺序返回
* value_counts  返回Series，其索引为唯一值，其值为频率，按计算值降序排列

如果要得到DataFrame中多个相关列的一张柱状图，将pandas.value_counts传给该DataFrame的apply函数即可。结果中的行标签是所有列的唯一值。后面的频率值是每个列中这些值的相应计数。

```python
data = pd.DataFrame({'Qu1': [1, 3, 4, 3, 4],
                     'Qu2': [2, 3, 1, 2, 3],
                     'Qu3': [1, 5, 2, 4, 4]})
result = data.apply(pd.value_counts).fillna(0)
```

## 第6章 数据加载、存储和文件格式
输入输出通常可以划分为几个大类：读取文本文件和其他更高效的磁盘存储格式，加载数据库中的数据，利用Web	API操作网络资源。

### 6.1	读写文本格式的数据

pandas提供了一些用于将表格型数据读取为DataFrame对象的函数。下表对它们进行了总结，其中read_csv和read_table可能会是你今后用得最多的。

* read_csv  从文件、URL、文件型对象中加载带分隔符的数据。默认分隔符为逗号
* read_table  从文件、URL、文件型对象中加载带分隔符的数据。默认分隔符为制表符（`\t`）
* read_fwf  读取定宽列格式数据（也就是说，没有分隔符）
* read_clipboard  读取剪贴板中的数据，可以看做read_table的剪贴板版。在将网页转换为表格时很有用
* read_excel  从Excel的XLS或XLSX文件中读取表格数据
* read_hdf  读取pandas写的HDF5文件
* read_html  读取HTML文档中的所有表格
* read_json 读取JSON字符串中的数据
* read_msgpack  读取二进制格式编码的pandas数据
* read_pickle  读取Python Pickle格式中存储的任意对象
* read_sas  读取存储于SAS系统自定义存储格式的SAS数据集
* read_sql  （使用SQLAlchemy）读取SQL查询结果为DataFrame
* read_stata  读取Stata文件格式的数据集
* read_feather  读取Feather二进制文件格式

这些函数的选项可以划分为以下几个大类：

* 索引：将一个或多个列当做返回的DataFrame处理，以及是否从文件、用户获取列名。
* 类型推断和数据转换：包括用户定义值的转换、和自定义的缺失值标记列表等。
* 日期解析：包括组合功能，比如将分散在多个列中的日期时间信息组合成结果中的单个列。
* 迭代：支持对大文件进行逐块迭代。
* 不规整数据问题：跳过一些行、页脚、注释或其他一些不重要的东西（比如由成千上万个逗号隔开的数值数据）。

读取一个以逗号分隔的（CSV）文本文件，如果文件没有标题行，可以让pandas为其分配默认的列名，也可以自己定义。

```python
pd.read_csv('examples/ex2.csv', header=None)
pd.read_csv('examples/ex2.csv', names=['a', 'b', 'c', 'd', 'message'])
```

如果希望将message列做成DataFrame的索引，可以通过index_col参数指定"message"。如果希望将多个列做成一个层次化索引，只需传入由列编号或列名组成的列表即可。

```python
names = ['a', 'b', 'c', 'd', 'message']
pd.read_csv('examples/ex2.csv', names=names, index_col='message')
parsed = pd.read_csv('examples/csv_mindex.csv', index_col=['key1', 'key2'])
```

有些情况下，有些表格可能不是用固定的分隔符去分隔字段的（比如空白符或其它模式）。这里的字段是被数量不同的空白字符间隔开的。这种情况下，你可以传递一个正则表达式作为read_table的分隔符。可以用正则表达式表达为`\s+`，于是有：

```python
result = pd.read_table('examples/ex3.txt', sep='\s+')
```

这里，由于列名比数据行的数量少，所以read_table推断第一列应该是DataFrame的索引。

缺失值处理是文件解析任务中的一个重要组成部分。缺失数据经常是要么没有（空字符串），要么用某个标记值表示。默认情况下，pandas会用识别一些经常出现的标记值，比如NA及NULL。参数na_values可以用一个列表或集合的字符串表示缺失值，甚至可以用字典指定各列使用不同的NA标记值。

```python
result = pd.read_csv('examples/ex5.csv', na_values=['NULL'])
sentinels = {'message': ['foo', 'NA'], 'something': ['two']}
pd.read_csv('examples/ex5.csv', na_values=sentinels)
```

pandas.read_csv和pandas.read_table常用的选项：

* path  表示文件系统位置、URL、文件型对象的字符串
* sep或delimiter  用于对行中各字段进行拆分的字符序列或正则表达式
* header  用作列名的行号。默认为0（第一行）如果没有header行，就应该设置为None
* index_col  用作行索引的列编号或列名。可以是单个名称/数字或多个名称/数字组成的列表（层次化索引）
* names  用于结果的列名列表，结合header=None
* skiprows  需要忽略的行数（从文件开始处算起），或需要跳过的行号列表（从0开始）
* na_values  一组用于标记NA的值
* comments  用于将注释信息从行尾拆分出去的字符（一个或多个）
* parse_dates  尝试将数据解析为日期，默认为False。如果为True，则尝试解析所有列。此外，还可以指定需要解析的一组列号或列名。如果列表的元素为列表或元组，就会将多个列组合到一起在进行日期解析工作（例如，日期/时间分别位于两个列中）。
* keep_date_col  如果连接多列解析日期，则保持参与连接的列。默认为False。
* converters  由列号/列名跟函数之间的映射关系组成的字典。例如，{'foo':f}会对foo列的所有值应用函数f
* dayfirst  当解析有歧义的日期时，将其看做国际格式（例如，7/6/2020 -> June 7, 2020）。默认为False
* date_parser  用于解析日期的函数
* nrows  需要读取的行数（从文件开始处算起）
* iterator  返回一个TextParser以便逐块读取文件
* chunksize  文件块的大小（用于迭代）
* skip_footer  需要忽略的行数（从文件末尾处算起）
* verbose  打印各种解析器输出信息，比如“非数值列中缺失值的数量”等
* encoding  用于unicode的文本编码格式。例如，"utf-8"表示UTF-8编码的文本
* squeeze  如果数据经解析后仅含一列，则返回Series
* thousands  千分位分隔符，如，","或"."

#### 逐块读取文本文件

在处理很大的文件时，或找出大文件中的参数集以便于后续处理时，你可能只想读取文件的一小部分或逐块对文件进行迭代。

在看大文件之前，我们先设置pandas显示地更紧些：

如果只想读取几行（避免读取整个文件），通过nrows进行指定即可。要逐块读取文件，可以指定chunksize（行数）。read_csv返回一个TextParser对象使你可以根据chunksize对文件进行逐块迭代。比如，可以迭代处理ex6.csv，将值计数聚合到"key"列中，如下所示

```python
pd.options.display.max_rows = 10
pd.read_csv('examples/ex6.csv', nrows=5)
chunker = pd.read_csv('examples/ex6.csv', chunksize=1000)
tot = pd.Series([])
for piece in chunker:
    tot = tot.add(piece['key'].value_counts(), fill_value=0)
tot = tot.sort_values(ascending=False)
```

T extParser还有一个get_chunk方法，它使你可以读取任意大小的块。

#### 将数据写出到文本格式

利用DataFrame的to_csv方法，我们可以将数据写到一个以逗号分隔的文件中，当然，还可以使用其他分隔符。缺失值在输出结果中会被表示为空字符串。你可能希望将其表示为别的标记值。如果没有设置其他选项，则会写出行和列的标签。当然，它们也都可以被禁用。还可以只写出一部分的列，并以你指定的顺序排列。

```python
data.to_csv('examples/out.csv', sep='|')
data.to_csv('examples/out.csv', na_rep='NULL')
data.to_csv('examples/out.csv', index=False, header=False)
data.to_csv('examples/out.csv', index=False, columns=['a', 'b', 'c'])
```

#### 处理分隔符格式
大部分存储在磁盘上的表格型数据都能用pandas.read_table进行加载。然而，对于含有畸形行的文件，有时
还是需要做一些手工处理。

对于任何单字符分隔符文件，可以直接使用Python内置的csv模块。将任意已打开的文件或文件型的对象传给csv .reader。对这个reader进行迭代将会为每行产生一个元组（并移除了所有的引号）。

```python
import csv
f = open('examples/ex7.csv')
reader = csv.reader(f)
for line in reader:
    print(line)
```

对数据格式做一些整理工作。首先，读取文件到一个多行的列表中；然后，我们将这些行分为标题行和数据行；然后，我们可以用字典构造式和zip(*values)，后者将行转置为列，创建数据列的字典；

```python
with open('examples/ex7.csv') as f:
    lines = list(csv.reader(f))
header, values = lines[0], lines[1:]
data_dict = {h: v for h, v in zip(header, zip(*values))}

```

CSV文件的形式有很多。只需定义csv .Dialect的一个子类即可定义出新格式（如专门的分隔符、字符串引用约定、行结束符等）。各个CSV语义的参数也可以用关键字的形式提供给csv .reader，而无需定义子类。

```python
class my_dialect(csv.Dialect):
    lineterminator = '\n'
    delimiter = ';'
    quotechar = '"'
    quoting = csv.QUOTE_MINIMAL
reader = csv.reader(f, dialect=my_dialect)
reader = csv.reader(f, delimiter='|')
```

可用的选项（csv .Dialect的属性）：

* delimiter  用于分隔字段的单字符字符串。默认为","
* lineterminator  用于写操作的行结束符，默认为"\r\n"。读操作将忽略此选项，它能识别出跨平台的行结束符
* quotechar  用于带有特殊字符（如分隔符）的字段的引用符号。默认为'"'
* quoting  引用约定。可选值包括csv.QUOTE_ALL（引用所有字段），csv.QUOTE_MINIMAL（只引用带有诸如分隔符之类特殊字符字段），csv.QUOTE_NONNUMBERIC（引用所有非数字字段）以及csv.QUOTE_NONE（不引用）
* skipinitialspace  忽略分隔符后面的空白符。默认为False
* doublequote  如何处理字段内的应用符号。如果为True，则双写，如果为False，则要使用转义符。默认为True。
* escapechar  用于对分隔符进行转义的字符串（如果quoting被设置为csv.QUOTE_NONE的话）。默认禁用

对于那些使用复杂分隔符或多字符分隔符的文件，csv模块就无能为力了。这种情况下，就只能使用字符串的split方法或正则表达式方法re.split进行行拆分和其他整理工作了。

要手工输出分隔符文件，可以使用csv .writer。它接受一个已打开且可写的文件对象以及跟csv .reader相同的那些语义和格式化选项：

```python
with open('mydata.csv', 'w') as f:
    writer = csv.writer(f, dialect=my_dialect)
    writer.writerow(('one', 'two', 'three'))
    writer.writerow(('1', '2', '3'))
    writer.writerow(('4', '5', '6'))
    writer.writerow(('7', '8', '9'))
```

#### JSON数据
JSON（JavaScript Object Notation的简称）已经成为通过HTTP请求在Web浏览器和其他应用程序之间发送数据的标准格式之一。它是一种比表格型文本格式（如CSV）灵活得多的数据格式。例如：

```python
obj = """
{"name": "Wes",
 "places_lived": ["United States", "Spain", "Germany"],
 "pet": null,
 "siblings": [{"name": "Scott", "age": 30, "pets": ["Zeus", "Zuko"]},
              {"name": "Katie", "age": 38,
               "pets": ["Sixes", "Stache", "Cisco"]}]
}
"""
```

除其空值null和一些其他的细微差别（如列表末尾不允许存在多余的逗号）之外，JSON非常接近于有效的Python代码。基本类型有对象（字典）、数组（列表）、字符串、数值、布尔值以及null。对象中所有的键都必须是字符串。

许多Python库都可以读写JSON数据。使用Python标准库json，通过json.loads即可将JSON字符串转换成Python形式。json.dumps则将Python对象转换成JSON格式。

```python
import json
result = json.loads(obj)
asjson = json.dumps(result)
```

如何将（一个或一组）JSON对象转换为DataFrame或其他便于分析的数据结构，最简单方便的方式是：向DataFrame构造器传入一个字典的列表（就是原先的JSON对象），并选取数据字段的子集。

```python
siblings = pd.DataFrame(result['siblings'], columns=['name', 'age'])
```

pandas.read_json可以自动将特别格式的JSON数据集转换为Series或DataFrame。pandas.read_json的默认选项假设JSON数组中的每个对象是表格中的一行。如果需要将数据从pandas输出到JSON，可以使用to_json方法。

```python
data = pd.read_json('examples/example.json')
data.to_json('out.json')
data.to_json('out.json', orient='records')
```

#### XML和HTML：Web信息收集

Python有许多可以读写常见的HTML和XML格式数据的库，包括lxml、Beautiful Soup和html5lib。lxml的速度比较快，但其它的库处理有误的HTML或XML文件更好。

pandas有一个内置的功能，read_html，它可以使用lxml和Beautiful Soup自动将HTML文件中的表格解析为DataFrame对象。例如，读取美国联邦存款保险公司下载的一个HTML文件，它记录了银行倒闭的情
况，然后做一些数据清洗和分析，比如计算按年份计算倒闭的银行数：

```python
tables = pd.read_html('examples/fdic_failed_bank_list.html')
failures = tables[0]
close_timestamps = pd.to_datetime(failures['Closing Date'])
close_timestamps.dt.year.value_counts()
```

#### 利用lxml.objectify解析XML
XML（Extensible Markup Language）是另一种常见的支持分层、嵌套数据以及元数据的结构化数据格式。

利用lxml从XML格式解析数据。纽约大都会运输署发布的有关其公交和列车服务的数据，其中每条XML记录就是一条月度数据。

先用lxml.objectify解析该文件，然后通过getroot得到该XML文件的根节点的引用。

```python
from lxml import objectify
path = 'examples/Performance_MNR.xml'
parsed = objectify.parse(open(path))
root = parsed.getroot()   
```

root.INDICATOR返回一个用于产生各个XML元素的生成器。对于每条记录，可以用标记名和数据值填充一个字典（排除几个标记）。

```python
data = []
skip_fields = ['PARENT_SEQ', 'INDICATOR_SEQ', 'DESIRED_CHANGE', 'DECIMAL_PLACES']
for elt in root.INDICATOR:
    el_data = {}
    for child in elt.getchildren():
        if child.tag in skip_fields:
            continue
        el_data[child.tag] = child.pyval
    data.append(el_data)   
```

最后，将这组字典转换为一个DataFrame。

```python
perf = pd.DataFrame(data)
perf.head()
```

XML数据可以很复杂，每个标记都可以有元数据。例如下面这个HTML的链接标签（它也算是一段有效的XML）。

```python
from io import StringIO
tag = '<a href="http://www.google.com">Google</a>'
root = objectify.parse(StringIO(tag)).getroot()
root.get('href')
root.text
```

### 6.2 二进制数据格式

实现数据的高效二进制格式存储最简单的办法之一是使用Python内置的pickle序列化。

pandas对象都有一个用于将数据以pickle格式保存到磁盘上的to_pickle方法。可以通过pickle直接读取被pickle化的数据，或是使用更为方便的pandas.read_pickle。

pickle仅建议用于短期存储格式。其原因是很难保证该格式永远是稳定的；今天pickle的对象可能无法被后续版本的库unpickle出来。

pandas内置支持两个二进制数据格式：HDF5和MessagePack。pandas或NumPy数据的其它存储格式有：

* bcolz：一种可压缩的列存储二进制格式，基于Blosc压缩库。
* Feather：一种跨语言的列存储文件格式，使用了Apache Arrow的列式内存格式。

#### 使用HDF5格式
HDF5是一种存储大规模科学数组数据的非常好的文件格式。它可以被作为C标准库，带有许多语言的接口，如Java、Python和MA TLAB等。HDF5中的HDF指的是层次型数据格式（hierarchical data format）。每个HDF5文件都含有一个文件系统式的节点结构，它使你能够存储多个数据集并支持元数据。与其他简单格式相比，HDF5支持多种压缩器的即时压缩，还能更高效地存储重复模式数据。对于那些非常大的无法直接放入内存的数据集，HDF5就是不错的选择，因为它可以高效地分块读写。

虽然可以用PyTables或h5py库直接访问HDF5文件，pandas提供了更为高级的接口，可以简化存储Series和DataFrame对象。HDFStore类可以像字典一样，处理低级的细节。HDF5文件中的对象可以通过与字典一样的API进行获取：

```python
frame = pd.DataFrame({'a': np.random.randn(100)})
store = pd.HDFStore('mydata.h5')
store['obj1'] = frame
store['obj1_col'] = frame['a']
store['obj1']
```

HDFStore支持两种存储模式，'fixed'和'table'。后者通常会更慢，但是支持使用特殊语法进行查询操作。

```python
store.put('obj2', frame, format='table')
store.select('obj2', where=['index >= 10 and index <= 15'])
store.close()
```

put是`store['obj2']=frame`方法的显示版本，允许我们设置其它的选项，比如格式。

pandas.read_hdf函数可以快捷使用这些工具：

```python
frame.to_hdf('mydata.h5', 'obj3', format='table')
pd.read_hdf('mydata.h5', 'obj3', where=['index < 5'])
```

如果需要本地处理海量数据，应该好好研究一下PyT ables和h5py。由于许多数据分析问题都是IO密集型（而不是CPU密集型），利用HDF5这样的工具能显著提升应用程序的效率。

注意：HDF5不是数据库。它最适合用作“一次写多次读”的数据集。虽然数据可以在任何时候被添加到文件中，但如果同时发生多个写操作，文件就可能会被破坏。

#### 读取Microsoft	Excel文件
pandas的ExcelFile类或pandas.read_excel函数支持读取存储在Excel 2003（或更高版本）中的表格型数据。这两个工具分别使用扩展包xlrd和openpyxl读取XLS和XLSX文件。

要使用ExcelFile，通过传递xls或xlsx路径创建一个实例。存储在表单中的数据可以read_excel读取到DataFrame。如果要读取一个文件中的多个表单，创建ExcelFile会更快，但你也可以将文件名传递到pandas.read_excel。

```python
xlsx = pd.ExcelFile('examples/ex1.xlsx')
pd.read_excel(xlsx, 'Sheet1')
frame = pd.read_excel('examples/ex1.xlsx', 'Sheet1')
```

如果要将pandas数据写入为Excel格式，你必须首先创建一个ExcelWriter，然后使用pandas对象的to_excel方法将数据写入到其中。你也可以不使用ExcelWriter，而是传递文件的路径到to_excel。

```python
writer = pd.ExcelWriter('examples/ex2.xlsx')
frame.to_excel(writer, 'Sheet1')
writer.save()
frame.to_excel('examples/ex2.xlsx')
```

### 6.3 Web APIs交互
许多网站都有一些通过JSON或其他格式提供数据的公共API。通过Python访问这些API的办法有不少。一个简单易用的办法（推荐）是requests包。

为了搜索最新的30个GitHub上的pandas主题，我们可以发一个HTTP GET请求，使用requests扩展库。响应对象的json方法会返回一个包含被解析过的JSON字典，加载到一个Python对象中。data中的每个元素都是一个包含所有GitHub主题页数据（不包含评论）的字典。可以直接传递数据到DataFrame，并提取感兴趣的字段。

```python
import requests
url = 'https://api.github.com/repos/pandas-dev/pandas/issues'
resp = requests.get(url)
data = resp.json()
issues = pd.DataFrame(data, columns=['number', 'title', 'labels', 'state'])
```

### 6.4 数据库交互
在商业场景下，大多数数据可能不是存储在文本或Excel文件中。基于SQL的关系型数据库（如SQL	Server、PostgreSQL和MySQL等）使用非常广泛，其它一些数据库也很流行。数据库的选择通常取决于性能、数据完整性以及应用程序的伸缩性需求。

将数据从SQL加载到DataFrame的过程很简单，此外pandas还有一些能够简化该过程的函数。例如，使用SQLite数据库，通过Python内置的sqlite3驱动器连接数据库，然后插入几行数据。

```python
import sqlite3
query = """
CREATE TABLE test
(a VARCHAR(20), b VARCHAR(20),
 c REAL,        d INTEGER
);"""
con = sqlite3.connect('mydata.sqlite')
con.execute(query)
con.commit()
data = [('Atlanta', 'Georgia', 1.25, 6),
        ('Tallahassee', 'Florida', 2.6, 3),
        ('Sacramento', 'California', 1.7, 5)]
stmt = "INSERT INTO test VALUES(?, ?, ?, ?)"
con.executemany(stmt, data)
con.commit()
```

从表中选取数据时，大部分Python	SQL驱动器（PyODBC、psycopg2、MySQLdb、pymssql等）都会返回一个元组列表。可以将这个元组列表传给DataFrame构造器，但还需要列名（位于光标的description属性中）。

```python
cursor = con.execute('select * from test')
rows = cursor.fetchall()
pd.DataFrame(rows, columns=[x[0] for x in cursor.description])
```

SQLAlchemy项目是一个流行的Python SQL工具，它抽象出了SQL数据库中的许多常见差异。pandas有一个read_sql函数，可以让你轻松的从SQLAlchemy连接读取数据。

```python
import sqlalchemy as sqla
db = sqla.create_engine('sqlite:///mydata.sqlite')
pd.read_sql('select * from test', db)
```

## 第7章 数据清理和准备

在数据分析和建模的过程中，相当多的时间要用在数据准备上：加载、清理、转换以及重塑。这些工作会占到分析师时间的80%或更多。有时，存储在文件和数据库中的数据的格式不适合某个特定的任务。许多研究者都选择使用通用编程语言（如Python、Perl、R或Java）或UNIX文本处理工具（如sed或awk）对数据格式进行专门处理。幸运的是，pandas和内置的Python标准库提供了一组高级的、灵活的、快速的工具，可以让你轻松地将数据规整为想要的格式。

### 7.1	处理缺失数据
在许多数据分析工作中，缺失数据是经常发生的。pandas的目标之一就是尽量轻松地处理缺失数据。例如，pandas对象的所有描述性统计默认都不包括缺失数据。

缺失数据在pandas中呈现的方式有些不完美，但对于大多数用户可以保证功能正常。对于数值数据，pandas使用浮点值NaN（Not a Number）表示缺失数据。我们称其为哨兵值（ sentinel value ），可以方便的检测出来。

```python
string_data = pd.Series(['aardvark', 'artichoke', np.nan, 'avocado'])
string_data.isnull()
```

在pandas中，我们采用了R语言中的惯用法，即将缺失值表示为NA，它表示不可用not available。在统计应用中，NA数据可能是不存在的数据或者虽然存在，但是没有观察到（例如，数据采集中发生了问题）。当进行数据清洗以进行分析时，最好直接对缺失数据进行分析，以判断数据采集的问题或缺失数据可能导致的偏差。

Python内置的None值在对象数组中也可以作为NA。

一些关于缺失数据处理的函数：

* dropna  根据各标签的值中是否存在缺失值数据对轴标签进行过滤，可通过阈值调节对缺失值的容忍度
* fillna  用指定值或插值方法（如ffill或bfill）填充缺失数据
* isnull  返回一个含有布尔值的对象，这些布尔值表示哪些值是缺失值，该对象的类型与源类型一样
* notnull  isnull的否定式

#### 滤除缺失数据
过滤掉缺失数据的办法有很多种。你可以通过pandas.isnull或布尔索引的手工方法，但dropna可能会更实用一些。对于一个Series，dropna返回一个仅含非空数据和索引值的Series。而对于DataFrame对象，事情就有点复杂了。你可能希望丢弃全NA或含有NA的行或列。dropna默认丢弃任何含有缺失值的行。传入how='all'将只丢弃全为NA的那些行。用这种方式丢弃列，只需传入axis=1即可。

```python
from numpy import nan as NA
data = pd.Series([1, NA, 3.5, NA, 7])
data.dropna()
data[data.notnull()]  # 与上一句等价
data = pd.DataFrame([[1., 6.5, 3.], [1., NA, NA],
                     [NA, NA, NA], [NA, 6.5, 3.]])
cleaned = data.dropna()
data.dropna(how='all')
data[4] = NA
data.dropna(axis=1, how='all')  # 丢弃全NA列
```

另一个滤除DataFrame行的问题涉及时间序列数据。假设你只想留下一部分观测数据，可以用thresh参数实现此目的。

```python
df = pd.DataFrame(np.random.randn(7, 3))
df.iloc[:4, 1] = NA
df.iloc[:2, 2] = NA
df.dropna(thresh=2)
```

#### 填充缺失数据
你可能不想滤除缺失数据（有可能会丢弃跟它有关的其他数据），而是希望通过其他方式填补那些“空洞”。对于大多数情况而言，fillna方法是最主要的函数。

通过一个常数调用fillna就会将缺失值替换为那个常数值。若是通过一个字典调用fillna，就可以实现对不同的列填充不同的值。fillna默认会返回新对象，但也可以对现有对象进行就地修改。对reindexing有效的那些插值方法也可用于fillna。

```python
df.fillna(0)
df.fillna({1: 0.5, 2: 0})
_ = df.fillna(0, inplace=True)
df = pd.DataFrame(np.random.randn(6, 3))
df.iloc[2:, 1] = NA
df.iloc[4:, 2] = NA
df.fillna(method='ffill')
df.fillna(method='ffill', limit=2)
```

可以利用fillna实现许多创新的功能。比如，可以传入Series的平均值或中位数：

```python
data = pd.Series([1., NA, 3.5, NA, 7])
data.fillna(data.mean())
```

fillna函数参数：

* value  用于填充缺失值的标量值或字典对象
* method  插值方式。如果未指定其他参数的话，默认为"ffill"
* axis  待填充的轴，默认为axis=0
* inplace  修改调用者对象而不是产生副本
* limit  对于前向或后向填充，可连续填充的最大数量

### 7.2 数据转换

前面的工作都是数据的重排。另一类重要操作则是过滤、清理以及其他的转换工作。

#### 移除重复数据

DataFrame中经常出现重复行。DataFrame的duplicated方法返回一个布尔型Series，表示各行是否是重复行（前面出现过的行）。还有一个与此相关的drop_duplicates方法，它会返回一个DataFrame，重复的行会被丢弃。这两个方法默认会判断全部列，你也可以指定部分列进行重复项判断。假设我们还有一列值，且只希望根据k1列过滤重复项。duplicated和drop_duplicates默认保留的是第一个出现的值组合。传入keep='last'则保留最后一个。

```python
data = pd.DataFrame({'k1': ['one', 'two'] * 3 + ['two'],
                     'k2': [1, 1, 2, 3, 3, 4, 4]})
data.duplicated()
data.drop_duplicates()
data['v1'] = range(7)
data.drop_duplicates(['k1'])
data.drop_duplicates(['k1', 'k2'], keep='last')
```

#### 利用函数或映射进行数据转换
对于许多数据集，可能希望根据数组、Series或DataFrame列中的值来实现转换工作。

例如下面这组有关肉类的数据，假设想要添加一列表示该肉类食物来源的动物类型。我们先编写一个不同肉类到
动物的映射。

```python
data = pd.DataFrame({'food': ['bacon', 'pulled pork', 'bacon',
                              'Pastrami', 'corned beef', 'Bacon',
                              'pastrami', 'honey ham', 'nova lox'],
                     'ounces': [4, 3, 12, 6, 7.5, 8, 3, 5, 6]})
meat_to_animal = {
  'bacon': 'pig',
  'pulled pork': 'pig',
  'pastrami': 'cow',
  'corned beef': 'cow',
  'honey ham': 'pig',
  'nova lox': 'salmon'
}
```

Series的map方法可以接受一个函数或含有映射关系的字典型对象，但是这里有一个小问题，即有些肉类的首字母大写了，而另一些则没有。因此，还需要使用Series的str.lower方法，将各个值转换为小写。

```python
lowercased = data['food'].str.lower()
data['animal'] = lowercased.map(meat_to_animal)
```

也可以传入一个能够完成全部这些工作的函数。

```python
data['animal']=data['food'].map(lambda x: meat_to_animal[x.lower()])
```

使用map是一种实现元素级转换以及其他数据清理工作的便捷方式。

#### 替换值
利用fillna方法填充缺失数据可以看做值替换的一种特殊情况。map可用于修改对象的数据子集，而replace则提供了一种实现该功能的更简单、更灵活的方式。

例如下面这个Series，-999这个值可能是一个表示缺失数据的标记值。要将其替换为pandas能够理解的NA值，可以利用replace来产生一个新的Series（除非传入inplace=True）。如果你希望一次性替换多个值，可以传入一个由待替换值组成的列表以及一个替换值。要让每个值有不同的替换值，可以传递一个替换列表，或传入的参数也可以是字典。

```python
data = pd.Series([1., -999., 2., -999., -1000., 3.])
data.replace(-999, np.nan)
data.replace([-999, -1000], np.nan)
data.replace([-999, -1000], [np.nan, 0])
data.replace({-999: np.nan, -1000: 0})
```

#### 重命名轴索引
跟Series中的值一样，轴标签也可以通过函数或映射进行转换，从而得到一个新的不同标签的对象。轴还可以被就地修改，而无需新建一个数据结构。

跟Series一样，轴索引也有一个map方法，你可以将其赋值给index，这样就可以对DataFrame进行就地修改。如果想要创建数据集的转换版（而不是修改原始数据），比较实用的方法是rename。特别的，rename可以结合字典型对象实现对部分轴标签的更新。rename可以实现复制DataFrame并对其索引和列标签进行赋值。如果希望就地修改某个数据集，传入inplace=True即可。

```python
data = pd.DataFrame(np.arange(12).reshape((3, 4)),
                    index=['Ohio', 'Colorado', 'New York'],
                    columns=['one', 'two', 'three', 'four'])
transform = lambda x: x[:4].upper()
data.index = data.index.map(transform)
data.rename(index=str.title, columns=str.upper)
data.rename(index={'OHIO': 'INDIANA'},
            columns={'three': 'peekaboo'})
data.rename(index={'OHIO': 'INDIANA'}, inplace=True)
```

#### 离散化和面元划分
为了便于分析，连续数据常常被离散化或拆分为“面元”（bin）。

假设有一组人员数据，希望将它们划分为不同的年龄组。接下来将这些数据划分为“18到25”、“26到35”、“35到60”以及“60以上”几个面元。要实现该功能，使用pandas的cut函数。

```python
ages = [20, 22, 25, 27, 21, 23, 37, 31, 61, 45, 41, 32]
bins = [18, 25, 35, 60, 100]
cats = pd.cut(ages, bins)
```

pandas.cut返回的是一个特殊的Categorical对象，可以将其看做一组表示面元名称的字符串。它的底层含有一个表示不同分类名称的类型数组，以及一个codes属性中的年龄数据的标签。跟“区间”的数学符号一样，圆括号表示开端，而方括号则表示闭端（包括）。哪边是闭端可以通过right=False进行修改。可以通过传递一个列表或数组到labels，设置自己的面元名称。

```python
cats.codes
cats.categories
pd.value_counts(cats)  # 对pandas.cut结果的面元计数
pd.cut(ages, [18, 26, 36, 61, 100], right=False)
group_names = ['Youth', 'YoungAdult', 'MiddleAged', 'Senior']
pd.cut(ages, bins, labels=group_names)
```

如果向cut传入的是面元的数量而不是确切的面元边界，则它会根据数据的最小值和最大值计算等长面元。下面的例子中，将一些均匀分布的数据分成四组，选项precision=2，限定小数只有两位。

```python
data = np.random.rand(20)
pd.cut(data, 4, precision=2)
```

qcut是一个非常类似于cut的函数，它可以根据样本分位数对数据进行面元划分。根据数据的分布情况，cut可能无法使各个面元中含有相同数量的数据点。而qcut由于使用的是样本分位数，因此可以得到大小基本相等的面元。与cut类似，你也可以传递自定义的分位数（0到1之间的数值，包含端点）。

```python
data = np.random.randn(1000)  # Normally distributed
cats = pd.qcut(data, 4)  # Cut into quartiles
pd.value_counts(cats)
pd.qcut(data, [0, 0.1, 0.5, 0.9, 1.])
```

#### 检测和过滤异常值
过滤或变换异常值（outlier）在很大程度上就是运用数组运算。

例如，一个含有正态分布数据的DataFrame，假设想要找出某列中绝对值大小超过3的值。或要选出全部含有“超过3或－3的值”的行，你可以在布尔型DataFrame中使用any方法，根据这些条件，就可以对值进行设置，可以将值限制在区间－3到3以内：

```python
data = pd.DataFrame(np.random.randn(1000, 4))
data.describe()
col = data[2]
col[np.abs(col) > 3]
data[(np.abs(data) > 3).any(1)]
data[np.abs(data) > 3] = np.sign(data) * 3
data.describe()
```

#### 排列和随机采样

利用numpy .random.permutation函数可以轻松实现对Series或DataFrame的列的排列工作（permuting，随机重排序）。

通过需要排列的轴的长度调用permutation，可产生一个表示新顺序的整数数组。然后就可以在基于iloc的索引操作或take函数中使用该数组了。如果不想用替换的方式选取随机子集，可以在Series和DataFrame上使用sample方法。要通过替换的方式产生样本（允许重复选择），可以传递replace=True到sample。

```python
df = pd.DataFrame(np.arange(5 * 4).reshape((5, 4)))
sampler = np.random.permutation(5)
df.take(sampler)
df.sample(n=3)
choices = pd.Series([5, 7, -1, 6, 4])
draws = choices.sample(n=10, replace=True)
```

#### 计算指标/哑变量
另一种常用于统计建模或机器学习的转换方式是：将分类变量（categorical variable）转换为“哑变量”或“指标矩阵”。

如果DataFrame的某一列中含有k个不同的值，则可以派生出一个k列矩阵或DataFrame（其值全为1和0）。pandas有一个get_dummies函数可以实现该功能（其实自己动手做一个也不难）。使用之前的一个DataFrame例子，可能想给指标DataFrame的列加上一个前缀，以便能够跟其他数据进行合并。get_dummies的prefix参数可以实现该功能。有时候，你可能想给指标DataFrame的列加上一个前缀，以便能够跟其他数据进行合并。get_dummies的prefix参数可以实现该功能。

```python
df = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'b'],
                   'data1': range(6)})
pd.get_dummies(df['key'])
dummies = pd.get_dummies(df['key'], prefix='key')
df_with_dummy = df[['data1']].join(dummies)
```

如果DataFrame中的某行同属于多个分类，则事情就会有点复杂。例如下面的MovieLens 1M数据集：

```
 	movie_id 	title 	genres
0 	1 	Toy Story (1995) 	Animation|Children's|Comedy
1 	2 	Jumanji (1995) 	Adventure|Children's|Fantasy
2 	3 	Grumpier Old Men (1995) 	Comedy|Romance
3 	4 	Waiting to Exhale (1995) 	Comedy|Drama
4 	5 	Father of the Bride Part II (1995) 	Comedy
```

要为每个genre添加指标变量就需要做一些数据规整操作。首先，我们从数据集中抽取出不同的genre值；构建指标DataFrame的方法之一是从一个全零DataFrame开始；迭代每一部电影，并将dummies各行的条目设为1。要这么做，使用dummies.columns来计算每个类型的列索引；然后，根据索引，使用.iloc设定值；然后，再将其与movies合并起来。

```python
mnames = ['movie_id', 'title', 'genres']
movies = pd.read_table('examples/movies.dat', sep='::',
                       header=None, names=mnames, engine='python')
all_genres = []
for x in movies.genres:
    all_genres.extend(x.split('|'))
genres = pd.unique(all_genres)
zero_matrix = np.zeros((len(movies), len(genres)))
dummies = pd.DataFrame(zero_matrix, columns=genres)
for i, gen in enumerate(movies.genres):
    indices = dummies.columns.get_indexer(gen.split('|'))
    dummies.iloc[i, indices] = 1
movies_windic = movies.join(dummies.add_prefix('Genre_'))
```

对于很大的数据，用这种方式构建多成员指标变量就会变得非常慢。最好使用更低级的函数，将其写入NumPy数组，然后结果包装在DataFrame中。

一个对统计应用有用的秘诀是：结合get_dummies和诸如cut之类的离散化函数。

```python
np.random.seed(12345)
values = np.random.rand(10)
bins = [0, 0.2, 0.4, 0.6, 0.8, 1]
pd.get_dummies(pd.cut(values, bins))
```

用numpy .random.seed，使这个例子具有确定性。

### 7.3 字符串操作
Python能够成为流行的数据处理语言，部分原因是其简单易用的字符串和文本处理功能。大部分文本运算都直接做成了字符串对象的内置方法。对于更为复杂的模式匹配和文本操作，则可能需要用到正则表达式。pandas对此进行了加强，它使你能够对整组数据应用字符串表达式和正则表达式，而且能处理烦人的缺失数据。

#### 字符串对象方法

对于许多字符串处理和脚本应用，内置的字符串方法已经能够满足要求了。例如，以逗号分隔的字符串可以用split拆分成数段；split常常与strip一起使用，以去除空白符（包括换行符）；利用加法，可以将这些子字符串以双冒号分隔符的形式连接起来；但这种方式并不是很实用。一种更快更符合Python风格的方式是，向字符串"::"的
join方法传入一个列表或元组；检测子串的最佳方式是利用Python的in关键字，还可以使用index和find，注意find和index的区别：如果找不到字符串，index将会引发一个异常（而不是返回－1）；count可以返回指定子串的出现次数；replace用于将指定模式替换为另一个模式。通过传入空字符串，它也常常用于删除模式。

```python
val = 'a,b,  guido'
val.split(',')
pieces = [x.strip() for x in val.split(',')]
first, second, third = pieces
first + '::' + second + '::' + third
'::'.join(pieces)
'guido' in val
val.index(',')
val.find(':')
val.count(',')
val.replace(',', '::')
val.replace(',', '')
```

Python内置的字符串方法：

- count  返回子串在字符串中的出现次数（非重叠）
- endswith, startswith  如果字符串以某个后缀结尾，或某个前缀开头，则返回True
- join  将字符串用作连接其他字符串序列的分隔符
- index  如果在字符串中找到子串，则返回第一个字符所在的位置。如果没有找到，则引发ValueError
- find  如果在字符串中找到子串，则返回第一个发现的子串的第一个字符所在的位置。如果没有找到，则返回-1
- rfind  如果在字符串中找到子串，则返回最后一个发现的子串的第一个字符所在的位置。如果没有找到，则返回-1
- replace  用另一个字符串替换指定的子串
- strip, rstrip, lstrip  去除字符串首尾的空白字符（包括换行符），或单独去除首、尾空白字符
- split  通过指定的分隔符将字符串拆分为一组子串
- lower, upper  将字母转换成小写或大写
- ljust, rjust  用空格（或其他字符）填充字符串的空白侧以返回符合最低宽度的字符串
- casefold  将字符转换为小写，并将任何特定区域的变量字符组合转换成一个通用的可比较形式

#### 正则表达式
正则表达式提供了一种灵活的在文本中搜索或匹配（通常比前者复杂）字符串模式的方式。正则表达式，常称作regex，是根据正则表达式语言编写的字符串。Python内置的re模块负责对字符串应用正则表达式。

re模块的函数可以分为三个大类：模式匹配、替换以及拆分。当然，它们之间是相辅相成的。一个regex描述了需要在文本中定位的一个模式，它可以用于许多目的。

假设我想要拆分一个字符串，分隔符为数量不定的一组空白符（制表符、空格、换行符等）。描述一个或多个空白符的regex是`\s+`；调用`re.split('\s+',text)`时，正则表达式会先被编译，然后再在text上调用其split方法。你可以用re.compile自己编译regex以得到一个可重用的regex对象；如果只希望得到匹配regex的所有模式，则可以使用findall方法。

```python
import re
text = "foo    bar\t baz  \tqux"
re.split('\s+', text)
regex = re.compile('\s+')
regex.split(text)
regex.findall(text)
```

如果打算对许多字符串应用同一条正则表达式，强烈建议通过re.compile创建regex对象。这样将可以节省大量的CPU时间。

match和search跟findall功能类似。findall返回的是字符串中所有的匹配项，而search则只返回第一个匹配项。match更加严格，它只匹配字符串的首部。假设我们有一段文本以及一条能够识别大部分电子邮件地址的正则表达
式。对text使用findall将得到一组电子邮件地址；search返回的是文本中第一个电子邮件地址（以特殊的匹配项对象形式返回）。对于上面那个regex，匹配项对象只能告诉我们模式在原字符串中的起始和结束位置；regex.match则将返回None，因为它只匹配出现在字符串开头的模式。sub方法可以将匹配到的模式替换为指定字符串，并返回所得到的新字符串。

```python
text = """Dave dave@google.com
Steve steve@gmail.com
Rob rob@gmail.com
Ryan ryan@yahoo.com
"""
pattern = r'[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}'
regex = re.compile(pattern, flags=re.IGNORECASE)  # re.IGNORECASE makes the regex case-insensitive
m = regex.search(text)
text[m.start():m.end()]
print(regex.match(text))
print(regex.sub('REDACTED', text))
```

假设你不仅想要找出电子邮件地址，还想将各个地址分成3个部分：用户名、域名以及域后缀。要实现此功能，只需将待分段的模式的各部分用圆括号包起来即可。由这种修改过的正则表达式所产生的匹配项对象，可以通过其groups方法返回一个由模式各段组成的元组。对于带有分组功能的模式，findall会返回一个元组列表。sub还能通过诸如\1、\2之类的特殊符号访问各匹配项中的分组。符号\1对应第一个匹配的组，\2对应第二个匹配的组，以此类推。

```python
pattern = r'([A-Z0-9._%+-]+)@([A-Z0-9.-]+)\.([A-Z]{2,4})'
regex = re.compile(pattern, flags=re.IGNORECASE)
m = regex.match('wesm@bright.net')
m.groups()
regex.findall(text)
print(regex.sub(r'Username: \1, Domain: \2, Suffix: \3', text))
```

正则表达式常用方法：

- findall, finditer  返回字符串中所有的非重叠匹配模式。findall返回的是有所有模式组成的列表，而finditer则通过一个迭代器逐个返回
- match  从字符串起始位置匹配模式，还可以对模式各部分进行分组。如果匹配到模式，则返回一个匹配项对象，否则返回None
- search  扫描整个字符串以匹配模式。如果找到则返回一个匹配项对象。跟match不同，其匹配项可以位于字符串的任意位置，而不仅仅是起始处
- split  根据找到的模式将字符串拆分为数段
- sub, subn  将字符串中所有的（sub）或前n个（subn）模式替换为指定表达式。在替换字符串中可以通过\1，\2等符号表示各分组项

#### pandas的矢量化字符串函数
清理待分析的散乱数据时，常常需要做一些字符串规整化工作。更为复杂的情况是，含有字符串的列有时还含有缺失数据。通过data.map，所有字符串和正则表达式方法都能被应用于（传入lambda表达式或其他函数）各个值，但是如果存在NA（null）就会报错。为了解决这个问题，Series有一些能够跳过NA值的面向数组方法，进行字符串操作。通过Series的str属性即可访问这些方法。例如，可以通过str .contains检查各个电子邮件地址是否含有"gmail"。也可以使用正则表达式，还可以加上任意re选项（如IGNORECASE）。

有两个办法可以实现矢量化的元素获取操作：要么使用str .get，要么在str属性上使用索引。要访问嵌入列表中的元素，我们可以传递索引到这两个函数中。你可以利用这种方法对字符串进行截取。

```python
data = {'Dave': 'dave@google.com', 'Steve': 'steve@gmail.com',
        'Rob': 'rob@gmail.com', 'Wes': np.nan}
data = pd.Series(data)
data.isnull()
data.str.contains('gmail')
pattern = r'([A-Z0-9._%+-]+)@([A-Z0-9.-]+)\.([A-Z]{2,4})'
data.str.findall(pattern, flags=re.IGNORECASE)
matches = data.str.match(pattern, flags=re.IGNORECASE)
matches.str.get(1)
matches.str[0]
data.str[:5]
```

常用的pandas字符串方法：

- cat  实现元素级的字符串连接操作，可指定分隔符
- contains  返回表示字符串是否含有指定模式的布尔型数组
- count  模式在字符串中的出现次数
- extract  使用带分组的正则表达式从字符串Series中提取一个或多个字符串，结果是一个DataFrame，每组一列
- endswith  相当于对每个元素执行x.endswith(pattern)
- startswith  相当于对每个元素执行x.startswith(pattern)
- findall  计算各字符串的模式列表
- get  获取各元素的第i个字符
- isalnum  相当于内置的str.isalnum
- isalpha  相当于内置的str.isalpha
- isdecimal  相当于内置的str.isdecimal
- isdigit  相当于内置的str.isdigit
- islower  相当于内置的str.islower
- isnumeric  相当于内置的str.isnumeric
- isupper  相当于内置的str.isupper
- join  根据指定的分隔符将Series中各元素的字符串连接起来
- len  计算各字符串的长度
- lower, upper 转换成小写或大写。相当于对各个元素执行x.lower或x.upper
- match  根据指定的正则表达式对各个元素执行re.match，返回匹配的组为列表
- pad  在字符串的左边、右边或两边添加空白符
- center  相当于pad(side='both')
- repeat  重复值。例如，s.str.repeat(3)相当于对各个字符串执行x*3
- replace  用指定的字符串替换找到的模式
- slice  对Series中的各个字符串进行子串截取
- split  根据分隔符或正则表达式对字符串进行拆分
- strip  去除字符串首尾的空白字符，包括换行符
- rstrip  去除右边的空白字符
- lstrip  去除左边的空白符

## 第8章 数据规整：聚合、合并和重塑

在许多应用中，数据可能分散在许多文件或数据库中，存储的形式也不利于分析。本章将关注可以聚合、合并、重塑数据的方法。

### 8.1 层次化索引
层次化索引（hierarchical	indexing）是pandas的一项重要功能，它使你能在一个轴上拥有多个（两个以上）索引级别。抽象点说，它使你能以低维度形式处理高维度数据。

例如，创建一个Series，并用一个由列表或数组组成的列表作为索引，结果是经过美化的带有MultiIndex索引的Series的格式。索引之间的“间隔”表示“直接使用上面的标签”。对于一个层次化索引的对象，可以使用所谓的部分索引，使用它选取数据子集的操作更简单。有时甚至还可以在“内层”中进行选取。

```python
data = pd.Series(np.random.randn(9),
                 index=[['a', 'a', 'a', 'b', 'b', 'c', 'c', 'd', 'd'],
                        [1, 2, 3, 1, 3, 1, 2, 2, 3]])
data.index

data['b']
data['b':'c']
data.loc[['b', 'd']]
data.loc[:, 2]
```

层次化索引在数据重塑和基于分组的操作（如透视表生成）中扮演着重要的角色。

例如，可以通过unstack方法将这段数据重新安排到一个DataFrame中。unstack的逆运算是stack。

```python
data.unstack()
data.unstack().stack()
```

对于一个DataFrame，每条轴都可以有分层索引。各层都可以有名字（可以是字符串，也可以是别的Python对象）。如果指定了名称，它们就会显示在控制台输出中。有了部分列索引，因此可以轻松选取列分组。

```python
frame = pd.DataFrame(np.arange(12).reshape((4, 3)),
                     index=[['a', 'a', 'b', 'b'], [1, 2, 1, 2]],
                     columns=[['Ohio', 'Ohio', 'Colorado'],
                              ['Green', 'Red', 'Green']])
frame.index.names = ['key1', 'key2']
frame.columns.names = ['state', 'color']
frame['Ohio']
```

可以单独创建MultiIndex然后复用。上面那个DataFrame中的（带有分级名称）列可以这样创建：

```python
MultiIndex.from_arrays([['Ohio', 'Ohio', 'Colorado'], ['Green', 'Red', 'Green']],
                       names=['state', 'color'])
```

#### 重排与分级排序
有时，需要重新调整某条轴上各级别的顺序，或根据指定级别上的值对数据进行排序。swaplevel接受两个级别编号或名称，并返回一个互换了级别的新对象（但数据不会发生变化）。而sort_index则根据单个级别中的值对数据进行排序。交换级别时，常常也会用到sort_index，这样最终结果就是按照指定顺序进行字母排序了。

```python
frame.swaplevel('key1', 'key2')
frame.sort_index(level=1)
frame.swaplevel(0, 1).sort_index(level=0)
```

#### 根据级别汇总统计
许多对DataFrame和Series的描述和汇总统计都有一个level选项，它用于指定在某条轴上求和的级别。再以上面那个DataFrame为例，我们可以根据行或列上的级别来进行求和。这其实是利用了pandas的groupby功能。

```python
frame.sum(level='key2')
frame.sum(level='color', axis=1)
```

#### 使用DataFrame的列进行索引
人们经常想要将DataFrame的一个或多个列当做行索引来用，或者可能希望将行索引变成DataFrame的列。

以下面这个DataFrame为例，DataFrame的set_index函数会将其一个或多个列转换为行索引，并创建一个新的
DataFrame。默认情况下，那些列会从DataFrame中移除，但也可以将其保留下来。reset_index的功能跟set_index刚好相反，层次化索引的级别会被转移到列里面。

```python
frame = pd.DataFrame({'a': range(7), 'b': range(7, 0, -1),
                      'c': ['one', 'one', 'one', 'two', 'two',
                            'two', 'two'],
                      'd': [0, 1, 2, 0, 1, 2, 3]})
frame2 = frame.set_index(['c', 'd'])
frame.set_index(['c', 'd'], drop=False)
frame2.reset_index()
```

### 8.2 合并数据集
pandas对象中的数据可以通过一些方式进行合并：

- pandas.merge可根据一个或多个键将不同DataFrame中的行连接起来。SQL或其他关系型数据库的用户对此应该会比较熟悉，因为它实现的就是数据库的join操作。
- pandas.concat可以沿着一条轴将多个对象堆叠到一起。
- 实例方法combine_first可以将重复数据拼接在一起，用一个对象中的值填充另一个对象中的缺失值。

#### 数据库风格的DataFrame合并
数据集的合并（merge）或连接（join）运算是通过一个或多个键将行连接起来的。这些运算是关系型数据库（基于SQL）的核心。pandas的merge函数是对数据应用这些算法的主要切入点。

例如下面两个DataFrame，这是一种多对一的合并。df1中的数据有多个被标记为a和b的行，而df2中key列的
每个值则仅对应一行。对这些对象调用merge即可。注意，我并没有指明要用哪个列进行连接。如果没有指定，merge就会将重叠列的列名当做键。不过，最好明确指定一下。

```python
df1 = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'a', 'b'],
                    'data1': range(7)})
df2 = pd.DataFrame({'key': ['a', 'b', 'd'],
                    'data2': range(3)})
pd.merge(df1, df2)
pd.merge(df1, df2, on='key')
```

如果两个对象的列名不同，也可以分别进行指定。默认情况下，merge做的是“内连接”；结果中的键是交集，结果里面c和d以及与之相关的数据消失了。其他方式还有"left"、"right"以及"outer"。外连接求取的是键的并集，组合了左连接和右连接的效果。

```python
df3 = pd.DataFrame({'lkey': ['b', 'b', 'a', 'c', 'a', 'a', 'b'],
                    'data1': range(7)})
df4 = pd.DataFrame({'rkey': ['a', 'b', 'd'],
                    'data2': range(3)})
pd.merge(df3, df4, left_on='lkey', right_on='rkey')
pd.merge(df1, df2, how='outer')
```

merge的合并选项：

- inner  使用两个表中都有的键
- left  使用左表中所有的键
- right  使用右表中所有的键
- outer  使用两个表中所有的键

多对多的合并有些不直观。多对多连接产生的是行的笛卡尔积。由于左边的DataFrame有3个"b"行，右边的有2
个，所以最终结果中就有6个"b"行。连接方式只影响出现在结果中的不同的键的值。

```python
df1 = pd.DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'b'],
                    'data1': range(6)})
df2 = pd.DataFrame({'key': ['a', 'b', 'a', 'b', 'd'],
                    'data2': range(5)})
pd.merge(df1, df2, on='key', how='left')
pd.merge(df1, df2, how='inner')
```

要根据多个键进行合并，传入一个由列名组成的列表即可。

```python
left = pd.DataFrame({'key1': ['foo', 'foo', 'bar'],
                     'key2': ['one', 'two', 'one'],
                     'lval': [1, 2, 3]})
right = pd.DataFrame({'key1': ['foo', 'foo', 'bar', 'bar'],
                      'key2': ['one', 'one', 'one', 'two'],
                      'rval': [4, 5, 6, 7]})
pd.merge(left, right, on=['key1', 'key2'], how='outer')
pd.merge(left, right, on='key1')
pd.merge(left, right, on='key1', suffixes=('_left', '_right'))
```

结果中会出现哪些键组合取决于所选的合并方式，你可以这样来理解：多个键形成一系列元组，并将其当做单个连接键（当然，实际上并不是这么回事）。

注意：在进行列－列连接时，DataFrame对象中的索引会被丢弃。

对于合并运算需要考虑的最后一个问题是对重复列名的处理。虽然你可以手工处理列名重叠的问题（查看前面介绍的重命名轴标签），但merge有一个更实用的suffixes选项，用于指定附加到左右两个DataFrame对象的重叠列名上的字符串：

```python
pd.merge(left, right, on='key1')
pd.merge(left, right, on='key1', suffixes=('_left', '_right'))
```

merge函数的参数：

- left  参与合并的左侧DataFrame
- right  参与合并的右侧DataFrame
- how  合并方式：inner, outer, left, right其中之一。默认为inner
- on  用于连接的列名。必须存在于左右两个DataFrame对象中。如果未指定，且其他连接键也未指定，则以left和right列名的交集作为连接键
- left_on  左侧DataFrame中用作连接键的列
- right_on  右侧DataFrame中用作连接键的列
- left_index  将左侧的行索引用作其连接键
- right_index  将右侧的行索引用作其连接键
- sort  根据连接键对合并后的数据进行排序，默认为True。有时在处理大数据集时，禁用该选项可获得更好的性能
- suffixes  字符串值元组，用于追加到重叠列名的末尾，默认为`('_x', '_y')`
- copy  设置为False，可以在某些特殊情况下避免将数据复制到结果数据结构中。默认总是复制。
- indicator  添加一个特殊的列_merge，指明每一行的数据来源，它的值有：left_only, right_only, both

#### 索引上的合并
有时候，DataFrame中的连接键位于其索引中。在这种情况下，你可以传入left_index=True或right_index=True（或两个都传）以说明索引应该被用作连接键。由于默认的merge方法是求取连接键的交集，因此你可以通过外连接的方式得到它们的并集。

```python
left1 = pd.DataFrame({'key': ['a', 'b', 'a', 'a', 'b', 'c'],
                      'value': range(6)})
right1 = pd.DataFrame({'group_val': [3.5, 7]}, index=['a', 'b'])
pd.merge(left1, right1, left_on='key', right_index=True)
pd.merge(left1, right1, left_on='key', right_index=True, how='outer')
```

对于层次化索引的数据，事情就有点复杂了，因为索引的合并默认是多键合并。这种情况下，你必须以列表的形式指明用作合并键的多个列（注意用how='outer'对重复索引值的处理）。

```python
lefth = pd.DataFrame({'key1': ['Ohio', 'Ohio', 'Ohio',
                               'Nevada', 'Nevada'],
                      'key2': [2000, 2001, 2002, 2001, 2002],
                      'data': np.arange(5.)})
righth = pd.DataFrame(np.arange(12).reshape((6, 2)),
                      index=[['Nevada', 'Nevada', 'Ohio', 'Ohio',
                              'Ohio', 'Ohio'],
                             [2001, 2000, 2000, 2000, 2001, 2002]],
                      columns=['event1', 'event2'])
pd.merge(lefth, righth, left_on=['key1', 'key2'], right_index=True)
pd.merge(lefth, righth, left_on=['key1', 'key2'], right_index=True, how='outer')
```

同时使用合并双方的索引也没问题。

```python
left2 = pd.DataFrame([[1., 2.], [3., 4.], [5., 6.]],
                     index=['a', 'c', 'e'],
                     columns=['Ohio', 'Nevada'])
right2 = pd.DataFrame([[7., 8.], [9., 10.], [11., 12.], [13, 14]],
                      index=['b', 'c', 'd', 'e'],
                      columns=['Missouri', 'Alabama'])
pd.merge(left2, right2, how='outer', left_index=True, right_index=True)
```

DataFrame还有一个便捷的join实例方法，它能更为方便地实现按索引合并。它还可用于合并多个带有相同或相似索引的DataFrame对象，但要求没有重叠的列。

因为一些历史版本的遗留原因，DataFrame的join方法默认使用的是左连接，保留左边表的行索引。它还支持在调用的DataFrame的列上（这里指left1的key列），连接传递的DataFrame索引（这里指right1的索引）。

```python
left2.join(right2, how='outer')
left1.join(right1, on='key')
```

对于简单的索引合并，你还可以向join传入一组DataFrame，更为通用的concat函数，也能实现此功能。

```python
another = pd.DataFrame([[7., 8.], [9., 10.], [11., 12.], [16., 17.]],
                       index=['a', 'c', 'e', 'f'],
                       columns=['New York', 'Oregon'])
left2.join([right2, another])
left2.join([right2, another], how='outer')
```

#### 轴向连接
另一种数据合并运算也被称作连接（concatenation）、绑定（binding）或堆叠（stacking）。

NumPy的concatenation函数可以对NumPy数组来做。

```python
arr = np.arange(12).reshape((3, 4))
np.concatenate([arr, arr], axis=1)
```

对于pandas对象（如Series和DataFrame），带有标签的轴使你能够进一步推广数组的连接运算。具体点说，还需要考虑以下这些东西：

- 如果对象在其它轴上的索引不同，我们应该合并这些轴的不同元素还是只使用交集？

- 连接的数据集是否需要在结果对象中可识别？

- 连接轴中保存的数据是否需要保留？许多情况下，DataFrame默认的整数标签最好在连接时删掉。

pandas的concat函数提供了一种能够解决这些问题的可靠方式。假设有三个没有重叠索引的Series，对这些对象调用concat可以将值和索引粘合在一起。默认情况下，concat是在axis=0上工作的，最终产生一个新的Series。如果传入axis=1，则结果就会变成一个DataFrame（axis=1是列）。这种情况下，另外的轴上没有重叠，从索引的有序并集（外连接）上就可以看出来。传入join='inner'即可得到它们的交集。这样，f和g标签消失了，是因为使用的是join='inner'选项。

```python
s1 = pd.Series([0, 1], index=['a', 'b'])
s2 = pd.Series([2, 3, 4], index=['c', 'd', 'e'])
s3 = pd.Series([5, 6], index=['f', 'g'])
pd.concat([s1, s2, s3])
pd.concat([s1, s2, s3], axis=1)
s4 = pd.concat([s1, s3])
pd.concat([s1, s4], axis=1)
pd.concat([s1, s4], axis=1, join='inner')
```

可以通过join_axes指定要在其它轴上使用的索引。不过有个问题，参与连接的片段在结果中区分不开。假设你想要在连接轴上创建一个层次化索引。使用keys参数即可达到这个目的。如果沿着axis=1对Series进行合并，则keys就会成为DataFrame的列头。

```python
pd.concat([s1, s4], axis=1, join_axes=[['a', 'c', 'b', 'e']])
result = pd.concat([s1, s1, s3], keys=['one', 'two', 'three'])
result.unstack()
pd.concat([s1, s2, s3], axis=1, keys=['one', 'two', 'three'])
```

同样的逻辑也适用于DataFrame对象。如果传入的不是列表而是一个字典，则字典的键就会被当做keys选项的值。此外还有两个用于管理层次化索引创建方式的参数。举个例子，我们可以用names参数命名创建的轴级别。

```python
df1 = pd.DataFrame(np.arange(6).reshape(3, 2), index=['a', 'b', 'c'],
                   columns=['one', 'two'])
df2 = pd.DataFrame(5 + np.arange(4).reshape(2, 2), index=['a', 'c'],
                   columns=['three', 'four'])
pd.concat([df1, df2], axis=1, keys=['level1', 'level2'])
pd.concat({'level1': df1, 'level2': df2}, axis=1)
pd.concat([df1, df2], axis=1, keys=['level1', 'level2'], names=['upper', 'lower'])
```

最后一个关于DataFrame的问题是，DataFrame的行索引不包含任何相关数据。在这种情况下，传入ignore_index=True即可。

```python
df1 = pd.DataFrame(np.random.randn(3, 4), columns=['a', 'b', 'c', 'd'])
df2 = pd.DataFrame(np.random.randn(2, 3), columns=['b', 'd', 'a'])
pd.concat([df1, df2], ignore_index=True)
```

concat函数的参数选项：

- objs  参与连接的Pandas对象的列表或字典。唯一必需的参数
- axis  指明连接的轴向，默认为0
- join  指明其他轴向上的索引是按交集（inner）还是并集（outer）进行合并，默认为outer
- join_axes  指明用于其他n-1条轴的索引，不执行交集或并集运算
- keys  与连接对象有关的值，用于形成连接轴向上的层次化索引。可以是任意值的列表或数组、元组数组、数组列表（如果将levels设置成多级数组的话）
- levels  指定用作层次化索引各级别上的索引，如果设置了keys的话
- names  用于创建分层级别的名称，如果设置了keys和（或）levels的话
- verify_integrity  检查结果对象新轴上的重复情况，如果发现则引发异常。默认（False）允许重复
- ignore_index  不保留连接轴上的索引，产生一组新索引range(total_length)

#### 合并重叠数据
还有一种数据组合问题不能用简单的合并（merge）或连接（concatenation）运算来处理。比如说，你可能有索引全部或部分重叠的两个数据集。

举个有启发性的例子，我们使用NumPy的where函数，它表示一种等价于面向数组的if-else。Series有一个combine_first方法，实现的也是一样的功能，还带有pandas的数据对齐。

```python
a = pd.Series([np.nan, 2.5, np.nan, 3.5, 4.5, np.nan],
              index=['f', 'e', 'd', 'c', 'b', 'a'])
b = pd.Series(np.arange(len(a), dtype=np.float64),
              index=['f', 'e', 'd', 'c', 'b', 'a'])
b[-1] = np.nan
np.where(pd.isnull(a), b, a)
b[:-2].combine_first(a[2:])
```

对于DataFrame，combine_first自然也会在列上做同样的事情，因此你可以将其看做：用传递对象中的数据为调用对象的缺失数据“打补丁”。

```python
df1 = pd.DataFrame({'a': [1., np.nan, 5., np.nan],
                    'b': [np.nan, 2., np.nan, 6.],
                    'c': range(2, 18, 4)})
df2 = pd.DataFrame({'a': [5., 4., np.nan, 3., 7.],
                    'b': [np.nan, 3., 4., 6., 8.]})
df1.combine_first(df2)
```

### 8.3 重塑和轴向旋转
有许多用于重新排列表格型数据的基础运算。这些函数也称作重塑（reshape）或轴向旋转（pivot）运算。

#### 重塑层次化索引

层次化索引为DataFrame数据的重排任务提供了一种具有良好一致性的方式。主要功能有二：

- stack：将数据的列“旋转”为行。
- unstack：将数据的行“旋转”为列。

例如一个简单的DataFrame，其中的行列索引均为字符串数组。对该数据使用stack方法即可将列转换为行，得到一个Series。对于一个层次化索引的Series，你可以用unstack将其重排为一个DataFrame。默认情况下，unstack操作的是最内层（stack也是如此）。传入分层级别的编号或名称即可对其它级别进行unstack操作。

```python
data = pd.DataFrame(np.arange(6).reshape((2, 3)),
                    index=pd.Index(['Ohio', 'Colorado'], name='state'),
                    columns=pd.Index(['one', 'two', 'three'], name='number'))
result = data.stack()
result.unstack()
result.unstack(0)
result.unstack('state')  # 与前一句等价
```

如果不是所有的级别值都能在各分组中找到的话，则unstack操作可能会引入缺失数据。stack默认会滤除缺失数据，因此该运算是可逆的。

```python
s1 = pd.Series([0, 1, 2, 3], index=['a', 'b', 'c', 'd'])
s2 = pd.Series([4, 5, 6], index=['c', 'd', 'e'])
data2 = pd.concat([s1, s2], keys=['one', 'two'])
data2.unstack()
data2.unstack().stack()
data2.unstack().stack(dropna=False)
```

在对DataFrame进行unstack操作时，作为旋转轴的级别将会成为结果中的最低级别。当调用stack，我们可以指明轴的名字。

```python
df = pd.DataFrame({'left': result, 'right': result + 5},
                  columns=pd.Index(['left', 'right'], name='side'))
df.unstack('state')
df.unstack('state').stack('side')
```

#### 将“长格式”旋转为“宽格式”
多个时间序列数据通常是以所谓的“长格式”（long）或“堆叠格式”（stacked）存储在数据库和CSV中的。

先加载一些示例数据，做一些时间序列规整和数据清洗。这就是多个时间序列（或者其它带有两个或多个键的可观察数据，这里，我们的键是date和item）的长格式。表中的每行代表一次观察。

关系型数据库（如MySQL）中的数据经常都是这样存储的，因为固定架构（即列名和数据类型）有一个好处：随着表中数据的添加，item列中的值的种类能够增加。在前面的例子中，date和item通常就是主键（用关系型数据库的说法），不仅提供了关系完整性，而且提供了更为简单的查询支持。有的情况下，使用这样的数据会很麻烦，你可能会更喜欢DataFrame，不同的item值分别形成一列，date列中的时间戳则用作索引。DataFrame的pivot方法完全可以实现这个转换。前两个传递的值分别用作行和列索引，最后一个可选值则是用于填充DataFrame的
数据列。

```python
data = pd.read_csv('examples/macrodata.csv')
data.head()
periods = pd.PeriodIndex(year=data.year, quarter=data.quarter,
                         name='date')
columns = pd.Index(['realgdp', 'infl', 'unemp'], name='item')
data = data.reindex(columns=columns)
data.index = periods.to_timestamp('D', 'end')
ldata = data.stack().reset_index().rename(columns={0: 'value'})
pivoted = ldata.pivot('date', 'item', 'value')
```

假设有两个需要同时重塑的数据列。如果忽略最后一个参数，得到的DataFrame就会带有层次化的列。pivot其实就是用set_index创建层次化索引，再用unstack重塑。

```python
ldata['value2'] = np.random.randn(len(ldata))
pivoted = ldata.pivot('date', 'item')
pivoted[:5]
pivoted['value'][:5]
unstacked = ldata.set_index(['date', 'item']).unstack('item')  # 与前面得到的结果相同
unstacked[:5]
```

#### 将“宽格式”旋转为“长格式”
旋转DataFrame的逆运算是pandas.melt。它不是将一列转换到多个新的DataFrame，而是合并多个列成为一个，产生一个比输入长的DataFrame。例如，key列可能是分组指标，其它的列是数据值。当使用pandas.melt，我们必须指明哪些列是分组指标。下面使用key作为唯一的分组指标。使用pivot，可以重塑回原来的样子。因为pivot的结果从列创建了一个索引，用作行标签，可以使用reset_index将数据移回列。

你还可以指定列的子集，作为值的列。pandas.melt也可以不用分组指标。

```python
df = pd.DataFrame({'key': ['foo', 'bar', 'baz'],
                   'A': [1, 2, 3],
                   'B': [4, 5, 6],
                   'C': [7, 8, 9]})
melted = pd.melt(df, ['key'])
reshaped = melted.pivot('key', 'variable', 'value')
reshaped.reset_index()

pd.melt(df, id_vars=['key'], value_vars=['A', 'B'])
pd.melt(df, value_vars=['A', 'B', 'C'])
pd.melt(df, value_vars=['key', 'A', 'B'])
```

## 第9章 绘图和可视化

信息可视化（也叫绘图）是数据分析中最重要的工作之一。它可能是探索过程的一部分，例如，帮助我们找出异常值、必要的数据转换、得出有关模型的idea等。另外，做一个可交互的数据可视化也许是工作的最终目标。Python有许多库进行静态或动态的数据可视化，但这里重要关注于matplotlib（http://matplotlib.org/）和基
于它的库。

matplotlib是一个用于创建出版质量图表的桌面绘图包（主要是2D方面）。该项目是由John Hunter于2002年启动的，其目的是为Python构建一个MA TLAB式的绘图接口。matplotlib和IPython社区进行合作，简化了从IPython	shell（包括现在的Jupyter	notebook）进行交互式绘图。matplotlib支持各种操作系统上许多不同的
GUI后端，而且还能将图片导出为各种常见的矢量（vector）和光栅（raster）图：PDF、SVG、JPG、PNG、BMP、GIF等。

随着时间的发展，matplotlib衍生出了多个数据可视化的工具集，它们使用matplotlib作为底层。其中之一是seaborn（http://seaborn.pydata.org/）。

### 9.1 matplotlib API入门
虽然seaborn这样的库和pandas的内置绘图函数能够处理许多普通的绘图任务，但如果需要自定义一些高级功能的话就必须学习matplotlib API。

在Jupyter中运行`%matplotlib notebook`（或在IPython中运行`%matplotlib`），就可以进入交互式绘图模式。

#### Figure和Subplot
matplotlib的图像都位于Figure对象中。你可以用plt.figure创建一个新的Figure。plt.figure有一些选项，特别是figsize，它用于确保当图片保存到磁盘时具有一定的大小和纵横比。

不能通过空Figure绘图，必须用add_subplot创建一个或多个subplot才行。

```python
fig = plt.figure()
# 图像应该是2×2的（即最多4张图），且当前选中的是4个subplot中的第一个（编号从1开始）
ax1 = fig.add_subplot(2, 2, 1)
ax2 = fig.add_subplot(2, 2, 2)
ax3 = fig.add_subplot(2, 2, 3)
```

如果这时执行一条绘图命令，matplotlib就会在最后一个用过的subplot上进行绘制，如果没有则创建一个，隐藏创建figure和subplot的过程。"k--"是一个线型选项，用于告诉matplotlib绘制黑色虚线图。上面那些由
fig.add_subplot所返回的对象是AxesSubplot对象，直接调用它们的实例方法就可以
在其它空着的格子里面画图了。

```python
plt.plot(np.random.randn(50).cumsum(), 'k--')
ax1.hist(np.random.randn(100), bins=20, color='k', alpha=0.3)
ax2.scatter(np.arange(30), np.arange(30) + 3 * np.random.randn(30))
plt.close('all')
```

创建包含subplot网格的figure是一个非常常见的任务，matplotlib有一个更为方便的方法plt.subplots，它可以创建一个新的Figure，并返回一个含有已创建的subplot对象的NumPy数组。

```python
fig, axes = plt.subplots(2, 3)
```

这是非常实用的，因为可以轻松地对axes数组进行索引，就好像是一个二维数组一样，例如axes[0,1]。你还可以通过sharex和sharey指定subplot应该具有相同的X轴或Y轴。在比较相同范围的数据时，这也是非常实用的，否则，matplotlib会自动缩放各图表的界限。

plt.subplots的参数选项：

- nrows  subplot的行数
- ncols  subplot的列数
- sharex  所有subplot应该使用相同的X轴刻度（调节xlim将会影响所有subplot）
- sharey  所有subplot应该使用相同的Y轴刻度（调节ylim将会影响所有subplot）
- subplot_kw  用于创建各subplot的关键字字典
- `**fig_kw`  创建figure时的其他关键字，如plt.subplots(2,2,figsize=(8,6))

#### 调整subplot周围的间距
默认情况下，matplotlib会在subplot外围留下一定的边距，并在subplot之间留下一定的间距。间距跟图像的高度和宽度有关，因此，如果你调整了图像大小（不管是编程还是手工），间距也会自动调整。利用Figure的subplots_adjust方法可以轻而易举地修改间距，此外，它也是个顶级函数。

```
subplots_adjust(left=None, bottom=None, right=None, top=None,
                wspace=None, hspace=None)
```

wspace和hspace用于控制宽度和高度的百分比，可以用作subplot之间的间距。下面的例子，将间距收缩到了0，不难看出，其中的轴标签重叠了。matplotlib不会检查标签是否重叠，所以对于这种情况，只能自己设定刻度位置和刻度标签。

```python
fig, axes = plt.subplots(2, 2, sharex=True, sharey=True)
for i in range(2):
    for j in range(2):
        axes[i, j].hist(np.random.randn(500), bins=50, color='k', alpha=0.5)
plt.subplots_adjust(wspace=0, hspace=0)
```

#### 颜色、标记和线型
matplotlib的plot函数接受一组X和Y坐标，还可以接受一个表示颜色和线型的字符串缩写。这种在一个字符串中指定颜色和线型的方式非常方便。在实际中，如果你是用代码绘图，你可能不想通过处理字符串来获得想要的格式。也可以使用关键字参数得到同样的效果。

常用的颜色可以使用颜色缩写，你也可以指定颜色码（例如，'#CECECE'）。你可以通过查看plot的文档字符串查看所有线型的合集（在IPython和Jupyter中使用plot?）。

```
ax.plot(x, y, 'g--')
ax.plot(x, y, linestyle='--', color='g')
```

线图可以使用标记强调数据点。因为matplotlib可以创建连续线图，在点之间进行插值，因此有时可能不太容易看出真实数据点的位置。标记也可以放到格式字符串中，但标记类型和线型必须放在颜色后面。

```python
from numpy.random import randn
plt.plot(randn(30).cumsum(), 'ko--')
plot(randn(30).cumsum(), color='k', linestyle='dashed', marker='o')  # 与上一句效果相同
```

在线型图中，非实际数据点默认是按线性方式插值的。可以通过drawstyle选项修改。

```python
data = np.random.randn(30).cumsum()
plt.plot(data, 'k--', label='Default')
plt.plot(data, 'k-', drawstyle='steps-post', label='steps-post')
plt.legend(loc='best')
```

这里，因为我们传递了label参数到plot，我们可以创建一个plot图例，指明每条使用plt.legend的线。必须调用plt.legend（或使用ax.legend，如果引用了轴的话）来创建图例，无论你绘图时是否传递label标签选项。

#### 刻度、标签和图例
对于大多数的图表装饰项，其主要实现方式有二种：

- 使用过程型的pyplot接口，例如，matplotlib.pyplot
- 更为面向对象的原生matplotlib API

pyplot接口的设计目的就是交互式使用，含有诸如xlim、xticks和xticklabels之类的方法。它们分别控制图表的范围、刻度位置、刻度标签等。其使用方式有以下两种：

- 调用时不带参数，则返回当前的参数值（例如，plt.xlim()返回当前的X轴绘图范围）。
- 调用时带参数，则设置参数值（例如，plt.xlim([0,10])会将X轴的范围设置为0到10）。

所有这些方法都是对当前或最近创建的AxesSubplot起作用的。它们各自对应subplot对象上的两个方法，以xlim为例，就是ax.get_xlim和ax.set_xlim。

使用subplot的实例方法，使事情更明确，而且在处理多个subplot时这样也更清楚一些。当然完全可以选择任意一种方法。

#### 设置标题、轴标签、刻度以及刻度标签

为了说明自定义轴，创建一个简单的图像并绘制一段随机漫步。改变x轴刻度，最简单的办法是使用set_xticks和set_xticklabels。前者告诉matplotlib要将刻度放在数据范围中的哪些位置，默认情况下，这些位置也就是刻度
标签。但我们可以通过set_xticklabels将任何其他的值用作标签。其中，rotation选项设定x刻度标签倾斜30度。最后，再用set_xlabel为X轴设置一个名称，并用set_title设置一个标题。

```python
fig = plt.figure()
ax = fig.add_subplot(1, 1, 1)
ax.plot(np.random.randn(1000).cumsum())
ticks = ax.set_xticks([0, 250, 500, 750, 1000])
labels = ax.set_xticklabels(['one', 'two', 'three', 'four', 'five'],
                            rotation=30, fontsize='small')
ax.set_title('My matplotlib plot')
ax.set_xlabel('Stages')
```

Y轴的修改方式与此类似，只需将上述代码中的x替换为y即可。轴的类有集合方，可以批量设定绘图选项。前面的例子，也可以写为：

```python
props = {
    'title': 'My first matplotlib plot',
    'xlabel': 'Stages'
}
ax.set(**props)
```

#### 添加图例
图例（legend）是另一种用于标识图表元素的重要工具。添加图例的方式有多种。最简单的是在添加subplot的时候传入label参数。在此之后，你可以调用ax.legend()或plt.legend()来自动创建图例。

```python
from numpy.random import randn
fig = plt.figure(); ax = fig.add_subplot(1, 1, 1)
ax.plot(randn(1000).cumsum(), 'k', label='one')
ax.plot(randn(1000).cumsum(), 'k--', label='two')
ax.plot(randn(1000).cumsum(), 'k.', label='three')
ax.legend(loc='best')
```

legend方法其它的loc位置参数选项查看文档字符串（使用ax.legend?）。

loc告诉matplotlib要将图例放在哪。如果你不是吹毛求疵的话，"best"是不错的选择，因为它会选择最不碍事的位置。要从图例中去除一个或多个元素，不传入label或传入label='nolegend'即可。

#### 注解以及在Subplot上绘图
除标准的绘图类型，你可能还希望绘制一些子集的注解，可能是文本、箭头或其他图形等。注解和文字可以通过text、arrow和annotate函数进行添加。

text可以将文本绘制在图表的指定坐标(x,y)，还可以加上一些自定义格式：

```python
ax.text(x, y, 'Hello world!',
        family='monospace', fontsize=10)
```

注解中可以既含有文本也含有箭头。例如，我们根据最近的标准普尔500指数价格（来自Y ahoo!Finance）绘制一张曲线图，并标出2008年到2009年金融危机期间的一些重要日期。

```python
from datetime import datetime
fig = plt.figure()
ax = fig.add_subplot(1, 1, 1)
data = pd.read_csv('examples/spx.csv', index_col=0, parse_dates=True)
spx = data['SPX']
spx.plot(ax=ax, style='k-')
crisis_data = [
    (datetime(2007, 10, 11), 'Peak of bull market'),
    (datetime(2008, 3, 12), 'Bear Stearns Fails'),
    (datetime(2008, 9, 15), 'Lehman Bankruptcy')
]
for date, label in crisis_data:
    ax.annotate(label, xy=(date, spx.asof(date) + 75),
                xytext=(date, spx.asof(date) + 225),
                arrowprops=dict(facecolor='black', headwidth=4, width=2, headlength=4),
                horizontalalignment='left', verticalalignment='top')
# Zoom in on 2007-2010
ax.set_xlim(['1/1/2007', '1/1/2011'])
ax.set_ylim([600, 1800])
ax.set_title('Important dates in the 2008-2009 financial crisis')
```

这张图中有几个重要的点要强调：ax.annotate方法可以在指定的x和y坐标轴绘制标签。我们使用set_xlim和set_ylim人工设定起始和结束边界，而不使用matplotlib的默认方法。最后，用ax.set_title添加图标标题。

图形的绘制要麻烦一些。matplotlib有一些表示常见图形的对象。这些对象被称为块（patch）。其中有些（如Rectangle和Circle），可以在matplotlib.pyplot中找到，但完整集合位于matplotlib.patches。

要在图表中添加一个图形，你需要创建一个块对象shp，然后通过ax.add_patch(shp)将其添加到subplot中。

```python
fig = plt.figure(figsize=(12, 6))
ax = fig.add_subplot(1, 1, 1)
rect = plt.Rectangle((0.2, 0.75), 0.4, 0.15, color='k', alpha=0.3)
circ = plt.Circle((0.7, 0.2), 0.15, color='b', alpha=0.3)
pgon = plt.Polygon([[0.15, 0.15], [0.35, 0.4], [0.2, 0.6]], color='g', alpha=0.5)
ax.add_patch(rect)
ax.add_patch(circ)
ax.add_patch(pgon)
```

如果查看许多常见图表对象的具体实现代码，你就会发现它们其实就是由块patch组装而成的。

#### 将图表保存到文件
利用plt.savefig可以将当前图表保存到文件。该方法相当于Figure对象的实例方法savefig。

文件类型是通过文件扩展名推断出来的。因此，如果你使用的是.pdf，就会得到一个PDF文件。在发布图片时最常用到两个重要的选项是dpi（控制“每英寸点数”分辨率）和bbox_inches（可以剪除当前图表周围的空白部分）。例如，要得到一张带有最小白边且分辨率为400DPI的PNG图片。savefig并非一定要写入磁盘，也可以写入任何文件型的对象，比如BytesIO。

```python
plt.savefig('figpath.svg')
plt.savefig('figpath.png', dpi=400, bbox_inches='tight')

from io import BytesIO
buffer = BytesIO()
plt.savefig(buffer)
plot_data = buffer.getvalue()
```

savefig的其它选项：

- fname  含有文件路径的字符串或Python的文件型对象。图像格式由文件扩展名推断得出
- dpi  图像分辨率（每英寸点数），默认为100
- facecolor, edgecolor  图像背景颜色，默认为'w'（白色）
- format  显式设置文件格式（'png', 'pdf', 'svg', 'ps', 'eps'...）
- bbox_inches  图表需要保存的部分。如果设置为'tight'，则将尝试剪除图表周围的空白部分

#### matplotlib配置
matplotlib自带一些配色方案，以及为生成出版质量的图片而设定的默认配置信息。幸运的是，几乎所有默认行为都能通过一组全局参数进行自定义，它们可以管理图像大小、subplot边距、配色方案、字体大小、网格类型等。一种Python编程方式配置系统的方法是使用rc方法。例如，要将全局的图像默认大小设置为10×10。

rc的第一个参数是希望自定义的对象，如'figure'、'axes'、'xtick'、'ytick'、'grid'、'legend'等。其后可以跟上一系列的关键字参数。一个简单的办法是将这些选项写成一个字典。

```python
plt.rc('figure', figsize=(10, 10))
font_options = {'family' : 'monospace',
                'weight' : 'bold',
                'size'   : 'small'}
plt.rc('font', **font_options)
```

要了解全部的自定义选项，请查阅matplotlib的配置文件matplotlibrc（位于matplotlib/mpl-data目录中）。如果对该文件进行了自定义，并将其放在你自己的.matplotlibrc目录中，则每次使用matplotlib时就会加载该文件。

### 9.2 使用pandas和seaborn绘图
matplotlib实际上是一种比较低级的工具。要绘制一张图表，你组装一些基本组件就行：数据展示（即图表类型：线型图、柱状图、盒形图、散布图、等值线图等）、图例、标题、刻度标签以及其他注解型信息。

在pandas中，我们有多列数据，还有行和列标签。pandas自身就有内置的方法，用于简化从DataFrame和Series绘制图形。另一个库[seaborn](https://seaborn.pydata.org/)，由Michael Waskom创建的静态图形库。Seaborn简化了许多常见可视类型的创建。

提示：引入seaborn会修改matplotlib默认的颜色方案和绘图类型，以提高可读性和美观度。即使你不使用seaborn API，你可能也会引入seaborn，作为提高美观度和绘制常见matplotlib图形的简化方法。

#### 线型图
Series和DataFrame都有一个用于生成各类图表的plot方法。默认情况下，它们所生成的是线型图。例如：

```python
s = pd.Series(np.random.randn(10).cumsum(), index=np.arange(0, 100, 10))
s.plot()
```

该Series对象的索引会被传给matplotlib，并用以绘制X轴。可以通过use_index=False禁用该功能。X轴的刻度和界限可以通过xticks和xlim选项进行调节，Y轴就用yticks和ylim。

plot参数的完整列表：

- label  用于图例的标签
- ax  要在其上进行绘图的matplotlib subplot对象。如果没有设置，则使用当前matplotlib subplot
- style  将要传给matplotlib的风格字符串
- alpha  图表的填充不透明度（0到1之间）
- kind  可以是'line', 'bar', 'barh', 'kde'
- logy  在Y轴上使用对数标尺
- use_index  将对象的索引用作刻度标签
- rot  旋转刻度标签（0到360度）
- xticks  用作X轴刻度的值
- yticks  用作Y轴刻度的值
- xlim  X轴的界限
- ylim  Y轴的界限
- grid  显示轴网格线（默认打开）

pandas的大部分绘图方法都有一个可选的ax参数，它可以是一个matplotlib的subplot对象。这使你能够在网格布局中更为灵活地处理subplot的位置。

DataFrame的plot方法会在一个subplot中为各列绘制一条线，并自动创建图例。

```python
df = pd.DataFrame(np.random.randn(10, 4).cumsum(0),
                  columns=['A', 'B', 'C', 'D'],
                  index=np.arange(0, 100, 10))
df.plot()
```

plot属性包含一批不同绘图类型的方法。例如，df.plot()等价于df.plot.line()。

plot的其他关键字参数会被传给相应的matplotlib绘图函数，所以要更深入地自定义图表，就必须学习更多有关matplotlib	API的知识。

DataFrame.plot还有一些用于对列进行灵活处理的选项，例如，是要将所有列都绘制到一个subplot中还是创建各自的subplot。详细信息如下：

- subplots  将各个DataFrame列绘制到单独的subplot中
- sharex  如果subplots=True，则共用同一个X轴，包括刻度和界限
- sharey  如果subplots=True，则共用同一个Y轴
- figsize  表示图像大小的元组
- title  表示图像标题的字符串
- legend  添加一个subplot图例
- sort_columns  以字母顺序绘制各列，默认使用当前列顺序

#### 柱状图
plot.bar()和plot.barh()分别绘制水平和垂直的柱状图。这时，Series和DataFrame的索引将会被用作X（bar）或Y（barh）刻度。

```python
np.random.seed(12345)
fig, axes = plt.subplots(2, 1)
data = pd.Series(np.random.rand(16), index=list('abcdefghijklmnop'))
data.plot.bar(ax=axes[0], color='k', alpha=0.7)
data.plot.barh(ax=axes[1], color='k', alpha=0.7)
```

color='k'和alpha=0.7设定了图形的颜色为黑色，并使用部分的填充透明度。

对于DataFrame，柱状图会将每一行的值分为一组，并排显示。DataFrame各列的名称"Genus"被用作了图例的标题。设置stacked=True即可为DataFrame生成堆积柱状图，这样每行的值就会被堆积在一起。

```python
df = pd.DataFrame(np.random.rand(6, 4),
                  index=['one', 'two', 'three', 'four', 'five', 'six'],
                  columns=pd.Index(['A', 'B', 'C', 'D'], name='Genus'))
df.plot.bar()
df.plot.barh(stacked=True, alpha=0.5)
```

柱状图有一个非常不错的用法：利用value_counts图形化显示Series中各值的出现频率，比如s.value_counts().plot.bar()。

以有关小费的数据集为例，假设我们想要做一张堆积柱状图以展示每天各种聚会规模的数据点的百分比。首先用read_csv将数据加载进来，然后根据日期和聚会规模创建一张交叉表，然后进行规格化，使得各行的和为1，并生成图表。

```python
tips = pd.read_csv('examples/tips.csv')
party_counts = pd.crosstab(tips['day'], tips['size'])
# Not many 1- and 6-person parties
party_counts = party_counts.loc[:, 2:5]
# Normalize to sum to 1
party_pcts = party_counts.div(party_counts.sum(1), axis=0)
party_pcts.plot.bar()
```

通过该数据集就可以看出，聚会规模在周末会变大。

对于在绘制一个图形之前，需要进行合计的数据，使用seaborn可以减少工作量。用seaborn来看每天的小费比例。

```python
import seaborn as sns
tips['tip_pct'] = tips['tip'] / (tips['total_bill'] - tips['tip'])
tips.head()
sns.barplot(x='tip_pct', y='day', data=tips, orient='h')
```

seaborn的绘制函数使用data参数，它可能是pandas的DataFrame。其它的参数是关于列的名字。因为一天的每个值有多次观察，柱状图的值是tip_pct的平均值。绘制在柱状图上的黑线代表95%置信区间（可以通过可选参数配置）。

seaborn.barplot有颜色选项，使我们能够通过一个额外的值设置。

```python
sns.barplot(x='tip_pct', y='day', hue='time', data=tips, orient='h')
```

seaborn已经自动修改了图形的美观度：默认调色板，图形背景和网格线的颜色。你可以用seaborn.set在不同的图形外观之间切换。

```python
sns.set(style="whitegrid")
```

#### 直方图和密度图
直方图（histogram）是一种可以对值频率进行离散化显示的柱状图。数据点被拆分到离散的、间隔均匀的面元中，绘制的是各面元中数据点的数量。再以小费数据为例，通过在Series使用plot.hist方法，我们可以生成一张“小费占消费总额百分比”的直方图。

```python
tips['tip_pct'].plot.hist(bins=50)
```

与此相关的一种图表类型是密度图，它是通过计算“可能会产生观测数据的连续概率分布的估计”而产生的。一般的过程是将该分布近似为一组核（即诸如正态分布之类的较为简单的分布）。因此，密度图也被称作KDE（Kernel	Density	Estimate，核密度估计）图。使用plot.kde和标准混合正态分布估计即可生成一张密度图。

```python
tips['tip_pct'].plot.density()
```

seaborn的distplot方法绘制直方图和密度图更加简单，还可以同时画出直方图和连续密度估计图。作为例子，考虑一个双峰分布，由两个不同的标准正态分布组成。

```python
comp1 = np.random.normal(0, 1, size=200)
comp2 = np.random.normal(10, 2, size=200)
values = pd.Series(np.concatenate([comp1, comp2]))
sns.distplot(values, bins=100, color='k')
```

#### 散布图或点图
点图或散布图是观察两个一维数据序列之间的关系的有效手段。在下面这个例子中，加载了来自statsmodels项目的macrodata数据集，选择了几个变量，然后计算对数差。然后可以使用seaborn的regplot方法，它可以做一个散布图，并加上一条线性回归的线。

```python
macro = pd.read_csv('examples/macrodata.csv')
data = macro[['cpi', 'm1', 'tbilrate', 'unemp']]
trans_data = np.log(data).diff().dropna()
trans_data[-5:]
sns.regplot('m1', 'unemp', data=trans_data)
plt.title('Changes in log %s versus log %s' % ('m1', 'unemp'))
```

在探索式数据分析工作中，同时观察一组变量的散布图是很有意义的，这也被称为散布图矩阵（scatter	plot	matrix）。纯手工创建这样的图表很费工夫，所以seaborn提供了一个便捷的pairplot函数，它支持在对角线上放置每个变量的直方图或密度估计。

```python
sns.pairplot(trans_data, diag_kind='kde', plot_kws={'alpha': 0.2})
```

你可能注意到了plot_kws参数。它可以让我们传递配置选项到非对角线元素上的图形使用。

#### 分面网格（facet grid）和类型数据
要是数据集有额外的分组维度呢？有多个分类变量的数据可视化的一种方法是使用小面网格。seaborn有一个有用的内置函数catplot，可以简化制作多种分面图。

```python
sns.catplot(x='day', y='tip_pct', hue='time', col='smoker',
               kind='bar', data=tips[tips.tip_pct < 1])
```

除了在分面中用不同的颜色按时间分组，我们还可以通过给每个时间值添加一行来扩展分面网格。

```python
sns.catplot(x='day', y='tip_pct', row='time',
               col='smoker',
               kind='bar', data=tips[tips.tip_pct < 1])
```

factorplot支持其它的绘图类型，你可能会用到。例如，盒图（它可以显示中位数，四分位数，和异常值）就是一个有用的可视化类型。

```python
sns.catplot(x='tip_pct', y='day', kind='box',
               data=tips[tips.tip_pct < 0.5])
```

使用更通用的seaborn.FacetGrid类，你可以创建自己的分面网格。

### 9.3 其它的Python可视化工具

与其它开源库类似，Python创建图形的方式非常多（根本罗列不完）。自从2010年，许多开发工作都集中在创建交互式图形以便在Web上发布。利用工具如[Boken](https://bokeh.pydata.org/en/latest/)和[Plotly](https://github.com/plotly/plotly.py)，现在可以创建动态交互图形，用于网页浏览器。

对于创建用于打印或网页的静态图形，我建议默认使用matplotlib和附加的库，比如pandas和seaborn。对于其它数据可视化要求，学习其它的可用工具可能是有用的。

## 第10章 数据聚合与分组运算

对数据集进行分组并对各组应用一个函数（无论是聚合还是转换），通常是数据分析工作中的重要环节。在将数据集加载、融合、准备好之后，通常就是计算分组统计或生成透视表。pandas提供了一个灵活高效的gruopby功能，它使你能以一种自然的方式对数据集进行切片、切块、摘要等操作。

关系型数据库和SQL（Structured	Query	Language，结构化查询语言）能够如此流行的原因之一就是其能够方便地对数据进行连接、过滤、转换和聚合。但是，像SQL这样的查询语言所能执行的分组运算的种类很有限。在本章中你将会看到，由于Python和pandas强大的表达能力，我们可以执行复杂得多的分组运算（利用任何
可以接受pandas对象或NumPy数组的函数）。

在本章的主要内容：

- 使用一个或多个键（形式可以是函数、数组或DataFrame列名）分割pandas对象。
- 计算分组的概述统计，比如数量、平均值或标准差，或是用户定义的函数。
- 应用组内转换或其他运算，如规格化、线性回归、排名或选取子集等。
- 计算透视表或交叉表。
- 执行分位数分析以及其它统计分组分析。

### 10.1 GroupBy机制

Hadley Wickham（许多热门R语言包的作者）创造了一个用于表示分组运算的术语"split-apply-combine"（拆分－应用－合并）。

第一个阶段，pandas对象（无论是Series、DataFrame还是其他的）中的数据会根据你所提供的一个或多个键被拆分（split）为多组。拆分操作是在对象的特定轴上执行的。例如，DataFrame可以在其行（axis=0）或列（axis=1）上进行分组。

然后，将一个函数应用（apply）到各个分组并产生一个新值。

最后，所有这些函数的执行结果会被合并（combine）到最终的结果对象中。结果对象的形式一般取决于数据上所执行的操作。下图大致说明了一个简单的分组聚合过程。

![分组聚合](/assets/images/group-aggregation.jpg)

分组键可以有多种形式，且类型不必相同：

- 列表或数组，其长度与待分组的轴一样。
- 表示DataFrame某个列名的值。
- 字典或Series，给出待分组轴上的值与分组名之间的对应关系。
- 函数，用于处理轴索引或索引中的各个标签。

注意，后三种都只是快捷方式而已，其最终目的仍然是产生一组用于拆分对象的值。

首先来看看下面这个非常简单的表格型数据集（以DataFrame的形式）。假设你想要按key1进行分组，并计算data1列的平均值。实现该功能的方式有很多，而我们这里要用的是：访问data1，并根据key1调用groupby。变量grouped是一个GroupBy对象。它实际上还没有进行任何计算，只是含有一些有关分组键df['key1']的中间数据而已。换句话说，该对象已经有了接下来对各分组执行运算所需的一切信息。例如，我们可以调用GroupBy的mean方法来计算分组平均值。

```python
df = pd.DataFrame({'key1' : ['a', 'a', 'b', 'b', 'a'],
                   'key2' : ['one', 'two', 'one', 'two', 'one'],
                   'data1' : np.random.randn(5),
                   'data2' : np.random.randn(5)})
grouped = df['data1'].groupby(df['key1'])
grouped.mean()
```

这里最重要的是，数据（Series）根据分组键进行了聚合，产生了一个新的Series，其索引为key1列中的唯一值。之所以结果中索引的名称为key1，是因为原始DataFrame的列df['key1']就叫这个名字。

如果我们一次传入多个数组的列表，就会得到不同的结果。这里，我通过两个键对数据进行了分组，得到的Series具有一个层次化索引（由唯一的键对组成）。

```python
means = df['data1'].groupby([df['key1'], df['key2']]).mean()
means.unstack()
```

在这个例子中，分组键均为Series。实际上，分组键可以是任何长度适当的数组。

```python
states = np.array(['Ohio', 'California', 'California', 'Ohio', 'Ohio'])
years = np.array([2005, 2005, 2006, 2005, 2006])
df['data1'].groupby([states, years]).mean()
```

通常，分组信息就位于相同的要处理DataFrame中。这里，你还可以将列名（可以是字符串、数字或其他Python对象）用作分组键。

```python
df.groupby('key1').mean()
df.groupby(['key1', 'key2']).mean()
```

你可能已经注意到了，第一个例子在执行df.groupby('key1').mean()时，结果中没有key2列。这是因为df['key2']不是数值数据（俗称“麻烦列”），所以被从结果中排除了。默认情况下，所有数值列都会被聚合，虽然有时可能会被过滤为一个子集。

无论你准备拿groupby做什么，都有可能会用到GroupBy的size方法，它可以返回一个含有分组大小的Series。

```python
df.groupby(['key1', 'key2']).size()
```

注意，任何分组关键词中的缺失值，都会被从结果中除去。

#### 对分组进行迭代
GroupBy对象支持迭代，可以产生一组二元元组（由分组名和数据块组成）。对于多重键的情况，元组的第一个元素将会是由键值组成的元组。

```python
for name, group in df.groupby('key1'):
    print(name)
    print(group)
for (k1, k2), group in df.groupby(['key1', 'key2']):
    print((k1, k2))
    print(group)
```

可以对这些数据片段做任何操作。有一个你可能会觉得有用的运算：将这些数据片段做成一个字典。

```python
pieces = dict(list(df.groupby('key1')))
pieces['b']
```

groupby默认是在axis=0上进行分组的，通过设置也可以在其他任何轴上进行分组。拿上面例子中的df来说，我们可以根据dtype对列进行分组。

```python
df.dtypes
grouped = df.groupby(df.dtypes, axis=1)
for dtype, group in grouped:
    print(dtype)
    print(group)
```

#### 选取一列或列的子集
对于由DataFrame产生的GroupBy对象，如果用一个（单个字符串）或一组（字符串数组）列名对其进行索引，就能实现选取部分列进行聚合的目的。

```python
df.groupby('key1')['data1']
df.groupby('key1')[['data2']]
# 上面的代码与下面的代码等价
df['data1'].groupby(df['key1'])
df[['data2']].groupby(df['key1'])
```

尤其对于大数据集，很可能只需要对部分列进行聚合。例如，在前面那个数据集中，如果只需计算data2列的平均值并以DataFrame形式得到结果，可以这样写：

```python
df.groupby(['key1', 'key2'])[['data2']].mean()
s_grouped = df.groupby(['key1', 'key2'])['data2']
s_grouped.mean()
```

这种索引操作所返回的对象是一个已分组的DataFrame（如果传入的是列表或数组）或已分组的Series（如果传入的是标量形式的单个列名）。

#### 通过字典或Series进行分组
除数组以外，分组信息还可以其他形式存在。

来看另一个示例DataFrame，现在，假设已知列的分组关系，并希望根据分组计算列的和。现在，你可以将这个字典传给groupby，来构造数组，但我们可以直接传递字典（我包含了键“f”来强调，存在未使用的分组键是可以的）。

```python
people = pd.DataFrame(np.random.randn(5, 5),
                      columns=['a', 'b', 'c', 'd', 'e'],
                      index=['Joe', 'Steve', 'Wes', 'Jim', 'Travis'])
people.iloc[2:3, [1, 2]] = np.nan # Add a few NA values
mapping = {'a': 'red', 'b': 'red', 'c': 'blue',
           'd': 'blue', 'e': 'red', 'f' : 'orange'}
by_column = people.groupby(mapping, axis=1)
by_column.sum()
```

Series也有同样的功能，它可以被看做一个固定大小的映射。

```python
map_series = pd.Series(mapping)
people.groupby(map_series, axis=1).count()
```

#### 通过函数进行分组
比起使用字典或Series，使用Python函数是一种更原生的方法定义分组映射。任何被当做分组键的函数都会在各个索引值上被调用一次，其返回值就会被用作分组名称。具体点说，以上一小节的示例DataFrame为例，其索引值为人的名字。你可以计算一个字符串长度的数组，更简单的方法是传入len函数。

```
people.groupby(len).sum()
```

将函数跟数组、列表、字典、Series混合使用也不是问题，因为任何东西在内部都会被转换为数组。

```
key_list = ['one', 'one', 'one', 'two', 'two']
people.groupby([len, key_list]).min()
```

#### 根据索引级别分组
层次化索引数据集最方便的地方就在于它能够根据轴索引的一个级别进行聚合。要根据级别分组，使用level关键字传递级别序号或名字。

```python
columns = pd.MultiIndex.from_arrays([['US', 'US', 'US', 'JP', 'JP'],
                                    [1, 3, 5, 1, 3]],
                                    names=['cty', 'tenor'])
hier_df = pd.DataFrame(np.random.randn(4, 5), columns=columns)
hier_df.groupby(level='cty', axis=1).count()
```

### 10.2 数据聚合

聚合指的是任何能够从数组产生标量值的数据转换过程。之前的例子已经用过一些，比如mean、count、min以及sum等。常见的聚合运算：

- count  分组中非NA值的数量
- sum  非NA值的和
- mean  非NA值的平均值
- median  非NA值的算术中位数
- std, var  无偏（分母为n-1）标准差和方差
- min, max  非NA值的最小值和最大值
- prod  非NA值的积
- first, last  第一个和最后一个非NA值

你可以使用自己发明的聚合运算，还可以调用分组对象上已经定义好的任何方法。例如，quantile可以计算Series或DataFrame列的样本分位数。

虽然quantile并没有明确地实现于GroupBy，但它是一个Series方法，所以这里是能用的。实际上，GroupBy会高效地对Series进行切片，然后对各片调用piece.quantile(0.9)，最后将这些结果组装成最终结果。

如果要使用你自己的聚合函数，只需将其传入aggregate或agg方法即可。

你可能注意到注意，有些方法（如describe）也是可以用在这里的，即使严格来讲，它们并非聚合运算。

```python
grouped = df.groupby('key1')
grouped['data1'].quantile(0.9)

def peak_to_peak(arr):
    return arr.max() - arr.min()
grouped.agg(peak_to_peak)
grouped.describe()
```

自定义聚合函数要比表10-1中那些经过优化的函数慢得多。这是因为在构造中间分组数据块时存在非常大的开销（函数调用、数据重排等）。

#### 面向列的多函数应用

回到前面小费的例子。使用read_csv导入数据之后，我们添加了一个小费百分比的列tip_pct。

你已经看到，对Series或DataFrame列的聚合运算其实就是使用aggregate（使用自定义函数）或调用诸如mean、std之类的方法。然而，你可能希望对不同的列使用不同的聚合函数，或一次应用多个函数。

首先，我根据day和smoker对tips进行分组；对于上表中的那些描述统计，可以将函数名以字符串的形式传入；如果传入一组函数或函数名，得到的DataFrame的列就会以相应的函数命名；这里，我们传递了一组聚合函数进行聚合，独立对数据分组进行评估。

```python
tips = pd.read_csv('examples/tips.csv')
# Add tip percentage of total bill
tips['tip_pct'] = tips['tip'] / tips['total_bill']
grouped = tips.groupby(['day', 'smoker'])
grouped_pct = grouped['tip_pct']
grouped_pct.agg('mean')
grouped_pct.agg(['mean', 'std', peak_to_peak])
```

你并非一定要接受GroupBy自动给出的那些列名，特别是lambda函数，它们的名称是''，这样的辨识度就很低了（通过函数的name属性看看就知道了）。因此，如果传入的是一个由(name,function)元组组成的列表，则各元组的第一个元素就会被用作DataFrame的列名（可以将这种二元元组列表看做一个有序映射）。

```python
grouped_pct.agg([('foo', 'mean'), ('bar', np.std)])
```

对于DataFrame，你还有更多选择，你可以定义一组应用于全部列的一组函数，或不同的列应用不同的函数。假设我们想要对tip_pct和total_bill列计算三个统计信息。结果DataFrame拥有层次化的列，这相当于分别对各列进行聚合，然后用concat将结果组装到一起，使用列名用作keys参数。跟前面一样，这里也可以传入带有自定义名称的一组元组。

```python
functions = ['count', 'mean', 'max']
result = grouped['tip_pct', 'total_bill'].agg(functions)
result['tip_pct']
ftuples = [('Durchschnitt', 'mean'), ('Abweichung', np.var)]
grouped['tip_pct', 'total_bill'].agg(ftuples)
```

现在，假设你想要对一个列或不同的列应用不同的函数。具体的办法是向agg传入一个从列名映射到函数的字典。

```python
grouped.agg({'tip' : np.max, 'size' : 'sum'})
grouped.agg({'tip_pct' : ['min', 'max', 'mean', 'std'],
             'size' : 'sum'})
```

只有将多个函数应用到至少一列时，DataFrame才会拥有层次化的列。

#### 以“没有行索引”的形式返回聚合数据

到目前为止，所有示例中的聚合数据都有由唯一的分组键组成的索引（可能还是层次化的）。由于并不总是需要如此，所以你可以向groupby传入as_index=False以禁用该功能。

```python
tips.groupby(['day', 'smoker'], as_index=False).mean()
```

当然，对结果调用reset_index也能得到这种形式的结果。使用as_index=False方法
可以避免一些不必要的计算。

### 10.3 apply：一般性的“拆分－应用－合并”

最通用的GroupBy方法是apply。apply会将待处理的对象拆分成多个片段，然后对各片段调用传入的函数，最后尝试将各片段组合到一起。

回到之前那个小费数据集，假设你想要根据分组选出最高的5个tip_pct值。首先，编写一个选取指定列具有最大值的行的函数。现在，如果对smoker分组并用该函数调用apply，top函数在DataFrame的各个片段上调用，然后结果由pandas.concat组装到一起，并以分组名称进行了标记。于是，最终结果就有了一个层次化索引，其内层索引值来自原DataFrame。

如果传给apply的函数能够接受其他参数或关键字，则可以将这些内容放在函数名后面一并传入。

```python
def top(df, n=5, column='tip_pct'):
    return df.sort_values(by=column)[-n:]
top(tips, n=6)
tips.groupby('smoker').apply(top)
tips.groupby(['smoker', 'day']).apply(top, n=1, column='total_bill')
```

除这些基本用法之外，能否充分发挥apply的威力很大程度上取决于你的创造力。传入的那个函数能做什么全由你说了算，它只需返回一个pandas对象或标量值即可。

之前在GroupBy对象上调用过describe，在GroupBy中，当你调用诸如describe之类的方法时，实际上只是应用了下面两条代码的快捷方式而已。

```python
result = tips.groupby('smoker')['tip_pct'].describe()
result.unstack('smoker')
f = lambda x: x.describe()
grouped.apply(f)
```

#### 禁止分组键
从上面的例子中可以看出，分组键会跟原始对象的索引共同构成结果对象中的层次化索引。将group_keys=False传入groupby即可禁止该效果。

```python
tips.groupby('smoker', group_keys=False).apply(top)
```

#### 分位数和桶分析
pandas有一些能根据指定面元或样本分位数将数据拆分成多块的工具（比如cut和qcut）。将这些函数跟groupby结合起来，就能非常轻松地实现对数据集的桶（bucket）或分位数（quantile）分析了。

以下面这个简单的随机数据集为例，我们利用cut将其装入长度相等的桶中。由cut返回的Categorical对象可直接传递到groupby。因此，可以像下面这样对data2列做一些统计计算。

```python
frame = pd.DataFrame({'data1': np.random.randn(1000),
                      'data2': np.random.randn(1000)})
quartiles = pd.cut(frame.data1, 4)
quartiles[:10]
def get_stats(group):
    return {'min': group.min(), 'max': group.max(),
            'count': group.count(), 'mean': group.mean()}
grouped = frame.data2.groupby(quartiles)
grouped.apply(get_stats).unstack()
```

这些都是长度相等的桶。要根据样本分位数得到大小相等的桶，使用qcut即可。传入labels=False即可只获取分位数的编号。

```python
# Return quantile numbers
grouping = pd.qcut(frame.data1, 10, labels=False)
grouped = frame.data2.groupby(grouping)
grouped.apply(get_stats).unstack()
```

#### 示例：用特定于分组的值填充缺失值
对于缺失数据的清理工作，有时你会用dropna将其替换掉，而有时则可能会希望用一个固定值或由数据集本身所衍生出来的值去填充NA值。这时就得使用fillna这个工具了。

在下面这个例子中，用平均值去填充NA值。

```python
s = pd.Series(np.random.randn(6))
s[::2] = np.nan
s.fillna(s.mean())
```

假设你需要对不同的分组填充不同的值。一种方法是将数据分组，并使用apply和一个能够对各数据块调用fillna的函数即可。下面是一些有关美国几个州的示例数据，这些州又被分为东部和西部，`['East']*4`产生了一个列表，包括了`['East']`中元素的四个拷贝。将这些列表串联起来。将一些值设为缺失。我们可以用分组平均值去填充NA值。

```python
states = ['Ohio', 'New York', 'Vermont', 'Florida',
          'Oregon', 'Nevada', 'California', 'Idaho']
group_key = ['East'] * 4 + ['West'] * 4
data = pd.Series(np.random.randn(8), index=states)
data[['Vermont', 'Nevada', 'Idaho']] = np.nan
data.groupby(group_key).mean()
fill_mean = lambda g: g.fillna(g.mean())
data.groupby(group_key).apply(fill_mean)
```

另外，也可以在代码中预定义各组的填充值。由于分组具有一个name属性，所以可以拿来用一下。

```python
fill_values = {'East': 0.5, 'West': -1}
fill_func = lambda g: g.fillna(fill_values[g.name])
data.groupby(group_key).apply(fill_func)
```

#### 示例：随机采样和排列
假设你想要从一个大数据集中随机抽取（进行替换或不替换）样本以进行蒙特卡罗模拟（Monte	Carlo	simulation）或其他分析工作。“抽取”的方式有很多，这里使用的方法是对Series使用sample方法。

现在有了一个长度为52的Series，其索引包括牌名，值则是21点或其他游戏中用于计分的点数（为了简单起见，我当A的点数为1）。现在，从整副牌中抽出5张。

```python
# Hearts, Spades, Clubs, Diamonds
suits = ['H', 'S', 'C', 'D']
card_val = (list(range(1, 11)) + [10] * 3) * 4
base_names = ['A'] + list(range(2, 11)) + ['J', 'K', 'Q']
cards = []
for suit in ['H', 'S', 'C', 'D']:
    cards.extend(str(num) + suit for num in base_names)
deck = pd.Series(card_val, index=cards)
def draw(deck, n=5):
    return deck.sample(n)
draw(deck)
```

假设想要从每种花色中随机抽取两张牌。由于花色是牌名的最后一个字符，所以可以据此进行分组，并使用apply。或者，也可以写成下面的形式。

```python
get_suit = lambda card: card[-1] # last letter is suit
deck.groupby(get_suit).apply(draw, n=2)
deck.groupby(get_suit, group_keys=False).apply(draw, n=2)
```

#### 示例：分组加权平均数和相关系数
根据groupby的“拆分－应用－合并”范式，可以进行DataFrame的列与列之间或两个Series之间的运算（比如分组加权平均）。

以下面这个数据集为例，它含有分组键、值以及一些权重值。然后可以利用category计算分组加权平均数。

```python
df = pd.DataFrame({'category': ['a', 'a', 'a', 'a',
                                'b', 'b', 'b', 'b'],
                   'data': np.random.randn(8),
                   'weights': np.random.rand(8)})
grouped = df.groupby('category')
get_wavg = lambda g: np.average(g['data'], weights=g['weights'])
grouped.apply(get_wavg)
```

另一个例子，考虑一个来自Y ahoo!Finance的数据集，其中含有几只股票和标准普尔500指数（符号SPX）的收盘价。来做一个比较有趣的任务：计算一个由日收益率（通过百分数变化计算）与SPX之间的年度相关系数组成的DataFrame。下面是一个实现办法，我们先创建一个函数，用它计算每列和SPX列的成对相关系数。接下来，我们使用pct_change计算close_px的百分比变化。最后，我们用年对百分比变化进行分组，可以用一个一行的函数，从每行的标签返回每个datetime标签的year属性。

当然，你还可以计算列与列之间的相关系数。这里，我们计算Apple和Microsoft的年相关系数。

```python
close_px = pd.read_csv('examples/stock_px_2.csv', parse_dates=True,
                       index_col=0)
close_px.info()
close_px[-4:]
spx_corr = lambda x: x.corrwith(x['SPX'])
rets = close_px.pct_change().dropna()
get_year = lambda x: x.year
by_year = rets.groupby(get_year)
by_year.apply(spx_corr)
by_year.apply(lambda g: g['AAPL'].corr(g['MSFT']))
```

#### 示例：组级别的线性回归

继续上一个例子，你可以用groupby执行更为复杂的分组统计分析，只要函数返回的是pandas对象或标量值即可。例如，我可以定义下面这个regress函数（利用statsmodels计量经济学库）对各数据块执行普通最小二乘法（Ordinary Least Squares，OLS）回归。然后，按年计算AAPL对SPX收益率的线性回归。

```python
import statsmodels.api as sm
def regress(data, yvar, xvars):
    Y = data[yvar]
    X = data[xvars]
    X['intercept'] = 1.
    result = sm.OLS(Y, X).fit()
    return result.params
by_year.apply(regress, 'AAPL', ['SPX'])
```

### 10.4 透视表和交叉表
透视表（pivot table）是各种电子表格程序和其他数据分析软件中一种常见的数据汇总工具。它根据一个或多个键对数据进行聚合，并根据行和列上的分组键将数据分配到各个矩形区域中。在Python和pandas中，可以通过本章所介绍的groupby功能以及（能够利用层次化索引的）重塑运算制作透视表。DataFrame有一个pivot_table方法，此外还有一个顶级的pandas.pivot_table函数。除能为groupby提供便利之外，pivot_table还可以添加分项小计，也叫做margins。

使用小费数据集，假设我想要根据day和smoker计算分组平均数（pivot_table的默认聚合类型），并将day和smoker放到行上，可以用groupby直接来做。现在，假设我们只想聚合tip_pct和size，而且想根据time进行分组。我将smoker放到列上，把day放到行上。

还可以对这个表作进一步的处理，传入margins=True添加分项小计。这将会添加标签为All的行和列，其值对应于单个等级中所有数据的分组统计。这里，All值为平均数：不单独考虑烟民与非烟民（All列），不单独考虑行分组两个级别中的任何单项（All行）。

要使用其他的聚合函数，将其传给aggfunc即可。例如，使用count或len可以得到有关分组大小的交叉表（计数或频率）。如果存在空的组合（也就是NA），你可能会希望设置一个fill_value。

```python
tips.pivot_table(index=['day', 'smoker'])
tips.pivot_table(['tip_pct', 'size'], index=['time', 'day'],
                 columns='smoker')
tips.pivot_table(['tip_pct', 'size'], index=['time', 'day'],
                 columns='smoker', margins=True)
tips.pivot_table('tip_pct', index=['time', 'smoker'], columns='day',
                 aggfunc=len, margins=True)
tips.pivot_table('tip_pct', index=['time', 'size', 'smoker'],
                 columns='day', aggfunc='mean', fill_value=0)
```

pivot_table的参数选项：

- values  待聚合的列的名称。默认聚合所有数值列
- index  用于分组的列名或其他分组键，出现在结果透视表的行
- columns  用于分组的列名或其他分组键，出现在结果透视表的列
- aggfunc  聚合函数或函数列表，默认为mean。可以是任何对groupby有效的函数
- fill_value  用于替换结果表中的缺失值
- dropna  如果为True，不添加条目都为NA的列
- margins  添加行/列小计和总计，默认为False

#### 交叉表：crosstab
交叉表（cross-tabulation，简称crosstab）是一种用于计算分组频率的特殊透视表。例如下面的数据：

```python
from io import StringIO
data = """\
Sample  Nationality  Handedness
1   USA  Right-handed
2   Japan    Left-handed
3   USA  Right-handed
4   Japan    Right-handed
5   Japan    Left-handed
6   Japan    Right-handed
7   USA  Right-handed
8   USA  Left-handed
9   Japan    Right-handed
10  USA  Right-handed"""
data = pd.read_table(StringIO(data), sep='\s+')
```

作为调查分析的一部分，我们可能想要根据国籍和用手习惯对这段数据进行统计汇总。虽然可以用pivot_table实现该功能，但是pandas.crosstab函数会更方便。

crosstab的前两个参数可以是数组或Series，或是数组列表。就像小费数据。

```python
pd.crosstab(data.Nationality, data.Handedness, margins=True)
pd.crosstab([tips.time, tips.day], tips.smoker, margins=True)
```

## 第11章 时间序列

时间序列（time	series）数据是一种重要的结构化数据形式，应用于多个领域，包括金融学、经济学、生态学、神经科学、物理学等。在多个时间点观察或测量到的任何事物都可以形成一段时间序列。很多时间序列是固定频率的，也就是说，数据点是根据某种规律定期出现的（比如每15秒、每5分钟、每月出现一次）。时间序列也可以是不定期的，没有固定的时间单位或单位之间的偏移量。时间序列数据的意义取决于具体的应用场景，主要有以下几种：

- 时间戳（timestamp），特定的时刻。
- 固定时期（period），如2007年1月或2010年全年。
- 时间间隔（interval），由起始和结束时间戳表示。时期（period）可以被看做间隔（interval）的特例。
- 实验或过程时间，每个时间点都是相对于特定起始时间的一个度量。例如，从放入烤箱时起，每秒钟饼干的直径。

本章主要讲解前3种时间序列。许多技术都可用于处理实验型时间序列，其索引可能是一个整数或浮点数（表示从实验开始算起已经过去的时间）。最简单也最常见的时间序列都是用时间戳进行索引的。

pandas也支持基于timedeltas的指数，它可以有效代表实验或经过的时间。这本书不涉及timedelta指数，但你可以学习pandas的文档（http://pandas.pydata.org/）。

pandas提供了许多内置的时间序列处理工具和数据算法。因此，你可以高效处理非常大的时间序列，轻松地进行切片/切块、聚合、对定期/不定期的时间序列进行重采样等。有些工具特别适合金融和经济应用，你当然也可以用它们来分析服务器日志数据。

### 11.1 日期和时间数据类型及工具

Python标准库包含用于日期（date）和时间（time）数据的数据类型，而且还有日历方面的功能。我们主要会用到datetime、time以及calendar模块。datetime.datetime（也可以简写为datetime）是用得最多的数据类型。

datetime以毫秒形式存储日期和时间。timedelta表示两个datetime对象之间的时间差。可以给datetime对象加上（或减去）一个或多个timedelta，这样会产生一个新对象。

```python
from datetime import datetime, timedelta
now = datetime.now()
now.year, now.month, now.day
now.hour, now.minute, now.second
delta = datetime(2011, 1, 7) - datetime(2008, 6, 24, 8, 15)
delta.days
delta.seconds
start = datetime(2011, 1, 7)
start + timedelta(12)
start - 2 * timedelta(12)
```

datetime模块中的数据类型：

- date  以公历形式存储日历日期（年、月、日）
- time  将时间存储为时、分、秒、毫秒
- datetime  存储日期和时间
- timedelta  表示两个datetime值直接的差（日、秒、毫秒）
- tzinfo  存储时区信息的基本类型

#### 字符串和datetime的相互转换
利用str或strftime方法（传入一个格式化字符串），datetime对象和pandas的Timestamp对象（稍后就会介绍）可以被格式化为字符串。

```python
stamp = datetime(2011, 1, 3)
str(stamp)
stamp.strftime('%Y-%m-%d')
```

datetime格式化编码（兼容ISO C89）：

- %Y  4位数的年
- %y  2位数的年
- %m  2位数的月[01, 12]
- %d  2位数的日[01, 31]
- %H  时（24小时制）[00, 23]
- %I  时（12小时制）[00, 12]
- %M  2位数的分[00, 59]
- %S  秒[00, 61]（秒60和61用于闰秒）
- %w  用整数表示的星期几[0, 6]
- %U  每年的第几周[00, 53]。星期天是每周的第一天，每年第一个星期天之前的那几天是第0周
- %W  每年的第几周[00, 53]。星期一是每周的第一天，每年第一个星期一之前的那几天是第0周
- %z  以+HHMM或-HHMM表示的UTC时区偏移量，如果时区为naive，则返回空字符串
- %F  %Y-%m-%d简写形式，例如：2012-04-18
- %D  %m/%d/%y简写形式，例如：04/18/2012

注：有两种日期和时间对象： “naive” 和 “aware”，其中aware对象包含时区信息，naive对象不包含，需要程序手动处理。

datetime.strptime可以用这些格式化编码将字符串转换为日期。

```python
value = '2011-01-03'
datetime.strptime(value, '%Y-%m-%d')
datestrs = ['7/6/2011', '8/6/2011']
[datetime.strptime(x, '%m/%d/%Y') for x in datestrs]
```

datetime.strptime是通过已知格式进行日期解析的最佳方式。但是每次都要编写格式定义是很麻烦的事情，尤其是对于一些常见的日期格式。这种情况下，你可以用dateutil这个第三方包中的parser .parse方法（pandas中已经自动安装好了）。dateutil可以解析几乎所有人类能够理解的日期表示形式。在国际通用的格式中，日出现在月的前面很普遍，传入dayfirst=True即可解决这个问题。

```python
from dateutil.parser import parse
parse('2011-01-03')
parse('Jan 31, 1997 10:45 PM')
parse('6/12/2011', dayfirst=True)
```

pandas通常是用于处理成组日期的，不管这些日期是DataFrame的轴索引还是列。to_datetime方法可以解析多种不同的日期表示形式。对标准日期格式（如ISO8601）的解析非常快。它还可以处理缺失值（None、空字符串等）。

```python
datestrs = ['2011-07-06 12:00:00', '2011-08-06 00:00:00']
pd.to_datetime(datestrs)
idx = pd.to_datetime(datestrs + [None])
idx
idx[2]
pd.isnull(idx)
```

NaT（Not a Time）是pandas中时间戳数据的null值。

注意：dateutil.parser是一个实用但不完美的工具。比如说，它会把一些原本不是日期的字符串认作是日期（比如"42"会被解析为2042年的今天）。

datetime对象还有一些特定于当前环境（位于不同国家或使用不同语言的系统）的格式化选项。例如，德语或法语系统所用的月份简写就与英语系统所用的不同。

### 11.2 时间序列基础
pandas最基本的时间序列类型就是以时间戳（通常以Python字符串或datatime对象表示）为索引的Series。

这些索引datetime对象实际上是被放在一个DatetimeIndex中的，DatetimeIndex中的各个标量值是pandas的Timestamp对象。跟其他Series一样，不同索引的时间序列之间的算术运算会自动按日期对齐。

pandas用NumPy的datetime64数据类型以纳秒形式存储时间戳。

```python
from datetime import datetime
dates = [datetime(2011, 1, 2), datetime(2011, 1, 5),
         datetime(2011, 1, 7), datetime(2011, 1, 8),
         datetime(2011, 1, 10), datetime(2011, 1, 12)]
ts = pd.Series(np.random.randn(6), index=dates)
ts.index
ts.index.dtype
ts + ts[::2]
stamp = ts.index[0]
stamp
```

只要有需要，TimeStamp可以随时自动转换为datetime对象。此外，它还可以存储频率信息（如果有的话），且知道如何执行时区转换以及其他操作。

#### 索引、选取、子集构造
当你根据标签索引选取数据时，时间序列和其它的pandas.Series很像。还有一种更为方便的用法：传入一个可以被解释为日期的字符串。

```python
stamp = ts.index[2]
ts[stamp]
ts['1/10/2011']
ts['20110110']
```

对于较长的时间序列，只需传入“年”或“年月”即可轻松选取数据的切片。字符串“2001”被解释成年，并根据它选取时间区间。指定月也同样奏效。datetime对象也可以进行切片。

```python
longer_ts = pd.Series(np.random.randn(1000),
                      index=pd.date_range('1/1/2000', periods=1000))
longer_ts['2001']
longer_ts['2001-05']
ts[datetime(2011, 1, 7):]
```

由于大部分时间序列数据都是按照时间先后排序的，因此你也可以用不存在于该时间序列中的时间戳对其进行切片（即范围查询）。此外，还有一个等价的实例方法也可以截取两个日期之间TimeSeries。

```python
ts['1/6/2011':'1/11/2011']
ts.truncate(after='1/9/2011')
```

跟之前一样，你可以传入字符串日期、datetime或Timestamp。注意，这样切片所产生的是原时间序列的视图，跟NumPy数组的切片运算是一样的。这意味着，没有数据被复制，对切片进行修改会反映到原始数据上。

上面这些操作对DataFrame也有效。例如，对DataFrame的行进行索引：

```python
dates = pd.date_range('1/1/2000', periods=100, freq='W-WED')
long_df = pd.DataFrame(np.random.randn(100, 4),
                       index=dates,
                       columns=['Colorado', 'Texas',
                                'New York', 'Ohio'])
long_df.loc['5-2001']
```

#### 带有重复索引的时间序列
在某些应用场景中，可能会存在多个观测数据落在同一个时间点上的情况。下面就是一个例子，通过检查索引的is_unique属性，我们就可以知道它是不是唯一的。对这个时间序列进行索引，要么产生标量值，要么产生切片，具体要看所选的时间点是否重复。

假设你想要对具有非唯一时间戳的数据进行聚合。一个办法是使用groupby，并传入level=0。

```python
dates = pd.DatetimeIndex(['1/1/2000', '1/2/2000', '1/2/2000',
                          '1/2/2000', '1/3/2000'])
dup_ts = pd.Series(np.arange(5), index=dates)
dup_ts.index.is_unique
dup_ts['1/3/2000']  # not duplicated
dup_ts['1/2/2000']  # duplicated
grouped = dup_ts.groupby(level=0)
grouped.mean()
grouped.count()
```

#### 11.3 日期的范围、频率以及移动
pandas中的原生时间序列一般被认为是不规则的，也就是说，它们没有固定的频率。对于大部分应用程序而言，这是无所谓的。但是，它常常需要以某种相对固定的频率进行分析，比如每日、每月、每15分钟等（这样自然会在时间序列中引入缺失值）。幸运的是，pandas有一整套标准时间序列频率以及用于重采样、频率推断、生成固定频率日期范围的工具。

例如，我们可以将之前那个时间序列转换为一个具有固定频率（每日）的时间序列，只需调用resample即可，字符串“D”是每天的意思。

```python
resampler = ts.resample('D')
```

#### 生成日期范围
虽然我之前用的时候没有明说，但你可能已经猜到pandas.date_range可用于根据指定的频率生成指定长度的DatetimeIndex。默认情况下，date_range会产生按天计算的时间点。如果只传入起始或结束日期，那就还得传入一个表示一段时间的数字。

起始和结束日期定义了日期索引的严格边界。例如，如果你想要生成一个由每月最后一个工作日组成的日期索引，可以传入"BM"频率（表示business end of month），这样就只会包含时间间隔内（或刚好在边界上的）符合频率要求的日期。

```python
index = pd.date_range('2012-04-01', '2012-06-01')
pd.date_range(start='2012-04-01', periods=20)
pd.date_range(end='2012-06-01', periods=20)
pd.date_range('2000-01-01', '2000-12-01', freq='BM')
```

基本的时间序列频率（不完整）：

| 别名                  | 偏移量类型           | 说明                                                         |
| --------------------- | -------------------- | ------------------------------------------------------------ |
| D                     | Day                  | 每日历日                                                     |
| B                     | BusinessDay          | 每工作日                                                     |
| H                     | Hour                 | 每小时                                                       |
| T或min                | Minute               | 每分钟                                                       |
| S                     | Second               | 每秒                                                         |
| L或ms                 | Milli                | 每毫秒                                                       |
| U                     | Macro                | 每微秒                                                       |
| M                     | MonthEnd             | 每月最后一个日历日                                           |
| BM                    | BusinessMonthEnd     | 每月最后一个工作日                                           |
| MS                    | MonthBegin           | 每月第一个日历日                                             |
| BMS                   | BusinessMonthBegin   | 每月第一个工作日                                             |
| W-MON, W-TUE...       | Week                 | 每周，从指定的星期几（MON、TUE、WED、THU、FRI、SAT、SUN）开始算起 |
| WOM-1MON, WOM-2MON... | WeekOfMonth          | 每月第一、第二、第三或第四周的星期几                         |
| Q-JAN, Q-FEB...       | QuarterEnd           | 每季度最后一个日历日，年度以指定月份（JAN, FEB, MAR, APR, MAY, JUN, JUL, AUG, SEP, OCT, NOV, DEC）结束 |
| BQ-JAN, BQ-FEB...     | BusinessQuarterEnd   | 每季度最后一个工作日，年度以指定月份结束                     |
| QS-JAN, QS-FEB...     | QuarterBegin         | 每季度第一个日历日，年度以指定月份结束                       |
| BQS-JAN, BQS-FEB...   | BusinessQuarterBegin | 每季度第一个工作日，年度以指定月份结束                       |
| A-JAN, A-FEB...       | YearEnd              | 每年指定月份的最后一个日历日                                 |
| BA-JAN, BA-FEB...     | BusinessYearEnd      | 每年指定月份的最后一个工作日                                 |
| AS-JAN, AS-FEB...     | YearBegin            | 每年指定月份的第一个日历日                                   |
| BAS-JAN, BAS-FEB...   | BusinessYearBegin    | 每年指定月份的第一个工作日                                   |

date_range默认会保留起始和结束时间戳的时间信息（如果有的话）。有时，虽然起始和结束日期带有时间信息，但你希望产生一组被规范化（normalize）到午夜的时间戳。normalize选项即可实现该功能。

```python
pd.date_range('2012-05-02 12:56:31', periods=5)
pd.date_range('2012-05-02 12:56:31', periods=5, normalize=True)
```

#### 频率和日期偏移量
pandas中的频率是由一个基础频率（base frequency）和一个乘数组成的。基础频率通常以一个字符串别名表示，比如"M"表示每月，"H"表示每小时。对于每个基础频率，都有一个被称为日期偏移量（date offset）的对象与之对应。例如，按小时计算的频率可以用Hour类表示。传入一个整数即可定义偏移量的倍数。

一般来说，无需明确创建这样的对象，只需使用诸如"H"或"4H"这样的字符串别名即可。在基础频率前面放上一个整数即可创建倍数。大部分偏移量对象都可通过加法进行连接。同理，你也可以传入频率字符串（如"2h30min"），这种字符串可以被高效地解析为等效的表达式。

```python
from pandas.tseries.offsets import Hour, Minute
hour = Hour()
four_hours = Hour(4)
pd.date_range('2000-01-01', '2000-01-03 23:59', freq='4h')
Hour(2) + Minute(30)
pd.date_range('2000-01-01', periods=10, freq='1h30min')
```

有些频率所描述的时间点并不是均匀分隔的。例如，"M"（日历月末）和"BM"（每月最后一个工作日）就取决于每月的天数，对于后者，还要考虑月末是不是周末。由于没有更好的术语，我将这些称为锚点偏移量（anchored	offset）。

#### WOM日期

WOM（Week Of Month）是一种非常实用的频率类，它以WOM开头。它使你能获得诸如“每月第3个星期五”之类的日期。

```python
rng = pd.date_range('2012-01-01', '2012-09-01', freq='WOM-3FRI')
list(rng)
```

#### 移动（超前和滞后）数据
移动（shifting）指的是沿着时间轴将数据前移或后移。Series和DataFrame都有一个shift方法用于执行单纯的前移或后移操作，保持索引不变。

当我们这样进行移动时，就会在时间序列的前面或后面产生缺失数据。由于单纯的移位操作不会修改索引，所以部分数据会被丢弃。因此，如果频率已知，则可以将其传给shift以便实现对时间戳进行位移而不是对数据进行简单位移。这里还可以使用其他频率，于是你就能非常灵活地对数据进行超前和滞后处理了。

```python
ts = pd.Series(np.random.randn(4),
               index=pd.date_range('1/1/2000', periods=4, freq='M'))
ts.shift(2)
ts.shift(-2)
ts.shift(2, freq='M')
ts.shift(3, freq='D')
ts.shift(1, freq='90T')
```

shift通常用于计算一个时间序列或多个时间序列（如DataFrame的列）中的百分比变化。可以这样表达：

```
ts / ts.shift(1) - 1
```

#### 通过偏移量对日期进行位移

pandas的日期偏移量还可以用在datetime或Timestamp对象上。

如果加的是锚点偏移量（比如MonthEnd），第一次增量会将原日期向前滚动到符合频率规则的下一个日期。通过锚点偏移量的rollforward和rollback方法，可明确地将日期向前或向后“滚动”。

```python
from pandas.tseries.offsets import Day, MonthEnd
now = datetime(2011, 11, 17)
now + 3 * Day()
now + MonthEnd()
now + MonthEnd(2)
offset = MonthEnd()
offset.rollforward(now)
offset.rollback(now)
```

日期偏移量还有一个巧妙的用法，即结合groupby使用这两个“滚动”方法。当然，更简单、更快速地实现该功能的办法是使用resample。

```python
ts = pd.Series(np.random.randn(20),
               index=pd.date_range('1/15/2000', periods=20, freq='4d'))
ts.groupby(offset.rollforward).mean()
ts.resample('M').mean()
```

### 11.4 时区处理
时间序列处理工作中最让人不爽的就是对时区的处理。许多人都选择以协调世界时（UTC，它是格林尼治标准时间（Greenwich Mean Time）的接替者，目前已经是国际标准了）来处理时间序列。时区是以UTC偏移量的形式表示的。例如，夏令时期间，纽约比UTC慢4小时，而在全年其他时间则比UTC慢5小时。

在Python中，时区信息来自第三方库pytz，它使Python可以使用Olson数据库（汇编了世界时区信息）。这对历史数据非常重要，这是因为由于各地政府的各种突发奇想，夏令时转变日期（甚至UTC偏移量）已经发生过多次改变了。就拿美国来说，DST转变时间自1900年以来就改变过多次！

由于pandas包装了pytz的功能，因此你可以不用记忆其API，只要记得时区的名称即可。时区名可以在shell中
看到，也可以通过文档查看。

要从pytz中获取时区对象，使用pytz.timezone即可。pandas中的方法既可以接受时区名也可以接受这些对象。

```python
import pytz
pytz.common_timezones[-5:]
tz = pytz.timezone('America/New_York')
```

#### 时区本地化和转换
默认情况下，pandas中的时间序列是单纯（naive）的时区。

看看下面这个时间序列，其索引的tz字段为None。可以用时区集生成日期范围。

```python
rng = pd.date_range('3/9/2012 9:30', periods=6, freq='D')
ts = pd.Series(np.random.randn(len(rng)), index=rng)
print(ts.index.tz)
pd.date_range('3/9/2012 9:30', periods=10, freq='D', tz='UTC')
```

从单纯到本地化的转换是通过tz_localize方法处理的。一旦时间序列被本地化到某个特定时区，就可以用tz_convert将其转换到别的时区了。

对于这种时间序列（它跨越了美国东部时区的夏令时转变期），我们可以将其本地化到EST，然后转换为UTC或柏林时间。

tz_localize和tz_convert也是DatetimeIndex的实例方法。

```python
ts_utc = ts.tz_localize('UTC')
ts_utc.index
ts_utc.tz_convert('America/New_York')

ts_eastern = ts.tz_localize('America/New_York')
ts_eastern.tz_convert('UTC')
ts_eastern.tz_convert('Europe/Berlin')

ts.index.tz_localize('Asia/Shanghai')
```

#### 操作时区意识型Timestamp对象
跟时间序列和日期范围差不多，独立的Timestamp对象也能被从单纯型（naive）本地化为时区意识型（time	zone-aware），并从一个时区转换到另一个时区。在创建Timestamp时，还可以传入一个时区信息。

时区意识型Timestamp对象在内部保存了一个UTC时间戳值（自UNIX纪元（1970年1月1日）算起的纳秒数）。这个UTC值在时区转换过程中是不会发生变化的。

```python
stamp = pd.Timestamp('2011-03-12 04:00')
stamp_utc = stamp.tz_localize('utc')
stamp_utc.tz_convert('America/New_York')
stamp_moscow = pd.Timestamp('2011-03-12 04:00', tz='Europe/Moscow')

stamp_utc.value
stamp_utc.tz_convert('America/New_York').value
```

当使用pandas的DateOffset对象执行时间算术运算时，运算过程会自动关注是否存在夏令时转变期。

#### 不同时区之间的运算

如果两个时间序列的时区不同，在将它们合并到一起时，最终结果就会是UTC。由于时间戳其实是以UTC存储的，所以这是一个很简单的运算，并不需要发生任何转换。

```python
rng = pd.date_range('3/7/2012 9:30', periods=10, freq='B')
ts = pd.Series(np.random.randn(len(rng)), index=rng)
ts
ts1 = ts[:7].tz_localize('Europe/London')
ts2 = ts1[2:].tz_convert('Europe/Moscow')
result = ts1 + ts2
result.index
```

### 11.5 时期及其算术运算
时期（period）表示的是时间区间，比如数日、数月、数季、数年等。Period类所表示的就是这种数据类型，其构造函数需要用到一个字符串或整数，以及频率。

下例这个Period对象表示的是从2007年1月1日到2007年12月31日之间的整段时间。只需对Period对象加上或减去一个整数即可达到根据其频率进行位移的效果。如果两个Period对象拥有相同的频率，则它们的差就是它们之间的单位数量。

```python
p = pd.Period(2007, freq='A-DEC')
p + 5
p - 2
pd.Period('2014', freq='A-DEC') - p
```

period_range函数可用于创建规则的时期范围。PeriodIndex类保存了一组Period，它可以在任何pandas数据结构中被用作轴索引。如果有一个字符串数组，也可以使用PeriodIndex类。

```python
rng = pd.period_range('2000-01-01', '2000-06-30', freq='M')
pd.Series(np.random.randn(6), index=rng)
values = ['2001Q3', '2002Q2', '2003Q1']
index = pd.PeriodIndex(values, freq='Q-DEC')
```

### 时期的频率转换
Period和PeriodIndex对象都可以通过其asfreq方法被转换成别的频率。

假设我们有一个年度时期，希望将其转换为当年年初或年末的一个月度时期。可以将Period('2007','A-DEC')看做一个被划分为多个月度时期的时间段中的游标。对于一个不以12月结束的财政年度，月度子时期的归属情况就不一样了。

在将高频率转换为低频率时，超时期（superperiod）是由子时期（subperiod）所属的位置决定的。例如，在A-JUN频率中，月份“2007年8月”实际上是属于周期“2008年”的。

```python
p = pd.Period('2007', freq='A-DEC')  # from 2007-01 to 2007-12
p.asfreq('M', how='start')
p.asfreq('M', how='end')

p = pd.Period('2007', freq='A-JUN')  # from 2006-07 to 2007-06
p.asfreq('M', 'start')  # output: Period('2006-07', 'M')
p.asfreq('M', 'end')  # output: Period('2007-06', 'M')

p = pd.Period('Aug-2007', 'M')
p.asfreq('A-JUN')  # output: Period('2008', 'A-JUN')
```

完整的PeriodIndex或TimeSeries的频率转换方式也是如此。这里，根据年度时期的第一个月，每年的时期被取代为每月的时期。如果我们想要每年的最后一个工作日，我们可以使用“B”频率，并指明想要该时期的末尾。

```python
rng = pd.period_range('2006', '2009', freq='A-DEC')
ts = pd.Series(np.random.randn(len(rng)), index=rng)
ts.asfreq('M', how='start')
ts.asfreq('B', how='end')
```

#### 按季度计算的时期频率
季度型数据在会计、金融等领域中很常见。许多季度型数据都会涉及“财年末”的概念，通常是一年12个月中某月的最后一个日历日或工作日。就这一点来说，时期"2012Q4"根据财年末的不同会有不同的含义。pandas支持12种可能的季度型频率，即Q-JAN到Q-DEC。在以1月结束的财年中，2012Q4是从1 1月到1月（将其转换为日型频率就明白了）。

```python
p = pd.Period('2012Q4', freq='Q-JAN')
p.asfreq('D', 'start')
p.asfreq('D', 'end')
```

Period之间的算术运算会非常简单。例如，要获取该季度倒数第二个工作日下午4点的时间戳，你可以这样；period_range可用于生成季度型范围。季度型范围的算术运算与此类似。

```python
p4pm = (p.asfreq('B', 'e') - 1).asfreq('T', 's') + 16 * 60
p4pm.to_timestamp()

rng = pd.period_range('2011Q3', '2012Q4', freq='Q-JAN')
ts = pd.Series(np.arange(len(rng)), index=rng)
new_rng = (rng.asfreq('B', 'e') - 1).asfreq('T', 's') + 16 * 60
ts.index = new_rng.to_timestamp()
```

#### 将Timestamp转换为Period（及其反向过程）

通过使用to_period方法，可以将由时间戳索引的Series和DataFrame对象转换为以时期索引。

由于时期指的是非重叠时间区间，因此对于给定的频率，一个时间戳只能属于一个时期。新PeriodIndex的频率默认是从时间戳推断而来的，你也可以指定任何别的频率。结果中允许存在重复时期。要转换回时间戳，使用to_timestamp即可。

```python
rng = pd.date_range('2000-01-01', periods=3, freq='M')
ts = pd.Series(np.random.randn(3), index=rng)
pts = ts.to_period()

rng = pd.date_range('1/29/2000', periods=6, freq='D')
ts2 = pd.Series(np.random.randn(6), index=rng)
ts2.to_period('M')

pts = ts2.to_period()
pts.to_timestamp(how='end')
```

#### 通过数组创建PeriodIndex

固定频率的数据集通常会将时间信息分开存放在多个列中。例如，在下面这个宏观经济数据集中，年度和季度就分别存放在不同的列中。通过将这些数组以及一个频率传入PeriodIndex，就可以将它们合并成DataFrame的一个索引。





```python
data = pd.read_csv('examples/macrodata.csv')
data.head(5)
data.year
data.quarter
index = pd.PeriodIndex(year=data.year, quarter=data.quarter,
                       freq='Q-DEC')
data.index = index
data.infl
```

### 11.6 重采样及频率转换
重采样（resampling）指的是将时间序列从一个频率转换到另一个频率的处理过程。将高频率数据聚合到低频率称为降采样（downsampling），而将低频率数据转换到高频率则称为升采样（upsampling）。并不是所有的重采样都能被划分到这两个大类中。例如，将W-WED（每周三）转换为W-FRI既不是降采样也不是升采样。

pandas对象都带有一个resample方法，它是各种频率转换工作的主力函数。resample有一个类似于groupby的API，调用resample可以分组数据，然后会调用一个聚合函数。

```python
rng = pd.date_range('2000-01-01', periods=100, freq='D')
ts = pd.Series(np.random.randn(len(rng)), index=rng)
ts.resample('M').mean()
ts.resample('M', kind='period').mean()
```

resample是一个灵活高效的方法，可用于处理非常大的时间序列。它的一些参数选项。

- freq  表示重采样频率的字符串或DateOffset，例如'M', '5min'或Second(15)
- axis  重采样的轴，默认为axis=0
- fill_method  升采样如何插值，例如'ffill'或'lfill'。默认不插值
- closed  在降采样中，各时间段的那一端是闭合的（即包含），right或left。默认right
- label  在降采样中，如何设置聚合值的标签，right或left（面元的右边界或左边界）。默认right
- loffset  面元标签的时间校正值，例如'-1s'/Second(-1)用于将聚合标签调早1秒
- limit  在前向或后向填充时，允许填充的最大时期数
- kind  聚合到周期（'period'）或时间戳（'timestamp'），默认聚合到时间序列的索引类型
- convention  当对周期进行重采样，将低频周期转换为高频周期的惯用法，'start'或'end'，默认是'start'

#### 降采样
将数据聚合到规律的低频率是一件非常普通的时间序列处理任务。待聚合的数据不必拥有固定的频率，期望的频率会自动定义聚合的面元边界，这些面元用于将时间序列拆分为多个片段。例如，要转换到月度频率（'M'或'BM'），数据需要被划分到多个单月时间段中。各时间段都是半开放的。一个数据点只能属于一个时间段，所有时间段的并集必须能组成整个时间帧。

在用resample对数据进行降采样时，需要考虑两样东西：

- 各区间哪边是闭合的。
- 如何标记各个聚合面元，用区间的开头还是末尾。

看一些“1分钟”数据。假设你想要通过求和的方式将这些数据聚合到“5分钟”块中。传入的频率将会以“5分钟”的增量定义面元边界。默认情况下，面元的右边界是包含的，因此00:00到00:05的区间中是包含00:05的。传入closed='left'会让区间以左边界闭合。

默认的最终的时间序列是以各面元右边界的时间戳进行标记的。传入label='right'即可用面元的右边界对其进行标记。

最后，你可能希望对结果索引做一些位移，比如从右边界减去一秒以便更容易明白该时间戳到底表示的是哪个区间。只需通过loffset设置一个字符串或日期偏移量即可实现这个目的。

```python
rng = pd.date_range('2000-01-01', periods=12, freq='T')
ts = pd.Series(np.arange(12), index=rng)
ts.resample('5min', closed='right').sum()
ts.resample('5min', closed='left').sum()
ts.resample('5min', closed='right', label='right').sum()
ts.resample('5min', closed='right', label='right', loffset='-1s').sum()
```

此外，也可以通过调用结果对象的shift方法来实现该目的，这样就不需要设置loffset了。

#### OHLC重采样

金融领域中有一种无所不在的时间序列聚合方式，即计算各面元的四个值：第一个值（open，开盘）、最后一个值（close，收盘）、最大值（high，最高）以及最小值（low，最低）。传入how='ohlc'即可得到一个含有这四种聚合值的DataFrame。整个过程很高效，只需一次扫描即可计算出结果：

```python
ts.resample('5min').ohlc()
```

#### 升采样和插值
在将数据从低频率转换到高频率时，就不需要聚合了。

看一个带有一些周型数据的DataFrame。当你对这个数据进行聚合，每组只有一个值，这样就会引入缺失值。使用asfreq方法转换成高频，不经过聚合。

假设你想要用前面的周型值填充“非星期三”。resample的填充和插值方式跟fillna和reindex的一样。也可以只填充指定的时期数（目的是限制前面的观测值的持续使用距离）。

注意，新的日期索引完全没必要跟旧的重叠。

```python
frame = pd.DataFrame(np.random.randn(2, 4),
                     index=pd.date_range('1/1/2000', periods=2,
                                         freq='W-WED'),
                     columns=['Colorado', 'Texas', 'New York', 'Ohio'])
df_daily = frame.resample('D').asfreq()
frame.resample('D').ffill()
frame.resample('D').ffill(limit=2)
frame.resample('W-THU').ffill()
```

#### 通过时期进行重采样
对那些使用时期索引的数据进行重采样与时间戳很像。

升采样要稍微麻烦一些，因为你必须决定在新频率中各区间的哪端用于放置原来的值，就像asfreq方法那样。convention参数默认为'start'，也可设置为'end'。

```python
frame = pd.DataFrame(np.random.randn(24, 4),
                     index=pd.period_range('1-2000', '12-2001',
                                           freq='M'),
                     columns=['Colorado', 'Texas', 'New York', 'Ohio'])
annual_frame = frame.resample('A-DEC').mean()

# Q-DEC: Quarterly, year ending in December
annual_frame.resample('Q-DEC').ffill()
annual_frame.resample('Q-DEC', convention='end').ffill()
```

由于时期指的是时间区间，所以升采样和降采样的规则就比较严格：

- 在降采样中，目标频率必须是源频率的子时期（subperiod）。
- 在升采样中，目标频率必须是源频率的超时期（superperiod）。

如果不满足这些条件，就会引发异常。这主要影响的是按季、年、周计算的频率。例如，由Q-MAR定义的时间区间只能升采样为A-MAR、A-JUN、A-SEP、A-DEC等。

```python
annual_frame.resample('Q-MAR').ffill()
```

### 11.7 移动窗口函数
在移动窗口（可以带有指数衰减权数）上计算的各种统计函数也是一类常见于时间序列的数组变换。这样可以圆滑噪音数据或断裂数据。我将它们称为移动窗口函数（moving window function），其中还包括那些窗口不定长的函数（如指数加权移动平均）。跟其他统计函数一样，移动窗口函数也会自动排除缺失值。

先加载一些时间序列数据，将其重采样为工作日频率。现在引入rolling运算符，它与resample和groupby很像。可以在TimeSeries或DataFrame以及一个window（表示期数）上调用它。

对DataFrame调用移动窗口函数会将转换应用到所有的列上。

rolling函数也可以接受一个指定固定大小时间补偿字符串，而不是一组时期。这样可以方便处理不规律的时间序列。这些字符串也可以传递给resample。例如，我们可以计算20天的滚动均值。

```python
close_px_all = pd.read_csv('examples/stock_px_2.csv',
                           parse_dates=True, index_col=0)
close_px = close_px_all[['AAPL', 'MSFT', 'XOM']]
close_px = close_px.resample('B').ffill()
close_px.AAPL.plot()
close_px.AAPL.rolling(250).mean().plot()
close_px.rolling(60).mean().plot(logy=True)
close_px.rolling('20D').mean()
```

表达式rolling(250)与groupby很像，但不是对其进行分组，而是创建一个按照250天分组的滑动窗口对象。然后，我们就得到了苹果公司股价的250天的移动窗口。默认情况下，rolling函数需要窗口中所有的值为非NA值。可以修改该行为以解决缺失数据的问题。其实，在时间序列开始处尚不足窗口期的那些数据就是个特例。

要计算扩展窗口平均（expanding	window	mean），可以使用expanding而不是rolling。“扩展”意味着，从时间序列的起始处开始窗口，增加窗口直到它超过所有的序列。apple_std250时间序列的扩展窗口平均如下所示。

```python
appl_std250 = close_px.AAPL.rolling(250, min_periods=10).std()
appl_std250[5:12]
appl_std250.plot()
expanding_mean = appl_std250.expanding().mean()
```

#### 指数加权函数
另一种使用固定大小窗口及相等权数观测值的办法是，定义一个衰减因子（decay factor）常量，以便使近期的观测值拥有更大的权数。衰减因子的定义方式有很多，比较流行的是使用时间间隔（span），它可以使结果兼容于窗口大小等于时间间隔的简单移动窗口（simple	moving	window）函数。

由于指数加权统计会赋予近期的观测值更大的权数，因此相对于等权统计，它能“适应”更快的变化。

除了rolling和expanding，pandas还有ewm运算符。下面这个例子对比了苹果公司股价的30日移动平均和span=30的指数加权移动平均。

```python
aapl_px = close_px.AAPL['2006':'2007']
ma60 = aapl_px.rolling(30, min_periods=20).mean()
ewma60 = aapl_px.ewm(span=30).mean()
ma60.plot(style='k--', label='Simple MA')
ewma60.plot(style='k-', label='EW MA')
plt.legend()
```

#### 二元移动窗口函数
有些统计运算（如相关系数和协方差）需要在两个时间序列上执行。例如，金融分析师常常对某只股票对某个参考指数（如标准普尔500指数）的相关系数感兴趣。

要进行说明，先计算我们感兴趣的时间序列的百分数变化。调用rolling之后，corr聚合函数开始计算与spx_rets滚动相关系数。

假设你想要一次性计算多只股票与标准普尔500指数的相关系数。虽然编写一个循环并新建一个DataFrame不是什么难事，但比较啰嗦。其实，只需传入一个TimeSeries和一个DataFrame，rolling_corr就会自动计算TimeSeries（本例中就是spx_rets）与DataFrame各列的相关系数。

```python
spx_px = close_px_all['SPX']
spx_rets = spx_px.pct_change()
returns = close_px.pct_change()
corr = returns.AAPL.rolling(125, min_periods=100).corr(spx_rets)
corr.plot()

corr = returns.rolling(125, min_periods=100).corr(spx_rets)
corr.plot()
```

#### 用户定义的移动窗口函数
rolling方法的apply函数使你能够在移动窗口上应用自己设计的数组函数。唯一要求的就是：该函数要能从数组的各个片段中产生单个值（即约简）。比如说，当我们用rolling(...).quantile(q)计算样本分位数时，可能对样本中特定值的百分等级感兴趣。scipy .stats.percentileofscore函数就能达到这个目的。

```python
from scipy.stats import percentileofscore
score_at_2percent = lambda x: percentileofscore(x, 0.02)
result = returns.AAPL.rolling(250).apply(score_at_2percent)
result.plot()
```

## 第12章 pandas高级应用

### 12.1 分类数据
这一节介绍的是pandas的分类类型，通过使用它可以提高性能和内存的使用率。

#### 背景和目的

表中的一列通常会有重复的包含不同值的小集合的情况。已经学过了unique和value_counts，它们可以从数组提取出不同的值，并分别计算频率。

许多数据系统（数据仓库、统计计算或其它应用）都发展出了特定的表征重复值的方法，以进行高效的存储和计算。在数据仓库中，最好的方法是使用所谓的包含不同值的维表(Dimension Table)，将主要的参数存储为引用维表整数键。可以使用take方法存储原始的字符串Series。

```python
values = pd.Series(['apple', 'orange', 'apple',
                    'apple'] * 2)
pd.unique(values)
pd.value_counts(values)
values = pd.Series([0, 1, 0, 0] * 2)
dim = pd.Series(['apple', 'orange'])
dim.take(values)
```

这种用整数表示的方法称为分类或字典编码表示法。不同值得数组称为分类、字典或数据级。本书中使用分类的说法。表示分类的整数值称为分类编码或简单地称为编码。

分类表示可以在进行分析时大大的提高性能。也可以在保持编码不变的情况下，对分类进行转换。一些相对简单的转变例子包括：

- 重命名分类。
- 加入一个新的分类，不改变已经存在的分类的顺序或位置。

#### pandas的分类类型
pandas有一个特殊的分类类型，用于保存使用整数分类表示法的数据。

看一个例子，这里，df['fruit']是一个Python字符串对象的数组。我们可以通过调用它，将它转变
为分类。fruit_cat的值不是NumPy数组，而是一个pandas.Categorical实例。分类对象有categories和codes属性。

你可将DataFrame的列通过分配转换结果，转换为分类。

```python
fruits = ['apple', 'orange', 'apple', 'apple'] * 2
N = len(fruits)
df = pd.DataFrame({'fruit': fruits,
                   'basket_id': np.arange(N),
                   'count': np.random.randint(3, 15, size=N),
                   'weight': np.random.uniform(0, 4, size=N)},
                  columns=['basket_id', 'fruit', 'count', 'weight'])
fruit_cat = df['fruit'].astype('category')
c = fruit_cat.values
type(c)
c.categories
c.codes
df['fruit'] = df['fruit'].astype('category')
```

你还可以从其它Python序列直接创建pandas.Categorical。

如果你已经从其它源获得了分类编码，你还可以使用from_codes构造器。与显示指定不同，分类变换不认定指定的分类顺序。因此取决于输入数据的顺序，categories数组的顺序会不同。当使用from_codes或其它的构造器时，你可以指定分类一个有意义的顺序。

输出[foo<bar<baz]指明‘foo’位于‘bar ’的前面，以此类推。无序的分类实例可以通过as_ordered排序。

```python
my_categories = pd.Categorical(['foo', 'bar', 'baz', 'foo', 'bar'])
categories = ['foo', 'bar', 'baz']
codes = [0, 1, 2, 0, 0, 1]
my_cats_2 = pd.Categorical.from_codes(codes, categories)
ordered_cat = pd.Categorical.from_codes(codes, categories, ordered=True)
my_cats_2.as_ordered()
```

最后要注意，分类数据不需要字符串，尽管我仅仅展示了字符串的例子。分类数组可以包括任意不可变类型。

#### 用分类进行计算
与非编码版本（比如字符串数组）相比，使用pandas的Categorical有些类似。某些pandas组件，比如groupby函数，更适合进行分类。还有一些函数可以使用有序标志位。

来看一些随机的数值数据，使用pandas.qcut面元函数。它会返回pandas.Categorical，我们之前使用过pandas.cut，但没解释分类是如何工作的。计算这个数据的分位面元，提取一些统计信息。虽然有用，确切的样本分位数与分位的名称相比，不利于生成汇总。我们可以使用labels参数qcut，实现目的。加上标签的面元分类不包含数据面元边界的信息，因此可以使用groupby提取一些汇总信息。

分位数列保存了原始的面元分类信息，包括排序。

```python
np.random.seed(12345)
draws = np.random.randn(1000)
draws[:5]
bins = pd.qcut(draws, 4)
bins = pd.qcut(draws, 4, labels=['Q1', 'Q2', 'Q3', 'Q4'])
bins = pd.Series(bins, name='quartile')
results = (pd.Series(draws)
           .groupby(bins)
           .agg(['count', 'min', 'max'])
           .reset_index())
results['quartile']
```

#### 用分类提高性能
如果你是在一个特定数据集上做大量分析，将其转换为分类可以极大地提高效率。DataFrame列的分类使用的内存通常少的多。

来看一些包含一千万元素的Series，和一些不同的分类。现在，将标签转换为分类。这时，可以看到标签使用的内存远比分类多。转换为分类不是没有代价的，但这是一次性的代价。

```python
N = 10000000
draws = pd.Series(np.random.randn(N))
labels = pd.Series(['foo', 'bar', 'baz', 'qux'] * (N // 4))
labels.memory_usage()
categories.memory_usage()
%time _ = labels.astype('category')
```

GroupBy使用分类操作明显更快，是因为底层的算法使用整数编码数组，而不是字符串数组。

#### 分类方法
包含分类数据的Series有一些特殊的方法，类似于Series.str字符串方法。它还提供了方便的分类和编码的使用方法。

看下面的Series，特别的cat属性提供了分类方法的入口。

假设我们知道这个数据的实际分类集，超出了数据中的四个值。我们可以使用set_categories方法改变它们。虽然数据看起来没变，新的分类将反映在它们的操作中。例如，value_counts表示分类下的计数。

```python
s = pd.Series(['a', 'b', 'c', 'd'] * 2)
cat_s = s.astype('category')
cat_s.cat.codes
cat_s.cat.categories
actual_categories = ['a', 'b', 'c', 'd', 'e']
cat_s2 = cat_s.cat.set_categories(actual_categories)
cat_s.value_counts()
cat_s2.value_counts()
```

在大数据集中，分类经常作为节省内存和高性能的便捷工具。过滤完大DataFrame或Series之后，许多分类可能不会出现在数据中。我们可以使用remove_unused_categories方法删除没看到的分类。

```python
cat_s3 = cat_s[cat_s.isin(['a', 'b'])]
cat_s3.cat.remove_unused_categories()
```

下表列出了可用的分类方法：

- add_categories  在已存在的分类后面添加新的（未使用的）分类
- as_ordered  使分类有序
- as_unordered  使分类无序
- remove_categories  移除分类，设置任何被移除的值为Null
- remove_unused_categories  移除任意不出现在数据中的分类值
- rename_categories  用指定的新分类的名字替换分类；不能改变分类的数目
- reorder_categories  与rename_categories类似，但可以改变结果，使分类有序
- set_cateogories  用指定的新分类的名字替换分类；可以添加或删除分类

#### 为建模创建虚拟变量
当你使用统计或机器学习工具时，通常会将分类数据转换为虚拟变量，也称为one-hot编码。这包括创建一个不同类别的列的DataFrame；这些列包含给定分类的1s，其它为0。

pandas.get_dummies函数可以转换这个分类数据为包含虚拟变量的DataFrame。

```python
cat_s = pd.Series(['a', 'b', 'c', 'd'] * 2, dtype='category')
pd.get_dummies(cat_s)
```

### 12.2	GroupBy高级应用
#### 分组转换和“解封”GroupBy

在第10章的分组操作中学习了apply方法，进行转换。还有另一个transform方法，它与apply很像，但是对使用的函数有一定限制：

- 它可以产生向分组形状广播标量值
- 它可以产生一个和输入组形状相同的对象
- 它不能修改输入

看一个简单的例子，按键进行分组。假设我们想产生一个和df['value']形状相同的Series，但值替换为按键分组的平均值。我们可以传递函数`lambda x: x.mean()`进行转换。

对于内置的聚合函数，我们可以传递一个字符串假名作为GroupBy的agg方法。

```python
df = pd.DataFrame({'key': ['a', 'b', 'c'] * 4,
                   'value': np.arange(12.)})
g = df.groupby('key').value
g.mean()
g.transform(lambda x: x.mean())
g.transform('mean')
```

与apply类似，transform的函数会返回Series，但是结果必须与输入大小相同。举个例子，我们可以用lambda函数将每个分组乘以2。再举一个复杂的例子，我们可以计算每个分组的降序排名。看一个由简单聚合构造的的分组转换函数，我们用transform或apply可以获得等价的结果。

```python
g.transform(lambda x: x * 2)
g.transform(lambda x: x.rank(ascending=False))
def normalize(x):
    return (x - x.mean()) / x.std()
g.transform(normalize)
g.apply(normalize)
```

内置的聚合函数，比如mean或sum，通常比apply函数快，也比transform快。这允许我们进行一个所谓的解封（unwrapped）分组操作。解封分组操作可能包括多个分组聚合，但是矢量化操作还是会带来收益。

```python
g.transform('mean')
normalized = (df['value'] - g.transform('mean')) / g.transform('std')
```

#### 分组的时间重采样
对于时间序列数据，resample方法从语义上是一个基于内在时间的分组操作。

下面是一个示例表，可以用time作为索引，然后重采样。

```python
N = 15
times = pd.date_range('2017-05-20 00:00', freq='1min', periods=N)
df = pd.DataFrame({'time': times,
                   'value': np.arange(N)})
df.set_index('time').resample('5min').count()
```

假设DataFrame包含多个时间序列，用一个额外的分组键的列进行标记。要对每个key值进行相同的重采样，我们引入pandas.TimeGrouper对象。然后设定时间索引，用key和time_key分组，然后聚合。

```python
df2 = pd.DataFrame({'time': times.repeat(3),
                    'key': np.tile(['a', 'b', 'c'], N),
                    'value': np.arange(N * 3.)})
time_key = pd.TimeGrouper('5min')
resampled = (df2.set_index('time')
             .groupby(['key', time_key])
             .sum())
resampled.reset_index()
```

使用TimeGrouper的限制是时间必须是Series或DataFrame的索引。

### 12.3 链式编程技术
当对数据集进行一系列变换时，你可能发现创建的多个临时变量其实并没有在分析中用到。

看下面的例子，虽然这里没有使用真实的数据，这个例子却指出了一些新方法。首先，DataFrame.assign方法是一个`df[k]=v`形式的函数式的列分配方法。它不是就地修改对象，而是返回新的修改过的DataFrame。因此，下面的语句是等价的。就地分配可能会比assign快，但是assign可以方便地进行链式编程。

```python
df = load_data()
df2 = df[df['col2'] < 0]
df2['col1_demeaned'] = df2['col1'] - df2['col1'].mean()
result = df2.groupby('key').col1_demeaned.std()

# Usual non-functional way
df2 = df.copy()
df2['k'] = v
# Functional assign way
df2 = df.assign(k=v)

result = (df2.assign(col1_demeaned=df2.col1 - df2.col2.mean())  # 使用外括号，这样便于添加换行符。
          .groupby('key')
          .col1_demeaned.std())
```

使用链式编程时要注意，你可能会需要涉及临时对象。在前面的例子中，我们不能使用load_data的结果，直到它被赋值给临时变量df。为了这么做，assign和许多其它pandas函数可以接收类似函数的参数，即可调用对象（callable）。

为了展示可调用对象，看一个前面例子的片段，这里，load_data的结果没有赋值给某个变量，因此传递到[	]的函数在这一步被绑定到了对象。我们可以把整个过程写为一个单链表达式。是否将代码写成这种形式只是习惯而已，将它分开成若干步可以提高可读性。

```python
df = load_data()
df2 = df[df['col2'] < 0]
# 上面的语句可以重写为
df = (load_data()
      [lambda x: x['col2'] < 0])

result = (load_data()
          [lambda x: x.col2 < 0]
          .assign(col1_demeaned=lambda x: x.col1 - x.col1.mean())
          .groupby('key')
          .col1_demeaned.std())
```

#### 管道方法
你可以用Python内置的pandas函数和方法，用带有可调用对象的链式编程做许多工作。但是，有时你需要使用自己的函数，或是第三方库的函数。这时就要用到管道方法。

看下面的函数调用，当使用接收、返回Series或DataFrame对象的函数式，你可以调用pipe将其重写。`f(df)和df.pipe(f)`是等价的，但是pipe使得链式声明更容易。

```python
a = f(df, arg1=v1)
b = g(a, v2, arg3=v3)
c = h(b, arg4=v4)
result = (df.pipe(f, arg1=v1)
          .pipe(g, v2, arg3=v3)
          .pipe(h, arg4=v4))
```

pipe的另一个有用的地方是提炼操作为可复用的函数。看一个从列减去分组方法的例子，假设你想转换多列，并修改分组的键。另外，你想用链式编程做这个转换。下面就是一个方法。

```python
g = df.groupby(['key1', 'key2'])
df['col1'] = df['col1'] - g.transform('mean')
def group_demean(df, by, cols):
    result = df.copy()
    g = df.groupby(by)
    for c in cols:
        result[c] = df[c] - g[c].transform('mean')
    return result
result = (df[df.col1 < 0]
          .pipe(group_demean, ['key1', 'key2'], ['col1']))
```

## 第13章 Python建模库介绍

前面介绍了Python数据分析的编程基础。因为数据分析师和科学家总是在数据规整和准备上花费大量时间，这本书的重点在于掌握这些功能。

开发模型选用什么库取决于应用本身。许多统计问题可以用简单方法解决，比如普通的最小二乘回归，其它问题可能需要复杂的机器学习方法。幸运的是，Python已经成为了运用这些分析方法的语言之一，因此读完此书，你可以探索许多工具。

本章中，我会回顾一些pandas的特点，在你胶着于pandas数据规整和模型拟合和评分时，它们可能派上用场。然后我会简短介绍两个流行的建模工具，statsmodels和scikit-learn。这二者每个都值得再写一本书，我就不做全面的介绍，而是建议你学习两个项目的线上文档和其它基于Python的数据科学、统计和机器学习的书籍。

### 13.1	pandas与模型代码的接口
模型开发的通常工作流是使用pandas进行数据加载和清洗，然后切换到建模库进行建模。开发模型的重要一环是机器学习中的“特征工程”。它可以描述从原始数据集中提取信息的任何数据转换或分析，这些数据集可能在建模中有用。本书中学习的数据聚合和GroupBy工具常用于特征工程中。

pandas与其它分析库通常是靠NumPy的数组联系起来的。将DataFrame转换为NumPy数组，可以使用.values属性。要转换回DataFrame，可以传递一个二维ndarray，可带有列名。对于一些模型，你可能只想使用列的子集。建议使用loc，用values作索引。

```python
data = pd.DataFrame({
    'x0': [1, 2, 3, 4, 5],
    'x1': [0.01, -0.01, 0.25, -4.1, 0.],
    'y': [-1.5, 0., 3.6, 1.3, -2.]})
data.columns
data.values
df2 = pd.DataFrame(data.values, columns=['one', 'two', 'three'])
model_cols = ['x0', 'x1']
data.loc[:, model_cols].values
```

注意：最好当数据是均匀的时候使用.values属性。例如，全是数值类型。如果数据是不均匀的，结果会是Python对象的ndarray。

一些库原生支持pandas，会自动完成工作：从DataFrame转换到NumPy，将模型的参数名添加到输出表的列或Series。其它情况，你可以手工进行“元数据管理”。

在第12章学习了pandas的Categorical类型和pandas.get_dummies函数。假设数据集中有一个非数值列。如果我们想替换category列为虚变量，我们可以创建虚变量，删除category列，然后添加到结果。

```python
data['category'] = pd.Categorical(['a', 'b', 'a', 'a', 'b'],
                                  categories=['a', 'b'])
dummies = pd.get_dummies(data.category, prefix='category')
data_with_dummies = data.drop('category', axis=1).join(dummies)
```

用虚变量拟合某些统计模型会有一些细微差别。当你不只有数字列时，使用Patsy可能更简单，更不容易出错。

### 13.2 用Patsy创建模型描述
Patsy是Python的一个库，使用简短的字符串“公式语法”描述统计模型（尤其是线性模型），可能是受到了R和S统计编程语言的公式语法的启发。Patsy适合描述statsmodels的线性模型，其公式是一个特殊的字符串语法，如下所示：

```
y ~ x0 + x1
```

a+b不是将a与b相加的意思，而是为模型创建的设计矩阵。patsy .dmatrices函数接收一个公式字符串和一个数据集（可以是DataFrame或数组的字典），为线性模型创建设计矩阵。这些Patsy的DesignMatrix实例是NumPy的ndarray，带有附加元数据。Intercept是线性模型（比如普通最小二乘回归）的惯例用法。添加`+0`到模型可以不显示intercept。

```python
import patsy
data = pd.DataFrame({
    'x0': [1, 2, 3, 4, 5],
    'x1': [0.01, -0.01, 0.25, -4.1, 0.],
    'y': [-1.5, 0., 3.6, 1.3, -2.]})
y, X = patsy.dmatrices('y ~ x0 + x1', data)
np.asarray(y)
np.asarray(X)
patsy.dmatrices('y ~ x0 + x1 + 0', data)[1]
```

Patsy对象可以直接传递到算法（比如numpy .linalg.lstsq）中，它执行普通最小二乘回归。模型的元数据保留在design_info属性中，因此你可以重新附加列名到拟合系数，以获得一个Series。

```python
coef, resid, _, _ = np.linalg.lstsq(X, y)
coef = pd.Series(coef.squeeze(), index=X.design_info.column_names)
```

#### 用Patsy公式进行数据转换
你可以将Python代码与patsy公式结合。在评估公式时，库将尝试查找在封闭作用域内使用的函数。

常见的变量转换包括标准化（平均值为0，方差为1）和中心化（减去平均值）。Patsy有内置的函数进行这样的工作。

```python
y, X = patsy.dmatrices('y ~ x0 + np.log(np.abs(x1) + 1)', data)
y, X = patsy.dmatrices('y ~ standardize(x0) + center(x1)', data)
```

作为建模的一步，你可能拟合模型到一个数据集，然后用另一个数据集评估模型。另一个数据集可能是剩余的部分或是新数据。当执行中心化和标准化转变，用新数据进行预测要格外小心。因为你必须使用平均值或标准差转换新数据集，这也称作状态转换。

patsy .build_design_matrices函数可以使用原始样本数据集的保存信息，来转换新数据。

因为Patsy中的加号不是加法的意义，当你按照名称将数据集的列相加时，你必须用特殊`I()`函数将它们封装起来 。

```python
new_data = pd.DataFrame({
    'x0': [6, 7, 8, 9],
    'x1': [3.1, -0.5, 0, 2.3],
    'y': [1, 2, 3, 4]})
new_X = patsy.build_design_matrices([X.design_info], new_data)
y, X = patsy.dmatrices('y ~ I(x0 + x1)', data)
```

#### 分类数据和Patsy
非数值数据可以用多种方式转换为模型设计矩阵。当你在Patsy公式中使用非数值数据，它们会默认转换为虚变量。如果有截距，会去掉一个，避免共线性。如果你从模型中忽略截距，每个分类值的列都会包括在设计矩阵的模型中。使用`C()`函数，数值列可以截取为分类量。

```python
data = pd.DataFrame({
    'key1': ['a', 'a', 'b', 'b', 'a', 'b', 'a', 'b'],
    'key2': [0, 1, 0, 1, 0, 1, 0, 0],
    'v1': [1, 2, 3, 4, 5, 6, 7, 8],
    'v2': [-1, 0, 2.5, -0.5, 4.0, -1.2, 0.2, -1.7]
})
y, X = patsy.dmatrices('v2 ~ key1', data)
y, X = patsy.dmatrices('v2 ~ key1 + 0', data)
y, X = patsy.dmatrices('v2 ~ C(key2)', data)
```

当你在模型中使用多个分类名，事情就会变复杂，因为会包括key1:key2形式的相交部分，它可以用在方差（ANOV A）模型分析中。

```python
data['key2'] = data['key2'].map({0: 'zero', 1: 'one'})
y, X = patsy.dmatrices('v2 ~ key1 + key2', data)
y, X = patsy.dmatrices('v2 ~ key1 + key2 + key1:key2', data)
```

Patsy提供转换分类数据的其它方法，包括以特定顺序转换。

### 13.3 statsmodels介绍
statsmodels是Python进行拟合多种统计模型、进行统计试验和数据探索可视化的库。Statsmodels包含许多经典的统计方法，但没有贝叶斯方法和机器学习模型。

statsmodels包含的模型有：

- 线性模型，广义线性模型和健壮线性模型
- 线性混合效应模型
- 方差（ANOV A）方法分析
- 时间序列过程和状态空间模型
- 广义矩估计



下面使用一些基本的statsmodels工具，探索Patsy公式和pandasDataFrame对象如何使用模型接口。

#### 估计线性模型

statsmodels有多种线性回归模型，包括从基本（比如普通最小二乘）到复杂（比如
迭代加权最小二乘法）的。

statsmodels的线性模型有两种不同的接口：基于数组和基于公式。它们可以通过API模块引入：

```python
import statsmodels.api as sm
import statsmodels.formula.api as smf
```

为了展示它们的使用方法，我们从一些随机数据生成一个线性模型。这里，使用了“真实”模型和可知参数beta。此时，dnorm可用来生成正态分布数据，带有特定均值和方差。

```python
def dnorm(mean, variance, size=1):
    if isinstance(size, int):
        size = size,
    return mean + np.sqrt(variance) * np.random.randn(*size)

# For reproducibility
np.random.seed(12345)

N = 100
X = np.c_[dnorm(0, 0.4, size=N),
          dnorm(0, 0.6, size=N),
          dnorm(0, 0.2, size=N)]
eps = dnorm(0, 0.1, size=N)
beta = [0.1, 0.3, 0.5]

y = np.dot(X, beta) + eps
```

像之前Patsy看到的，线性模型通常要拟合一个截距。sm.add_constant函数可以添加一个截距的列到现存的矩阵。sm.OLS类可以拟合一个普通最小二乘回归。这个模型的fit方法返回了一个回归结果对象，它包含估计的模型参数和其它内容。对结果使用summary方法可以打印模型的详细诊断结果。

```python
X_model = sm.add_constant(X)
model = sm.OLS(y, X)
results = model.fit()
results.params
print(results.summary())
```

假设所有的模型参数都在一个DataFrame中，现在，使用statsmodels的公式API和Patsy的公式字符串，观察下statsmodels是如何返回Series结果的，附带有DataFrame的列名。当使用公式和pandas对象时，我们不需要使用add_constant。

给出一个样本外数据，你可以根据估计的模型参数计算预测值。

```python
data = pd.DataFrame(X, columns=['col0', 'col1', 'col2'])
data['y'] = y
results = smf.ols('y ~ col0 + col1 + col2', data=data).fit()
results.params
results.tvalues
results.predict(data[:5])
```

statsmodels的线性模型结果还有其它的分析、诊断和可视化工具。除了普通最小二乘模型，还有其它的线性模型。

#### 估计时间序列过程
statsmodels的另一模型类是进行时间序列分析，包括自回归过程、卡尔曼滤波和其它状态空间模型，和多元自回归模型。

用自回归结构和噪声来模拟一些时间序列数据。这个数据有AR(2)结构（两个延迟），参数是0.8和-0.4。拟合AR模型时，你可能不知道滞后项的个数，因此可以用较多的滞后量来拟合这个模型。结果中的估计参数首先是截距，其次是前两个参数的估计值。

```python
init_x = 4
values = [init_x, init_x]
N = 1000
b0 = 0.8
b1 = -0.4
noise = dnorm(0, 0.1, N)
for i in range(N):
    new_x = values[-1] * b0 + values[-2] * b1 + noise[i]
    values.append(new_x)

MAXLAGS = 5
model = sm.tsa.AR(values)
results = model.fit(MAXLAGS)   
results.params
```

### 13.4 scikit-learn介绍
scikit-learn是一个广泛使用、用途多样的Python机器学习库。它包含多种标准监督和非监督机器学习方法和模型选择和评估、数据转换、数据加载和模型持久化工具。这些模型可以用于分类、聚合、预测和其它任务。

pandas非常适合在模型拟合前处理数据集。举个例子，用一个Kaggle竞赛的经典数据集，关于泰坦尼克号乘客的生还率。用pandas加载测试和训练数据集。statsmodels和scikit-learn通常不能接收缺失数据，因此我们要查看列是否包含缺失值。

```python
train = pd.read_csv('datasets/titanic/train.csv')
test = pd.read_csv('datasets/titanic/test.csv')
train.isnull().sum()
test.isnull().sum()
```

在统计和机器学习的例子中，根据数据中的特征，一个典型的任务是预测乘客能否生还。模型先在训练数据集中拟合，然后用样本外测试数据集评估。

假如想用年龄作为预测值，但是它包含缺失值。缺失数据补全的方法有多种，这里用的是一种简单方法，用训练数据集的中位数补全两个表的空值。

```python
impute_value = train['Age'].median()
train['Age'] = train['Age'].fillna(impute_value)
test['Age'] = test['Age'].fillna(impute_value)
```

现在我们需要指定模型。我增加了一个列IsFemale，作为“Sex”列的编码。然后，我们确定一些模型变量，并创建NumPy数组。我不能保证这是一个好模型，但它的特征都符合。我们用scikit-learn的LogisticRegression模型，创建一个模型实例。与statsmodels类似，我们可以用模型的fit方法，将它拟合到训练数据。现在，我们可以用model.predict，对测试数据进行预测。如果你有测试数据集的真实值，你可以计算准确率或其它错误度量值。

```python
train['IsFemale'] = (train['Sex'] == 'female').astype(int)
test['IsFemale'] = (test['Sex'] == 'female').astype(int)
predictors = ['Pclass', 'IsFemale', 'Age']
X_train = train[predictors].values
X_test = test[predictors].values
y_train = train['Survived'].values

from sklearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fit(X_train, y_train)
y_predict = model.predict(X_test)
(y_true == y_predict).mean()
```

在实际中，模型训练经常有许多额外的复杂因素。许多模型有可以调节的参数，有些方法（比如交叉验证）可以用来进行参数调节，避免对训练数据过拟合。这通常可以提高预测性或对新数据的健壮性。

交叉验证通过分割训练数据来模拟样本外预测。基于模型的精度得分（比如均方差），可以对模型参数进行网格搜索。有些模型，如logistic回归，有内置的交叉验证的估计类。例如，logisticregressioncv类可以用一个参数指定网格搜索对模型的正则化参数C的粒度。

要手动进行交叉验证，你可以使用cross_val_score帮助函数，它可以处理数据分割。例如，要交叉验证我们的带有四个不重叠训练数据的模型，可以这样做：

```python
from sklearn.linear_model import LogisticRegressionCV
model_cv = LogisticRegressionCV(10)
model_cv.fit(X_train, y_train)

from sklearn.model_selection import cross_val_score
model = LogisticRegression(C=10)
scores = cross_val_score(model, X_train, y_train, cv=4)
```

默认的评分指标取决于模型本身，但是可以明确指定一个评分。交叉验证过的模型需要更长时间来训练，但会有更高的模型性能。

## 第14章 数据分析案例

### 14.1 来自Bitly的USA.gov数据
2011年，URL缩短服务Bitly跟美国政府网站USA.gov合作，提供了一份从生成.gov或.mil短链接的用户那里收集来的匿名数据。在201 1年，除实时数据之外，还可以下载文本文件形式的每小时快照。

以每小时快照为例，文件中各行的格式为JSON（即JavaScript Object Notation）。Python有内置或第三方模块可以将JSON字符串转换成Python字典对象。这里，使用json模块及其loads函数逐行加载已经下载好的数据文件。现在，records对象就成为一组Python字典了。

```python
import json
path = 'datasets/usagov_bitly_data.txt'
records = [json.loads(line) for line in open(path)]
```

#### 用纯Python代码对时区进行计数

假设我们想要知道该数据集中最常出现的是哪个时区（即tz字段），得到答案的办法有很多。首先，我们用列表推导式取出一组时区。并不是所有记录都有时区字段。只需在列表推导式末尾加上一个`if 'tz' in rec`判断即可。

```python
time_zones = [rec['tz'] for rec in records if 'tz' in rec]
time_zones[:10]
```

只看前10个时区，我们发现有些是未知的（即空的）。虽然可以将它们过滤掉，但现在暂时先留着。

接下来，为了对时区进行计数，这里介绍两个办法：一个较难（只使用标准Python库），另一个较简单（使用pandas）。计数的办法之一是在遍历时区的过程中将计数值保存在字典中。将逻辑写到函数中是为了获得更高的复用性。要用它对时区进行处理，只需将time_zones传入即可。

```python
def get_counts(sequence):
    counts = {}
    for x in sequence:
        if x in counts:
            counts[x] += 1
        else:
            counts[x] = 1
    return counts
# 如果使用Python标准库的更高级工具，可以将代码写得更简洁一些
from collections import defaultdict
def get_counts2(sequence):
    counts = defaultdict(int) # values will initialize to 0
    for x in sequence:
        counts[x] += 1
    return counts
counts = get_counts(time_zones)
counts['America/New_York']
len(time_zones)
```

如果想要得到前10位的时区及其计数值，我们需要用到一些有关字典的处理技巧。如果你搜索Python的标准库，你能找到collections.Counter类，它可以使这项工作更简单。

```python
def top_counts(count_dict, n=10):
    value_key_pairs = [(count, tz) for tz, count in count_dict.items()]
    value_key_pairs.sort()
    return value_key_pairs[-n:]
top_counts(counts)

from collections import Counter
counts = Counter(time_zones)
counts.most_common(10)
```

#### 用pandas对时区进行计数
从原始记录的集合创建DateFrame，与将记录列表传递到pandas.DataFrame一样简单。这里frame的输出形式是摘要视图（summary view），主要用于较大的DataFrame对象。我们然后可以对Series使用value_counts方法。

```python
frame = pd.DataFrame(records)
frame.info()
frame['tz'][:10]
tz_counts = frame['tz'].value_counts()
tz_counts[:10]
```

我们可以用matplotlib可视化这个数据。为此，我们先给记录中未知或缺失的时区填上一个替代值。fillna函数可以替换缺失值（NA），而未知值（空字符串）则可以通过布尔型数组索引加以替换。然后可以用seaborn包创建水平柱状图。

```python
clean_tz = frame['tz'].fillna('Missing')
clean_tz[clean_tz == ''] = 'Unknown'
tz_counts = clean_tz.value_counts()
tz_counts[:10]
plt.figure(figsize=(10, 4))
import seaborn as sns
subset = tz_counts[:10]
sns.barplot(y=subset.index, x=subset.values)
```

a字段含有执行URL短缩操作的浏览器、设备、应用程序的相关信息。将这些"agent"字符串中的所有信息都解析出来是一件挺郁闷的工作。一种策略是将这种字符串的第一节（与浏览器大致对应）分离出来并得到另外一份用户行为摘要。

```python
frame['a'][1]
frame['a'][50]
frame['a'][51][:50]  # long line
results = pd.Series([x.split()[0] for x in frame.a.dropna()])
results[:5]
results.value_counts()[:8]
```

现在，假设你想按Windows和非Windows用户对时区统计信息进行分解。为了简单起见，我们假定只要agent字符串中含有"Windows"就认为该用户为Windows用户。由于有的agent缺失，所以首先将它们从数据中移除。然后计算出各行是否含有Windows的值。接下来就可以根据时区和新得到的操作系统列表对数据进行分组了。分组计数，类似于value_counts函数，可以用size来计算。并利用unstack对计数结果进行重塑。最后，我们来选取最常出现的时区。为了达到这个目的，我根据agg_counts中的行数构造了一个间接索引数组。然后我通过take按照这个顺序截取了最后10行最大值。pandas有一个简便方法nlargest，可以做同样的工作。

```python
cframe = frame[frame.a.notnull()]
cframe['os'] = np.where(cframe['a'].str.contains('Windows'),
                        'Windows', 'Not Windows')
cframe['os'][:5]
by_tz_os = cframe.groupby(['tz', 'os'])
agg_counts = by_tz_os.size().unstack().fillna(0)
agg_counts[:10]
# Use to sort in ascending order
indexer = agg_counts.sum(1).argsort()
indexer[:10]
count_subset = agg_counts.take(indexer[-10:])
count_subset
agg_counts.sum(1).nlargest(10)
```

然后，可以用柱状图表示。我传递一个额外参数到seaborn的barpolt函数，来画一个堆积条形图。这张图不容易看出Windows用户在小分组中的相对比例，因此标准化分组百分比之和为1，再次画图。

还可以用groupby的transform方法，更高效的计算标准化的和。

```python
# Rearrange the data for plotting
count_subset = count_subset.stack()
count_subset.name = 'total'
count_subset = count_subset.reset_index()
count_subset[:10]
sns.barplot(x='total', y='tz', hue='os',  data=count_subset)

def norm_total(group):
    group['normed_total'] = group.total / group.total.sum()
    return group
results = count_subset.groupby('tz').apply(norm_total)
sns.barplot(x='normed_total', y='tz', hue='os',  data=results)

g = count_subset.groupby('tz')
results2 = count_subset.total / g.total.transform('sum')
```

### 14.2 MovieLens 1M数据集

[GroupLens Research](http://www .grouplens.org/node/73)采集了一组从20世纪90年末到21世纪初由MovieLens用户提供的电影评分数据。这些数据中包括电影评分、电影元数据（风格类型和年代）以及关于用户的人口统计学数据（年龄、邮编、性别和职业等）。基于机器学习算法的推荐系统一般都会对此类数据感兴趣，本案例介绍如何对这种数据进行切片切块以满足实际需求。

MovieLens 1M数据集含有来自6000名用户对4000部电影的100万条评分数据。它分为三个表：评分、用户信息和电影信息。将该数据从zip文件中解压出来之后，可以通过pandas.read_table将各个表分别读到一个pandas	DataFrame对象中。利用Python的切片语法，通过查看每个DataFrame的前几行即可验证数据加载工作是否一切顺利。

```python
# Make display smaller
pd.options.display.max_rows = 10

unames = ['user_id', 'gender', 'age', 'occupation', 'zip']
users = pd.read_table('datasets/movielens/users.dat', sep='::',
                      header=None, names=unames)

rnames = ['user_id', 'movie_id', 'rating', 'timestamp']
ratings = pd.read_table('datasets/movielens/ratings.dat', sep='::',
                        header=None, names=rnames)

mnames = ['movie_id', 'title', 'genres']
movies = pd.read_table('datasets/movielens/movies.dat', sep='::',
                       header=None, names=mnames)

users[:5]
ratings[:5]
movies[:5]
ratings
```

注意，其中的年龄和职业是以编码形式给出的，它们的具体含义请参考该数据集的README文件。分析散布在三个表中的数据可不是一件轻松的事情。假设我们想要根据性别和年龄计算某部电影的平均得分，如果将所有数据都合并到一个表中的话问题就简单多了。

先用pandas的merge函数将ratings跟users合并到一起，然后再将movies也合并进去。pandas会根据列名的重叠情况推断出哪些列是合并（或连接）键。

为了按性别计算每部电影的平均得分，我们可以使用pivot_table方法。该操作产生了另一个DataFrame，其内容为电影平均得分，行标为电影名称（索引），列标为性别。

现在，我打算过滤掉评分数据不够250条的电影（随便选的一个数字）。为了达到这个目的，我先对title进行分组，然后利用size()得到一个含有各电影分组大小的Series对象。标题索引中含有评分数据大于250条的电影名称，然后我们就可以据此从前面的mean_ratings中选取所需的行了。为了了解女性观众最喜欢的电影，我们可以对F列降序排列。

```python
data = pd.merge(pd.merge(ratings, users), movies)
data
data.iloc[0]

mean_ratings = data.pivot_table('rating', index='title',
                                columns='gender', aggfunc='mean')
mean_ratings[:5]

ratings_by_title = data.groupby('title').size()
ratings_by_title[:10]
active_titles = ratings_by_title.index[ratings_by_title >= 250]
active_titles

# Select rows on the index
mean_ratings = mean_ratings.loc[active_titles]
mean_ratings

top_female_ratings = mean_ratings.sort_values(by='F', ascending=False)
top_female_ratings[:10]
```

#### 计算评分分歧

假设我们想要找出男性和女性观众分歧最大的电影。一个办法是给mean_ratings加上一个用于存放平均得分之差的列，并对其进行排序。按"diff"排序即可得到分歧最大且女性观众更喜欢的电影。对排序结果反序并取出前10行，得到的则是男性观众更喜欢的电影。如果只是想要找出分歧最大的电影（不考虑性别因素），则可以计算得分数据的方差或标准差。

```python
mean_ratings['diff'] = mean_ratings['M'] - mean_ratings['F']
sorted_by_diff = mean_ratings.sort_values(by='diff')
sorted_by_diff[:10]

# Reverse order of rows, take first 10 rows
sorted_by_diff[::-1][:10]

# Standard deviation of rating grouped by title
rating_std_by_title = data.groupby('title')['rating'].std()
# Filter down to active_titles
rating_std_by_title = rating_std_by_title.loc[active_titles]
# Order Series by value in descending order
rating_std_by_title.sort_values(ascending=False)[:10]
```

可能你已经注意到了，电影分类是以竖线（|）分隔的字符串形式给出的。如果想对电影分类进行分析的话，就需要先将其转换成更有用的形式才行。

### 14.3 1880-2010年间全美婴儿姓名
美国社会保障总署（SSA）提供了一份从1880年到现在的婴儿名字频率数据。你可以用这个数据集做很多事，例如：

- 计算指定名字（可以是你自己的，也可以是别人的）的年度比例。
- 计算某个名字的相对排名。
- 计算各年度最流行的名字，以及增长或减少最快的名字。
- 分析名字趋势：元音、辅音、长度、总体多样性、拼写变化、首尾字母等。
- 分析外源性趋势：圣经中的名字、名人、人口结构变化等。

由于这是一个非常标准的以逗号隔开的格式，所以可以用pandas.read_csv将其加载到DataFrame中。这些文件中仅含有当年出现超过5次的名字。为了简单起见，我们可以用births列的sex分组小计表示该年度的births总计。

由于该数据集按年度被分隔成了多个文件，所以第一件事情就是要将所有数据都组装到一个DataFrame里面，并加上一个year字段。使用pandas.concat即可达到这个目的。

```python
names1880 = pd.read_csv('datasets/names/yob1880.txt',
                        names=['name', 'sex', 'births'])
names1880.groupby('sex').births.sum()

years = range(1880, 2011)
pieces = []
columns = ['name', 'sex', 'births']
for year in years:
    path = 'datasets/names/yob%d.txt' % year
    frame = pd.read_csv(path, names=columns)

    frame['year'] = year
    pieces.append(frame)
# Concatenate everything into a single DataFrame
names = pd.concat(pieces, ignore_index=True)
```

现在我们得到了一个非常大的DataFrame，它含有全部的名字数据。这里需要注意几件事情。

- 第一，concat默认是按行将多个DataFrame组合到一起的；
- 第二，必须指定ignore_index=True，因为我们不希望保留read_csv所返回的原始行号。

有了这些数据之后，我们就可以利用groupby或pivot_table在year和sex级别上对其进行聚合了。

```python
total_births = names.pivot_table('births', index='year',
                                 columns='sex', aggfunc=sum)
total_births.tail()
total_births.plot(title='Total births by sex and year')
```

下面我们来插入一个prop列，用于存放指定名字的婴儿数相对于总出生数的比例。prop值为0.02表示每100名婴儿中有2名取了当前这个名字。因此，我们先按year和sex分组，然后再将新列加到各个分组上。

在执行这样的分组处理时，一般都应该做一些有效性检查，比如验证所有分组的prop的总和是否为1。

```python
def add_prop(group):
    group['prop'] = group.births / group.births.sum()
    return group
names = names.groupby(['year', 'sex']).apply(add_prop)
names.groupby(['year', 'sex']).prop.sum()
```

为了便于实现更进一步的分析，我需要取出该数据的一个子集：每对sex/year组合的前1000个名字。这又是一个分组操作。

```python
def get_top1000(group):
    return group.sort_values(by='births', ascending=False)[:1000]
grouped = names.groupby(['year', 'sex'])
top1000 = grouped.apply(get_top1000)
# Drop the group index, not needed
top1000.reset_index(inplace=True, drop=True)

# 如果喜欢DIY的话，也可以这样
pieces = []
for year, group in names.groupby(['year', 'sex']):
    pieces.append(group.sort_values(by='births', ascending=False)[:1000])
top1000 = pd.concat(pieces, ignore_index=True)
```

#### 分析命名趋势
有了完整的数据集和刚才生成的top1000数据集，我们就可以开始分析各种命名趋势了。首先将前1000个名字分为男女两个部分。这是两个简单的时间序列，只需稍作整理即可绘制出相应的图表（比如每年叫做John和Mary的婴儿数）。我们先生成一张按year和name统计的总出生数透视表。现在，我们用DataFrame的plot方法绘制几个名字的曲线图。

```python
boys = top1000[top1000.sex == 'M']
girls = top1000[top1000.sex == 'F']
total_births = top1000.pivot_table('births', index='year',
                                   columns='name',
                                   aggfunc=sum)
total_births.info()
subset = total_births[['John', 'Harry', 'Mary', 'Marilyn']]
subset.plot(subplots=True, figsize=(12, 10), grid=False,
            title="Number of births per year")
```

从图中可以看出，这几个名字在美国人民的心目中已经风光不再了。但事实并非如此简单，我们在下一节中就能知道是怎么一回事了。

#### 评估命名多样性的增长

一种解释是父母愿意给小孩起常见的名字越来越少。这个假设可以从数据中得到验证。一个办法是计算最流行的1000个名字所占的比例，我按year和sex进行聚合并绘图。从图中可以看出，名字的多样性确实出现了增长（前1000项的比例降低）。

```python
table = top1000.pivot_table('prop', index='year',
                            columns='sex', aggfunc=sum)
table.plot(title='Sum of table1000.prop by year and sex',
           yticks=np.linspace(0, 1.2, 13), xticks=range(1880, 2020, 10))
```

另一个办法是计算占总出生人数前50%的不同名字的数量，这个数字不太好计算。我们只考虑2010年男孩的名字。在对prop降序排列之后，我们想知道前面多少个名字的人数加起来才够50%。虽然编写一个for循环确实也能达到目的，但NumPy有一种更聪明的矢量方式。先计算prop的累计和cumsum，然后再通过searchsorted方法找出0.5应该被插入在哪个位置才能保证不破坏顺序。由于数组索引是从0开始的，因此我们要给这个结果加1，即最终结果为117。拿1900年的数据来做个比较，这个数字要小得多。

```python
df = boys[boys.year == 2010]
prop_cumsum = df.sort_values(by='prop', ascending=False).prop.cumsum()
prop_cumsum[:10]
prop_cumsum.values.searchsorted(0.5)

df = boys[boys.year == 1900]
in1900 = df.sort_values(by='prop', ascending=False).prop.cumsum()
in1900.values.searchsorted(0.5) + 1
```

现在就可以对所有year/sex组合执行这个计算了。按这两个字段进行groupby处理，然后用一个函数计算各分组的这个值。现在，diversity这个DataFrame拥有两个时间序列（每个性别各一个，按年度索引）。通过IPython，你可以查看其内容，还可以像之前那样绘制图表。

```python
def get_quantile_count(group, q=0.5):
    group = group.sort_values(by='prop', ascending=False)
    return group.prop.cumsum().values.searchsorted(q) + 1

diversity = top1000.groupby(['year', 'sex']).apply(get_quantile_count)
diversity = diversity.unstack('sex')
diversity.head()
diversity.plot(title="Number of popular names in top 50%")
```

从图中可以看出，女孩名字的多样性总是比男孩的高，而且还在变得越来越高。可以自己分析一下具体是什么在驱动这个多样性（比如拼写形式的变化）。

#### “最后一个字母”的变革
2007年，一名婴儿姓名研究人员Laura Wattenberg在她自己的网站上指出：近百年来，男孩名字在最后一个字母上的分布发生了显著的变化。为了了解具体的情况，我首先将全部出生数据在年度、性别以及末字母上进行了聚合。然后，我选出具有一定代表性的三年，并输出前面几行。

```python
# extract last letter from name column
get_last_letter = lambda x: x[-1]
last_letters = names.name.map(get_last_letter)
last_letters.name = 'last_letter'

table = names.pivot_table('births', index=last_letters,
                          columns=['sex', 'year'], aggfunc=sum)
subtable = table.reindex(columns=[1910, 1960, 2010], level='year')
subtable.head()
```

接下来我们需要按总出生数对该表进行规范化处理，以便计算出各性别各末字母占总出生人数的比例。有了这个字母比例数据之后，就可以生成一张各年度各性别的条形图了。

```python
subtable.sum()
letter_prop = subtable / subtable.sum()

fig, axes = plt.subplots(2, 1, figsize=(10, 8))
letter_prop['M'].plot(kind='bar', rot=0, ax=axes[0], title='Male')
letter_prop['F'].plot(kind='bar', rot=0, ax=axes[1], title='Female',
                      legend=False)
```

可以看出，从20世纪60年代开始，以字母"n"结尾的男孩名字出现了显著的增长。

回到之前创建的那个完整表，按年度和性别对其进行规范化处理，并在男孩名字中选取几个字母，最后进行转置以便将各个列做成一个时间序列。有了这个时间序列的DataFrame之后，就可以通过其plot方法绘制出一张趋势图了。

```python
letter_prop = table / table.sum()
dny_ts = letter_prop.loc[['d', 'n', 'y'], 'M'].T
dny_ts.head()
dny_ts.plot()
```

#### 变成女孩名字的男孩名字（以及相反的情况）

另一个有趣的趋势是，早年流行于男孩的名字近年来“变性了”，例如Lesley或Leslie。

回到top1000数据集，找出其中以"lesl"开头的一组名字。然后利用这个结果过滤其他的名字，并按名字分组计算出生数以查看相对频率。接下来，我们按性别和年度进行聚合，并按年度进行规范化处理。最后，就可以轻松绘制一张分性别的年度曲线图了。

```python
all_names = pd.Series(top1000.name.unique())
lesley_like = all_names[all_names.str.lower().str.contains('lesl')]
filtered = top1000[top1000.name.isin(lesley_like)]
filtered.groupby('name').births.sum()
table = filtered.pivot_table('births', index='year',
                             columns='sex', aggfunc='sum')
table = table.div(table.sum(1), axis=0)
table.tail()
table.plot(style={'M': 'k-', 'F': 'k--'})
```

### 14.4 USDA食品数据库
美国农业部（USDA）制作了一份有关食物营养信息的数据库。Ashley	Williams制作了该数据的JSON版。每种食物都带有若干标识性属性以及两个有关营养成分和分量的列表。这种形式的数据不是很适合分析工作，因此我们需要做一些规整化以使其具有更好用的形式。

可以用任何喜欢的JSON库将其加载到Python中，这里用的是Python内置的json模块。db中的每个条目都是一个含有某种食物全部数据的字典。nutrients字段是一个字典列表，其中的每个字典对应一种营养成分。

在将字典列表转换为DataFrame时，可以只抽取其中的一部分字段。这里，我们将取出食物的名称、分类、编号以及制造商等信息。通过value_counts，你可以查看食物类别的分布情况。

```python
import json
db = json.load(open('datasets/foods.json'))
len(db)
db[0].keys()
db[0]['nutrients'][0]
nutrients = pd.DataFrame(db[0]['nutrients'])
nutrients[:7]
info_keys = ['description', 'group', 'id', 'manufacturer']
info = pd.DataFrame(db, columns=info_keys)
info[:5]
info.info()
pd.value_counts(info.group)[:10]
```

现在，为了对全部营养数据做一些分析，最简单的办法是将所有食物的营养成分整合到一个大表中。我们分几个步骤来实现该目的。首先，将各食物的营养成分列表转换为一个DataFrame，并添加一个表示编号的列，然后将该DataFrame添加到一个列表中。最后通过concat将这些东西连接起来就可以了。我发现这个DataFrame中无论如何都会有一些重复项，所以直接丢弃就可以了。

```python
nutrients = []
for rec in db:
    fnuts = pd.DataFrame(rec['nutrients'])
    fnuts['id'] = rec['id']
    nutrients.append(fnuts)
nutrients = pd.concat(nutrients, ignore_index=True)

nutrients.duplicated().sum()  # number of duplicates
nutrients = nutrients.drop_duplicates()
```

由于两个DataFrame对象中都有"group"和"description"，所以为了明确到底谁是谁，我们需要对它们进行重命名。做完这些，就可以将info跟nutrients合并起来。

```python
col_mapping = {'description' : 'food',
               'group'       : 'fgroup'}
info = info.rename(columns=col_mapping, copy=False)
info.info()
col_mapping = {'description' : 'nutrient',
               'group' : 'nutgroup'}
nutrients = nutrients.rename(columns=col_mapping, copy=False)

ndata = pd.merge(nutrients, info, on='id', how='outer')
ndata.info()
ndata.iloc[30000]
```

我们现在可以根据食物分类和营养类型画出一张中位值图。只要稍微动一动脑子，就可以发现各营养成分最为丰富的食物是什么了。

```python
result = ndata.groupby(['nutrient', 'fgroup'])['value'].quantile(0.5)
result['Zinc, Zn'].sort_values().plot(kind='barh')

by_nutrient = ndata.groupby(['nutgroup', 'nutrient'])
get_maximum = lambda x: x.loc[x.value.idxmax()]
get_minimum = lambda x: x.loc[x.value.idxmin()]
max_foods = by_nutrient.apply(get_maximum)[['value', 'food']]

# make the food a little smaller
max_foods.food = max_foods.food.str[:50]
max_foods.loc['Amino Acids']['food']
```

### 14.5 2012联邦选举委员会数据库
美国联邦选举委员会发布了有关政治竞选赞助方面的数据。其中包括赞助者的姓名、职业、雇主、地址以及出资额等信息。我们对2012年美国总统大选的数据集比较感兴趣，数据集是一个150MB的CSV文件（P00000001-ALL.csv），我们先用pandas.read_csv将其加载进来。

```python
fec = pd.read_csv('datasets/P00000001-ALL.csv')
fec.info()
fec.iloc[123456]
```

你可能已经想出了许多办法从这些竞选赞助数据中抽取有关赞助人和赞助模式的统计信息。我将在接下来的内容中介绍几种不同的分析工作（运用到目前为止已经学到的方法）。

不难看出，该数据中没有党派信息，因此最好把它加进去。通过unique，你可以获取全部的候选人名单。指明党派信息的方法之一是使用字典。现在，通过这个映射以及Series对象的map方法，你可以根据候选人姓名得到一组党派信息。

这里有两个需要注意的地方。该数据既包括赞助也包括退款（负的出资额）。为了简化分析过程，我限定该数据集只能有正的出资额。

由于Barack	Obama和Mitt	Romney是最主要的两名候选人，所以我还专门准备了一个子集，只包含针对他们两人的竞选活动的赞助信息。

```python
unique_cands = fec.cand_nm.unique()
unique_cands[2]
parties = {'Bachmann, Michelle': 'Republican',
           'Cain, Herman': 'Republican',
           'Gingrich, Newt': 'Republican',
           'Huntsman, Jon': 'Republican',
           'Johnson, Gary Earl': 'Republican',
           'McCotter, Thaddeus G': 'Republican',
           'Obama, Barack': 'Democrat',
           'Paul, Ron': 'Republican',
           'Pawlenty, Timothy': 'Republican',
           'Perry, Rick': 'Republican',
           "Roemer, Charles E. 'Buddy' III": 'Republican',
           'Romney, Mitt': 'Republican',
           'Santorum, Rick': 'Republican'}
fec.cand_nm[123456:123461]
fec.cand_nm[123456:123461].map(parties)
# Add it as a column
fec['party'] = fec.cand_nm.map(parties)
fec['party'].value_counts()

(fec.contb_receipt_amt > 0).value_counts()
fec = fec[fec.contb_receipt_amt > 0]

fec_mrbo = fec[fec.cand_nm.isin(['Obama, Barack', 'Romney, Mitt'])]
```

#### 根据职业和雇主统计赞助信息
基于职业的赞助信息统计是另一种经常被研究的统计任务。例如，律师们更倾向于资助民主党，而企业主则更倾向于资助共和党。你可以不相信我，自己看那些数据就知道了。

首先，根据职业计算出资总额。不难看出，许多职业都涉及相同的基本工作类型，或者同一样东西有多种变体。下面的代码片段可以清理一些这样的数据（将一个职业信息映射到另一个）。注意，这里巧妙地利用了dict.get，它允许没有映射关系的职业也能“通过”。对雇主信息也进行了同样的处理。

```python
fec.contbr_occupation.value_counts()[:10]
occ_mapping = {
   'INFORMATION REQUESTED PER BEST EFFORTS' : 'NOT PROVIDED',
   'INFORMATION REQUESTED' : 'NOT PROVIDED',
   'INFORMATION REQUESTED (BEST EFFORTS)' : 'NOT PROVIDED',
   'C.E.O.': 'CEO'
}
# If no mapping provided, return x
f = lambda x: occ_mapping.get(x, x)
fec.contbr_occupation = fec.contbr_occupation.map(f)

emp_mapping = {
   'INFORMATION REQUESTED PER BEST EFFORTS' : 'NOT PROVIDED',
   'INFORMATION REQUESTED' : 'NOT PROVIDED',
   'SELF' : 'SELF-EMPLOYED',
   'SELF EMPLOYED' : 'SELF-EMPLOYED',
}
# If no mapping provided, return x
f = lambda x: emp_mapping.get(x, x)
fec.contbr_employer = fec.contbr_employer.map(f)
```

可以通过pivot_table根据党派和职业对数据进行聚合，然后过滤掉总出资额不足200万美元的数据。把这些数据做成柱状图看起来会更加清楚（'barh'表示水平柱状图）。

你可能还想了解一下对Obama和Romney总出资额最高的职业和企业。为此，先对候选人进行分组，然后使用前面介绍的类似top的方法。然后根据职业和雇主进行聚合。

```python
by_occupation = fec.pivot_table('contb_receipt_amt',
                                index='contbr_occupation',
                                columns='party', aggfunc='sum')
over_2mm = by_occupation[by_occupation.sum(1) > 2000000]
over_2mm.plot(kind='barh')

def get_top_amounts(group, key, n=5):
    totals = group.groupby(key)['contb_receipt_amt'].sum()
    return totals.nlargest(n)
grouped = fec_mrbo.groupby('cand_nm')
grouped.apply(get_top_amounts, 'contbr_occupation', n=7)
grouped.apply(get_top_amounts, 'contbr_employer', n=10)
```

#### 对出资额分组
还可以对该数据做另一种非常实用的分析：利用cut函数根据出资额的大小将数据离散化到多个面元中。现在可以根据候选人姓名以及面元标签对奥巴马和罗姆尼数据进行分组，以得到一个柱状图。

从这个数据中可以看出，在小额赞助方面，Obama获得的数量比Romney多得多。你还可以对出资额求和并在面元内规格化，以便图形化显示两位候选人各种赞助额度的比例。

```python
bins = np.array([0, 1, 10, 100, 1000, 10000,
                 100000, 1000000, 10000000])
labels = pd.cut(fec_mrbo.contb_receipt_amt, bins)
grouped = fec_mrbo.groupby(['cand_nm', labels])
grouped.size().unstack(0)
bucket_sums = grouped.contb_receipt_amt.sum().unstack(0)
normed_sums = bucket_sums.div(bucket_sums.sum(axis=1), axis=0)
normed_sums[:-2].plot(kind='barh')
```

其中排除了两个最大的面元，因为这些不是由个人捐赠的。

还可以对该分析过程做许多的提炼和改进。比如说，可以根据赞助人的姓名和邮编对数据进行聚合，以便找出哪些人进行了多次小额捐款，哪些人又进行了一次或多次大额捐款。我强烈建议你下载这些数据并自己摸索一下。

#### 根据州统计赞助信息
根据候选人和州对数据进行聚合是常规操作。如果对各行除以总赞助额，就会得到各候选人在各州的总赞助额比例。

```python
grouped = fec_mrbo.groupby(['cand_nm', 'contbr_st'])
totals = grouped.contb_receipt_amt.sum().unstack(0).fillna(0)
totals = totals[totals.sum(1) > 100000]
totals[:10]
percent = totals.div(totals.sum(1), axis=0)
percent[:10]
```

## 附录A NumPy高级应用

### A.1 ndarray对象的内部机理
NumPy的ndarray提供了一种将同质数据块（可以是连续或跨越）解释为多维数组对象的方式。正如你之前所看到的那样，数据类型（dtype）决定了数据的解释方式，比如浮点数、整数、布尔值等。

ndarray如此强大的部分原因是所有数组对象都是数据块的一个跨度视图（strided view）。你可能想知道数组视图`arr[::2,::-1]`不复制任何数据的原因是什么。简单地说，ndarray不只是一块内存和一个dtype，它还有跨度信息，这使得数组能以各种步幅（step size）在内存中移动。更准确地讲，ndarray内部由以下内容组成：

- 一个指向数据（内存或内存映射文件中的一块数据）的指针。
- 数据类型或dtype，描述在数组中的固定大小值的格子。
- 一个表示数组形状（shape）的元组。
- 一个跨度元组（stride），其中的整数指的是为了前进到当前维度下一个元素需要“跨过”的字节数。

一个典型的（C顺序，稍后将详细讲解）3×4×5的float64（8个字节）数组，其跨度为(160,40,8)	——	知道跨度是非常有用的，通常，跨度在一个轴上越大，沿这个轴进行计算的开销就越大。

```python
np.ones((3, 4, 5), dtype=np.float64).strides
```

虽然NumPy用户很少会对数组的跨度信息感兴趣，但它们却是构建非复制式数组视图的重要因素。跨度甚至可以是负数，这样会使数组在内存中后向移动，比如在切片obj[::-1]或obj[:,::-1]中就是这样的。

#### NumPy数据类型体系
你可能偶尔需要检查数组中所包含的是否是整数、浮点数、字符串或Python对象。因为浮点数的种类很多（从float16到float128），判断dtype是否属于某个大类的工作非常繁琐。幸运的是，dtype都有一个超类（比如np.integer和np.floating），它们可以跟np.issubdtype函数结合使用。调用dtype的mro方法即可查看其所有的父类。

```python
ints = np.ones(10, dtype=np.uint16)
floats = np.ones(10, dtype=np.float32)
np.issubdtype(ints.dtype, np.integer)
np.issubdtype(floats.dtype, np.floating)
np.float64.mro()
np.issubdtype(ints.dtype, np.number)
```

大部分NumPy用户完全不需要了解这些知识，但是这些知识偶尔还是能派上用场的。下图说明了dtype体系以及父子类关系。

![dtype体系](/assets/images/np_dtype_hierachy.jpg)

### A.2	高级数组操作
除花式索引、切片、布尔条件取子集等操作之外，数组的操作方式还有很多。虽然pandas中的高级函数可以处理数据分析工作中的许多重型任务，但有时你还是需要编写一些在现有库中找不到的数据算法。

#### 数组重塑

多数情况下，你可以无需复制任何数据，就将数组从一个形状转换为另一个形状。只需向数组的实例方法reshape传入一个表示新形状的元组即可实现该目的。例如，假设有一个一维数组，我们希望将其重新排列为一个矩阵。多维数组也能被重塑。作为参数的形状的其中一维可以是－1，它表示该维度的大小由数据本身推断而来。

与reshape将一维数组转换为多维数组的运算过程相反的运算通常称为扁平化（flattening）或散开（raveling）。如果结果中的值与原始数组相同，ravel不会产生源数据的副本。flatten方法的行为类似于ravel，只不过它总是返回数据的副本。

```python
arr = np.arange(8)
arr.reshape((4, 2))
arr.reshape((4, 2)).reshape((2, 4))
arr = np.arange(15)
arr.reshape((5, -1))

other_arr = np.ones((3, 5))
arr.reshape(other_arr.shape)

arr = np.arange(15).reshape((5, 3))
arr.ravel()
arr.flatten()
```

#### C和Fortran顺序
NumPy允许你更为灵活地控制数据在内存中的布局。默认情况下，NumPy数组是按行优先顺序创建的。在空间方面，这就意味着，对于一个二维数组，每行中的数据项是被存放在相邻内存位置上的。另一种顺序是列优先顺序，它意味着每列中的数据项是被存放在相邻内存位置上的。

由于一些历史原因，行和列优先顺序又分别称为C和Fortran顺序。在FORTRAN 77中，矩阵全都是列优先的。

像reshape和reval这样的函数，都可以接受一个表示数组数据存放顺序的order参数。一般可以是'C'或'F'（还有'A'和'K'等不常用的选项，具体请参考NumPy的文档）。

```
arr = np.arange(12).reshape((3, 4))
arr.ravel()
arr.ravel('F')
```

二维或更高维数组的重塑过程比较令人费解。C和Fortran顺序的关键区别就是维度的行进顺序：

- C/行优先顺序：先经过更高的维度（例如，轴1会先于轴0被处理）。
- Fortran/列优先顺序：后经过更高的维度（例如，轴0会先于轴1被处理）。

#### 数组的合并和拆分
numpy .concatenate可以按指定轴将一个由数组组成的序列（如元组、列表等）连接到一起。对于常见的连接操作，NumPy提供了一些比较方便的方法（如vstack和hstack）。

与此相反，split用于将一个数组沿指定轴拆分为多个数组。传入到np.split的值[1,3]指示在哪个索引处分割数组。

```
arr1 = np.array([[1, 2, 3], [4, 5, 6]])
arr2 = np.array([[7, 8, 9], [10, 11, 12]])
np.concatenate([arr1, arr2], axis=0)
np.concatenate([arr1, arr2], axis=1)
np.vstack((arr1, arr2))
np.hstack((arr1, arr2))
arr = np.random.randn(5, 2)
first, second, third = np.split(arr, [1, 3])
```

下表中列出了所有关于数组连接和拆分的函数，其中有些是专门为了方便常见的连接运算而提供的。

- concatenate  最一般化的连接，沿一条轴连接一组数组
- vstack, row_stack  以面向行的方式对数组进行堆叠（沿轴0）
- hstack  以面向列的方式对数组进行堆叠（沿轴1）
- column_stack  类似于hstack，但是会先将一维数组转换为二维列向量
- dstack  以面向“深度”的方式对数组进行堆叠（沿轴2）
- split  沿指定的轴在指定的位置拆分数组
- hsplit, vsplit, dsplit  split的便捷化函数，分别沿轴0、轴1、轴2进行拆分

#### 堆叠辅助类：r和c
NumPy命名空间中有两个特殊的对象——r和c，它们可以使数组的堆叠操作更为简洁。它还可以将切片转换成数组。

```
arr = np.arange(6)
arr1 = arr.reshape((3, 2))
arr2 = np.random.randn(3, 2)
np.r_[arr1, arr2]
np.c_[np.r_[arr1, arr2], arr]
np.c_[1:6, -10:-5]
```

#### 元素的重复操作：tile和repeat
对数组进行重复以产生更大数组的工具主要是repeat和tile这两个函数。

repeat会将数组中的各个元素重复一定次数，从而产生一个更大的数组。默认情况下，如果传入的是一个整数，则各元素就都会重复那么多次。如果传入的是一组整数，则各元素就可以重复不同的次数。对于多维数组，还可以让它们的元素沿指定轴重复。

注意，如果没有设置轴向，则数组会被扁平化，这可能不会是你想要的结果。同样，在对多维进行重复时，也可以传入一组整数，这样就会使各切片重复不同的次数。

```
arr = np.arange(3)
arr.repeat(3)
arr.repeat([2, 3, 4])
arr = np.random.randn(2, 2)
arr.repeat(2, axis=0)
arr.repeat([2, 3], axis=0)
arr.repeat([2, 3], axis=1)
```

tile的功能是沿指定轴向堆叠数组的副本。你可以形象地将其想象成“铺瓷砖”。第二个参数是瓷砖的数量。对于标量，瓷砖是水平铺设的，而不是垂直铺设。它可以是一个表示“铺设”布局的元组。

```
arr = np.random.randn(2, 2)
np.tile(arr, 2)
np.tile(arr, (2, 1))
np.tile(arr, (3, 2))
```

注意：跟其他流行的数组编程语言（如MA TLAB）不同，NumPy中很少需要对数组进行重复（replicate）。这主要是因为广播（broadcasting）能更好地满足该需求。

#### 花式索引的等价函数：take和put
在第4章中讲过，获取和设置数组子集的一个办法是通过整数数组使用花式索引。ndarray还有其它方法用于获取单个轴向上的选区。要在其它轴上使用take，只需传入axis关键字即可。

```
arr = np.arange(10) * 100
inds = [7, 1, 2, 6]
arr[inds]
arr.take(inds)
arr.put(inds, 42)
arr.put(inds, [40, 41, 42, 43])

inds = [2, 0, 2, 1]
arr = np.random.randn(2, 4)
arr.take(inds, axis=1)
```

put不接受axis参数，它只会在数组的扁平化版本（一维，C顺序）上进行索引。因此，在需要用其他轴向的索引设置元素时，最好还是使用花式索引。

### A.3 广播
广播（broadcasting）指的是不同形状的数组之间的算术运算的执行方式。它是一种非常强大的功能，但也容易令人误解，即使是经验丰富的老手也是如此。将标量值跟数组合并时就会发生最简单的广播。看一个例子，我们可以通过减去列平均值的方式对数组的每一列进行距平化处理。

```
arr = np.arange(5)
arr * 4
arr = np.random.randn(4, 3)
arr.mean(0)
demeaned = arr - arr.mean(0)
demeaned.mean(0)
```

在这个乘法运算中，标量值4被广播到了其他所有的元素上。

用广播的方式对行进行距平化处理会稍微麻烦一些。幸运的是，只要遵循一定的规则，低维度的值是可以被广播到数组的任意维度的（比如对二维数组各列减去行平均值）。

**广播的原则**：如果两个数组的后缘维度（trailing dimension，即从末尾开始算起的维度）的轴长度相符或其中一方的长度为1，则认为它们是广播兼容的。广播会在缺失和（或）长度为1的维度上进行。

一名经验丰富的NumPy老手，经常还是得停下来画张图并想想广播的原则。

再来看一下最后那个例子，假设你希望对各行减去那个平均值。由于arr .mean(0)的长度为3，所以它可以在0轴向上进行广播：因为arr的后缘维度是3，所以它们是兼容的。根据该原则，要在1轴向上做减法（即各行减去行平均值），较小的那个数组的形状必须是(4,1)。

```
row_means = arr.mean(1)
row_means.shape
row_means.reshape((4, 1))
demeaned = arr - row_means.reshape((4, 1))
demeaned.mean(1)
```

#### 沿其它轴向广播
高维度数组的广播似乎更难以理解，而实际上它也是遵循广播原则的。

人们经常需要通过算术运算过程将较低维度的数组在除0轴以外的其他轴向上广播。根据广播的原则，较小数组的“广播维”必须为1。在上面那个行距平化的例子中，这就意味着要将行平均值的形状变成(4,1)而不是(4,)。

对于三维的情况，在三维中的任何一维上广播其实也就是将数据重塑为兼容的形状而已。于是就有了一个非常普遍的问题（尤其是在通用算法中），即专门为了广播而添加一个长度为1的新轴。虽然reshape是一个办法，但插入轴需要构造一个表示新形状的元组。这是一个很郁闷的过程。因此，NumPy数组提供了一种通过索引机制插入
轴的特殊语法。

下面这段代码通过特殊的np.newaxis属性以及“全”切片来插入新轴。因此，如果我们有一个三维数组，并希望对轴2进行距平化，那么只需要编写下面这样的代码就可以了。

```python
arr = np.zeros((4, 4))
arr_3d = arr[:, np.newaxis, :]
arr_3d.shape
arr_1d = np.random.normal(size=3)
arr_1d[:, np.newaxis]
arr_1d[np.newaxis, :]

arr = np.random.randn(3, 4, 5)
depth_means = arr.mean(2)
depth_means.shape
demeaned = arr - depth_means[:, :, np.newaxis]
demeaned.mean(2)
```

在对指定轴进行距平化时，一种既通用又不牺牲性能的方法呢，需要一些索引方面的技巧：

```python
def demean_axis(arr, axis=0):
    means = arr.mean(axis)

    # This generalizes things like [:, :, np.newaxis] to N dimensions
    indexer = [slice(None)] * arr.ndim
    indexer[axis] = np.newaxis
    return arr - means[indexer]
```

#### 通过广播设置数组的值
算术运算所遵循的广播原则同样也适用于通过索引机制设置数组值的操作。

对于最简单的情况，我们可以这样做。但是，假设我们想要用一个一维数组来设置目标数组的各列，只要保证形状兼容就可以了。

```python
arr = np.zeros((4, 3))
arr[:] = 5
col = np.array([1.28, -0.42, 0.44, 1.6])
arr[:] = col[:, np.newaxis]
arr[:2] = [[-1.37], [0.509]]
```

### A.4 ufunc高级应用
虽然许多NumPy用户只会用到通用函数所提供的快速的元素级运算，但通用函数实际上还有一些高级用法能使我们丢开循环而编写出更为简洁的代码。

#### ufunc实例方法

NumPy的各个二元ufunc都有一些用于执行特定矢量化运算的特殊方法。

reduce接受一个数组参数，并通过一系列的二元运算对其值进行聚合（可指明轴向）。例如，我们可以用np.add.reduce对数组中各个元素进行求和。

```python
arr = np.arange(10)
np.add.reduce(arr)
arr.sum()
```

起始值取决于ufunc（对于add的情况，就是0）。如果设置了轴号，约简运算就会沿该轴向执行。这就使你能用一种比较简洁的方式得到某些问题的答案。在下面这个例子中，我们用np.logical_and检查数组各行中的值是否是有序的。

```python
np.random.seed(12346)  # for reproducibility
arr = np.random.randn(5, 5)
arr[::2].sort(1) # sort a few rows
arr[:, :-1] < arr[:, 1:]
np.logical_and.reduce(arr[:, :-1] < arr[:, 1:], axis=1)
```

注意，logical_and.reduce跟all方法是等价的。

accumulate跟reduce的关系就像cumsum跟sum的关系那样。它产生一个跟原数组大小相同的中间“累计”值数组。

```python
arr = np.arange(15).reshape((3, 5))
np.add.accumulate(arr, axis=1)
```

outer用于计算两个数组的叉积。outer输出结果的维度是两个输入数据的维度之和。

```python
arr = np.arange(3).repeat([1, 2, 2])
np.multiply.outer(arr, np.arange(5))

x, y = np.random.randn(3, 4), np.random.randn(5)
result = np.subtract.outer(x, y)
result.shape
```

最后一个方法reduceat用于计算“局部约简”，其实就是一个对数据各切片进行聚合的groupby运算。它接受一组用于指示如何对值进行拆分和聚合的“面元边界”。最终结果是在arr[0:5]、arr[5:8]以及arr[8:]上执行的约简。跟其他方法一样，这里也可以传入一个axis参数。

```python
arr = np.arange(10)
np.add.reduceat(arr, [0, 5, 8])
arr = np.multiply.outer(np.arange(4), np.arange(5))
np.add.reduceat(arr, [0, 2, 4], axis=1)
```

部分的ufunc方法：

- reduce(x)  通过连续执行原始运算的方式对值进行聚合
- accumulate(x)  聚合值，保留所有局部聚合结果
- reduceat(x, bins)  局部约简（也就是groupby）。约简数据的各个切片以产生聚合型数组
- outer(x, y)  对x和y中的每对元素应用原始运算。结果数组的形状为x.shape+y.shape

#### 编写新的ufunc
有多种方法可以让你编写自己的NumPy	ufuncs。最常见的是使用NumPy C API，在本节讲纯粹的Python	ufunc。

numpy .frompyfunc接受一个Python函数以及两个分别表示输入输出参数数量的参数。例如，下面是一个能够实现元素级加法的简单函数。

```python
def add_elements(x, y):
    return x + y
add_them = np.frompyfunc(add_elements, 2, 1)
add_them(np.arange(8), np.arange(8))
```

用frompyfunc创建的函数总是返回Python对象数组，这一点很不方便。幸运的是，还有另一个办法，即numpy.vectorize。虽然没有frompyfunc那么强大，但可以让你指定输出类型。

```python
add_them = np.vectorize(add_elements, otypes=[np.float64])
add_them(np.arange(8), np.arange(8))
```

虽然这两个函数提供了一种创建ufunc型函数的手段，但它们非常慢，因为它们在计算每个元素时都要执行一次Python函数调用，这就会比NumPy自带的基于C的ufunc慢很多。

```python
arr = np.random.randn(10000)
%timeit add_them(arr, arr)
%timeit np.add(arr, arr)
```

### A.5 结构化和记录式数组
你可能已经注意到了，到目前为止我们所讨论的ndarray都是一种同质数据容器，也就是说，在它所表示的内存块中，各元素占用的字节数相同（具体根据dtype而定）。

结构化数组是一种特殊的ndarray，其中的各个元素可以被看做C语言中的结构体或SQL表中带有多个命名字段的行。定义结构化dtype（请参考NumPy的在线文档）的方式有很多。最典型的办法是元组列表，各元组的格式为(field_name,field_data_type)。这样，数组的元素就成了元组式的对象，该对象中各个元素可以像字典那样进行访问。字段名保存在dtype.names属性中。在访问结构化数组的某个字段时，返回的是该数据的视图，所以不会发生数据复制。

```python
dtype = [('x', np.float64), ('y', np.int32)]
sarr = np.array([(1.5, 6), (np.pi, -2)], dtype=dtype)
sarr[0]
sarr[0]['y']
sarr['x']
```

#### 嵌套dtype和多维字段

在定义结构化dtype时，你可以再设置一个形状（可以是一个整数，也可以是一个元组）。在这种情况下，各个记录的x字段所表示的是一个长度为3的数组。这样，访问arr['x']即可得到一个二维数组，而不是前面那个例子中的一维数组。

这就使你能用单个数组的内存块存放复杂的嵌套结构。你还可以嵌套dtype，作出更复杂的结构。

```python
dtype = [('x', np.int64, 3), ('y', np.int32)]
arr = np.zeros(4, dtype=dtype)
arr[0]['x']
arr['x']

dtype = [('x', [('a', 'f8'), ('b', 'f4')]), ('y', np.int32)]
data = np.array([((1, 2), 5), ((3, 4), 6)], dtype=dtype)
data['x']
data['y']
data['x']['a']

```

pandas的DataFrame并不直接支持该功能，但它的分层索引机制跟这个差不多。

#### 为什么要用结构化数组
跟pandas的DataFrame相比，NumPy的结构化数组是一种相对较低级的工具。它可以将单个内存块解释为带有任意复杂嵌套列的表格型结构。由于数组中的每个元素在内存中都被表示为固定的字节数，所以结构化数组能够提供非常快速高效的磁盘数据读写（包括内存映像）、网络传输等功能。

结构化数组的另一个常见用法是，将数据文件写成定长记录字节流，这是C和C++代码中常见的数据序列化手段（业界许多历史系统中都能找得到）。只要知道文件的格式（记录的大小、元素的顺序、字节数以及数据类型等），就可以用np.fromfile将数据读入内存。

### A.6 更多有关排序的话题
跟Python内置的列表一样，ndarray的sort实例方法也是就地排序。也就是说，数组内容的重新排列是不会产生新数组的。

在对数组进行就地排序时要注意一点，如果目标数组只是一个视图，则原始数组将会被修改。

相反，numpy.sort会为原数组创建一个已排序副本。另外，它所接受的参数（比如kind）跟ndarray.sort一样。

这两个排序方法都可以接受一个axis参数，以便沿指定轴向对各块数据进行单独排序。

```python
arr = np.random.randn(6)
arr.sort()

arr = np.random.randn(3, 5)
arr[:, 0].sort()  # Sort first column values in-place

arr = np.random.randn(5)
np.sort(arr)

arr = np.random.randn(3, 5)
arr.sort(axis=1)

arr[:, ::-1]
```

你可能注意到了，这两个排序方法都不可以被设置为降序。其实这也无所谓，因为数组切片会产生视图（也就是说，不会产生副本，也不需要任何其他的计算工作）。许多Python用户都很熟悉一个有关列表的小技巧：values[::-1]可以返回一个反序的列表。对ndarray也是如此。

#### 间接排序：argsort和lexsort
在数据分析工作中，常常需要根据一个或多个键对数据集进行排序。例如，一个有关学生信息的数据表可能需要以姓和名进行排序（先姓后名），这就是间接排序的一个例子。

给定一个或多个键，你就可以得到一个由整数组成的索引数组（我称之为索引器），其中的索引值说明了数据在新顺序下的位置。argsort和numpy .lexsort就是实现该功能的两个主要方法。下面是一个简单的例子；一个更复杂的例子，下面这段代码根据数组的第一行对其进行排序。

```python
values = np.array([5, 0, 1, 3, 2])
indexer = values.argsort()
values[indexer]

arr = np.random.randn(3, 5)
arr[0] = values
arr[:, arr[0].argsort()]
```

lexsort跟argsort差不多，只不过它可以一次性对多个键数组执行间接排序（字典序）。假设我们想对一些以姓和名标识的数据进行排序。

```python
first_name = np.array(['Bob', 'Jane', 'Steve', 'Bill', 'Barbara'])
last_name = np.array(['Jones', 'Arnold', 'Arnold', 'Jones', 'Walters'])
sorter = np.lexsort((first_name, last_name))
zip(last_name[sorter], first_name[sorter])
```

刚开始使用lexsort的时候可能会比较容易头晕，这是因为键的应用顺序是从最后一个传入的算起的。不难看出，last_name是先于first_name被应用的。

实际上，Series和DataFrame的sort_index以及Series的order方法就是通过这些函数的变体（它们还必须考虑缺失值）实现的。

#### 其他排序算法
稳定的（stable）排序算法会保持等价元素的相对位置。对于相对位置具有实际意义的那些间接排序而言，这一点非常重要。

```python
values = np.array(['2:first', '2:second', '1:first', '1:second',
                   '1:third'])
key = np.array([2, 2, 1, 1, 1])
indexer = key.argsort(kind='mergesort')
values.take(indexer)
```

mergesort（合并排序）是唯一的稳定排序，它保证有O(n log n)的性能（空间复杂度），但是其平均性能比默认的quicksort（快速排序）要差。下表列出了可用的排序算法及其相关的性能指标。

| 排序算法-kind | 速度 | 稳定性 | 工作空间 | 最坏的情况 |
| ------------- | ---- | ------ | -------- | ---------- |
| quicksort     | 1    | 否     | 0        | O(n^2)     |
| mergesort     | 2    | 是     | n/2      | O(n log n) |
| heapsort      | 3    | 否     | 0        | O(n log n) |

#### 部分排序数组
排序的目的之一可能是确定数组中最大或最小的元素。NumPy有两个优化方法，np.partition和np.argpartition，可以在第k个最小元素划分数组。

当你调用partition(arr ,	3)，结果中的头三个元素是最小的三个，没有特定的顺序。numpy .argpartition与numpy .argsort相似，会返回索引，重排数据为等价的顺序。

```python
np.random.seed(12345)
arr = np.random.randn(20)
np.partition(arr, 3)
indices = np.argpartition(arr, 3)
arr.take(indices)
```

#### numpy .searchsorted：在有序数组中查找元素
searchsorted是一个在有序数组上执行二分查找的数组方法，只要将值插入到它返回的那个位置就能维持数组的有序性。你可以传入一组值就能得到一组索引。

从结果中可以看出，对于元素0，searchsorted会返回0。这是因为其默认行为是返回相等值组的左侧索引。

```python
arr = np.array([0, 1, 7, 12, 15])
arr.searchsorted(9)
arr.searchsorted([0, 8, 11, 16])

arr = np.array([0, 0, 0, 1, 1, 1, 1])
arr.searchsorted([0, 1])
arr.searchsorted([0, 1], side='right')
```

再来看searchsorted的另一个用法，假设我们有一个数据数组（其中的值在0到10000之间），还有一个表示“面元边界”的数组，我们希望用它将数据数组拆分开。然后，为了得到各数据点所属区间的编号（其中1表示面元[0,100)），我们可以直接使用searchsorted。通过pandas的groupby使用该结果即可非常轻松地对原数据集进行拆分。

```python
data = np.floor(np.random.uniform(0, 10000, size=50))
bins = np.array([0, 100, 1000, 5000, 10000])
labels = bins.searchsorted(data)
pd.Series(data).groupby(labels).mean()
```

### A.7 用Numba编写快速NumPy函数
Numba是一个开源项目，它可以利用CPUs、GPUs或其它硬件为类似NumPy的数据创建快速函数。它使用了[LL VM项目](http://llvm.org/)，将Python代码转换为机器代码。

为了介绍Numba，来考虑一个纯粹的Python函数，它使用for循环计算表达式(x	-y).mean()，这个函数很慢。

```python
def mean_distance(x, y):
    nx = len(x)
    result = 0.0
    count = 0
    for i in range(nx):
        result += x[i] - y[i]
        count += 1
    return result / count

x = np.random.randn(10000000)
y = np.random.randn(10000000)
%timeit mean_distance(x, y)
%timeit (x - y).mean()
```

NumPy的版本要比它快过100倍。我们可以转换这个函数为编译的Numba函数，使用numba.jit函数。也可以写成装饰器。它要比矢量化的NumPy快。

```python
import numba as nb
numba_mean_distance = nb.jit(mean_distance)
@nb.jit
def mean_distance(x, y):
    nx = len(x)
    result = 0.0
    count = 0
    for i in range(nx):
        result += x[i] - y[i]
        count += 1
    return result / count
%timeit numba_mean_distance(x, y)
```

Numba不能编译Python代码，但它支持纯Python写的一个部分，可以编写数值算法。

Numba是一个深厚的库，支持多种硬件、编译模式和用户插件。它还可以编译NumPy Python API的一部分，而不用for循环。Numba也可以识别可以编译为机器编码的结构体，但是若调用CPython API，它就不知道如何编译。

Numba的jit函数有一个选项，nopython=True，它限制了可以被转换为Python代码的代码，这些代码可以编译为LL VM，但没有任何Python C API调用。jit(nopython=True)有一个简短的别名numba.njit。前面的例子，我们还可以这样写。

```python
from numba import float64, njit

@njit(float64(float64[:], float64[:]))
def mean_distance(x, y):
    return (x - y).mean()
```

#### 用Numba创建自定义numpy.ufunc对象
numba.vectorize创建了一个编译的NumPy	ufunc，它与内置的ufunc很像。考虑一个numpy .add的Python例子。

```python
from numba import vectorize

@vectorize
def nb_add(x, y):
    return x + y

x = np.arange(10)
nb_add(x, x)
nb_add.accumulate(x, 0)
```

### A.8 高级数组输入输出
我在第4章中讲过，np.save和np.load可用于读写磁盘上以二进制格式存储的数组。其实还有一些工具可用于更为复杂的场景。尤其是内存映像（memory	map），它使你能处理在内存中放不下的数据集。

#### 内存映像文件

内存映像文件是一种将磁盘上的非常大的二进制数据文件当做内存中的数组进行处理的方式。NumPy实现了一个类似于ndarray的memmap对象，它允许将大文件分成小段进行读写，而不是一次性将整个数组读入内存。另外，memmap也拥有跟普通数组一样的方法，因此，基本上只要是能用于ndarray的算法就也能用于memmap。

要创建一个内存映像，可以使用函数np.memmap并传入一个文件路径、数据类型、形状以及文件模式。对memmap切片将会返回磁盘上的数据的视图。如果将数据赋值给这些视图：数据会先被缓存在内存中（就像是Python的文件对象），调用flush即可将其写入磁盘。

```python
mmap = np.memmap('mymmap', dtype='float64', mode='w+',
                 shape=(10000, 10000))
section = mmap[:5]
section[:] = np.random.randn(5, 10000)
mmap.flush()
del mmap
```

只要某个内存映像超出了作用域，它就会被垃圾回收器回收，之前对其所做的任何修改都会被写入磁盘。当打开一个已经存在的内存映像时，仍然需要指明数据类型和形状，因为磁盘上的那个文件只是一块二进制数据而已，没有任何元数据。

内存映像可以使用前面介绍的结构化或嵌套dtype。

### HDF5及其他数组存储方式
PyT ables和h5py这两个Python项目可以将NumPy的数组数据存储为高效且可压缩的HDF5格式（HDF意思是“层次化数据格式”）。你可以安全地将好几百GB甚至TB的数据存储为HDF5格式。

### A.9 性能建议
使用NumPy的代码的性能一般都很不错，因为数组运算一般都比纯Python循环快得多。下面大致列出了一些需要注意的事项：

- 将Python循环和条件逻辑转换为数组运算和布尔数组运算。
- 尽量使用广播。
- 避免复制数据，尽量使用数组视图（即切片）。
- 利用ufunc及其各种方法。



如果单用NumPy无论如何都达不到所需的性能指标，就可以考虑一下用C、Fortran或Cython来编写代码。[Cython](http://cython.org)，因为它不用花费我太多精力就能得到C语言那样的性能。

#### 连续内存的重要性

在某些应用场景中，数组的内存布局可以对计算速度造成极大的影响。这是因为性能差别在一定程度上跟CPU的高速缓存（cache）体系有关。运算过程中访问连续内存块（例如，对以C顺序存储的数组的行求和）一般是最快的，因为内存子系统会将适当的内存块缓存到超高速的L1或L2 CPU Cache中。此外，NumPy的C语言基础代码（某些）对连续存储的情况进行了优化处理，这样就能避免一些跨越式的内存访问。

一个数组的内存布局是连续的，就是说元素是以它们在数组中出现的顺序（即Fortran型（列优先）或C型（行优先））存储在内存中的。默认情况下，NumPy数组是以C型连续的方式创建的。列优先的数组（比如C型连续数组的转置）也被称为Fortran型连续。通过ndarray的flags属性即可查看这些信息。

```python
arr_c = np.ones((1000, 1000), order='C')
arr_f = np.ones((1000, 1000), order='F')
arr_c.flags
arr_f.flags
arr_f.flags.f_contiguous
```

在这个例子中，对两个数组的行进行求和计算，理论上说，arr_c会比arr_f快，因为arr_c的行在内存中是连续的。我们可以在IPython中用%timeit来确认一下。

如果想从NumPy中提升性能，这里就应该是下手的地方。如果数组的内存顺序不符合你的要求，使用copy并传入'C'或'F'即可解决该问题。

```python
%timeit arr_c.sum(1)
%timeit arr_f.sum(1)
arr_f.copy('C').flags
arr_c[:50].flags.contiguous
arr_c[:, :50].flags
```

注意，在构造数组的视图时，其结果不一定是连续的。

## 附录B 更多关于IPython的内容

### B.1 使用命令历史
Ipython维护了一个位于磁盘的小型数据库，用于保存执行的每条指令。它的用途有：

- 只用最少的输入，就能搜索、补全和执行先前运行过的指令；
- 在不同session间保存命令历史；
- 将日志输入/输出历史到一个文件。

这些功能在shell中，要比notebook更为有用，因为notebook从设计上是将输入和输出的代码放到每个代码格子中。

#### 搜索和重复使用命令历史

Ipython可以让你搜索和执行之前的代码或其他命令。这个功能非常有用，因为你可能需要重复执行同样的命令，例如%run命令，或其它代码。假设你必须要执行：

```
%run first/second/third/data_script.py
```

运行成功，然后检查结果，发现计算有错。解决完问题，然后修改了data_script.py，你就可以输入一些%run命令，然后按Ctrl+P或上箭头。这样就可以搜索历史命令，匹配输入字符的命令。多次按Ctrl+P或上箭头，会继续搜索命令。如果错过了想要执行的命令，可以按下Ctrl-N或下箭头，向前移动历史命令。

Ctrl-R可以带来如同Unix风格shell（比如bash	shell）的readline的部分增量搜索功能。在Windows上，readline功能是被IPython模仿的。要使用这个功能，先按Ctrl-R，然后输入一些包含于输入行的想要搜索的字符。Ctrl-R会循环历史，找到匹配字符的每一行。

#### 输入输出变量

IPython的一个session会在一个特殊变量，存储输入和输出Python对象的引用。最近的两个输出结果分别保持在`_`（一个下划线）和`__`（两个下划线）两个变量中。

输入变量是存储在名字类似`_iX`的变量中，X是输入行的编号。对于每个输入变量，都有一个对应的输出变量`_X`。因此在输入第27行之后，会有两个新变量`_27`（输出）和`_i27`（输入）。因为输入变量是字符串，它们可以用Python的exec关键字再次执行。

```
exec(_i27)
```

有几个魔术函数可以让你利用输入和输出历史。%hist可以打印所有或部分的输入历史，加上或不加上编号。%reset可以清理交互命名空间，或输入和输出缓存。%xdel魔术函数可以去除IPython中对一个特别对象的所有引用。

注意：当处理非常大的数据集时，要记住IPython的输入和输出的历史会造成被引用的对象不被垃圾回收（释放内存），即使你使用del关键字从交互命名空间删除变量。在这种情况下，小心使用%xdel和%reset可以帮助你避免陷入内存问题。

### B.2 与操作系统交互
IPython的另一个功能是无缝连接文件系统和操作系统。这意味着，在同时做其它事时，无需退出IPython，就可以像Windows或Unix使用命令行操作，包括shell命令、更改目录、用Python对象（列表或字符串）存储结果。它还有简单的命令别名和目录书签功能。

下表总结了调用shell命令的魔术函数和语法。

- `!cmd`  在系统shell执行cmd
- `output=!cmd args`  运行cmd，在output存储stdout
- %alias alias_name cmd  为一个系统命令定义别名
- %bookmark  使用IPython的目录书签系统
- %cd directory  改变系统的工作目录
- %pwd  返回当前的系统工作目录
- %pushd directory  将当前目标放在堆栈上，并将其更改为目标目录
- %popd  切换到从堆栈顶部弹出的目录
- %dirs  返回包含当前目录堆栈的列表
- %dhist  打印访问过的目录
- %env  返回系统的环境变量的字典
- %matplotlib  配置matplotlib的选项

#### Shell命令和别名
用叹号开始一行，是告诉IPython执行叹号后面的所有内容。这意味着你可以删除文件（取决于操作系统，用rm或del）、改变目录或执行任何其他命令。

加上叹号的命令赋值给变量，你可以在一个变量中存储命令的控制台输出。例如，在联网的基于Linux的主机上，我可以获得IP地址为Python变量：

```
In [1]: ip_info	=	!ifconfig	wlan0	|	grep	"inet"
In [2]: ip_info[0].strip()
```

返回的Python对象ip_info实际上是一个自定义的列表类型，它包含着多种版本的控制台输出。

当使用！命令时，IPython还可以在其中使用定义在当前环境的Python值。要这么做，可以在变量名前面加上$符号。

```
In [3]: foo = 'test*'
In [4]: !ls $foo
```

%alias魔术函数可以自定义shell命令的快捷方式。可以执行多个命令，就像在命令行中一样，只需用分号隔开。

```
%alias ll ls -l
%alias test_alias (cd examples; ls; cd ..)
```

当session结束，你定义的别名就会失效。要创建恒久的别名，需要使用配置。

#### 目录书签系统
IPython有一个简单的目录书签系统，可以让你保存常用目录的别名，这样在跳来跳去的时候会非常方便。例如，假设你想创建一个书签，指向本书的补充内容。这么做之后，当使用%cd魔术命令，就可以使用定义的书签。

如果书签的名字，与当前工作目录的一个目录重名，你可以使用-b标志来覆写，使用书签的位置。使用%bookmark的-l选项，可以列出所有的书签。

```
In [6]: %bookmark py4da /home/wesm/code/pydata-book
In [7]: cd py4da
In [8]: %bookmark -l
```

书签，和别名不同，在session之间是保持的。

### B.3 软件开发工具

除了作为优秀的交互式计算和数据探索环境，IPython也是有效的Python软件开发工具。在数据分析中，最重要的是要有正确的代码。IPython紧密集成了和加强了Python内置的pdb调试器。其次，需要快速的代码。IPython有
易于使用的代码计时和性能分析工具。

#### 交互调试器

IPython的调试器用tab补全、语法增强、逐行异常追踪增强了pdb。调试代码的最佳时间就是刚刚发生错误。异常发生之后就输入%debug，就启动了调试器，进入抛出异常的堆栈框架。

一旦进入调试器，你就可以执行任意的Python代码，在每个堆栈框架中检查所有的对象和数据（解释器会保持它们活跃）。默认是从错误发生的最低级开始。通过u（up）和d（down），你可以在不同等级的堆栈踪迹切换。

执行%pdb命令，可以在发生任何异常时让IPython自动启动调试器，许多用户会发现这个功能非常好用。

用调试器帮助开发代码也很容易，特别是当你希望设置断点或在函数和脚本间移动，以检查每个阶段的状态。有多种方法可以实现。

第一种是使用%run和-d，它会在执行传入脚本的任何代码之前调用调试器。你必须马上按s（step）以进入脚本。然后，你就可以决定如何工作。例如，在前面的异常，我们可以设置一个断点（b 行号），就在调用works_fine之前，然后按c（continue）运行脚本，直到遇到断点。这时，你可以step进入works_fine()，或通过按n（next）执行works_fine()，进入下一行。然后，我们可以进入throws_an_exception，到达发生错误的一行，查看变量。注
意，调试器的命令是在变量名之前，在变量名前面加叹号！可以查看内容。

提高使用交互式调试器的熟练度需要练习和经验。下表列出了所有调试器命令。如果你习惯了IDE，你可能觉得终端的调试器在一开始会不顺手，但会觉得越来越好用。

- h(elp)  显示命令列表
- help command  显示命令的文档
- c(ontinue)  继续程序的执行
- q(uit)  退出调试器，不再执行任何代码
- b(reak) number  在当前文件的number行设置一个断点
- b path/to/file.py:number  在指定文件的number行设置一个断点
- s(tep)  单步进入函数调用
- n(ext)  执行当前行，并前进到当前级别的下一行
- u(p)/d(own)  在函数调用栈中向上或向下移动
- a(rgs)  显示当前函数的参数
- debug statement  在新的（递归）调试器中执行statement
- l(ist) statement  在当前栈级别，显示当前位置和上下文参考代码
- w(here)  打印当前位置的完整栈跟踪，包括上下文参考代码

#### 使用调试器的其它方式

还有一些其它工作可以用到调试器。第一个是使用特殊的set_trace函数（根据pdb.set_trace命名的），这是一个简装的断点。还有两种方法是你可能想用的（可以将其添加到IPython的配置）。

```python
from IPython.core.debugger import Pdb

def set_trace():
    Pdb(color_scheme='Linux').set_trace(sys._getframe().f_back)
    
def debug(f, *args, **kwargs):
    pdb = Pdb(color_scheme='Linux')
    return pdb.runcall(f, *args, **kwargs)
```

第一个函数set_trace非常简单。如果你想暂时停下来进行仔细检查（比如发生异常之前），可以在代码的任何位置使用set_trace。按c（continue）可以让代码继续正常行进。

我们刚看的debug函数，可以让你方便的在调用任何函数时使用调试器。假设我们写了一个下面的函数，想逐步分析它的逻辑。普通地使用f，就会像`f(1, 2, z=3)`。而要想进入f，将f作为第一个参数传递给debug，再将位置和关键词参数传递给f。

```python
def f(x, y, z=1):
    tmp = x + y
    return tmp / z

In [6]: debug(f, 1, 2, z=3)
```

最后，调试器可以和%run一起使用。脚本通过运行`%run -d`，就可以直接进入调试器，随意设置断点并启动脚本。加上-b和行号，可以预设一个断点。

```
In [1]: %run -d examples/ipython_bug.py
In [2]: %run -d -b2 examples/ipython_bug.py
```

#### 代码计时：`%time`和`%timeit`
对于大型和长时间运行的数据分析应用，你可能希望测量不同组件或单独函数调用语句的执行时间。你可能想知道哪个函数占用的时间最长。IPython可以让你开发和测试代码时，很容易地获得这些信息。

手动用time模块和它的函数time.clock和time.time给代码计时，必须要写一些相同的模板化代码：

```python
import time
start = time.time()
for i in range(iterations):
    # some code to run here
elapsed_per = (time.time() - start) / iterations
```

IPython提供两个魔术命令 `%time` 和 `%timeit` 自动完成该过程。

`%time`会运行一次语句，报告总共的执行时间。假设我们有一个大的字符串列表，我们想比较不同的可以挑选出特定开头字符串的方法。这里有一个含有600000字符串的列表，和两个方法，用以选出foo开头的字符串。看起来它们的性能应该是同级别的，用%time进行一下测量。

```python
# a very large list of strings
strings = ['foo', 'foobar', 'baz', 'qux',
           'python', 'Guido Van Rossum'] * 100000
method1 = [x for x in strings if x.startswith('foo')]
method2 = [x for x in strings if x[:3] == 'foo']

%time method1 = [x for x in strings if x.startswith('foo')]
%time method2 = [x for x in strings if x[:3] == 'foo']
```

Wall time（wall-clock time的简写）是主要关注的。第一个方法是第二个方法的两倍多，但是这种测量方法并不准确。如果用%time多次测量，你就会发现结果是变化的。要想更准确，可以使用%timeit魔术函数。给出任意一条语句，它能多次运行这条语句以得到一个更为准确的平均执行时间。

```python
%timeit [x for x in strings if x.startswith('foo')]
%timeit [x for x in strings if x[:3] == 'foo']
```

这个例子说明了解Python标准库、NumPy、pandas和其它库的性能是很有价值的。在大型数据分析中，这些毫秒的时间就会累积起来！

%timeit特别适合分析执行时间短的语句和函数，即使是微秒或纳秒。这些时间可能看起来毫不重要，但是一个20微秒的函数执行1百万次就比一个5微秒的函数长15秒。在上一个例子中，我们可以直接比较两个字符串操作，以了解它们的性能特点。

```python
x = 'foobar'
y = 'foo'
%timeit x.startswith(y)
%timeit x[:3] == y
```

#### 基础分析：%prun和%run -p
代码性能分析与代码计时关系很紧密，它关注的是“时间花在了哪里”。Python主要的分析工具是cProfile模块，它并不局限于IPython。cProfile会执行一个程序或任意的代码块，并会跟踪每个函数执行的时间。

使用cProfile的通常方式是在命令行中运行一整段程序，输出每个函数的累积时间。假设我们有一个简单的在循环中进行线型代数运算的脚本（计算一系列的100×100矩阵的最大绝对特征值）。你可以用cProfile运行这个脚本，使用下面的命令行。运行结果是按函数名排序的。这样要看出谁耗费的时间多有点困难，最好用-s指定排序。

```python
# cprof_example.py
import numpy as np
from numpy.linalg import eigvals
def run_experiment(niter=100):
    K = 100
    results = []
    for _ in range(niter):
        mat = np.random.randn(K, K)
        max_eigenvalue = np.abs(eigvals(mat)).max()
        results.append(max_eigenvalue)
    return results
some_results = run_experiment()
print('Largest one we saw: %s' % np.max(some_results)) 

!python -m cProfile cprof_example.py
!python -m cProfile -s cumulative cprof_example.py
```

扫描cumtime列，可以容易地看出每个函数用了多少时间。如果一个函数调用了其它函数，计时并不会停止。cProfile会记录每个函数的起始和结束时间，使用它们进行计时。

除了在命令行中使用，cProfile也可以在程序中使用，分析任意代码块，而不必运行新进程。Ipython提供了便捷的接口实现这个功能：%prun和%run -p。

%prun使用类似cProfile的命令行选项，但是可以分析任意Python语句，而不用整个py文件。相似的，调用 	 %run -p有和命令行相似的作用，只是你不用离开Ipython。

```python
%prun -l 7 -s cumulative run_experiment()
%run -p -s cumulative cprof_example.py
```

在Jupyter	notebook中，你可以使用%%prun魔术方法（两个%为单元魔术命令，对整个代码单元进行代码分析）来分析一整段代码。这会弹出一个带有分析输出的独立窗口。便于快速回答一些问题，比如“为什么这段代码用了这么长时间”？

使用IPython或Jupyter，还有一些其它工具可以让分析工作更便于理解。其中之一是[SnakeViz](https://github.com/jiffyclub/snakeviz/)，它会使用d3.js产生一个分析结果的交互可视化界面。

#### 逐行分析函数
有些情况下，用%prun（或其它基于cProfile的分析方法）得到的信息，不能获得函数执行时间的整个过程，或者结果过于复杂，加上函数名，很难进行解读。对于这种情况，有一个小库叫做line_profiler（可以通过PyPI或包管理工具获得）。它包含IPython插件，可以启用一个新的魔术函数%lprun，可以对一个函数或多个函数进
行逐行分析。你可以通过修改IPython配置加入下面这行，启用这个插件：

```
# A list of dotted module names of IPython extensions to load.
c.TerminalIPythonApp.extensions = ['line_profiler']

# 也可以运行命令
%load_ext line_profiler
```

激活了IPython插件line_profiler，新的命令%lprun就能用了。使用中的不同点是，我们必须告诉%lprun要分析的函数是哪个。语法是：

```
%lprun -f func1 -f func2 statement_to_profile
```

line_profiler也可以在程序中使用，但是在IPython中使用是最为强大的。假设你有一个带有下面代码的模块prof_mod，做一些NumPy数组操作。如果想了解add_and_sum函数的性能，%prun可以给出下面内容。我们想分析add_and_sum，运行lprun。我们分析了和代码语句中一样的函数。看之前的模块代码，我们可以调用call_function并对它和add_and_sum进行分析，得到一个完整的代码性能概括。

```python
# prof_mod.py
from numpy.random import randn
def add_and_sum(x, y):
    added = x + y
    summed = added.sum(axis=1)
    return summed
def call_function():
    x = randn(1000, 1000)
    y = randn(1000, 1000)
    return add_and_sum(x, y)

%run prof_mod
x = randn(3000, 3000)
y = randn(3000, 3000)
%prun add_and_sum(x, y)
%lprun -f add_and_sum add_and_sum(x, y)
%lprun -f add_and_sum -f call_function call_function()
```

总的原则是用%prun (cProfile)进行宏观分析，%lprun (line_profiler)做微观分析。最好对这两个工具都了解清楚。

使用%lprun必须要指明函数名的原因是追踪每行的执行时间的损耗过多。追踪无用的函数会显著地改变结果。

### B.4	使用IPython高效开发的技巧
方便快捷地写代码、调试和使用是每个人的目标。除了代码风格，流程细节（比如代码重载）也需要一些调整。

#### 重载模块依赖

在Python中，当你输入`import some_lib`，some_lib中的代码就会被执行，所有的变量、函数和定义的引入，就会被存入到新创建的some_lib模块命名空间。当下一次输入some_lib，就会得到一个已存在的模块命名空间的引用。潜在的问题是当你`%run`一个脚本，它依赖于另一个模块，而这个模块做过修改，就会产生问题。假设我在test_script.py中有如下代码：

```python
import some_lib
x = 5
y = [1, 2, 3, 4]
result = some_lib.get_answer(x, y)
```

如果你运行过了`%run test_script.py`，然后修改了some_lib.py，下一次再执行`%run test_script.py`，还会得到旧版本的some_lib.py，这是因为Python模块系统的“一次加载”机制。这一点区分了Python和其它数据分析环境，比如MA TLAB，它会自动传播代码修改。

解决这个问题，有多种方法。第一种是在标准库importlib模块中使用reload函数：

```python
import some_lib
import importlib
importlib.reload(some_lib)
```


这可以保证每次运行test_script.py时可以加载最新的some_lib.py。很明显，如果依赖更深，在各处都使用reload是非常麻烦的。对于这个问题，IPython有一个特殊的dreload函数（它不是魔术函数）重载深层的模块。如果我运行过some_lib.py，然后输入dreload(some_lib)，就会尝试重载some_lib和它的依赖。不过，这个方法不适用于所有场景，但比重启IPython强多了。

#### 代码设计技巧
对此没有简单的对策，但是有一些原则，是我在工作中发现很好用的。

##### 保持相关对象和数据活跃

为命令行写一个下面示例中的程序是很少见的：

```python
from my_functions import g
def f(x, y):
    return g(x + y)
def main():
    x = 6
    y = 7.5
    result = x + y
if __name__ == '__main__':
    main()
```

在IPython中运行这个程序会发生问题，你发现是什么了吗？运行之后，任何定义在main函数中的结果和对象都不能在IPython中被访问到。更好的方法是将main中的代码直接在模块的命名空间中执行（或者在 `__name__	==	'__main__':`中，如果你想让这个模块可以被引用）。这样，当你%run代码的时候，就可以查看所有定义在main中的变量。这等价于在Jupyter notebook的代码格中定义一个顶级变量。

##### 扁平优于嵌套
深层嵌套的代码总让我联想到洋葱皮。当测试或调试一个函数时，你需要剥多少层洋葱皮才能到达目标代码呢？“扁平优于嵌套”是Python之禅的一部分，它也适用于交互式代码开发。尽量将函数和类去耦合和模块化，有利于测试（如果你是在写单元测试）、调试和交互式使用。

##### 克服对大文件的恐惧
如果你之前是写JA V A（或者其它类似的语言），你可能被告知要让文件简短。在多数语言中，这都是合理的建议：太长会让人感觉是坏代码，意味着重构和重组是必要的。但是，在用IPython开发时，运行10个相关联的小文件（小于100行），比起两个或三个长文件，会让你更头疼。更少的文件意味着重载更少的模块和更少的编辑时在文件中跳转。我发现维护大模块，每个模块都是紧密组织的，会更实用和Pythonic。经过方案迭代，有时会将大文件分解成小文件。

我不建议极端化这条建议，那样会形成一个单独的超大文件。找到一个合理和直观的大型代码模块库和封装结构往往需要一点工作，但这在团队工作中非常重要。每个模块都应该结构紧密，并且应该能直观地找到负责每个功能领域功能和类。

### B.5	IPython高级功能
要全面地使用IPython系统需要用另一种稍微不同的方式写代码，或深入IPython的配置。

#### 使自定义类对IPython友好

IPython会尽可能地在控制台美化展示每个字符串。对于许多对象，比如字典、列表和元组，内置的pprint模块可以用来美化格式。但是，在用户定义的类中，你必自己生成字符串输出。

假设有一个下面的简单的类，就会发现默认的输出不够美观。IPython会接收repr魔术方法返回的字符串（通过output = repr(obj)），并在控制台打印出来。因此，我们可以添加一个简单的repr方法到前面的类中，以得到一个更有用的输出。

```python
class Message:
    def __init__(self, msg):
        self.msg = msg
    def __repr__(self):
        return 'Message: %s' % self.msg

x = Message('I have a secret')
```

#### 文件和配置
通过扩展配置系统，大多数IPython和Jupyter	notebook的外观（颜色、提示符、行间距等等）和动作都是可以配置的。通过配置，你可以做到：

- 改变颜色主题
- 改变输入和输出提示符，或删除输出之后、输入之前的空行
- 执行任意Python语句（例如，引入总是要使用的代码或者每次加载IPython都要运行的内容）
- 启用IPython总是要运行的插件，比如line_profiler中的%lprun魔术函数
- 启用Jupyter插件
- 定义自己的魔术函数或系统别名



IPython的配置存储在特殊的ipython_config.py文件中，它通常是在用户home目录的.ipython/文件夹中。配置是一个特殊文件，当你启动IPython，就会默认加载这个存储在profile_default文件夹中的默认文件。

要启动这个文件，运行下面的命令；这个文件有注释，解释了每个配置选项的作用。另一点，可以有多个配置文件。假设你想要另一个IPython配置文件，专门是为另一个应用或项目的。然后在新创建的profile_secret_project目录下编辑配置文件，然后如下启动IPython。

```
ipython profile create
ipython profile create secret_project
ipython --profile=secret_project
```

配置Jupyter有些不同，因为你可以使用除了Python的其它语言。要创建一个类似的Jupyter配置文件，运行以下命令。这样会在home目录下创建一个默认的配置文件.jupyter/jupyter_notebook_config.py。编辑完之后，可以将它重命名。打开Jupyter时，可以添加--config参数选项。

```
jupyter notebook --generate-config
$ mv ~/.jupyter/jupyter_notebook_config.py ~/.jupyter/my_custom_config.py
jupyter notebook --config=~/.jupyter/my_custom_config.py
```

## 更新记录

* 2020-12-19：首次发布；