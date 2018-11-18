---
layout: post
title:  使用Liquid
---

# 使用Liquid

## 关于Liquid

Liquid 是一个灵活、安全的，并且开源的的模板语言，由 [Shopify](https://www.shopify.com/) 使用Ruby开发，被用在许多不同的环境中。目前，最流行的版本有：Liquid、Shopify Liquid 和 Jekyll Liquid。

Liquid的官网地址：<https://shopify.github.io/liquid/>

Jekyll 使用了 Liquid 作为其模板语言，并添加了一些对象、标签和过滤器。Jekyll 使用的不一定是最新版。Jekyll Liquid 的文档可以查看：<https://jekyllrb.com/docs/liquid/>。

## Liquid基础

### 一般概念

Liquid 语言包含三类元素：对象、标签和过滤器。

#### 对象

对象表示在页面什么位置显示内容。对象和变量使用双花括号：`{{ objects }}`。

#### 标签

标签用来创建逻辑和控制流。标签使用花括号和百分号：{% raw %}`{% tags %}`{% endraw %}。

标签不产生可视输出，可以使用标签进行变量赋值、判断条件、执行循环等操作。例如：

	{% if user %}
	  Hello {{ user.name }}!
	{% endif %}

标签可以分为三类：

*  控制流
*  迭代
*  变量赋值

#### 过滤器

过滤器用来改变对象的输出。过滤器包含在对象中，并用竖线隔开：`|`。例如：

	{{ "adam!" | capitalize | prepend: "Hello " }}

一个对象可以使用多个过滤器，它们从左到右依次应用。

### 运算符

Liquid支持许多逻辑和比较运算符。

#### 基本运算符

	== 	equals
	!= 	does not equal
	> 	greater than
	< 	less than
	>= 	greater than or equal to
	<= 	less than or equal to
	or 	logical or
	and 	logical and

例如：

	{% if product.type == "Shirt" or product.type == "Shoes" %}
	  This is a shirt or a pair of shoes.
	{% endif %}

#### 包含运算符（contains）

包含运算符用来检查一个字符串是否包含在另一个字符串，或字符串数组中。包含运算符只能用来检查字符串。例如：

	{% if product.title contains 'Pack' %}
	  This product's title contains the word Pack.
	{% endif %}

### 真和假

在条件语句中返回true的都称为真，返回false的都称为假。这样，所有对象都可描述为真或假。

#### 真

除了`nil`和`false`，Liquid中的所有变量都为真。即使是0或一个空字符串也是真。 

#### 假

Liquid中假的变量只有：`nil`和`false`。

### 类型

Liquid 对象有五种类型。使用 `assign` 或 `capture`标签初始化变量。

#### 字符串

使用单或双引号声明一个字符串。例如：

	{% assign my_string = "Hello World!" %}

#### 数值

数值包括浮点书和整数。例如：

	{% assign my_int = 25 %}
	{% assign my_float = 39.756 %}

#### 布尔

布尔变量可以是真或假，不需要使用引号。例如：

	{% assign foo = true %}
	{% assign bar = false %}

#### Nil

Nil 是一个特殊的空值，当 Liquid 代码没有结果时返回这个值，在条件判断语句中处理为假，页面中不会输出Nil。例如，当用户不存在是，下例无输出。

	{% if user %}
	  Hello {{ user.name }}!
	{% endif %}

#### 数组

数组为包含任何类型的变量列表。

#### 遍历数组元素

可以使用迭代标签循环访问数组中的每个元素。例如：

	<!-- if site.users = "Tobi", "Laura", "Tetsuro", "Adam" -->
	{% for user in site.users %}
	  {{ user }}
	{% endfor %}


#### 引用数组元素

使用方括号引用数组元素，索引从0开始。例如

	<!-- if site.users = "Tobi", "Laura", "Tetsuro", "Adam" -->
	{{ site.users[0] }}
	{{ site.users[1] }}
	{{ site.users[3] }}

#### 初始化数组

无法单独使用Liquid初始化数组。但可以使用`split`过滤器把字符串分解成字符串数组。

### 空格处理

一般来说，即使没有输出文本，Liquid的行也会输出一个空行。可以在标签中加短横，` {{-, -}}, {%-, and -%}` 来去掉输出的空格。例如：

	{%- assign username = "John G. Chalmers-Smith" -%}
	{%- if username and username.size > 10 -%}
	  Wow, {{ username }}, you have a long name!
	{%- else -%}
	  Hello there!
	{%- endif -%}

## 标签

###  注释

在两个注释标签之间的`{% comment %}` 任何文本和代码都不会被解释。例如：


	Anything you put between {% comment %} and {% endcomment %} tags
	is turned into a comment.

###  Control flow

使用程序逻辑控制 Liquid 输出。

####  if

只有当条件为真时执行代码块。例如：

	{% if product.title == 'Awesome Shoes' %}
	  These shoes are awesome!
	{% endif %}

####  unless

与 if 相反，只有当条件不满足时执行代码块。例如：

	{% unless product.title == 'Awesome Shoes' %}
	  These shoes are not awesome.
	{% endunless %}

####  elsif / else

在 if / unless 代码块中增加选择条件。

	<!-- If customer.name = 'anonymous' -->
	{% if customer.name == 'kevin' %}
	  Hey Kevin!
	{% elsif customer.name == 'anonymous' %}
	  Hey Anonymous!
	{% else %}
	  Hi Stranger!
	{% endif %}

####  case/when

分支选择语句，case 初始化分支语句，when 比较其变量。

	{% assign handle = 'cake' %}
	{% case handle %}
	  {% when 'cake' %}
		 This is a cake
	  {% when 'cookie' %}
		 This is a cookie
	  {% else %}
		 This is not a cake nor a cookie
	{% endcase %}

###  迭代

重复执行一个代码块。

####  for

重复执行一个代码块。例如：


	{% for product in collection.products %}
		{{ product.title }}
	{% endfor %}

####  break

停止循环。例如：

	{% for i in (1..5) %}
	  {% if i == 4 %}
		{% break %}
	  {% else %}
		{{ i }}
	  {% endif %}
	{% endfor %}

####  continue

跳过本次迭代。例如：

	{% for i in (1..5) %}
	  {% if i == 4 %}
		{% continue %}
	  {% else %}
		{{ i }}
	  {% endif %}
	{% endfor %}

#### for (parameters)

#####  limit

限制循环次数。例如：

	<!-- if array = [1,2,3,4,5,6] -->
	{% for item in array limit:2 %}
	  {{ item }}
	{% endfor %}

#####  offset

从指定索引开始迭代。例如：

	<!-- if array = [1,2,3,4,5,6] -->
	{% for item in array offset:2 %}
	  {{ item }}
	{% endfor %}

#####  range

定义迭代范围，可以使用数字或变量。例如：

	{% assign num = 4 %}
	{% for i in (1..num) %}
	  {{ i }}
	{% endfor %}

#####  reversed

反转迭代顺序。注意，与过滤器reverse的区别。例如：

	<!-- if array = [1,2,3,4,5,6] -->
	{% for item in array reversed %}
	  {{ item }}
	{% endfor %}

####  cycle

在一组字符串中循环。字符串组作为参数输入，每次调用时，输出参数中的下一个字符串。cycle 必须在 for 循环块中使用。例如：

输入

	{% cycle 'one', 'two', 'three' %}
	{% cycle 'one', 'two', 'three' %}
	{% cycle 'one', 'two', 'three' %}
	{% cycle 'one', 'two', 'three' %}

输出

	one
	two
	three
	one

cycle 接受一个参数叫 `cycle group`， 用在一个模板需要有多个 cycle 块的地方。如果没有为cycle group命名，则具有相同参数的多个调用被认为是一个组。

####  tablerow

产生 HTML 表格。必须包裹在` <table>  </table> `HTML 标签中。例如：

	<table>
	{% tablerow product in collection.products %}
	  {{ product.title }}
	{% endtablerow %}
	</table>

####  tablerow (parameters)

#####  cols

定义表格的列数。例如：

	{% tablerow product in collection.products cols:2 %}
	  {{ product.title }}
	{% endtablerow %}

#####  limit

达到限制后退出tablerow 。例如：

	{% tablerow product in collection.products cols:2 limit:3 %}
	  {{ product.title }}
	{% endtablerow %}

#####  offset

从指定的索引开始 tablerow 。例如：

	{% tablerow product in collection.products cols:2 offset:3 %}
	  {{ product.title }}
	{% endtablerow %}

#####  range

定义循环范围，可以使用数字或变量。例如：

	<!--variable number example-->
	{% assign num = 4 %}
	<table>
	{% tablerow i in (1..num) %}
	  {{ i }}
	{% endtablerow %}
	</table>

###  Raw

Raw 临时禁止标签的处理，用在使用相互冲突的语法输出内容的地方。例如：

	{% raw %}
	  In Handlebars, {{ this }} will be HTML-escaped, but
	  {{{ that }}} will not.
	{% endraw %}

###  变量

Variable 标签创建 Liquid 变量。

####  assign

创建一个新变量。引号中的变量会保存为字符串。例如

	{% assign my_variable = false %}
	{% if my_variable != true %}
	  This statement is valid.
	{% endif %}

####  capture

捕获一个字符串，并赋值给变量。通过包含assign创建的变量，可以创建复杂字符串。例如

	{% assign favorite_food = 'pizza' %}
	{% assign age = 35 %}

	{% capture about_me %}
	I am {{ age }} and my favorite food is {{ favorite_food }}.
	{% endcapture %}

	{{ about_me }}

####  increment

创建一个数值变量，每次调用时加1，初始值为0。例如：

	{% increment my_counter %}
	{% increment my_counter %}
	{% increment my_counter %}

注意：increment创建的变量与 assign 和 capture创建的变量保持独立。

####  decrement

创建一个数值变量，每次调用时减1，初始值为-1。

##  过滤器

###  abs

返回数值的绝对值。如果字符串包含数值，abs 也可以工作于字符串。例如：

	{{ "-19.86" | abs }}

###  append/prepend

在一个字符串后附加另一个字符串。例如：

	{% assign filename = "/index.html" %}
	{{ "website.com" | append: filename }}

###  at_least/at_most

限制一个数值的最小值或最大值。

###  capitalize

使字符串的首字母大写。

###  ceil/floor

近似到上方或下方最近的整数。应用该过滤器前先试图转换成数值。例如：

	{{ "3.5" | ceil }}

###  compact

移除数组中的 nil 值。例如：

	{% assign site_categories = site.pages | map: 'category' | compact %}

	{% for category in site_categories %}
	  {{ category }}
	{% endfor %}

###  concat

合并多个数组。例如：

	{% assign fruits = "apples, oranges, peaches" | split: ", " %}
	{% assign vegetables = "carrots, turnips, potatoes" | split: ", " %}

	{% assign everything = fruits | concat: vegetables %}

	{% for item in everything %}
	- {{ item }}
	{% endfor %}

###  date

将一个时间戳转换为另一个日期格式。日期的格式与 [strftime](http://strftime.net/) 相同。 输入使用Ruby的 [Time.parse](https://ruby-doc.org/stdlib-2.5.3/libdoc/time/rdoc/Time.html#method-c-parse)。如果字符串中包含格式化的日期，也可以用于字符串。例如：

	{{ "March 14, 2016" | date: "%b %d, %y" }}

要得到当前时间，输入"now" 或 "today"。例如：

	This page was last updated at {{ "now" | date: "%Y-%m-%d %H:%M" }}.

###  default

指定一个缺省值，当左侧为nil, false, 或 empty时，显示这个值。

###  divided_by

除一个数，结果类型与除数类型相同。如果除数是整数，结果近似到下方最近的整数。
 
###  downcase/upcase

将一个字符串转换成小写或大写。

###  escape

使用转义字符替换字符串中的特殊字符。例如：

输入

	{{ "Have you read 'James & the Giant Peach'?" | escape }}

输出

	Have you read &#39;James &amp; the Giant Peach&#39;?

###  escape_once

转义字符串但不改变原有的转义字符。

###  first／last

返回数组的第一项或最后一项。可以用于点号表达式，如 `{{ my_array.first }}`。

###  join/split

使用一个连接词将数组中的所有项连接成一个字符串或将一个字符串分解成数组。例如：

	{% assign beatles = "John, Paul, George, Ringo" | split: ", " %}

	{{ beatles | join: " and " }}

###  lstrip/rstrip/strip

移除字符串开头，或结尾，或首尾的空白（空白、制表符、回车符），对单词中间的空白不起作用。

###  map

从一个对象中提取包含了命名属性的值以创建一个数组。例如：

	{% assign all_categories = site.pages | map: "category" %}

	{% for item in all_categories %}
		{{ item }}
	{% endfor %}

###  minus/plus/times

一个数减去，或加上，或乘以另一个数。

###  modulo

返回除法的余数。

###  newline_to_br

将所以的 newline (\n) 替换成 HTML line break (<br>)。

###  remove/remove_firs

从一个字符串中移除所有或第一个子串。

###  replace/replace_first

将字符串中的一个子串替换为第二个子串。例如：
	
	{{ "Take my protein pills and put my helmet on" | replace: "my", "your" }}

###  reverse

反转数组中的所有项，但不能用于字符串。例如：


	{% assign my_array = "apples, oranges, peaches, plums" | split: ", " %}
	{{ my_array | reverse | join: ", " }}


可以把字符串分解成数组再反转。例如：

	{{ "Ground control to Major Tom." | split: "" | reverse | join: "" }}

###  round

将一个数近似到最近的整数，如果指定参数，则近似到指定的小数点位数。

###  size

返回字符串中的字符数或数组中的项目数。size 可以用于点号表达式，如 `{{ my_string.size }}`。

###  slice

返回字符串的子串，第一个参数指定位置，第二个可选参数指定长度。如果第一个参数为负，则从尾部开始。

###  sort/sort_natural

对数组中的元素排序。排序对大小写敏感或不敏感。

###  strip_html

移除字符串中的 HTML 标签。

###  strip_newlines

移除字符串中的换行符 newline characters (line breaks) 。

###  truncate/truncatewords

截取字符串到指定长度的字符或单词，结尾加省略号，ellipsis (…) ，并包含在字符数中。例如：

	{{ "Ground control to Major Tom." | truncate: 20 }}

可以使用第二个可选参数，指定字符串后的省略语，或指定空字符串，不加省略语。

###  uniq

移除数组中所有的重复项。

###  url_decode/url_encode

解码或编码URL 字符串。