---
layout: post
title:  使用Markdown
categories: [Markdown]
tags: [Markdown]
---


* Content
{:toc}

## 关于Markdown

Markdown 是一个文本到 HTML 的转换工具，可以将易读易写的文本转换成格式化的 XHTML 或 HTML。所以，Markdown 包括两个意思：一是文本格式语法；二是将文本转换到 HTML 的软件工具。

Markdown的官网地址：<https://daringfireball.net/projects/markdown/>



## Markdown主要语法

### 总览

#### 设计哲学

Markdown 设计的目标就是尽可能易读易写，而且易读性放在首位。它使用纯文本格式，不使用标签或格式化结构。

为实现这一目标，Markdown 的语法全部由标点符号组成，并且经过仔细选择，使这些标点符号基本保留了它们的意义，能够和文本融为一体。 

#### 内联HTML

Markdown 不是要取代 HTML，它的语法只对应了 HTML 标签很小的一个子集。HTML 是一种发布格式，Markdown 是一种写作格式。Markdown 的语法只是解决能够有纯文本可以表达的问题。

对于任何 Markdown 语法无法覆盖的标记，尽管使用 HTML 标签而无需声明。

唯一的限制是，块级的 HTML 元素必须与周围的内容用空行隔开，例如：`<div>, <table>, <pre>, <p>` 等，而且块标签的开始和结束不能用空格或制表符缩进。 

行内级的 HTML 标签可以在Markdown段落、列表项或头部使用，例如：` <span>, <cite>, <del>` ，甚至可以使用 HTML 标签代替 Markdown 语法。

注意，Markdown 语法在 HTML 块级的标签中不进行处理，但在行内级标签中被处理。

#### 特殊字符


在 HTML 中，有两个字符需要特殊处理：`<, &`。Left angle brackets表示标签的开始，ampersands表示HTML实体。如果要用作普通字符，需要作为实体处理，如 `&lt;, &amp;`。

Markdown 中可以自然地使用这些字符，解释器会自动处理。如果在 HTML 实体中使用&，会保持不变，否则会自动解释为 `&amp;`。比如文本中使用版权标志，写作：`&copy;`，Markdown 解释器会保持不变，但文本中的AT&T，Markdown 会解释为：`AT&amp;T`。

同样，由于Markdown 支持内联 HTML，如果将 < 用作 HTML 标签，Markdown会处理成HTML标签。但如果写作：4 < 5，Markdown 会解释成`4 &lt; 5`。

### 块元素

#### 段落和换行

段落就是被空行隔开的连续的文本行。普通段落不能被空格或制表符缩进。

如果想插入换行标签： `<br />` ，可以在行后加一个或多个空格，然后回车。

#### 标题

Markdown 支持两种标题样式：Setext 和 atx。

Setext样式的标题使用任意数量的=和-作为下划线表示一级和二级标题，例如：

	This is an H1
	=============

	This is an H2
	-------------

Atx样式的标题在行首使用1到6个#字符表示1到6级标题。例如：

	# This is an H1

	## This is an H2

	###### This is an H6

或者，可以使用#字符包围atx样式的标题，但标题后的#数量没必要与行前的数量一致。例如：

	# This is an H1 #

	## This is an H2 ##

	### This is an H3 ######

#### 块引用

Markdown 在使用 email 样式的 > 字符表示块引用。在每行之前加一个 > 最好，例如：

	> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
	> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
	> Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
	> 
	> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
	> id sem consectetuer libero luctus adipiscing.

Markdown 也允许只在段落的第一行前加一个 > 字符，例如：

	> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
	consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
	Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.

	> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
	id sem consectetuer libero luctus adipiscing.

块引用也可以嵌套，只需增加多级 > 字符，例如：

	> This is the first level of quoting.
	>
	> > This is nested blockquote.
	>
	> Back to the first level.

块引用中可以包含其他 Markdown 元素，如标题、列表、代码块等。例如：elements, including headers, lists, and code blocks:

	> ## This is a header.
	> 
	> 1.   This is the first list item.
	> 2.   This is the second list item.
	> 
	> Here's some example code:
	> 
	>     return shell_exec("echo $input | $markdown_script");


#### 列表

Markdown 支持有序列表和无序列表。

无序列表使用`*, +, -`作为列表符，可以互换使用。例如：

	*   Red
	*   Green
	*   Blue

与下列写法相同：

	+   Red
	+   Green
	+   Blue

和

	-   Red
	-   Green
	-   Blue

有序列表使用数字加句号，但数字并不决定列表的顺序。例如：

	1.  Bird
	2.  McHale
	3.  Parish

列表标示符做多可以有三个空格的缩进，其后必须至少有一个空格或制表符。

为了美观，列表项可以使用悬挂缩进，例如：

	*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
		Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
		viverra nec, fringilla in, laoreet vitae, risus.
	*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
		Suspendisse id sem consectetuer libero luctus adipiscing.

但不能这样：

	*   Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
	Aliquam hendrerit mi posuere lectus. Vestibulum enim wisi,
	viverra nec, fringilla in, laoreet vitae, risus.
	*   Donec sit amet nisl. Aliquam semper ipsum sit amet velit.
	Suspendisse id sem consectetuer libero luctus adipiscing.

如果列表项被空行隔开，Markdown 会在HTML输出的列表项上<p> 标签，例如：

	*   Bird

	*   Magic

会输出：

	<ul>
	<li><p>Bird</p></li>
	<li><p>Magic</p></li>
	</ul>

列表项中可以包含多个段落，每个段落必须使用四个空格或一个制表符缩进，每行缩进最好，也可以只缩进第一行，例如：

	1.  This is a list item with two paragraphs. Lorem ipsum dolor
		sit amet, consectetuer adipiscing elit. Aliquam hendrerit
		mi posuere lectus.

		Vestibulum enim wisi, viverra nec, fringilla in, laoreet
		vitae, risus. Donec sit amet nisl. Aliquam semper ipsum
		sit amet velit.

	2.  Suspendisse id sem consectetuer libero luctus adipiscing.

在列表项中使用块引用，限定符 > 需要缩进，例如：

	*   A list item with a blockquote:

		> This is a blockquote
		> inside a list item.

在列表项使用代码块，代码块需要双倍缩进（8个空格或2个制表符），例如：

	*   A list item with a code block:

			<code goes here>

为了避免行首的 number-period-space 序列触发有序列表，使用反斜杠转义句号，例如：

	1986\. What a great season.

#### 代码块

Markdown 使用 <pre> 和 <code> 包裹代码块。

在 Markdown 中插入代码块，只需将代码块每一行缩进至少4个空格或1个制表符，如下输入：

	This is a normal paragraph:

		This is a code block.

会产生如下输出：

	<p>This is a normal paragraph:</p>

	<pre><code>This is a code block.
	</code></pre>

代码块中的ampersands (&) 和 angle brackets (< and >) 会自动转换成 HTML 实体，这样可以方便地在Markdown中处理 HTML 源码，只需复制并缩进。例如：

    <div class="footer">
        &copy; 2004 Foo Corporation
    </div>

会输出为：

	<pre><code>&lt;div class="footer"&gt;
		&amp;copy; 2004 Foo Corporation
	&lt;/div&gt;
	</code></pre>

普通的Markdown语法不会在代码块中处理，这样也方便在Markdown书写Markdown自己的语法。

GitHub 也支持称为 code fencing 的代码块样式，使用三个斜点包围代码块，允许多行代码块不需要缩进。例如：

```
if (isAwesome){
  return true
}
```

如果想要进行语法高亮显示，需要在三个斜点后指定语言。例如：

```javascript
if (isAwesome){
  return true
}
```

####  水平分割线

要产生一个水平分割线(`<hr />`) ，只需在一行中使用至少三个-`, *, _`，甚至可以在`-, *`中间加空格。例如：

	* * *

	***

	*****

	- - -

	---------------------------------------

### 行内元素

#### 链接

Markdown 支持两种链接样式：行内链接和参考。两种样式都使用 [方括号]作为链接文本的限定符。

创建行内链接，使用圆括号紧跟链接文本的方括号，圆括号中是链接的 URL 和可选的标题（加引号），例如：

	This is [an example](http://example.com/ "Title") inline link.

	[This link](http://example.net/) has no title attribute.

会输出：

	<p>This is <a href="http://example.com/" title="Title">
	an example</a> inline link.</p>

	<p><a href="http://example.net/">This link</a> has no
	title attribute.</p>

如果参考同一服务器上的本地资源，可以使用相对路径。例如：

	See my [About](/about/) page for details.   

参考样式的链接使用第二个方括号，其中放置链接标识符，两个方括号可以间隔一个空格，例如：

	This is [an example] [id] reference-style link.

然后在文本的其他地方，在独立的行中定义链接标识符。例如：

	[id]: http://example.com/  "Optional Title Here"

也就是说：

*    方括号中是链接标识符，可以最多缩进3个空格，标示符可以使用字母、数字、空格和标点符号，并且对大小写不敏感；
*    紧跟一个分号；
*    紧跟至少一个空格或制表符；
*    紧跟链接的URL，可以使用尖括号包裹；
*    可选的跟随一个标题属性，使用双引号、单引号或圆括号包裹，对上URL，可以另起一行，并使用空格或制表符缩进。

隐含的链接标识符允许将链接文本作为标识符，后面使用空的方括号。例如：

	Visit [Daring Fireball][] for more information.

在其他地方定义链接：

	[Daring Fireball]: http://daringfireball.net/

使用参考样式的链接不是为了易写，主要是为了易读，使源文档看起来更像浏览器的输出，将链接定义移出段落，增加链接也不会破坏段落的叙述结构。

#### 锚点和页内链接

Markdown会自动给每一个标题生成一个锚点，其id就是标题内容。这个自动生成的id满足以下条件：

* 全部变成小写
* 只保留字母、数字、短横线
* 空格变成短横线
* 如果不唯一，添加"-1", "-2", "-3"等

也可以手动在页面中添加锚点。使用标签`<span>, <a>`等，如

    <a id="anchor">锚点</a>

要实现页内链接，则可以使用由标题自动生成的锚点或自定义锚点，如：

    [页内链接](#id) 

#### 插入图片

插图的格式与链接的格式非常类似，只是在前面加一个感叹号：

    ![Alt text](图片链接 "optional title")

其中，Alt text：是图片的Alt标签，用来描述图片的关键词，可以不写。图片链接：可以是图片的本地地址或者是网址。"optional title"：鼠标悬置于图片上会出现的标题文字，可以不写。

如果插入本地图片，只需要在图片链接中填入图片的位置路径即可，支持绝对路径和相对路径。例如：

    ![avatar](/home/picture/1.png)

如果插入网络图片，只需要在图片链接中填入图片的网络链接即可。例如：

    ![avatar](http://baidu.com/pic/doge.png)

#### 强调重点

Markdown 将 星号 (\*) 和下划线 (\_) 作为强调符号，两种样式效果相同。使用一个 * 或 _ 包裹文本会输出 HTML `<em>` 标签；; 两个 \* 或 \_ 包裹文本会输出 HTML `<strong>` 标签。例如：

	*single asterisks*

	_single underscores_

	**double asterisks**

	__double underscores__

会输出：

	<em>single asterisks</em>

	<em>single underscores</em>

	<strong>double asterisks</strong>

	<strong>double underscores</strong>

如果要在文本中显示星号和下划线，可以使用反斜杠进行转义，例如：

	\*this text is surrounded by literal asterisks\*

#### 代码

在行内显示代码，使用斜点号包裹(\`)，与格式化代码块不同，行内代码在普通段落中显示。例如：

	Use the `printf()` function.

会输出：

	<p>Use the <code>printf()</code> function.</p>

要在行内代码中显示斜点号，使用双斜点号包裹代码。例如：

	``There is a literal backtick (`) here.``

会输出：

	<p><code>There is a literal backtick (`) here.</code></p>

在斜点限定符内添加空格，可以在代码块的开始或结束显示斜点号。例如：

	A single backtick in a code span: `` ` ``

	A backtick-delimited string in a code span: `` `foo` ``

会输出：

	<p>A single backtick in a code span: <code>`</code></p>

	<p>A backtick-delimited string in a code span: <code>`foo`</code></p>

在行内代码中的和号和尖括号会自动编码成 HTML 实体，这样可以在行内代码中包含 HTML 标签。例如：

	Please don't use any `<blink>` tags.

Markdown会输出：

	<p>Please don't use any <code>&lt;blink&gt;</code> tags.</p>

你可以这样写：

	`&#8212;` is the decimal-encoded equivalent of `&mdash;`.

Markdown会输出：

	<p><code>&amp;#8212;</code> is the decimal-encoded
	equivalent of <code>&amp;mdash;</code>.</p>

#### 图像


说实话，设计一种在纯文本中插入图像的自然的语法确实比较困难。Markdown 使用了类似链接的语法（加一个感叹号），有两种样式：行内样式和参考样式。

行内样式图像语法就像这样：

	![Alt text](/path/to/img.jpg)

	![Alt text](/path/to/img.jpg "Optional title")

也就是：

*    一个感叹号开头 !；
*    紧跟一对方括号，包含图像的说明文本；, containing the alt attribute text for the image;
*    紧跟一对圆括号，包含图像的 URL 或路径，含有可选的标题属性（加单引号或双引号）。

参考样式图形语法像这样，其图像参考标识符的定义与链接参考一样：

	![Alt text][id]

Markdown 没有语法指定图像的尺寸，需要这样做的话，可以使用 HTML 的 `<img>` 标签。

### 其他语法

#### 自动链接

Markdown 支持自动生成 URLs 和 email 的链接，只需在 URL 或 email地址上加尖括号就可以，这样在既要显示 URL 或 email 地址，又要生成链接的地方比较方便。自动的 email 地址链接生成方式相似，只是 Markdown 会做一些随机编码，来模糊email地址，以防止垃圾邮件。例如：

	<http://example.com/>

	<address@example.com>

Markdown会输出：

	<a href="http://example.com/">http://example.com/</a>

	<a href="&#x6D;&#x61;i&#x6C;&#x74;&#x6F;:&#x61;&#x64;&#x64;&#x72;&#x65;
	&#115;&#115;&#64;&#101;&#120;&#x61;&#109;&#x70;&#x6C;e&#x2E;&#99;&#111;
	&#109;">&#x61;&#x64;&#x64;&#x72;&#x65;&#115;&#115;&#64;&#101;&#120;&#x61;
	&#109;&#x70;&#x6C;e&#x2E;&#99;&#111;&#109;</a>

#### 反斜杠转义

如果需要在文本中显示Markdown语法用到的特殊字符，可以使用反斜杠转义字符。Markdown 为以下字符提供转义:

	\   backslash
	`   backtick
	*   asterisk
	_   underscore
	{}  curly braces
	[]  square brackets
	()  parentheses
	#   hash mark
	+   plus sign
	-   minus sign (hyphen)
	.   dot
	!   exclamation mark

##  GitHub扩展

GitHub在官方规范基础上进行了扩展，定义了其自己的规范，称为[GitHub Flavored Markdown Spec](https://github.github.com/gfm/)。

## 更新记录

  * 2018-11-10：首次发布；
  * 2020-01-15：增加插入图片内容；
  * 2020-05-06：增加锚点和页内链接；