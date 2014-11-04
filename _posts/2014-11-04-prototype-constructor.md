---
layout: post
title: 带原型的Constructor
description: ""
category: JavaScript
tags: [设计模式]
imagefeature: 
comments: true
share: true
---

JavaScript有一个prototype的属性,调用JavaScript构造器创建一个对象后，新对象就会具有构造器的所有属性。通过这种方式，可以创建多个Car对象，并访问相同的原型。

{% highlight javascript %}
function Person(name, sex, age) {
	this.name = name;
	this.sex = sex;
	this.age = age;
}

Person.prototype.toString = function() {
	return this.name + " is a " + this.sex + ",age is " + this.age;
}

var c1avie = new Person("c1avie","male",1991);
var niuzan = new Person("niuzan","male",1992);

console.log(c1avie.toString());
console.log(niuzan.toString());
{% endhighlight %}

现在toString()的单一实例就能在所有Person对象之间共享