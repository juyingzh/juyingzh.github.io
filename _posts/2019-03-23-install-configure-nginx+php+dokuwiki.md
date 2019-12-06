---
layout: post
title: nginx+php+dokuwiki安装与配置
categories: [网站]
tags: [nginx, PHP, dokuwiki]
---


* Content
{:toc}

在VPS上要安装dokuwiki，这个wiki引擎使用PHP做后端语言，文本文件存储信息，不需要数据库，所以安装配置一个Nginx+PHP服务器环境。以下过程基于Debian 9 x86 （Stretch）系统环境。



## nginx安装与配置

[nginx](http://nginx.org) \[engine x\] 是一个 HTTP ，反向代理，邮件代理，通用 TCP/UDP 代理服务器，最初由 Igor Sysoev 设计实现。可以配置为静态网页服务器，代理服务器，或连接 FastCGI 提供动态网页服务。

nginx 有一个主进程和若干工作进程。主进程的主要作用是读取和评估配置信息，维护工作进程。工作进程处理对请求的响应。nginx 使用基于事件的模型和依赖OS的工作机制，在多个工作进程间分配请求。工作进程的数量在配置文件中定义，可以固定为某个值，也可以根据可用CPU核进行自动调整。

### nginx安装

使用Debian软件库中的预编译包进行安装。

	apt-get update
	apt-get install nginx

### nginx启动与停止

启动nginx，直接运行nginx。启动后，可以使用带-s参数控制nginx的运行。

    nginx -s signal

其中 signal可以为以几个选项：

	* stop — 快速关闭
	* quit — 优雅地关闭（等待工作进程结束当前请求的响应）
	* reload — 重新载入配置文件（修改配置文件后需要重新载入）
	* reopen — 重新打开日志文件

查看所以启动的nginx进程：

    ps -ax | grep nginx
  
### nginx配置

nginx 及其模块的工作方式完全由配置文件决定。缺省的配置文件名为 nginx.conf ，默认位置在 /usr/local/nginx/conf，/etc/nginx，或 /usr/local/etc/nginx 下。

#### 配置文件结构

nginx由模块组成，模块由配置文件中的指令控制。指令分为简单指令和块指令。一条简单指令由指令名和参数构成，以空格分隔，以分号结尾；块指令的结构与简单指令相同，但不是以分号结尾，而是用花括号括起来的一系列命令。如果块指令可以包含其他指令，则称为上下文，如 events, http, server, location。#符号后面的内容为注释行。

配置文件中的顶层指令称为主上下文，events 和 http 包含在主上下文中，server 在 http中，location在 server中。基本结构为：

	events{
	}
	http{
	    server{
		    location{
			}
		}
	}

#### 具体配置

本系统安装nginx后，其配置文件在 /etc/nginx 下。不需要对此配置文件进行修改。

配置文件中创建了一个用户www-data，用于后台服务器进程。

	user www-data;

在http环境中定义了虚拟主机：

	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;

在/etc/nginx/sites-enabled/下是一个连接文件，指向/etc/nginx/sites-available/下的default配置文件，即默认的虚拟主机配置。

准备把dokuwiki放在网站的根目录，所以修改根目录的位置，指向dokuwiki的位置。

	root /var/www/dokuwiki;

修改主页的文件名，删除index.nginx-debian.html，添加index.php：

	index index.html index.htm index.php;

修改 FastCGI 代理服务器设置，使用php-fpm进程管理器，将.php文件传递给PHP解释：

	location ~ \.php$ {
		include snippets/fastcgi-php.conf;
	
		# With php-fpm (or other unix sockets):
		fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
	}

设置禁止访问目录：

	location ~ /(data|conf|bin|inc|vendor)/ {
      deny all;
    }

## PHP安装与配置

[PHP](http://php.net/) 是一个流行的通用脚本语言，它可嵌入到 HTML中，尤其适合进行网络后台开发。PHP 主要用在三个领域：

* 网站和网络应用程序（服务器端脚本）
* 命令行脚本
* 桌面（GUI）应用程序

有两个方法将 PHP 连接到服务器上。对于很多服务器，PHP 均有一个直接的模块接口（也叫做 SAPI）。这些服务器包括 Apache、Microsoft IIS等服务器。其它很多服务器支持 ISAPI，即微软的模块接口（OmniHTTPd 就是个例子）。如果 PHP 不能作为模块支持网络服务器，总是可以将其作为 CGI 或 FastCGI 处理器来使用。这意味着可以使用 PHP 的 CGI 可执行程序来处理所有服务器上的 PHP 文件请求。

FPM，即FastCGI 进程管理器。PHP-FPM对于PHP 5.3.3之前的php来说，是一个补丁包，旨在将FastCGI进程管理整合进PHP包中。  PHP5.3.3已经集成了php-fpm，不再是第三方的包了。PHP-FPM提供了更好的PHP进程管理方式，可以有效控制内存和进程、可以平滑重载PHP配置。在./configure的时候带 –enable-fpm参数即可开启PHP-FPM。

### PHP安装

在Nginx下使用PHP，需要安装PHP-FPM包，Debian包管理系统会自动安装PHP及其依赖包。dokuwiki需要php-gd、php-xml模块的支持。

	apt-get update
	apt-get install php-fpm php-gd php-xml

### PHP配置

配置文件php-fpm.conf、php.ini在/etc/php/7.0/fpm/下，使用默认配置，不需要修改。

### 测试PHP

在网站根目录下，删除原来的主页index.html，新建一个动态主页index.php，内容如下：

	<?php phpinfo(); ?>
	
打开浏览器访问网站，将会显示 phpinfo() 信息。 

## Dokuwiki安装与配置

[DokuWiki](https://www.dokuwiki.org/) 是一个使用简单、功能齐全的开源Wiki引擎，它使用文本文件存储数据，不需要数据库，而且语法清晰、易读，易于维护，内建访问控制，拥有大量的扩展，使其具有广泛的应用场景。

### Dokuwiki安装

Debian的安装源中没有Dokuwiki，需要手动下载安装。

从[官网](https://download.dokuwiki.org/)下载最新的稳定版，解压并复制到网站根目录。

	wget https://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz
	tar xzvf dokuwiki-stable.tgz
	cp -R dokuwiki-2018-04-22b \var\www\dokuwiki

服务器nginx使用用户www-data访问系统文件，把dokuwiki文件夹的拥有者改为www-data，服务器才能读写。

	chown -R www-data dokuwiki

在浏览器中运行安装程序install.php，进行简单的设置即可完成安装。
　　
### 安装扩展

Dokuwiki的扩展包括模板和插件。模板使用自带的缺省模板就可以，有些插件必须安装。使用Dokuwiki自带的扩展管理插件可以方便的安装和管理扩展。

#### 备份插件

搜索安装BackupTool for DokuWiki，这个插件可以备份重要的数据。不需要配置。

#### 数学公式插件

要在页面中显示公式，需要安装公式渲染插件MathJax，这个插件使用JavaScript把 LaTeX、MathML、AsciiMath 标记的数学公式转换成 HTML、 SVG、 MathML 显示在浏览器上。

使用默认的配置，预配置文件TeX-AMS_CHTML.js，接受TeX格式的公式输入，输出HTML-CSS格式的公式，其输出与 TeX 输出尽可能接近。

#### Google Analytics插件

搜索安装Google Analytics for DokuWiki，用来跟踪网站的访问情况，只需配置Google Analytics ID，其他使用默认配置。

## 更新记录

  * 2019-03-23：首次发布；