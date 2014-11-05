---
layout: post
title: IE下的onmouseover bug
date: 2012-12-31 21:35:14.000000000 +08:00
categories:
- FE
tags:
- IE bug
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  duoshuo_thread_id: '1203060334345060356'
author:
  login: C1avie
  email: 429465251@qq.com
  display_name: C1avie
  first_name: ''
  last_name: ''
---
昨天（这是以前在点点博客上发的博文，算是移植过来了吧）在帮一个同学看一个下拉菜单的时候，碰到了一段js代码

<figure>
  <img src="{{ site.url }}/images/wp/B1399C9E91F2D01D53576CC33963127F23AA33B79A397_500_206.jpg" alt="change home">
</figure>

那个onmouseover事件在Chrome下是好的，但是到了IE下，当鼠标移至下拉菜单时，往下移的过程下拉菜单就直接消失了，那是下拉菜单还没有移完。当时想了诸多方法都没解决，只好重写了一个下拉菜单。

事后，我又细细思索了一下，发现在加一个background时，可以修复IE下的这个bug。

<figure>
  <img src="{{ site.url }}/images/wp/4AA297BF755E57FB0CA26420E5BDD0D9AE71601D9C18B_500_118.jpg" alt="change home">
</figure>

样式中加个background

<figure>
  <img src="{{ site.url }}/images/wp/566C68E39C0E9AA0F9C755CE7B96D2DAD023A7EEFF4A6_282_189.jpg" alt="change home">
</figure>