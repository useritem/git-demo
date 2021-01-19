# 第十二天日志
## 一、web服务器
### 1.概述
提供web信息浏览的web服务器，只需支持HTTP协议、HTML文档格式及URL，与客户端的网络浏览器配合。

大多数web服务器支持的脚本语言：php、python、ruby。

主流的三个web服务器：Apache、Nginx、IIS

### 2.Web应用架构

[![s24y5R.png](https://s3.ax1x.com/2021/01/19/s24y5R.png)](https://imgchr.com/i/s24y5R)

解释：

Client： 主要为了方便用户向服务器，通过http请求数据

Server:代表接收方与响应方，相当于中转站。

Business 业务层通过web服务器进行用户想要的处理

Data：对数据库进行操作，（找数据）

### 3.**web服务器**的构建

通过**http模块**进行 http服务端以及客户端的调用。

以建立最基本的http服务器框架为例

	var http = require('http');
	var fs   = require('fs');
	var url  = require('url');
	http.createServer( function (req,res) { //一个请求，一个回应

    // 解析请求，包括文件名
       var pathname = url.parse(req.url).pathname;

       // 输出请求的文件名
   	console.log("Request for " + pathname + " received.");
  	//从文件系统进行请求文件的相关读取

  	fs.readFile(pathname.substr(1),function(err,data){ //将文件内容写入缓存区，第一个表示文件名
          // substr() 从字符串中提取一些字符

      if(err){      //表示报错
          console.log(err);
           // HTTP 状态码: 404 : NOT FOUND
         // Content Type: text/html

        res.writeHead(404,{'Content-Type':'text/html'});
      }else{

        // HTTP 状态码: 200 : OK
        // Content Type: text/html
         res.writeHead(200, {'Content-Type': 'text/html'});    
         
         // 响应文件内容
         res.write(data.toString());  
      }
      //发送相应数据
      res.end()
	  });
	}).listen(8080)    //表示http服务器架构端口为8080
	console.log('Server running at http://127.0.0.1:8080/')

之后在浏览器中点开地址就可以了：

接着我们在浏览器中打开地址：http://127.0.0.1:8080/index.html

### 4.node建立web客户端

	var http = require('http');
 
	// 用于请求的选项
	var options = {
  	 host: 'localhost',
  	 port: '8080',
  	 path: '/index.html'  
	};
 
	// 处理响应的回调函数
		var callback = function(response){
  	 // 不断更新数据
   	var body = '';
   	response.on('data', function(data) {
      body += data;
   	});
  	 
  	 response.on('end', function() {
   	   // 数据接收完成
   	   console.log(body);
   	});
	}
	// 向服务端发送请求
	var req = http.request(options, callback);
	req.end();

####注意
在运行此文件时，要保证对应web服务器处于打开的状态
例如：
[![s2OKFU.png](https://s3.ax1x.com/2021/01/19/s2OKFU.png)](https://imgchr.com/i/s2OKFU)

左边为客户端，右边为服务端，右边的Request for /index.html received.表示

用户请求信息。

##特别：ctrl + c 可以快速推出node操作。

## 二、Vue中Class与Style的绑定

### 1.前提

Vue.js对于v-bind针对class与style的操作做了增强：表达式结果的类型除了字符串之外，还可以是对象或数组。

（v-bind是用于绑定一个或多个属性值，或者像一个组件创建props值，）

### 2.绑定HTML class（对象）

####进行动态切换对象为class：
	<div>
 	 class="static"
 	 v-bind:class="{ active: isActive, 'text-danger': hasError }"
	</div>
v-bind也可以与普通的class共存

这里的active或者text-danger发生变化时，会更新class列表

当然这里的绑定也可以进行返回对象的计算属性的绑定，例如：

	<div v-bind:class="classObject"></div>
	data: {
 	 isActive: true,
 	 error: null
	},
	computed: {
 	 classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
   	 }
  	}
	}

### 3.绑定HTML class（数组）

传数组的方式如下：

	<div v-bind:class="[activeClass, errorClass]"></div>
	data: {
 	 activeClass: 'active',
  	errorClass: 'text-danger'
	}

渲染结果为：

	<div class="active text-danger"></div>

当然，也可以进行三元表达式：

	<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>

或者数组中加入对象：

	<div v-bind:class="[{ active: isActive }, errorClass]"></div>

### 4.对象语法

v-bind:style 的对象语法十分直观——看着非常像 CSS，但其实是一个 JavaScript 对象。CSS property 名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名：

使代码清晰：

	<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
	data: {
 	 activeColor: 'red',
 	 fontSize: 30
	}

### 5.多重值

为了保证浏览器可以正常使用，所以可以为 style 绑定中的 property 提供一个包含多个值的数组，常用于提供多个带前缀的值，例如：

	<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>

这样写只会渲染数组中最后一个被浏览器支持的值。

在本例中，如果浏览器支持不带浏览器前缀的 flexbox，那么就只会渲染 display: flex。