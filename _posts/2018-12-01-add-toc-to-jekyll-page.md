---
layout: post
title: 为Jekyll页面添加目录
---

# 为Jekyll页面添加目录

当页面很长时，如果没有目录，阅读起来会不方便。目录可以放在页面的文档中，也可以放在页面的侧边栏。放在侧边栏的目录不会随页面移动，而且可以实现随页面一起滚动的效果，对阅读更加方便。

## 在文档中放置目录

默认的 Github pages 的 Jekyll 使用 Kramdown 解析 Markdown。Kramdown 默认的自动为标题生成ID，并根据标题ID自动生成目录，所以几乎不需要做任何事就可以自动生成目录。

kramdown 支持为全部有ID的标题自动生成目录，只需使用IAL为一个有序或无序列表指定参考名：`toc`，这个列表将会被目录取代，使用有序列表或无序列表的形式。如果没有设置ID，则会自动获得ID：`markdown-toc`。 

如果设置了`auto_ids`选项，所有的标题都会获得一个自动ID，所以都出现在目录中。如果不想将其列入目录，为其分配一个对象名`no_toc`。例如：

无序列表目录：

	# Contents header
	{:.no_toc}

	* A markdown unordered list which will be replaced with the ToC, excluding the "Contents header" from above
	{:toc}

	# H1 header

	## H2 header

有序列表目录：

	1. The generated Toc will be an ordered list
	{:toc}

	# H1 header

	## H2 header

其中，行内属性列表（Inline Attribute Lists, IAL）是kramdown的一个元素，用来为另一个元素附加属性。包括块行内属性列表（Block Inline Attribute Lists）和行内行内属性列表（Span Inline Attribute Lists）。前者为一个块级元素附加属性，使用一个分号，紧靠着块级元素之前或之后。例如：

	A simple paragraph with an ID attribute.
	{: #para-one}

	> A blockquote with a title
	{:title="The blockquote title"}
	{: #myid}

	{:.ruby}
		Some code here

后者为一个行内级元素附加属性，与前者格式相同，但前后不允许有空格，必须放在行内级元素后面。例如：

	This *is*{:.underline} some `code`{:#id}{:.class}.
	A [link](test.html){:rel='something'} and some **tools**{:.tools}.

kramdown默认的自动生成目录的级别为1-6级，可以在 `_config.yml` 中对其设置选项 `toc_levels` 进行改变，这个选项可以使用逗号分隔的级别（如：1,2,3），也可以使用级别范围（如：1..6）。例如：

	markdown:  kramdown
	kramdown:
	  toc_levels:  1..3


##  在侧边栏放置目录

我要的效果是在页面的右侧边栏放置文章目录，目录不动，但能自动跟随页面滚动高亮显示当前的位置。

目前时间研究这个问题，后面在研究吧。

[这个](https://creeperdance.github.io/2017/05/jekyll-catalog.html) ，使用Kramdown生成的目录，在右边栏跟随显示

[这个](https://github.com/simpleyyt/jekyll-theme-next)主题效果不错

还有[这个](https://github.com/Simpleyyt/jekyll-jacman)

[这个](https://wklchris.github.io/Advanced-blog-skills.html) 用CSS在文章页面右侧开辟一个侧边栏，使用JavaScript生成文章目录


