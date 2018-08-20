---
title: 文件转Base64编码
date: 2017-08-05 13:35:49
categories:
- 前端
tags: 
- Base64
---

### 前言

周末在宿舍无聊，想起公司`H5`做的 App下周要做Base64附件上传，之前在期末项目用过图片用Base64编码用Ajax上传服务器感觉与其他的上传还是很方便。

### 上代码

**Html**

```html
<form onsubmit="return false;">
      <input type="hidden" name="file_base64" id="file_base64">
      <input type="file" id="fileup">
      <input type="submit" value="submit" onclick="post()">
</form>
```

**Js**

```javascript
<script src="https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"></script>
    <script>
        // name -> 文件名  base64 -> 文件编码
        var name, base64;
        // 获取文件并编码
        $(document).ready(function () {
            $("#fileup").change(function () {
                name = this.files[0].name;
                var reader = new FileReader();
                reader.readAsDataURL(this.files[0]);
                reader.onload = function (e) {
                    console.log(e.target.result);
                    base64 = e.target.result;
                };
            });
        });
        // 提交数据
        function post() {
            $.ajax({
                type: "POST",
                url: "uploader.php",
                data: {
                    "name": name,
                    "base64": base64,
                },
                dataType: "json",
                success: function (data) {
                    console.log(data)
                },
                error: function(XMLHttpRequest, textStatus, errorThrown) {
                        console.log(XMLHttpRequest.status);
                        console.log(XMLHttpRequest.readyState);
                        console.log(textStatus);
                    },
            })
        }
    </script>
```

**PHP**

```php
<?php
if (isset($_POST['name'],$_POST['base64'])){
    $name = $_POST['name'];
    $file_base64 = $_POST['base64'];
    $file_base64 = preg_replace('/data:.*;base64,/i', '', $file_base64);
    $file_base64 = base64_decode($file_base64);

    file_put_contents($name, $file_base64);
    
    $data['msg'] = 'success';
    echo json_encode($data); 
}
```

### 后记

这次之所以获取了文件名，是有的文件用`Base64`编码后无法分辨文件格式；最重要的是还没办法从`Base64`获取文件名🤔 准确说是没这个数据🙄
这次还遇到一个坑，如果服务器没有返回数据`Ajax`还会报错😶 第一次遇到，算是涨知识咯
写的代码还有点简陋，同志还需努力

> 大半个月没见我家智障了 保持微笑 🙃
> 2017-08-05

[](https://blog.dktoo.cc/2017/08/04/I'm%20back/)