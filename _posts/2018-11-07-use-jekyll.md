---
layout: post
title:  使用Jekyll 
---

# 使用Jekyll

## 关于Jekyll

Jekyll 是一个静态网站生成引擎，使用文本文件生成静态网站或博客。GitHub Pages 集成了 Jekyll，可以利用 Jekyll 的模板快速生成一个网站。

Jekyll的官网地址：<https://jekyllrb.com/>。

## Jekyll使用步骤

这里的步骤不包括使用本地服务器进行展示。

### 使用Liquid建立模板

[Liquid](https://shopify.github.io/liquid/) 是一个模板语言，Jekyll使用Liquid定义模板。 Liquid 语言包含三个主要的元素：对象、标签和过滤器。

### 使用Front Matter定义页面属性

Jekyll页面由两部分组成，一是页面属性定义，一是页面内容。Front matter 是一小段 YAML，用来定义页面变量。在文件头部，使用两个“三短划线”来标识。例如：

	---
	title: Home
	---
  
Front matter 所使用的变量在 Liquid 的 page 变量下。

##   创建布局

Jekyll页面支持 [Markdown](https://daringfireball.net/projects/markdown/syntax) 和 HTML。Markdown 比HTML结构简单，更适合静态页面。

可以为不同的页面使用不同的布局，布局就是一种页面模板，可以应用于所有的页面，布局文件放在目录：`_layouts`。

例如创建一个布局`_layouts/default.html`：

{% raw %}
	<!doctype html>
	<html>
	  <head>
		<meta charset="utf-8">
		<title>{{ page.title }}</title>
	  </head>
	  <body>
		{{ content }}
	  </body>
	</html>
{% endraw %}

注意：这个布局文件与一般的HTML文件结构相似，但没有Front Matter，且使用了Liquid变量。

使用这个布局，只需在Front Matter中设置`layout`变量，如`about.md`：

	---
	layout: default
	title: About
	---
	# About page

	This page tells you a little bit about me.

##  使用Include标签

`include` 标签可以在当前页面中包含保存在目录 `_includes` 中的页面，这一特性类似于编程语言中的包含或导入，对重复使用的内容非常有用。例如导航栏：

在目录`_includes/navigation.html` 创建一个导航文件：

	<nav>
	  <a href="/">Home</a>
	  <a href="/about.html">About</a>
	</nav>

将导航栏加到布局文件中 `_layouts/default.html`：

{% raw %}
	<!doctype html>
	<html>
	  <head>
		<meta charset="utf-8">
		<title>{{ page.title }}</title>
	  </head>
	  <body>
		{% include navigation.html %}
		{{ content }}
	  </body>
	</html>
{% endraw %}

##  使用Data文件

Jekyll 支持从目录 `_data`的YAML, JSON, 和 CSV 文件加载数据，使用数据文件有利于将内容与源码隔离开，以便于维护后期维护。例如把导航栏内容放在数据文件中，然后在导航包含中循环调用。

建立数据文件 `_data/navigation.yml` ：

	- name: Home
	  link: /
	- name: About
	  link: /about.html

Jekyll 可以在 `site.data.navigation` 调用这个数据文件，这样可以在`_includes/navigation.html`循环调用数据文件。

{% raw %}
	<nav>
	  {% for item in site.data.navigation %}
		<a href="{{ item.link }}" {% if page.url == item.link %}style="color: red;"{% endif %}>
		  {{ item.name }}
		</a>
	  {% endfor %}
	</nav>
{% endraw %}

##  资源的使用

在Jekyll中可以直接使用CSS, JS, images 等资源。将它们放在网站目录中，就可以在全站中使用。一般的目录结构：

	.
	├── assets
	|   ├── css
	|   ├── images
	|   └── js
	...

## 建立博客

Y博客发布放在目录 `_posts`中，其命名规则为：发布日期，题目，扩展，空格用短划线。如 `_posts/2018-08-20-bananas.md` 。


一般博客有一个发布文章的列表页。Jekyll 使用`site.posts`访问所有的博客发布。在根目录建立`/blog.html`：

{% raw %}
	---
	layout: default
	title: Blog
	---
	<h1>Latest Posts</h1>

	<ul>
	  {% for post in site.posts %}
		<li>
		  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
		  <p>{{ post.excerpt }}</p>
		</li>
	  {% endfor %}
	</ul>
{% endraw %}