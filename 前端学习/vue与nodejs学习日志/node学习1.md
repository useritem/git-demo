##1、node主要作用

（1）web服务器

（2）命令行工具

（3）桌面应用程序的开发

（4）网路爬虫：是按照一周规则，自动抓取网络信息的程序

（5）核心思想：异步与事件

## 2、文件操作

使用之后可以进行相关文件操作

const fs = require('fs')

该fs模块支持以标准POSIX函数为模型的方式与文件系统进行交互。

进行数据读取：

	fs.readFile(path[, options], callback)

注意：[]里面表示可以不必须参数

样例：

	var fs =require('fs')
	fs.readFile('./main.js','可以不写，也可以写为utf8'，function(err,data){
	                             //回调函数读完文件调用
	//注意utf8是读取文件的方法 必须 加
	//这里体现了异步思想：：
	
	//当这个文件的代码执行完之后，/main.js中的代码并未读取
	//之后/main.js读取时，本文件已经执行完毕，但是由于函数，所以/main.js读取完之后
	//还需要回到本文件，找到这个函数，再执行器其内部代码
	})

**此外需要注意的是：**

回调函数传入的个数，和函数本身属性有关（名字是自己定的），如果多加，就是undefined

#### 异步的体现：

<div style="color: red;">当某块代码执行完毕之后，并不代表这里所有的代码全部已经执行了</div>


##3、http操作

###1.服务器请求基本步骤：

1、引入模块

	var http = require('http')
2、创建服务

	var server = http.createServer() 

3、启动监听、表示开始使用

	server.listen(8080,function(){
    console.log('访问127.0.0.1:8080')
	}) 
	// 范围0-65535表示一个操作系统，最多的继承数

###2、客户端

进行事件的相关操作：

	server.on('request',function(res,re){}
	 // 第一个参数，res表示请求的所有内容，第二个表示服务器响应客户端的方法

之后表示事件结束：

	  re.end('哈哈法二覅哦吼i')//这个表示服务器的响应，必写表示结

进行文档的写入

	/ 设置响应头信息，防止二进制解析出现错误,且需要放在最前面(在write之前)
	// 且注意charset=utf-8中间表不能瞎加空格
	// text/plain表示纯文本，这样保证可以读取中文
	re.setHeader('Content-type','text/html;charset=utf-8')

	re.write('拉拉')
	re.write("nihao")  //服务器向相应的客户端写入你好

当涉及到外部引入时：

如：  实现图片的加载
 这时候需要判断请求

	server.on('request',function(res,re){
      	// url表示请求的东西
   		 var urls = res.url;
    	if(urls =='/'){
    	re.setHeader('Content-type','text/html;charset=utf-8')
   		 fs.readFile('./index.html','utf8',function(err,data){
   		 re.end(data)//这个表示服务器的响应，必写表示结束
	})
    }else {
        // 响应一切html的静资源
                        // 注意图片路径千万别写中文！！！！！！！！！！！！
        // 由于urls是当前路径下的urls，是/，所以应该加上.，表示当前路径下的urls
        fs.readFile('.'+urls,function(error,daa){
            re.end(daa)
        })
       
    }


##特别注意：注意文件的路径千万别写中文！！！，要不然以二进制读取会出错