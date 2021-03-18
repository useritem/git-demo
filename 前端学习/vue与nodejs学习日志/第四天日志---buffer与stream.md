# 第四天日志
## 一、node.js的个人理解
### 1.nodejs定义

Nodejs是一个基于Chrome V8引擎（为CPU解读JS，JS为解释性语言由引擎直接读取源码）的JavaScript运行环境，一个让JavaScript运行在服务端的开发平台。

####Node.js的地位

目前，Node.js在大部分领域都占有一席之地，尤其是I/O密集型的。

比如Web开发，微服务，前端构建等。不少大型网站都是使用 Node.js 作为后台开发语言的，用的最多的就是使用Node.js做前端渲染和架构优化，比如 淘宝 双十一、去哪儿网 的 PC 端核心业务等。

另外，有不少知名的前端库也是使用 Node.js 开发的，如Webpack是一个强大的打包器，React/Vue 是成熟的前端组件化框架。

Node.js通常被用来开发低延迟的网络应用，也就是那些需要在服务器端环境和前端实时收集和交换数据的应用（API、即时聊天、微服务）。阿里巴巴、腾讯、Qunar、百度、PayPal、道琼斯、沃尔玛和 LinkedIn 都采用了 Node.js 框架搭建应用。

### 3.Node.js的用途

Node.js最适合在流媒体应用程序中使用，还有一些聊天应用程序。

游戏服务器 - 需要一次处理数千个请求的快速和高性能服务器，这是一个理想的框架。

广告服务器 - 再次在这里你可以有数千个请求从中央服务器提取广告，Node.js可以是一个理想的框架来处理这个问题。

流服务器 - 使用Node的另一个理想方案是用于多媒体流服务器，其中客户端有请求从该服务器提取不同的多媒体内容。

## 二.Node.js Buffer(缓冲区)的学习

### 1、Buffer的引入：
JavaScript 语言自身只有字符串数据类型，没有二进制数据类型。

但在处理像TCP（传输控制协议）流或文件流（文件流是继承自iostream。与iostream、sstream共同作为头文件构成IO标准库。）时，必须使用到二进制数据。所以----->buffer类:存放二进制数据区

**流：建立在面向对象的基础上的一种抽象处理事件的工具**
	
	建议使用 Buffer.from() 接口去创建Buffer对象。

### 2、Buffer 与字符编码

Buffer 实例一般用于表示编码字符的序列，比如 UTF-8 、 UCS2 、 Base64 、或十六进制编码的数据。 通过使用显式的字符编码，就可以在 Buffer 实例与普通的 JavaScript 字符串之间进行相互转换。

	const buf = Buffer.from('runoob', 'ascii');

	// 输出 72756e6f6f62
	console.log(buf.toString('hex'));
### 3、创建方式
[![r8NHUA.png](https://s3.ax1x.com/2020/12/17/r8NHUA.png)](https://imgchr.com/i/r8NHUA)

### 4、执行nodejs的方式注意

用npm执行时，注意**文件匹配问题！！！**

### 5、Buffer的基本操作

### （1）写入缓冲区
	buf.write(string[, offset[, length]][, encoding])
参数描述如下：

a.string - 写入缓冲区的字符串。

b.offset - 缓冲区开始写入的索引值，默认为 0 。

c.length - 写入的字节数，默认为 buffer.length

d.encoding - 使用的编码。默认为 'utf8' 。

### （2）读入缓冲区
	buf.toString([encoding[, start[, end]]])
参数描述如下：

a.encoding - 使用的编码。默认为 'utf8' 。

b.start - 指定开始读取的索引位置，默认为 0。

c.end - 结束位置，默认为缓冲区的末尾。


### （3）将 Buffer 转换为 JSON 对象
将 Node Buffer 转换为 JSON 对象的函数语法格式如下：

	buf.toJSON()
当字符串化一个 Buffer 实例时，JSON.stringify() 会隐式地调用该 toJSON()。

返回值————————》返回 JSON 对象。

	实例
	const buf = Buffer.from([0x1, 0x2, 0x3, 0x4, 0x5]);
	const json = JSON.stringify(buf);

	// 输出: {"type":"Buffer","data":[1,2,3,4,5]}
	console.log(json);

	const copy = JSON.parse(json, (key, value) => {
  	return value && value.type === 'Buffer' ?
    Buffer.from(value.data) :
    value;
	});

	// 输出: <Buffer 01 02 03 04 05>
	console.log(copy);
执行以上代码，输出结果为：

	{"type":"Buffer","data":[1,2,3,4,5]}
	<Buffer 01 02 03 04 05>

### 之后还有

：：：缓冲区的的
裁剪（buf.slice([start[, end]])），

拷贝（buf.copy(targetBuffer[, targetStart[, sourceStart[, sourceEnd]]])），

长度，

比较（buf.compare(otherBuffer);）与

合并（Buffer.concat(list[, totalLength])），可以通过数组的相关知识理解。


**一个 Buffer 类似于一个整数数组，但它对应于 V8 堆内存之外的一块原始内存。**

学习网站：
https://www.runoob.com/nodejs/nodejs-buffer.html

### 扩展：解决git bash的问题：

	fatal: not a git repository (or any of the parent directories): .git
[![r8DEx1.png](https://s3.ax1x.com/2020/12/17/r8DEx1.png)](https://imgchr.com/i/r8DEx1)

git init操作

## 三.Node.js Stream(流)

####  前言：
Stream 是一个抽象接口，Node 中有很多对象实现了这个接口。例如，对http 服务器发起请求的request 对象就是一个 Stream，还有stdout（标准输出）。

### 1.Node.js，Stream 有四种流类型：

Readable - 可读操作。

Writable - 可写操作。

Duplex - 可读可写操作.

Transform - 操作被写入数据，然后读出结果。

所有的 Stream 对象都是 EventEmitter 的实例。
### 2.常用的事件有：

data - 当有数据可读时触发。

end - 没有更多的数据可读时触发。

error - 在接收和写入过程中发生错误时触发。

finish - 所有数据已被写入到底层系统时触发


内容分为：

###读取数据

	// 创建一个可读流
	var readerStream = fs.createReadStream('input.txt');


###写入流

	// 创建一个可写流
	var writerStream = fs.createWriteStream('output.txt');

**样例：**

[![rGYsXD.png](https://s3.ax1x.com/2020/12/17/rGYsXD.png)](https://imgchr.com/i/rGYsXD)


### 链式流
（链式是通过连接输出流到另外一个流并创建多个流操作链的机制。链式流一般用于管道操作。）

	var fs = require("fs");

	// 创建一个可读流
	var readerStream = fs.createReadStream('input.txt');

	// 创建一个可写流
	var writerStream = fs.createWriteStream('output.txt');

	// 管道读写操作
	// 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中
	readerStream.pipe(writerStream);


###管道流（我们把文件比作装水的桶，而水就是文件里的内容，我们用一根管子(pipe)连接两个桶使得水从一个桶流入另一个桶，这样就慢慢的实现了大文件的复制过程。）

生成压缩文件：

	var fs = require("fs");
	var zlib = require('zlib');

	// 压缩 input.txt 文件为 input.txt.gz
	fs.createReadStream('input.txt')
 	 .pipe(zlib.createGzip())
  	.pipe(fs.createWriteStream('input.txt.gz'));
  
	console.log("文件压缩完成。");

解压：

	var fs = require("fs");
	var zlib = require('zlib');

	// 解压 input.txt.gz 文件为 input.txt
	fs.createReadStream('input.txt.gz')
  	.pipe(zlib.createGunzip())
 	 .pipe(fs.createWriteStream('input.txt'));
  
	console.log("文件解压完成。");