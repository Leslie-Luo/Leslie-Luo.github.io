---

title: IOS微信H5-底部导航栏隐藏
categories:
  - 前端
tags:
  - JavaScript
date: 2018-09-03 14:31:37
---

IOS端微信H5发生页面跳转后微信浏览器会强制加载一个导航栏，今天终于找到办法把它干掉。

![TabBar](https://i.loli.net/2018/09/03/5b8ce35ea1168.png)

### 方法

利用HTML5的`history.replaceState()`来修改当前的页面地址完成页面之间的跳转。`replaceState()`是修改了当前的历史记录项而不是新建一个。 注意这并不会阻止其在全局浏览器历史记录中创建一个新的历史记录项。

### 浏览器支持

<iframe src="https://caniuse.com/#search=replaceState" width="100%" height="436px"></iframe>

### 注意

这个方法不会产生浏览的记录，所以在安卓平台上点击返回键会直接退出。

谨慎使用