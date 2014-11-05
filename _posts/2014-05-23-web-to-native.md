---
layout: post
title: 本地客户端调起解决方案
date: 2014-05-23 11:24:07.000000000 +08:00
categories: FE
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  duoshuo_thread_id: '1203060334345060396'
author:
  login: C1avie
  email: 429465251@qq.com
  display_name: c1avie
  first_name: ''
  last_name: ''
---
最近做了一个本地客户端调起方案的调研，收获不少，总结一下,这里先不收调起协议之类的事。<br />
先说下之前的方案： <br />
Iphone：直接吊起以及暴力弹框。如果没有调起客户端，则出现弹框。如果成功调起客户端，则返回到页面中时触发pageshow事件，消除弹框。不过IOS uc浏览器不支持这个事件，因此返回到页面中时弹框会出现。<br />
Android及其它：首先检测是否安装了客户端。如果安装了的话，则调起本地客户端。如果没有安装，则提供下载。由于检测是否安装了客户端的成功率不高，即可能用户已经安装了客户端，但是没有检测出来。所以需要改进。 <br />

{% highlight javascript %}
if (util.isIPhone()) {
	me.appbutton.html('点击打开手机地图，省90%流量');
	util.bindHrefStat(me.appbutton, function() {
		$(document.body).append("<iframe id="
			callapp " style="
			border: 0; display: none;
			" src="
			baidumap: //map/" width="0" height="0">");
			window.a = setTimeout(function() {
				util.DownBox.showTb();
			}, 1500);
			return false;
		});
} else {
	me.appbutton.attr('data', util.getClientUrl('download'));
	util.isInstalledClient(function(openurl) {
		me.appbutton.html('点击打开手机地图，省90%流量').attr('data', util.getNativeUrl());
		me.appbutton.addClass("open");
		util.bindHrefStat(me.appbutton, function() {
			pbstat.addStat(COM_PB_STAT.TOP_BANNER, "click", {
				is_install_clientapp: true,
				os: me.os
			});
		});
	}, function(downloadurl) {
		me.appbutton.html($info).attr('data', downloadurl);
		util.bindHrefStat(me.appbutton, function() {
			pbstat.addStat(COM_PB_STAT.TOP_BANNER, "click", {
				is_install_clientapp: false,
				os: me.os
			});
		});
	}, me.appbutton.attr('uid'));
}{% endhighlight %}

中间的策略：

IOS与Android都是直接调起以及暴力弹框，为支持android的uc和默认浏览器，新绑定了blur事件和visibilitychange事件。成功解决了android上uc和默认浏览器的显示问题。但是遗憾的是IOS uc浏览器对这几个事件都不支持。只能再想解决方案。

参考资料：
用户离开页面和回到页面事件（标签切换）
<a href="http://blog.csdn.net/do_it__/article/details/6983239">http://blog.csdn.net/do_it__/article/details/6983239</a>

html5 page visibility
<a href="http://jackyrong.iteye.com/blog/1300764">http://jackyrong.iteye.com/blog/1300764</a>

现在的解决方案：

看了贴吧那边的webapp，在IOS下调起方案很好，不过他们在android下兼容性好像不行，都是直接下载的,无论安装客户端与否。于是找永霞联系了下贴吧那边的技术人员，并参阅了一些资料，找到了解决方案。
Iphone下采用一个计时策略，如果调起了客户端，则时间会大于2000毫秒，不提供下载。如果没有调起，时间会小于2000毫秒，提供下载链接.

测试情况：如果安装了对应的客户端，则浏览器均能打开应用；如果未安装，safari会首先弹出一个错误提醒，然后调用widow.location的代码，uc直接调用window.location这行代码。
Android则是直接调起与暴力弹框，调起客户端回到页面中时通过触发事件消除弹框。

{% highlight javascript %}
me.appbutton.html('点击打开手机地图，省90%流量');
if(util.isIPhone()) {
	util.bindHrefStat(me.appbutton, function() {
		$(document.body).append("<iframe src="baidumap://map/" id="callapp" width="0" height="0" style="border: 0;display: none;">");
		var clickedAt = +new Date;
		setTimeout(function(){
			!window.document.webkitHidden && setTimeout(function(){
				if (+new Date - clickedAt < 2000){
					window.location = 'http://itunes.apple.com/cn/app/id452186370?ls=1&mt=8';
				}
			}, 500);
		}, 500)
		return false;
	});
}
else {
	util.bindHrefStat(me.appbutton, function() {
		$(document.body).append("<iframe src="baidumap://map/" id="callapp" width="0" height="0" style="border: 0;display: none;">");
		window.a = setTimeout(function() {
			util.DownBox.showTb();
		}, 1500);
		return false;
	});
}
{% endhighlight %}

参考资料：

<a href="http://liushuibird.blog.163.com/blog/static/1241915612013810113326699/">http://liushuibird.blog.163.com/blog/static/1241915612013810113326699/</a>

项目Demo(限内网):
<a href="http://cp01-rdqa-dev010.cp01.baidu.com:8088/mobile/webapp/index/index">http://cp01-rdqa-dev010.cp01.baidu.com:8088/mobile/webapp/index/index</a>
