---
layout: post
title: 一周总结
description: "总结"
category: JavaScript
tags: [总结]
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

因为菜单是动态拉取数据显示出来的，还需要权限控制，因为需要两个post请求。之前我是先等第一次请求success之后再请求权限，这样就会比较慢。
angular有个$q.all,then的用法，可以将两个请求异步的进行
{% highlight javascript %}
$q.all([
	$http.post(url,data).success(function() {

	}),
	$http.post(auth_url,data).success(function() {

	}),
]).then(function() {
	
});
{% endhighlight %}

权限控制这一块需要对比两个数组，取相同的部分，开始用的是双重循环，这样效率比较低
{% highlight javascript %}
Array.prototype.in_array = function(e) { 
	for(i = 0;i < this.length;i++) { 
		if(this[i] == e) return true; 
	} 
	return false; 
}

var powers = result.data.powers;
var newZnodes = new Array();
for(var i = 0;i < zNodes.length;i++) {
	var flag = powers.in_array(zNodes[i].access);
	if(flag) {
		newZnodes.push(zNodes[i]);
	}
}
{% endhighlight %}

后面就又想着将其中一个数组变为字符串，用字符串的indexOf方法进行查找
{% highlight javascript %}
var newZnodes = new Array();
for(var i = 0;i < zNodes.length;i++) {
	var val = zNodes[i].access;
	if(powers.indexOf(val) != -1) {
		newZnodes.push(zNodes[i]);
	}
}
{% endhighlight %}


