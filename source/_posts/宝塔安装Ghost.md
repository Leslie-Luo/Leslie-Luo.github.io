---
title: 宝塔安装Ghost
date: 2017-08-01 13:38:09
categories:
- 服务器
tags: 
- Ghost
- 宝塔面板
---

### 安装宝塔面板

请到宝塔官网安装最新版本 **点击传送**

### 为Centos安装Node.js

**1. 下载node.js**

```wiki
cd /usr/local/src/
wget https://npm.taobao.org/mirrors/node/v0.12.18/node-v0.12.18.tar.gz  //使用的是淘宝npm镜像
```

**2. 解压**

```wiki
tar zxvf node-v0.12.18.tar.gz
```

**3. 编译安装**

```wiki
cd node-v0.12.18  
./configure --prefix=/usr/local/node/0.12.18
make
make install
```

**4. 编辑环境变量**

```wiki
vi /etc/profile
```

设置node.js环境变量，在 `export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL`一行的上面添加如下内容:

```wiki
#set for nodejs
export NODE_HOME=/usr/local/node/0.12.18 
export PATH=$NODE_HOME/bin:$PATH
```

保存并退出编辑

**5. 编译profile，使node.js生效**

```wiki
source /etc/profile
```

**6. 验证安装是否成功**

```wiki
node -v
```

输出 `v0.12.18` 表示配置成功，node.js部署完成。

### 安装Ghost

**1. 下载Ghost到你的网站目录，并解压**

- 先切换到你要安装Ghost的网站目录，再执行下载命令

  ```wiki
  wget -P / http://dl.ghostchina.com/Ghost-0.7.4-zh-full.zip
  ```

- 解压

  ```Wiki
  unzip Ghost-0.7.4-zh-full.zip
  ```

**2. 复制config.example.js成config.js**

```wiki
cp config.example.js config.js
```

**3. 编辑config.js**

```wiki
production: {
        url: 'http://www.*.com',//填写你的博客域名
        mail: {},
        //需将这段代码注释
        /*database: {
            client: 'sqlite3',
            connection: {
                filename: path.join(__dirname, '/content/data/ghost.db')
            },
            debug: false
        },*/
        //END
        // 配置MySQL数据库
        database: {
            client: 'mysql',
            connection: {
                host     : '127.0.0.1',//数据库地址
                user     : '数据库账户',//数据库账户
                password : '数据库密码',//数据库密码
                database : '数据库名称',//数据库名称
                charset  : 'utf8'
            },
            debug: false
        },
        server: {
            host: '127.0.0.1',
            port: '2368'
        },
```

### 配置Nginx服务器

在宝塔面板选择你的站点，点击修改配置文件，清空里面的内容，替换成以下
`这是最基本的设置，保证能运行，其他的可以自行配置`

```wiki
server {  
    listen 80;
    server_name www.xxxx.com; //替换为你的域名
    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $http_host;
        proxy_pass http://127.0.0.1:2368;
    }
}
```

保存后到面板重启nginx服务器

### 启动Ghost

在你网站目录下运行

```wiki
npm start --production
```

如果不出差错，在浏览器输入你的域名就可以看见Ghost的界面了，但目前Ghost在我们SSH断开后就会结束进程，所以还要执行最后一步

### 安装forever守护Ghost进程

以下命令都请在网站根目录下运行

```wiki
npm install forever -g
```

```wiki
NODE_ENV=production forever start index.js //启动Ghost  
NODE_ENV=production forever stop index.js //停止Ghost  
NODE_ENV=production forever restart index.js //重启Ghost
```

**至此宝塔面板Centos 7 x64 安装Ghost结束**

> 感谢[豆采](https://blog.doocii.com/)提供的教程 `虽然他不知道`，本文是在他的原教程上修改而来