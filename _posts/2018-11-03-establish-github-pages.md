---
layout: post
title:  在Github上建立用户主页
categories: [Github]
tags: [GitHub, Jekyll]
---


* Content
{:toc}

## 在Github上建立主页

Github提供静态主页Github Pages，包括用户主页和项目主页。其中用户主页，每个用户只能建立一个，项目主页可以为每个项目建立一个。



### 建立用户主页

1. 在Github账户下建立一个项目：username.github.io，同时也是主页地址；
2. 克隆这个项目，添加你的页面，推送到项目库；
3. 通过 http://username.github.io/ 访问用户主页。

### 建立项目主页

1. 修改项目库设置，在项目主页下选择源文件位置或选择主题；
2. 在项目中添加页面；
3. 通过 http://username.github.io/repository 访问项目主页。

##  为主页添加主题

Github Pages支持HTML页面和静态文件，也支持Jekyll（一种静态页面生成引擎）生成静态页面。可以为主页使用Jekyll主题。

*  手动修改网站的 _config.yml 文件，添加 GitHub Pages 官方支持的主题，或Github上开源的Jekyll主题。
*  使用 Jekyll 主题选择器添加或修改网站主题。

## 绑定用户域名

1. 在根目录下新建CNAME文件，写明了这个站点的域名；
2. 在域名管理后台修改域名A记录，指向下来IP：

	*    185.199.108.153
	*    185.199.109.153
	*    185.199.110.153
	*    185.199.111.153

3. 建立www子域名的CNAME记录，指向主页地址。

## 添加网站访问统计

使用 [百度统计](https://tongji.baidu.com) 建立网站访问统计，了解主页的访问情况。

1. 在百度统计上添加网站域名和主页，复制跟踪代码。
2. 打开 _config.yml 配置文件中的百度统计项。
3. 更新 head.html 中百度统计跟踪代码。注意：须把<script>改为<script type="text/javascript">。

## 更新记录

  * 2018-11-03：首次发布；
  * 2020-03-08：添加网站访问统计
  