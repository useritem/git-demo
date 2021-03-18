# 第十三天日志
## 一、express框架
### 1.介绍
express作为nodejs的Web应用框架，提供了web应用与Http工具，它可以快速搭建一个完整网站。

核心特性：

1.可以设置中间件来响应 HTTP 请求。

2.定义了路由表用于执行不同的 HTTP 请求动作。

3。可以通过向模板传递参数来动态渲染 HTML 页面。

### 2.安装express

使用之前先通过node进行安装：

$ cnpm install express --save

**特别注意**

在配置express下载之前，应该先下载cnpm并且配置其环境，参考：

https://blog.csdn.net/bxllove/article/details/84784091

**特别注意：**

配置cnpm环境时应该，在路径path配置的时候，注意：

[![sRrNtO.png](https://s3.ax1x.com/2021/01/20/sRrNtO.png)](https://imgchr.com/i/sRrNtO)

注意在下载完express时，还应该下载一下匹配文件。

[![sRyFZq.png](https://s3.ax1x.com/2021/01/20/sRyFZq.png)](https://imgchr.com/i/sRyFZq)

### 3.实例及解释

	//express_demo.js 文件
	var express = require('express');
	var app = express();
 
	app.get('/', function (req, res) {
   		//传送HTTP响应
  	 res.send('Hello World');
	})
 
	var server = app.listen(8081, function () {
 
  	var host = server.address().address
  	var port = server.address().port
 
 	 console.log("应用实例，访问地址为 http://%s:%s", host, port)
 
	})

注意express框架在运行时，必须和express在一个文件夹下。

解释：

1.res与req为处理请求与响应的对象。

	app.get('/', function (req, res) {
 	  // --
	})
get为了连接建立的时候使用，

post请求，一般是指接受相应的信息的时候使用

对象属性见：

https://www.runoob.com/nodejs/nodejs-express-framework.html

### 4。路由
由于路由决定了谁去相应客户端的请求，所以我们可以通过路由提取**URL**与**POST\GET**参数，

来处理更多的http请求，（就是更多的网站）    

例：

	var express = require('express');
	var app = express();
 
	//  主页输出 "Hello World"
	app.get('/', function (req, res) {
 	  console.log("主页 GET 请求");
   	res.send('Hello GET');
	})
 
 
	//  POST 请求
	app.post('/', function (req, res) {
  	 console.log("主页 POST 请求");
  	 res.send('Hello POST');
	})
 
	//  /del_user 页面响应
	app.get('/del_user', function (req, res) {
   	console.log("/del_user 响应 DELETE 请求");
   	res.send('删除页面');
	})
 
	//  /list_user 页面 GET 请求
	app.get('/list_user', function (req, res) {
  	 console.log("/list_user GET 请求");
  	 res.send('用户列表页面');
	})
 
	// 对页面 abcd, abxcd, ab123cd, 等响应 GET 请求
	app.get('/ab*cd', function(req, res) {   
  	 console.log("/ab*cd GET 请求");
  	 res.send('正则匹配');
	})
 
 
var server = app.listen(8081, function () {
 
  var host = server.address().address
  var port = server.address().port
 
  console.log("应用实例，访问地址为 http://%s:%s", host, port)
 
})

###5.静态文件

express通过内置中间件-- express.static 来设置静态文件如：图片， CSS, JavaScript 等。

可以使用 express.static 中间件来设置静态文件路径。例如，如果你将图片， CSS, JavaScript 文件放在 public 目录下，你可以这么写：

	app.use('/public', express.static('public'));

### 6.get方法

	var express = require('express');
	var app = express();
 
	app.use('/public', express.static('public'));
 
	//这个代表需要建立的文件（个人理解为静态建立连接）
	app.get('/index.html', function (req, res) {
 	  res.sendFile( __dirname + "/" + "index.html" );
	})
 
	app.get('/process_get', function (req, res) {
 	
   	// 输出 JSON 格式,代表你提交之后的显示内容
  	 //后面的first——name与last-name为固定表达，不可改变
  	 var response = {
  	     "first":req.query.first_name,
    	   "last":req.query.last_name
   	};
 	  console.log(response);
   	res.end(JSON.stringify(response));
	})
 
	//这里表示的是在控制台显示的东西
	var server = app.listen(8081, function () {
 
 	 var host = server.address().address
 	 var port = server.address().port
 
	  console.log("应用实例，访问地址为 http://%s:%s", host, port)
 
	})

html：

	<html>
	<body>
	<form action="http://127.0.0.1:8081/process_get" method="GET">
     //form标签：formmethod="传送方式"action="服务器文件"，method传送方式
   	//action浏览者输入的数据被传送到的地方，比如一个php页面，(save.php)
	First Name: <input type="text" name="first_name">  <br>
 
	Last Name: <input type="text" name="last_name">
	<input type="submit" value="Submit">
	</form>
	</body>
	</html>

###7.POST方法

HTML:

	<html>
	<body>
	<form action="http://127.0.0.1:8081/process_post" method="POST">
                       8081端口后面代表着服务器         
	First Name: <input type="text" name="first_name">  <br>
 
	Last Name: <input type="text" name="last_name">
	<input type="submit" value="Submit">
	</form>
	</body>
	</html>

SERVER.JS:

	var express = require('express');
	var app = express();
	var bodyParser = require('body-parser');
 
	// 创建 application/x-www-form-urlencoded 编码解析
	var urlencodedParser = bodyParser.urlencoded({ extended: false })
 
	app.use('/public', express.static('public'));
 
	app.get('/index.html', function (req, res) {
  	 res.sendFile( __dirname + "/" + "index.html" );
	})
 
	app.post('/process_post', urlencodedParser, function (req, res) {
          //接收服务器信息 

  	 // 输出 JSON 格式
   	var response = {
       "first_name":req.body.first_name,
       "last_name":req.body.last_name
   	};
  	 console.log(response);
   	res.end(JSON.stringify(response));
	})
 
	var server = app.listen(8081, function () {
 
 	 var host = server.address().address
 	 var port = server.address().port
 
 	 console.log("应用实例，访问地址为 http://%s:%s", host, port)
 
	})

### 8.文件上传

创建一个用于上传文件的表单，使用 POST 方法，表单 enctype 属性设置为 multipart/form-data。

index.html 文件代码：

	<html>
	<head>
	<title>文件上传表单</title>
	</head>
	<body>
	<h3>文件上传：</h3>
	选择一个文件上传: <br />
	<form action="/file_upload" method="post" enctype="multipart/form-data">
	<input type="file" name="image" size="50" />
	<br />
	<input type="submit" value="上传文件" />
	</form>
	</body>
	</html>

js：

	var express = require('express');
	var app = express();
	var fs = require("fs");
 
	var bodyParser = require('body-parser');
	var multer  = require('multer');
 
	app.use('/public', express.static('public'));
	app.use(bodyParser.urlencoded({ extended: false }));
	app.use(multer({ dest: '/tmp/'}).array('image'));
 
	app.get('/index.html', function (req, res) {
 	  res.sendFile( __dirname + "/" + "index.html" );
	})
 
	app.post('/file_upload', function (req, res) {
 
  	 console.log(req.files[0]);  // 上传的文件信息
 
 	  var des_file = __dirname + "/" + req.files[0].originalname;
  	 fs.readFile( req.files[0].path, function (err, data) {
        fs.writeFile(des_file, data, function (err) {
         if( err ){
              console.log( err );
         }else{
               response = {
                   message:'File uploaded successfully', 
                   filename:req.files[0].originalname
              };
          }
          console.log( response );
          res.end( JSON.stringify( response ) );
       });
  	 });
	})
 
	var server = app.listen(8081, function () {
 
  	var host = server.address().address
  	var port = server.address().port
 
 	 console.log("应用实例，访问地址为 http://%s:%s", host, port)
 
	})

8.COOKie管理

Cookie是代表储存在用户本地端的数据

可以使用中间件向 Node.js 服务器发送 cookie 信息，以下代码输出了客户端发送的 cookie 信息：

	express_cookie.js 文件代码：
	// express_cookie.js 文件
	var express      = require('express')
	var cookieParser = require('cookie-parser')
	var util = require('util');
 
	var app = express()
	app.use(cookieParser())
 
	app.get('/', function(req, res) {
    console.log("Cookies: " + util.inspect(req.cookies));
	})
 
	app.listen(8081)