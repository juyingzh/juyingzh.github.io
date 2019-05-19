---
layout: post
title: Python网络爬虫与信息提取学习笔记
categories: [学习笔记, 爬虫]
tags: [Python, 爬虫]
---


* Content
{:toc}

2019年3月25日开始学习北京理工大学的网络公开课：[Python网络爬虫与信息提取](https://www.icourse163.org/course/BIT-1001870001)。

课程主要讲述Python的网络数据爬取和解析技术，具体包括构建网络爬虫的两条重要技术路线：requests-bs4-re和Scrapy。手工爬取信息使用Requests获取网页，BeautifulSoap4解析网页，RE提取关键信息；网络爬虫框架自动完成信息爬取过程。



## 网络爬虫之规则

### Requests库入门

#### HTTP协议

HTTP（Hypertext Transfer Protocol，超文本传输协议）是一个基于”请求与响应“模式的、无状态的应用层协议，HTTP协议采用URL作为定位网络资源的标识，URL格式如下：

    http://host[:port][path]
    
  * host: 合法的Internet主机域名或IP地址
  * port: 端口号，缺省端口为80
  * path: 请求资源的路径

URL是本地计算机通过HTTP协议存取资源的Internet路径，一个URL对应一个数据资源。

HTTP/1.1协议提供对资源的9种请求方法：

  * GET 请求获取URL位置的资源
  * HEAD 请求获取URL位置资源的响应消息报告，即获得该资源的头部信息
  * POST 请求向URL位置的资源后附加新的数据
  * PUT 请求向URL位置存储一个资源，覆盖原URL位置的资源
  * PATCH 请求局部更新URL位置的资源，即改变该处资源的部分内容
  * DELETE 请求删除URL位置存储的资源
  * OPTIONS 用于描述目标资源的通信选项。
  * CONNECT 建立一个到由目标资源标识的服务器的隧道。
  * TRACE 沿着到目标资源的路径执行一个消息环回测试。

#### Requests库

[Requests库](ttp://www.python-requests.org)自动化的允许发送 HTTP/1.1 请求，他的7个主要方法与HTTP的前6种请求方法一一对应，均返回Response对象的实例：

##### requests.request()

构造并发送一个请求对象，支撑以下各方法的基础方法

    requests.request(method, url, **kwargs)

其中：
  * method: 请求方式，对应HTTP/1.1的前7种请求方法
  * url : 拟获取页面的url链接
  * **kwargs: 控制访问的参数，共13个，均为可选参数：
    * params -- 字典或字节序列，作为参数增加到url中
    * data --  字典、元组列表、字节序列或文件对象，作为Request的内容
    * json -- JSON格式的数据，作为Request的内容
    * headers -- 字典，HTTP定制头。注意：headers是字典类型{name1:value1,name2:value2}
    * cookies -- 字典或CookieJar。注意：这里传入的参数是字典类型{name1:value1,name2:value2}，Request中的name是cookie，其值为字符串"name1=value1;name2=value2"。
    * files -- 字典类型，传输文件
    * auth -- 元组，支持HTTP认证功能
    * timeout -- 设定超时时间，秒为单位，浮点数或元组(connect timeout, read timeout)
    * allow_redirects -- True/False，默认为True，重定向开关
    * proxies -- 字典类型，设定访问代理服务器，可以增加登录认证
    * verify -- True/False，默认为True，认证SSL证书开关
    * stream -- True/False，默认为True，获取内容立即下载开关，如果关闭，内容会立即下载
    * cert -- 本地SSL证书路径

##### requests.get()

获取HTML网页的主要方法，对应于HTTP的GET

    requests.get(url, params=None, **kwargs)
    
其中：
  * url : 拟获取页面的url链接
  * params : url中的额外参数，字典或字节流格式，可选
  * **kwargs: 12个控制访问的参数

#####  requests.head()

获取HTML网页头信息的方法，对应于HTTP的HEAD

    requests.head(url, **kwargs)
    
其中：
  * url : 拟获取页面的url链接
  * **kwargs: 13个控制访问的参数
  
##### requests.post()

向HTML网页提交POST请求的方法，对应于HTTP的POST

    requests.post(url, data=None, json=None, **kwargs)

其中：
  * url : 拟更新页面的url链接
  * data  : 字典、字节序列或文件，Request的内容
  * json : JSON格式的数据，Request的内容
  * **kwargs: 11个控制访问的参数
  
##### requests.put()

向HTML网页提交PUT请求的方法，对应于HTTP的PUT

    requests.put(url, data=None, **kwargs)

其中：
  * url : 拟更新页面的url链接
  * data  : 字典、字节序列或文件，Request的内容
  * **kwargs: 12个控制访问的参数
  
##### requests.patch()

向HTML网页提交局部修改请求，对应于HTTP的PATCH

    requests.patch(url, data=None, **kwargs)
    
其中：
  * url : 拟更新页面的url链接
  * data  : 字典、字节序列或文件，Request的内容
  * **kwargs: 12个控制访问的参数
  
##### requests.delete()

向HTML页面提交删除请求，对应于HTTP的DELETE

    requests.delete(url, **kwargs)

其中：
  * url : 拟删除页面的url链接
  * **kwargs: 12个控制访问的参数
  
Requests库包含两个低级的类：Request和Response，前者为用户构建的请求对象，后者为服务器返回的响应对象。

Requests库常用异常：

  * requests.ConnectionError 网络连接错误异常，如DNS查询失败、拒绝连接等
  * requests.HTTPError HTTP错误异常
  * requests.URLRequired URL缺失异常
  * requests.TooManyRedirects 超过最大重定向次数，产生重定向异常
  * requests.ConnectTimeout 连接远程服务器超时异常
  * requests.ReadTimeout 服务器返回数据超时异常
  * requests.Timeout 请求URL超时，产生超时异常。包括ConnectTimeout和ReadTimeout。

Response对象的主要属性：

  * r.status_code HTTP请求的返回状态，200表示连接成功，404表示失败
  * r.text HTTP响应内容的字符串形式，即，url对应的页面内容。根据r.encoding显示。
  * r.encoding 从HTTP header中猜测的响应内容编码方式，如果header中不存在charset，则认为编码为ISO‐8859‐1。
  * r.apparent_encoding 从内容中分析出的响应内容编码方式（备选编码方式）
  * r.content HTTP响应内容的二进制形式
  
对Response对象处理的流程是先判断status_code，如果是200，则继续后续操作，如果是404或其他，则进行错误处理。

Response类提供raise_for_status()方法，判断status_code是否小于400（Response.ok），否则产生异常requests.HTTPError。使用此方法的通用代码框架如下:

```python
try:
    r = requests.request()
    r.raise_for_status()
    r.encoding = r.apparent_encoding
    return r.text
except:
    return: "Error"
```

### 网络爬虫的盗亦有道

网站对网络爬虫的限制包括：

  * 来源审查：判断User‐Agent进行限制，即检查来访HTTP协议头的User‐Agent域，只响应浏览器或友好爬虫的访问
  * 发布公告：Robots协议，即告知所有爬虫网站的爬取策略，要求爬虫遵守

Robots协议，全称是Robots Exclusion Standard，网络爬虫排除标准，其作用是网站告知网络爬虫哪些页面可以抓取，哪些不行。存在形式为在网站根目录下的robots.txt文件。其格式为：

```
#注释，*代表所有，/代表根目录
User-agent: * 
Disallow: /
```


## 网络爬虫之提取

### Beautiful Soup库入门

[Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/) 是一个可以从HTML或XML文件中提取数据的Python库。如果有一段HTML代码，使用BeautifulSoup解析这段代码,能够得到一个 BeautifulSoup 对象,并按照标准的缩进格式输出的代码实例如下：

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html_doc)
print(soup.prettify())
```

本例使用Python标准库的HTML解析器html.parser。BeautifulSoup第一个参数应该是要被解析的文档字符串或是文件句柄，第二个参数用来标识怎样解析文档。如果第二个参数为空,那么Beautiful Soup根据当前系统安装的库自动选择解析器，解析器的优先数序: lxml, html5lib, Python标准库。三个解析器的对比如下，推荐使用lxml作为解析器,因为效率更高。

| 解析器 | 使用方法 | 特点 |
|--|--|--|
| Python标准库 | `BeautifulSoup(markup, "html.parser")`     |  Python的内置标准库，执行速度适中，文档容错能力强 |
| lxml HTML 解析器 | `BeautifulSoup(markup, "lxml")` |  C语言库，速度快，文档容错能力强 |
| lxml XML 解析器  | `BeautifulSoup(markup, ["lxml", "xml"])` 或 `BeautifulSoup(markup, "xml")` | C语言库，速度快，唯一支持XML的解析器 |
| html5lib 解析器 | `BeautifulSoup(markup, "html5lib")` | 纯Python实现，最好的容错性，以浏览器的方式解析文档，生成HTML5格式的文档，速度慢 |

BeautifulSoup解析标签树结构文档的功能库，他将复杂HTML或XML文档转换成一个复杂的树形结构,每个节点都是Python对象,所有对象可以归纳为4种: Tag , NavigableString , BeautifulSoup , Comment 。

  * Tag 对象与XML或HTML原生文档中的tag相同，Tag有很多方法和属性，其中最重要的属性: name和attributes。
    * Name 每个tag都有自己的名字,通过 <tag>.name 来获取
    * Attributes 一个tag可能有很多个属性，tag的属性的操作方法与字典相同，也可以直接”点”取属性，<tag>.attrs。
  * NavigableString 可以遍历的字符串常被包含在tag内，使用<tag>.string
  * BeautifulSoup 对象表示的是一个文档的全部内容.大部分时候,可以把它当作 Tag 对象，但它没有name和attribute属性。
  * Comment  对象是文档的注释部分，是一个特殊类型的 NavigableString 对象。

遍历标签树的方法有三种：上行遍历、下行遍历、平行遍历；另外，也可以按照解析器解析标签树的顺序进行遍历：
 
下行遍历使用：

  * contents 获取子节点的列表，将<tag>所有子节点存入列表
  * children 子节点的迭代类型，与.contents类似，用于循环遍历子节点
  * descendants 子孙节点的迭代类型，包含所有子孙节点，用于循环遍历
  * 

上行遍历使用：

  * parent 节点的父亲标签
  * parents 节点先辈标签的迭代类型，用于循环遍历先辈节点
  
 平行遍历发生在同一个父节点下的各节点间，使用：
 
  * next_sibling 返回按照HTML文本顺序的下一个平行节点标签
  * previous_sibling 返回按照HTML文本顺序的上一个平行节点标签
  * next_siblings 迭代类型，返回按照HTML文本顺序的后续所有平行节点标签
  * previous_siblings 迭代类型，返回按照HTML文本顺序的前续所有平行节点标签
  
解析器顺序遍历使用回退和前进的方法：

  * next_element 指向解析过程中下一个被解析的对象
  * previous_element 指向当前被解析的对象的前一个解析对象:
  * next_elements 返回迭代器，向前访问文档的所有解析内容
  * previous_elements 返回迭代器，向后访问文档的所有解析内容

### 信息组织与提取方法

信息需要标记以便于理解：

  * 标记后的信息可形成信息组织结构，增加了信息维度
  * 标记的结构与信息一样具有重要价值
  * 标记后的信息可用于通信、存储或展示
  * 标记后的信息更利于程序理解和运用

HTML就是通过预定义的<>…</>标签形式组织不同类型的信息。计算机信息系统中进行信息标记的种类分为三种：

  * XML(eXtensible Markup Language) 使用标签进行信息标记。
    ```xml
    <name> … </name>
    <name />
    <!‐‐ ‐‐>
    ```
  * JSON(avsScript Object Notation) 使用有类型的键值对key:value进行信息标记，其值可以是双引号括起来的字符串（string）、数值(number)、true、false、 null、对象（object）或者数组（array）。
    ```json
    “key” : “value”
    “key” :[“value1”, “value2”]
    “key” : {“subkey” : “subvalue”}
    ```
  * YAML(YAML Ain't Markup Language) 使用无类型键值对key:value标记信息，缩进表达所属关系，- 表达并列关系，| 表达整块数据， # 表示注释。
    ```yaml
    key : value
    key : #Comment
    ‐value1
    ‐value2 
    key : 
        subkey: subvalue
    ```

三种信息标记格式的比较：

  * XML 最早的通用信息标记语言，可扩展性好，但繁琐；Internet上的信息交互与传递
  * JSON 信息有类型，适合程序处理(js)，较XML简洁；移动应用云端和节点的信息通信，无注释
  * YAML 信息无类型，文本信息比例最高，可读性好；各类系统的配置文件，有注释易读

从标记后的信息中提取所关注的内容的一般方法：
  * 方法一：完整解析信息的标记形式，再提取关键信息。如使用XML JSON YAML标记解析器对标签树遍历
    * 优点：信息解析准确
    * 缺点：提取过程繁琐，速度慢
  * 方法二：无视标记形式，直接搜索关键信息，即对信息的文本进行查找
    * 优点：提取过程简洁，速度较快
    * 缺点：提取结果准确性与信息内容相关
  * 融合方法：结合形式解析与搜索方法，提取关键信息，即使有标记解析器和文本查找函数

例如：提取HTML中所有URL链接，其思路：
  1) 搜索到所有<a>标签
  2) 解析<a>标签格式，提取href后的链接内容

使用查找函数进行信息检索，涉及到过滤器的概念。BeautifulSoup库的查找函数使用的过滤器有：

  * 字符串 在搜索方法中传入一个字符串参数,Beautiful Soup会查找与字符串完整匹配的内容；
  * 正则表达式 如果传入正则表达式作为参数,Beautiful Soup会通过正则表达式的 match() 来匹配内容；
  * 列表 如果传入列表参数,Beautiful Soup会将与列表中任一元素匹配的内容返回；
  * True 可以匹配任何值，但是不会返回字符串节点；
  * 方法 如果没有合适过滤器,那么还可以定义一个方法,方法只接受一个元素参数 ,如果这个方法返回 True 表示当前元素匹配并且被找到,如果不是则反回 False。

使用BeautifulSoup库进行信息搜索的常用方法：

    <>.find_all(name, attrs, recursive, string, **kwargs)

该方法搜索当前tag的所有tag子节点,并判断是否符合过滤器的条件，返回一个列表类型，存储查找的结果。其中的参数：

  * name : 对标签名称的检索字符串，搜索 name 参数的值可以是任一类型的过滤器（字符窜,正则表达式,列表,方法或是 True）
  * attrs: 对标签属性值的检索字符串，可关键词属性检索。因为class是Python的保留关键词，所以使用class_关键词搜索标签的CSS class属性。
  * recursive: 是否对子孙全部检索，默认True
  * string: <>…</>中字符串区域的检索字符串，可以使任一类型的过滤器（字符窜,正则表达式,列表,方法或是 True）
  * limit : 限制返回的结果数量
  
find_all()的等价调用方式是直接使用标签类：如<tag>(..)  等价于 <tag>.find_all(..)。

与find_all()类似的方法还有：

  * <>.find() 搜索且只返回一个结果，同.find_all()参数
  * <>.find_parents() 在先辈节点中搜索，返回列表类型，同.find_all()参数
  * <>.find_parent() 在先辈节点中返回一个结果，同.find()参数
  * <>.find_next_siblings() 在后续平行节点中搜索，返回列表类型，同.find_all()参数
  * <>.find_next_sibling() 在后续平行节点中返回一个结果，同.find()参数
  * <>.find_previous_siblings() 在前序平行节点中搜索，返回列表类型，同.find_all()参数
  * <>.find_previous_sibling() 在前序平行节点中返回一个结果，同.find()参数
  * <>.find_all_next() 按照解析器顺序向后遍历所有节点，返回列表类型
  * <>.find_next() 按照解析器顺序向后遍历所有节点，返回一个结果
  * <>.find_all_previous() 按照解析器顺序向前遍历所有节点，返回列表类型
  * <>.find_previous() 按照解析器顺序向前遍历所有节点，返回一个结果

## 网络爬虫之实战

### Re(正则表达式)库入门

regular expression, regex, RE
正则表达式（regular expression, regex, RE）是一种通用的字符串表达框架，体现了字符串表达“简洁”和“特征”的思想。正则表达式可以用来简洁表达一组字符串，也可以判断某字符串的特征归属。

正则表达式的语法参见 http://zynd.net/doku.php?id=information:regex

re库是Python的标准库，主要用于字符串匹配，其调用方式：`import re`。

re库采用raw string类型（原始字符串类型，忽略其中的转义字符）表示正则表达式，表示为：r'text'；也可以采用string类型表示正则表达式，但需要对对转义符再次转义。

一个正则表达式经过编译后才能使用，即将符合正则表达式语法的字符串转换成正则表达式对象。

```
regex = re.compile(pattern, flags=0)
```

其中，pattern为正则表达式字符串，flags 为正则表达式使用时的控制标记，具有以下值：

| 常用标记 | 说明 |
|--|--|
|re.I re.IGNORECASE | 忽略正则表达式的大小写，[A‐Z]能够匹配小写字符 |
|re.M re.MULTILINE | 正则表达式中的^操作符能够匹配每行的开始，$操作符能够匹配每行的结尾，缺省状态只匹配整个字符串的开始和结尾 |
|re.S re.DOTALL | 正则表达式中的.操作符能够匹配所有字符，默认匹配除换行外的所有字符 |

re库的使用包括两种方法：函数式用法和面向对象用法，前一种方法用于一次性操作（每次调用函数时，都要编译一次），后一种方法用于编译后的多次操作，效率更高。

#### 函数式用法


   re.search(pattern, string, flags=0)  # 在一个字符串中搜索匹配正则表达式的第一个位置，返回match对象

其中：string  : 待匹配字符串

  re.match(pattern, string, flags=0)  # 从一个字符串的开始位置起匹配正则表达式，返回match对象
  
  re.findall(pattern, string, flags=0)  # 搜索字符串，以列表类型返回全部能匹配的子串
  
  re.split(pattern, string, maxsplit=0, flags=0)  # 将一个字符串按照正则表达式匹配结果进行分割，返回列表类型
  
其中：maxsplit: 最大分割数，剩余部分作为最后一个元素输出

  re.finditer(pattern, string, flags=0)  # 搜索字符串，返回一个匹配结果的迭代类型，每个迭代元素是match对象
  
  re.sub(pattern, repl, string, count=0, flags=0)  # 在一个字符串中替换所有匹配正则表达式的子串，返回替换后的字符串
  
其中：repl : 替换匹配字符串的字符串；count  : 匹配的最大替换次数。

#### 面向对象用法

正则表达式对象的方法与上述方法相同，但是省去了重新编译正则表达式的过程。

```
prog = re.compile(pattern)
result = prog.match(string)
```

与下面的方法等效。

```
result = re.match(pattern, string)
```

#### re库的Match对象

Match 对象总是包含一个布尔值 True，因为 match() 和 search() 在没有匹配的时候返回 None ，因此可以使用简单的if语句检测是否有匹配结果。

```
match = re.search(pattern, string)
if match:
    process(match)
```

Match对象的主要属性：

  * Match.string 待匹配的文本
  * Matchre 匹配时使用的pattern对象（正则表达式）
  * Match.pos 正则表达式搜索文本的开始位置
  * Match.endpos 正则表达式搜索文本的结束位置

Match对象的主要方法：

  * Match.group(0) 获得匹配后的字符串
  * Match.start() 匹配字符串在原始字符串的开始位置
  * Match.end() 匹配字符串在原始字符串的结束位置
  * Match.span() 返回(.start(), .end())

#### re库的贪婪匹配和最小匹配

Re库默认采用贪婪匹配，即输出匹配最长的子串，例如：

```
>>> match = re.search(r'PY.*N', 'PYANBNCNDN')
>>> match.group(0)
'PYANBNCNDN'
```

只要长度输出可能不同的，都可以通过在操作符后增加?变成最小匹配，包括`*?`，`+?`，`??`，`{m,n}?`，例如：

```
>>> match = re.search(r'PY.*?N', 'PYANBNCNDN')
>>> match.group(0)
'PYAN'
```

## 网络爬虫之框架

### Scrapy爬虫框架

[Scrapy](https://scrapy.org/)是一个快速功能强大的网络爬虫框架。爬虫框架是实现爬虫功能的一个软件结构和功能组件集合，能够帮助用户实现专业网络爬虫。

Scrapy爬虫框架为5+2结构：5个核心部分（Engine、Scheduler、Spiders、Downloader、 Item Pipelines）加2个中间件（Downloader Middlewares、Spider Middlewares）。框架内的数据流如下：

  1. Engine 接受从Spider发出的初始请求；
  2. Engine 把请求安排在 Scheduler 中，并询问下一个爬取请求；
  3. Scheduler 向 Engine 返回下一个请求；
  4. Engine 把请求发送给 Downloader，这个过程要经过 Downloader Middlewares 中间件；
  5. 爬取到页面后，Downloader 产生一个响应并发送给 Engine，这个过程也要经过 Downloader Middlewares 中间件；
  6. Engine 接收到响应后，发送给 Spider 进行处理，这个过程要经过 Spider Middleware 中间件；
  7. Spider 处理完后，给 Engine 返回抓取项和新的请求，这个过程也要经过 Spider Middleware 中间件；
  8. Engine 将抓取项发送给 Item Pipelines，将新的请求发送给 Scheduler，并询问下一个爬取请求；
  9. 重复上述过程直到 Scheduler 中没有爬取请求。

其中，可以分为三个主要的数据流，一是1-2步，请求生成过程；二是3-6步，网页爬取过程；三是7-8，信息提取和新请求生成过程。框架的入口是Spiders，出口是Item Pipelines。框架的应用过程中也主要是编写（或配置）这两个模块。

Spider解析Downloader返回的响应（Response），产生爬取项（scraped item）和新的爬取请求（Request）。Item Pipelines以流水线方式处理Spider产生的爬取项，其由一组操作顺序组成，类似流水线，每个操作是一个Item Pipeline类型，可能操作包括：清理、检验和查重爬取项中的HTML数据、将数据存储到数据库等。

Downloader Middleware中间件是一个底层钩子，用来处理Engine和Downloader之间的请求和响应，可以修改、丢弃、新增请求或响应。Spider Middleware中间件也是一个底层钩子，用来处理 Spider 的输入（响应）和输出（爬取项和新的爬取请求），可以修改、丢弃、新增请求或爬取项。

Scrapy 使用命令行工具进行控制，其格式为：` scrapy <command> [options] [args]`。其中包括两类命令，一类是全局命令，另一类是工程命令。

Scrapy 主要的全局命令有：

  * `startproject <project_name> [project_dir]`，在 project_dir目录下创建一个新工程project_name；
  * `genspider [-t template] <name> <domain>`，在当前工程目录内或spiders目录内创建一个新的爬虫 <name>，其中 <domain> 用来设置 allowed_domains 和 start_urls 属性；
  * `settings  [options]`，获取 Scrapy 配置信息，如果在工程目录内，则显示工程配置信息，否则显示缺省配置信息；
  * `runspider <spider_file.py>`，运行一个包含在 Python 文件中的爬虫，不需要创建工程；
  * `shell [url]`，对指定URL启动Scrapy Shell进行调试；
  * `fetch <url>`，使用 downloader 下载指定URL，并输出到标准输出；
  * `view <url>`，使用Scrapy爬虫爬取指定URL，并在浏览器中显示；
  * `version [-v]`，输出 Scrapy 版本信息，如果使用 `-v`，同时输出所有依赖库和平台的信息。
  
Scrapy 主要的工程命令有：

  * `crawl <spider>`，启动一个爬虫；
  * `check [-l] <spider>`，进行协议检查；
  * `list`，列出当前工程下全部爬虫；
  * `edit <spider>`，编辑指定的爬虫，编辑器有环境变量或配置信息指定；
  * `parse <url> [options]`，抓取指定 URL 并使用 spider 进行解析；
  * `bench`，运行一个快速的性能测试。

新建一个工程时，框架自动生成的目录结构如下：

```
scrapy.cfg              --> 部署Scrapy爬虫的配置文件
myproject/              --> Scrapy框架的用户自定义Python代码
    __init__.py
    items.py            --> Items代码模板（继承类）
    middlewares.py      --> Middlewares代码模板（继承类）
    pipelines.py        --> Pipelines代码模板（继承类）
    settings.py         --> Scrapy爬虫的配置文件
    spiders/            --> Spiders代码模板目录（继承类）
        __init__.py
        spider1.py      --> 用户自定义的spider代码
        spider2.py
        ...
```

### Scrapy爬虫基本使用

应用Scrapy爬虫框架主要是编写配置型代码，其工作流程如下：

  1. 建立一个Scrapy爬虫工程
  2. 在工程中产生一个Scrapy爬虫
  3. 配置产生的spider爬虫
  4. 运行爬虫，获取网页
  
Scrapy使用yield遍历start_urls列表，产生爬取请求。生成器是一个不断产生值的函数，包含yield语句的函数是一个生成器。生成器每次产生一个值（yield语句），函数被冻结，被唤醒后再产生一个值。使用生成器相比一次列出所有内容的优势包括：更节省存储空间、响应更迅速、使用更灵活。

使用Scrapyp爬虫框架的基本步骤如下：  

  1. 创建一个工程和Spider模板
  2. 编写Spider
  3. 编写Item Pipeline
  4. 优化配置策略

使用Scrapyp爬虫框架的主要涉及三个类：Request、Response、Item。

class scrapy.http.Request()，Request对象表示一个HTTP请求，由Spider生成，由Downloader执行。具有以下主要的属性和方法：

  * .url Request对应的请求URL地址
  * .method 对应的请求方法，'GET' 'POST'等
  * .headers 字典类型风格的请求头
  * .body 请求内容主体，字符串类型
  * .meta 用户添加的扩展信息，在Scrapy内部模块间传递信息使用
  * .copy() 复制该请求

class scrapy.http.Response()，Response对象表示一个HTTP响应，由Downloader生成，由Spider处理。具有以下主要的属性和方法：

  * .url Response对应的URL地址
  * .status HTTP状态码，默认是200
  * .headers Response对应的头部信息
  * .body Response对应的内容信息，字符串类型
  * .flags 一组标记
  * .request 产生Response类型对应的Request对象
  * .copy() 复制该响应

class scrapy.item.Item()，Item对象表示一个从HTML页面中提取的信息内容，由Spider生成，由Item Pipeline处理。Item类似字典类型，可以按照字典类型操作。

Scrapy爬虫框架支持多种HTML信息提取方法，包括：Beautiful Soup、lxml、re、XPath Selector、CSS Selector。

使用Response 对象的.selector属性指定一个Selector实例，如：

```
response.selector.xpath('//span/text()').get()
response.selector.css('span::text').get()
```

同时，Response对象也提供一个快捷方法：

```
response.xpath('//span/text()').get()
response.css('span::text').get()
```

其中，`get()`返回第一个匹配结果，`getall()`返回所有匹配结果的列表。

W3C 的 CSS selectors 标准并不支持提取文本或属性值，Scrapy 实现了一个非标准的提取方法：

  * `::text`，提取文本节点
  * `::attr(name)`，提取属性值，其中 name 是属性名

Scrapy性能的优化需要修改settings.py文件的配置参数，主要有：

  * CONCURRENT_REQUESTS Downloader 最大并发请求下载数量，默认16
  * CONCURRENT_ITEMS Item Pipeline 最大并发ITEM处理数量，默认100
  * DOWNLOAD_DELAY 在同一个网站上连续爬取的时间间隔（秒），默认0
  * CONCURRENT_REQUESTS_PER_DOMAIN 每个目标域名最大的并发请求数量，默认8
  * CONCURRENT_REQUESTS_PER_IP 每个目标IP最大的并发请求数量，默认0，非0有效
  
## 总结

2019年5月2日，利用五一休假时间，结束了“Python网络爬虫与信息提取”课程的学习。

课程主要学习了网络爬虫获取信息的基本过程和方法、学习了Requests库和BeautifulSoup库，以及Scrapy爬虫框架。学习了实现网络爬虫的两种技术路径：一个是Requests + BeautifulSoup方法，适用于小型的网络爬虫项目（网页级）；另一个是Scrapy爬虫框架，适用于中型的网络爬虫项目（网站级）。