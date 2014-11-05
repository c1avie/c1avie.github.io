---
layout: post
title: 在外部如何调去wordpress的近期文章
date: 2013-01-22 18:20:58.000000000 +08:00
categories:
- RD
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  duoshuo_thread_id: '1203060334345060363'
author:
  login: C1avie
  email: 429465251@qq.com
  display_name: C1avie
  first_name: ''
  last_name: ''
---
这里介绍一个我自己写的一个接口，先填写一些连接数据库的基本配置

<figure>
  <img src="{{ site.url }}/images/wp/config.jpg" alt="change home">
</figure>

接着就是连接了，

<figure>
  <img src="{{ site.url }}/images/wp/connect.jpg" alt="change home">
</figure>

其中mysql_query("SET NAMES utf8");防止文字乱码

接着就是输出了

<figure>
  <img src="{{ site.url }}/images/wp/echo.jpg" alt="change home">
</figure>

我想输出去我在博客中发布的5篇文章的题目，因此有了以上代码
其中guid为表wp_posts的title
