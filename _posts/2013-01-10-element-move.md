---
layout: post
title: 元素移动
date: 2013-01-10 19:28:22.000000000 +08:00
categories:
- FE
tags:
- jquery
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  duoshuo_thread_id: '1203060334345060361'
author:
  login: C1avie
  email: 429465251@qq.com
  display_name: C1avie
  first_name: ''
  last_name: ''
---
一个方向键控制元素移动的函数

<figure>
  <img src="{{ site.url }}/images/wp/move.jpg" alt="change home">
</figure>

但是经过多次尝试终于找出了问题所在.

<figure>
  <img src="{{ site.url }}/images/wp/style.jpg" alt="change home">
</figure>

style.left并不能获取元素的左偏移量,alert之后始终是undefined.
最后查阅了下发现style并不能获取外部样式中left值

需要写成这样

<figure>
  <img src="{{ site.url }}/images/wp/offset.jpg" alt="change home">
</figure>

当然用jquery获取也行

<figure>
  <img src="{{ site.url }}/images/wp/jquery.jpg" alt="change home">
</figure>