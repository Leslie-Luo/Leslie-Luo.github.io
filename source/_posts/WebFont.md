---
title: 网页字体加载方案
date: 2018-04-23 10:38:33
categories:
- 前端
tags:
- JavaScript
- Onload
- 字体
---

Windows的字体优化对于处女座的我是不能接受的：( ，偶然发现  官网在Windows中浏览时，会在网页加载后改变网页的字体(⊙o⊙) 。。。
最近毕业答辩也结束了，闲来无事就开始研究利用JavaScript在网页加载完毕后修改网站字体

### addLoadEvent

> 网页加载完毕后执行函数

onload事件是HTML DOM Event 对象的一个属性，它会在整个页面记载完毕后立即执行。这里使用并使用 `addLoadEvent()`封装这个操作。

```javascript
function addLoadEvent(func) {
	var oldonload = window.onload;
	if (typeof window.onload != 'function') {
		window.onload = func;
	} else {
		window.onload = function () {
			if (oldonload) {
				oldonload();
			}
			func();
		}
	}
}
addLoadEvent(MyFont);
```



### fontFace

> 从外部加载新的字体文件，然后应用到网页

不同的网络作者可能需要针对标题字体或正文字体的不同策略。使用WebKit中新增的[CSS字体加载API](http://www.w3.org/TR/css-font-loading/)，Web作者可以完全控制自己的字体加载策略。

这个API公开了JavaScript的两个主要部分：`FontFace`接口和`FontFaceSet`接口。`FontFace`表示与CSS `@font-face`规则相同的概念，因此它包含有关字体的信息，如url，weight，unicode-range，family等。该文档`FontFaceSet`包含网页可用于呈现的所有字体。每个文档都有自己的`FontFaceSet`可访问的通道`document.fonts`。

使用这些对象去实行一个特别的字体加载策略变得十分直接，一旦`FontFace`被构造，`load()`方法就开始异步下载字体数据。当下载成功或因失败而时它会返回了一个`Promise`。用过链式调用返回的promise，JS就可以处理它了。因此一个网站可以实行这样的策略：在不超时的情况下立即使用降级兼容字体。

```javascript
function MyFont() {
            let fontFace = new FontFace("MyWebFont",
                "url('http://onmd9zf1p.bkt.clouddn.com/font/serif.ttf') format('woff2'),url('http://onmd9zf1p.bkt.clouddn.com/font/serif.ttf') format('woff')"
            );
            fontFace.load().then(function (loadedFontFace) {
                document.fonts.add(loadedFontFace);
                document.getElementById("app").style.fontFamily = "MyWebFont";
            });
        }
```

### 效果

![2018-04-23 15.05.08.gif](https://i.loli.net/2018/04/23/5add85c4072e8.gif)