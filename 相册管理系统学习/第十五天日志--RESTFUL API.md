# 第十五天日志

## 一、引言
REST---表述性状态传递，表述性状态传递时一组架构约束和原则，满足这些约束条件和原则的应用程序或设计就是RESTful。

REST是设计风格而不是标准。REST通常基于使用HTTP，URI，和XML（标准通用标记语言下的一个子集）以及HTML（标准通用标记语言下的一个应用）这些现有的广泛流行的协议和标准。REST 通常使用 JSON 数据格式。

## 二、实战

var fs = require("fs");  这是表示与文件系统进行交互的方式

### 1.获取用户方式

以下代码**创建了** RESTful API listUsers，用于读取用户的信息列表

	var express = require('express');
	var app = express();
	var fs = require("fs");

	app.get('/listUsers', function (req, res) {
                                    （users.json）表示自己创建的json文件的读取
   	fs.readFile( __dirname + "/" + "users.json", 'utf8', function (err, data) {
       console.log( data );
       res.end( data );    //end表示get的结束标志
   	});
	})
	
	var server = app.listen(8081, function () {

  	var host = server.address().address
  	var port = server.address().port

  	console.log("应用实例，访问地址为 http://%s:%s", host, port)

	})

之后，访问：：：： http://127.0.0.1:8081/listUsers（后面表示的是自己创建的服务器）

### 扩展：——dirname与 ./

__dirname 总是指向被执行 js 文件的绝对路境，比如你在 /d1/d2/myscript.js 文件中写了 __dirname， 它的值就是 /d1/d2 。

./返回你执行 node 命令的路径，例如你的工作路径。

有一个特殊情况是在 require() 中使用 ./ 时，这时的路径就会是含有 require() 的脚本文件的相对路径。


### 2.添加用户信息

核心代码：

	//添加的新用户数据
	var user = {
 	  "user4" : {
      "name" : "mohit",
      "password" : "password4",
      "profession" : "teacher",
      "id": 4
  	 }
	}

	app.get('/addUser', function (req, res) {
  	 // 先进行创建服务器，之后再读取已存在的数据
  	 fs.readFile( __dirname + "/" + "users.json", 'utf8', function (err, data) {
       data = JSON.parse( data );  转化JSON对象
       data["user4"] = user["user4"];     对自己创建的元素进行添加
       console.log( data );
       res.end( JSON.stringify(data));
   	});
	})

### 3.显示用户信息

	app.get('/:id', function (req, res) {
  	 // 首先我们读取已存在的用户
  	 fs.readFile( __dirname + "/" + "users.json", 'utf8', function (err, data) {
       data = JSON.parse( data );
       var user = data["user" + req.params.id] 
       console.log( user );
       res.end( JSON.stringify(user));
   	});
	})

###4.删除用户信息

	var id = 2;    //用于指定删除哪一个用户

	app.get('/deleteUser', function (req, res) {

   	// First read existing users.
   	fs.readFile( __dirname + "/" + "users.json", 'utf8', function (err, data) {
       data = JSON.parse( data );
       delete data["user" + id];
       
       console.log( data );
       res.end( JSON.stringify(data));
  	 });
	})