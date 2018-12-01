---
layout: post
title: 使用MathJax显示数学公式
---

# 使用MathJax显示数学公式

## 关于MathJax

MathJax 是个一个数学公式显示引擎，使用JavaScript把 LaTeX、MathML、AsciiMath 标记的数学公式转换成 HTML、 SVG、 MathML 显示在浏览器上。

MathJax的官网地址：<https://www.mathjax.org/>

## 使用MathJax

有两种获取并使用 MathJax 的途径：一是使用分布式网络服务；一是拷贝一份到服务器或本地硬盘。

### 从Content Delivery Network (CDN)载入MathJax

使用 MathJax 最简单的方法就是使用通过CDN直接获取公共的安装版本。通过CDN使用MathJax只需两步：

1.  在包含数学公式的网页链接到 MathJax
2.  在网页中输入数学公式

有许多免费的 CDN 提供了 MathJax。

-   cdnjs.com （推荐）
-   jsdelivr.com [rolling]
-   unpkg.com [rolling]
-   rawgit.com
-   gitcdn.xyz
-   raw.githack.com

如果使用 cdnjs，第一步需要做的就是在网页的`<head>`包含下列代码。

	<script type="text/javascript" async
	  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/latest.js?config=TeX-MML-AM_CHTML">
	</script>

这个配置会载入最新版 MathJax，识别 TeX/LaTeX、 MathML、AsciiMath 标记的数学公式，然后使用带CSS的HTML 显示数学公式。

### 安装自己的 MathJax 

在自己的服务器或本地硬盘安装 MathJax ，需要做下列事情：

1.   获取一份 MathJax 并放在自己的服务器或本地硬盘上
2.   根据需要配置 MathJax 
3.   在包含数学公式的网页链接到 MathJax 
4.   在网页中输入数学公式


MathJax 的源码在[GitHub](https://github.com/mathjax/MathJax/)。下载最新版的 MathJax ，解压并放在适当的位置，以便可以链接到其他网页。 比如，放在根目录，这样可以通过 URL `/MathJax/MathJax.js`引用 MathJax。 安装完后，可以使用目录 `MathJax/test` 下的文件测试 MathJax 。

默认情况下会载入配置文件 `config/TeX-MML-AM_CHTML.js` 。这个配置文件载入最常用的 MathJax组件，允许输入TeX/LaTeX 、 MathML、AsciiMath 标记的数学公式，输出带 带CSS的HTML 来显示数学公式。

在网页的`<head>` 中包含下列代码，将MathJax链接到网页，其中`path-to-MathJax`要与放置 MathJax 文件夹匹配。

	<script type="text/javascript" async 
	    src="path-to-MathJax/MathJax.js?config=TeX-MML-AM_CHTML">
	</script>

### 配置 MathJax

有两种配置方式：使用配置文件或在网页中包含配置命令。两种方式可以独立使用，也可以混合使用，但必须使用一种。

#### 使用配置文件配置 MathJax

在 `MathJax/config` 下包含很多预编译的配置文件，其中，配置文件`default.js` 包含了几乎全部的配置选项，可以使用这个文件定制自己的配置。只需包含以下代码：

	<script type="text/javascript" src="path-to-MathJax/MathJax.js?config=default"></script>

预编译的配置文件载入特定的预处理器、输入处理器、输出处理器和扩展，包括：

-    TeX-MML-AM_CHTML
-    TeX-MML-AM_HTMLorMML
-    TeX-MML-AM_SVG
-    TeX-AMS-MML_HTMLorMML
-    TeX-AMS-MML_SVG
-    TeX-AMS_CHTML
-    TeX-AMS_HTML
-    TeX-AMS_SVG
-    MML_CHTML
-    MML_SVG
-    MML_HTMLorMML
-    AM_CHTML
-    AM_SVG
-    AM_HTMLorMML

每个联合配置文件都包含两个版本，一个是标准版，一个是完全版。标准版只定义输出处理器，不载入实现代码。完全版会载入全部代码。一般在网页中包含大量公式的地方使用完全版，在配置文件名后加`-full`。在博客或Wiki等使用主题载入MathJax的地方，只是偶尔使用公式，一般使用标准版。

#### 使用行内命令配置MathJax

使用行内命令配置MathJax，需要两个独立的 `<script>` 标签，一个指定配置选项，一个载入 MathJax。而且由于 MathJax 载入后马上开始配置过程，所以配置脚本必须放在载入脚本前。

使用行内配置，使用一个带 `type="text/x-mathjax-config"` 的 `<script>`标签，其中包含一个 `MathJax.Hub.Config()` 调用，来执行 MathJax 配置，也可以包含其他 MathJax 命令。也可以有多个这样的标签，MathJax 会顺序执行他们。例如：

	<script type="text/x-mathjax-config">
	  MathJax.Hub.Config({
		extensions: ["tex2jax.js"],
		jax: ["input/TeX", "output/HTML-CSS"],
		tex2jax: {
		  inlineMath: [ ['$','$'], ["\\(","\\)"] ],
		  displayMath: [ ['$$','$$'], ["\\[","\\]"] ],
		  processEscapes: true
		},
		"HTML-CSS": { fonts: ["TeX"] }
	  });
	</script>
	<script type="text/javascript" src="path-to-MathJax/MathJax.js">
	</script>

这个实例没有载入预定义的配置文件，实际上，可以混合使用配置文件和行内配置，只需在载入MathJax.js时使用`config=filename`指定配置文件。例如，tex2jax 预处理器不允许使用`single-dollar`行内公式界定符，但可以在行内配置中打开这个选项。 

	<script type="text/x-mathjax-config">
	  MathJax.Hub.Config({
		tex2jax: {
		  inlineMath: [ ['$','$'], ["\\(","\\)"] ],
		  processEscapes: true
		}
	  });
	</script>
	<script type="text/javascript" src="path-to-MathJax/MathJax.js?config=TeX-AMS_HTML">
	</script>


### 在页面中输入数学公式

根据配置不同，可以使用 TeX/LaTeX、MathML、 AsciiMath 三种标识语言单独或混合输入公式。 

#### 使用 TeX 和 LaTeX 输入公式

有两种类型的公式：一种在段落中，称为行内公式(in-line mathematics)，一种是单独成行的大型公式，称为行间公式(displayed mathematics)。

默认情况下的公式界定符，行间公式使用 `$$...$$` 和 `\[...\]`，行内公式使用 `\(...\)` 。行内公式界定符 `$...$` 默认不启用，这是因为美元符号在一般文本中太常见了，容易引起错误。如果想启用这个界定符，必须进行显式地配置。

	<script type="text/x-mathjax-config">
		MathJax.Hub.Config({
		  tex2jax: {
			inlineMath: [['$','$'], ['\\(','\\)']],
			processEscapes: true
		  }
		});
	</script>

对 TeX、LaTeX 包括两部分：tex2jax 预处理器和 TeX 输入处理器。前者标识文本中的数学公式，后者将输入公式转换成一种中间格式，供输出处理器显示。 

请记住，数学公式是在HTML 文档中，浏览器会先于MathJax解释文档，所以公式中避免使用 HTML 的特殊字符，如`<  >  &` 等符号对浏览器来说有特殊的含义。一般来说在这些符号前后加空格就可以避免浏览器解释他们，如

	... when $x < y$ we have ...

或者可以使用 HTML 实体 `&lt;  &gt;  &amp;` 对这些符号进行编码，这样浏览器就不会解释他们，但 MathJax 可以识别。如

	... when $x &lt; y$ we have ...

或者可以使用TeX格式的宏 `\lt  \gt` 来输入 < 和 > 符号，如

... when $x \lt y$ we have ...

另外，在一些内容管理系统，如博客或Wiki中，会有自己的文档处理器，会在生成 HTML 页面前处理文档。例如在 Markdown 中，下划线（underscore）标识斜体字，而在 MathJax 中表示下标。由于 Markdown 会先处理文档，会把下划线转换成 `<i> or <em>` 标签，这样 MathJax 就无法识别了。这需要修改内容管理系统使其不去处理公式界定符内的符号，或许系统已经包含了这样的插件。

大部分内容管理系统都有原文本处理标识，以在文本中插入代码，可以使用这个标识来避免公式被处理。例如 Markdown 中，可以使用斜点符包围公式。例如：

	... we have `\(x_1 = 132\)` and `\(x_2 = 370\)` and so ...

一些内容管理系统使用反斜杠作为转义字符，但 TeX 使用反斜杠标识宏。在这样的系统中，需要使用双反斜杠来输出单反斜杠，如果要输出双反斜杠，需要使用四个。例如：

	\\begin{array}{cc}
	  a & b \\\\
	  c & c
	\\end{array}

如果启用了单美元符作为行内公式的界定符，需要使用反斜杠转义来避免被 MathJax 处理成公式界定符，在文本中显示美元符。或者使用 `<span>$</span>` 此类的标签隔离美元符。

#### 使用 MathML 输入公式

使用 MathML 输入数学公式时，`<math display="block"> 表示行间公式， `<math display="inline">` 或只是 `<math>` 标识行内公式。*输入太冗长，不适合人类使用！*


#### 使用 AsciiMath 输入公式

使用 AsciiMath 输入数学公式时，使用斜点符标识公式，如 \`...\`。

	<p>When `a != 0`, there are two solutions to `ax^2 + bx + c = 0` and
	they are</p>
	<p style="text-align:center">
	  `x = (-b +- sqrt(b^2-4ac))/(2a) .`
	</p>

##  在Jekyll中使用MathJax

在 Jekyll 站点根目录下的 `_includes/head.html` 文件中的 `<head>` 和 `<\head>` 标签之间添加以下代码：

	<!-- MathJax Support -->
	<script type="text/x-mathjax-config">
		MathJax.Hub.Config({
		  tex2jax: {
			inlineMath: [['$','$'], ['\\(','\\)']],
			processEscapes: true
		  }
		});
	</script>
	<script type="text/javascript" async
	  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-AMS_HTML">
	</script>

以上代码表示使用cdnjs.com载入MathJax，使用TeX-AMS_HTML组合配置文件。这个配置文件表示只使用 TeX 格式输入公式，使用HTML-CSS输出公式，其输出与 TeX 输出尽可能接近。使用行内配置命令启用了行内公式界定符 `$...$`，并启用了反斜杠转义，如果需要输入\$时，需要使用反斜杠转义。

##  一些公式实例

###  一元二次方程

当 $a \ne 0$, 方程 \(ax^2 + bx + c = 0\) 有两个根：

$$x = {-b \pm \sqrt{b^2-4ac} \over 2a}.$$
