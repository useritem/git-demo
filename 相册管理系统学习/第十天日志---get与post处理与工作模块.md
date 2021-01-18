# 第十天日志
## 一、node中get与post请求处理
#### 概述：实现用户对服务器的提交，例如表单提交一般用get/post请求
### 1.获取get请求
由于get请求直接被封装再路径之中，所以在url完整的路径中，可以手动解析后面的内容作为get请求的参数，如**parse**函数

	var http =require('http') // 构建http server服务

	var url = require('url')
	var util = require('util')

 	//获得util模块，主要针对字符串与对象的操作，或者对输出流的同步写入以及对象的增强等，函数：
 	//1.format接受一个格式化字符串作为第一个参数，并返回一个格式化后的字符串
 	//（格式化字符串的意bai思是使du用Format函数将指定的字符串转换为想要的输出格式。）
 	//参考：  https://blog.csdn.net/qq_39263663/article/details/80375427
	//当有比占位符更多的参数时，多余的参数被转换为字符串，然后用空格分隔符连接起来。例如 util.format('%s=%s','item1','item2','item3');    //'item1 = item2 item3'
	//stdout, stdin, stderr的中文名字分别是标准输出，标准输入和标准错误。

	http.createServer(function(req,res){
    res.writeHead(200,{'Content-Type':'text/plain;charset = utf-8'});
    res.end(util.inspect(url.parse(req.url,ture)))
	}).listen(3000);

### 2.post请求
由于post请求会消耗大量时间与资源，所以nodejs默认不会解析请求体，需要时手动

		var http = require('http');
	var querystring = require('querystring');
		var util = require('util');
 
	http.createServer(function(req, res){
    // 定义了一个post变量，用于暂存请求体的信息
    var post = '';     
 
    // 通过req的data事件监听函数，每当接受到请求体的数据，就累加到post变量中
    req.on('data', function(chunk){    
        post += chunk;
    });
 
    // 在end事件触发后，通过querystring.parse将post解析为真正的POST请求格式，然后向客户端返回。
    req.on('end', function(){    
        post = querystring.parse(post);
        res.end(util.inspect(post));
    });
 	}).listen(3000);

### http.createServer方法使用

，自动添加到 request 事件（request模块想象成一个简化版的第三方类http模块，同时支持https 和重定向），函数传递两个参数：

req 请求对象，想知道req有哪些属性，可以查看 “http.request 属性整合”。

res 响应对象 ，收到请求后要做出的响应。想知道res有哪些属性，可以查看 “http.response属性整合”。

res定义了相应内容，为nodejs并且以end结束，最后listen函数为监听3000端口

## 二、nodejs工作模块
[![syfDJS.png](https://s3.ax1x.com/2021/01/18/syfDJS.png)](https://imgchr.com/i/syfDJS)