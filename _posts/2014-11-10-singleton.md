---
layout: post
title: Singleton模式
description: ""
category: JavaScript
tags: [设计模式]
imagefeature: 
comments: true
share: true
---

在JavaScript中，Singleton充当共享资源命名空间，从全局命名空间中隔离出代码实现，从而为函数提供单一访问点。

Singleton模式实用性：

	当类只能有一个实例而且客户可以从一个众所周知的访问点访问它时
	该唯一的实例应该是通过子类化可扩展的，并且客户应该无需更改代码就能使用一个扩展的实例时

example：
{% highlight javascript %}
var my Singleton = (function() {
	// 实例保持了singleton的一个引用
	var instance;
	function init() {
		// 私有方法和变量
		function privateMethod() {
			console.log("I am private");
		}
		var privateVariable = "I am also private";
		var provateRandomNumber = Math.random();
		return {
			// 公有方法和变量
			publicMethod: function() {
				console.log("The public can see me!");
			},
			publicProperty: "I am also public",
			getRandomNumber: function() {
				return privateRandomNumber;
			}
		};
	};
	return {
		// 获取singleton的实例，如果存在就返回，不存在就创建新实例
		getInstance: function() {
			if(!instance) {
				instance = init();
			}
			return instance;
		}
	};
})();
{% endhighlight %}