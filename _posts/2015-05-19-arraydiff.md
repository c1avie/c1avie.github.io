---
layout: post
title: 两个数组取相同的部分
description: "算法优化"
category: JavaScript
tags: [算法]
imagefeature: 
comments: true
share: true
---

这几天在做一个菜单系统，要求可定制化，做的过程中遇到了不少问题，特来总结一下。

因为样式是可定制的，所以就需要动态加载。
{% highlight javascript %}
//动态加载css,js
var dynamicLoading = {
    css: function(path){
		if(!path || path.length === 0){
			throw new Error('argument "path" is required !');
		}
		var head = document.getElementsByTagName('head')[0];
        var link = document.createElement('link');
        link.href = path;
        link.rel = 'stylesheet';
        link.type = 'text/css';
        head.appendChild(link);
    },
    js: function(path){
		if(!path || path.length === 0){
			throw new Error('argument "path" is required !');
		}
		var head = document.getElementsByTagName('head')[0];
        var script = document.createElement('script');
        script.src = path;
        script.type = 'text/javascript';
        head.appendChild(script);
    }
}

//menu_theme表示页面风格
switch(menu_theme) {
	case "ACE风格":
		dynamicLoading.css("assets/css/boss_ace.css");
		break;
	case "888首页风格":
		//加载888首页风格样式
		dynamicLoading.css("assets/css/metro.css");
		break;
	default:
		dynamicLoading.css("assets/css/boss_ace.css");
}
{% endhighlight %}