---
title: One Api
date: 2017-04-13 13:37:57
categories:
- 前端
tags:
- Api
- One
---

前几天一直在研究在FM播放器，虽然进度太慢，也算了解了下API的使用方法，昨晚偶然发现了**「ONE · 一个」**的官方接口，今日果断开坑 **T_T**

目前设置的是每天 **早上6点** 从官方同步数据

经过一天的东拼西凑总算弄出来了，虽然代码简陋，可是也是自己的第一个像样的作品在以后自己的技术逐渐进步，逐步优化 **OvO**

**Demo**

### 请求

- 请求地址: `https://api.dktoo.cc/one/One.php`
- 请求方式: **GTE**
- 参数:
  - `encode - 输出格式`
  - **json** `默认`
  - **js**

### 返回参数

- json请求 `https://api.dktoo.cc/one/One.php?encode=json`

  ```json
  {
    "date": "2017-04-16",
    "words_info": "毛姆",
    "volume": "VOL.1653",
    "forward": "我一直发现在你无话可说的时候就别说话，在你不知如何回答别人的话的时候就保持沉默，这是生活中一个很好的策略。",
    "img": "https://api.dktoo.cc/one/img/20170416.jpg"
  }
  ```

- js请求 `https://api.dktoo.cc/one/One.php?encode=js`

在 head 区引用 javascript 版 API

```javascript
<script type="text/javascript" src="https://api.i-meto.com/one/One?encode=js"></script>
```

然后在需要显示的地方加入

```javascript
<script>date()</script>        //发布时间
<script>words_info()</script>  //作者信息
<script>volume()</script>      //版本号
<script>forward()</script>     //正文
<script>img()</script>         //图片地址
```

> 2017-04-13 发布json版本
> 2017-04-16 增加js版本，对代码进行优化