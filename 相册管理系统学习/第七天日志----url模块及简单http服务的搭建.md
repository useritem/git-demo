# 第七天日志
## 一、url模块
### 1.url是啥：
每一信息资源都有统一的且在网上唯一的地址，该地址就叫URL（Uniform Resource Locator,统一资源定位器），它是WWW的统一资源定位标志，就是指网络地址。

url模块只要提供一些实用函数，用于URL的处理和解析。
在Nodejs中可以直接引用url模块

例：


	const url = require("url");

url字符串就是我们经常在浏览器地址栏输入的一串字符，比如这样的一个url字符串
http://user:pass@host.com:8080/p/a/t/h?query=string#hash
url对象解析url字符串会返回一个url对象，它包含url字符串每个组成部分来作为它的属性。
下面我们来看下上面的url字符串解析后的url对象：

<a href="https://imgchr.com/i/r0pCHf"><img src="https://s3.ax1x.com/2020/12/21/r0pCHf.md.png" alt="r0pCHf.png" border="0" /></a>

### 2.对象参数
**（1）href**

href属性是解析后的完整的URL字符串，protocol和host都会被转换成小写。

**（2）protocol**

protocol用来表示URL的小写的协议，例如'http:'

**（3）slashes(不常用)**

slashes属性为布尔值，如果协议protocol冒号后跟的是两个斜杠字符（/）,那么值为true

**（4）auth(不常用)**

auth属性时URL的用户名与密码部分。对应上面的例子就是user:pass

**（5）host**

host属性时URL完整小写的主机部分。包括port（端口）。
例如host.com:8888

**（6）hostname(*)**

hostname是host属性排除端口port之后的小写的主机名部分
例如host.com

**（7）port**

端口号，如果没有则为null,

**（8）path**

path属性，由pathname与search组成的串接,不包含hash字符后面的东西
例如/p/a/t/h?query=string

**（9）pathname(*)**

pathname 属性包含URL的整个路径部分。跟在host后面，截止问号（？）或者哈希字符（#）分隔
例如/p/a/t/h

**（10）search**

search属性包含URL的查询字符串部分，包括开头的问号字符（？）
例如?query=string

**（11）query(*)**

query属性是不包含问号（？）的查询字符串
例如query=string

**（12）hash(*)**

hash属性包含URL的碎屏部分，包括开头的哈希字符（#）

技巧

host = hostname + port

path = pathname + search

search = ? + query



### 3.常用方法

url方法主要有
1.url.parse()，---url字符串化为对象，返回为对象

	const url = require("url");
    url.parse("https://cart.jd.com/cart.action?r=0.3662704717949117")
2.url.format()，----url对象化为字符串

	const url = require("url");
	var urlObj = {
    protocol:"https",
    slashes:true,
    hostname:"www.qihu.com",
    pathname:"/teach/content.html"
	};
	console.log(url.format(urlObj));

3.url.resolve()----替换

url.resolve(from,to)
url.resolve()方法主要用来插入或替换URL内容

from 源地址
to 需要添加或替换的标签

例

	const url = require("url");

	console.log(url.resolve("/one/two/three","four"));


## 二、搭建简单服务器

### 1。listen函数：int listen(int sockfd, int backlog);

此函数是把一个主动套接字（客户端的套接字，主动来连接服务器端），转换成一个被动套接字（由服务器端的内核来接受它的连接请求）

成功返回0， 失败返回-1；

第一个参数为socket函数返回的一个文件描述符。
