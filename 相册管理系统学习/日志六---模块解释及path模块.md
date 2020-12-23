# 日志六
## 一、模块
### 1.优势：
模块，包括Node内置的模块和来自第三方的模块且相同名字的函数和变量完全可以分别存在不同的模块中，

例：  模块引入;并把greet函数包装为模块并输出

	function greet(name) {
    	console.log(s + ', ' + name + '!');
	}

	module.exports = greet;//这里也可以是对象，函数，数组

	// 引入hello模块:
	var greet = require('./hello');   ./注意其相对路径

### 2.CommonJS规范：
由于相同名字的变量可以在不同模块中使用，（原理：把一个模块变为匿名函数的局部变量）
一个模块想要对外暴露变量（函数也是变量），可以用module.exports = variable;，一个模块要引用其他模块暴露的变量，用var ref = require('module_name');就拿到了引用模块的变量。

### 3.路由
路由简单的说就是对url的分层解析

首先去解析域名或ip

其次去解析到服务器的特定资源文件

再次去解析特定资源的特定状态

## 二、path函数：
### 1.意义及调用
提供了许多非常实用的函数来访问文件系统并与文件系统进行交互。
const path = require('path')
### 2.方法：
该模块提供了 path.sep（作为路径段分隔符，在 Windows 上是 \，在 Linux/macOS 上是 /）和 path.delimiter（作为路径定界符，在 Windows 上是 ;，在 Linux/macOS 上是 :）。

（1）path.basename返回路径的最后一部分。 第二个参数可以过滤掉文件的扩展名：

	require('path').basename('/test/something') //something
	require('path').basename('/test/something.txt') //something.txt
	require('path').basename('/test/something.txt', '.txt') //something

(2)path.dirname---->返回路径的目录部分：

（3）path.extname()---->返回路径的扩展名部分。

(4)path.isAbsolute()------->
如果是绝对路径，则返回 true。

	require('path').isAbsolute('/test/something') // true
	require('path').isAbsolute('./test/something') // false

(5)path.join()
连接路径的两个或多个部分：

	const name = 'joe'
	require('path').join('/', 'users', name, 'notes.txt') //'/users/joe/notes

(6)path.normalize
当包含类似 .、.. 或双斜杠等相对的说明符时，则尝试计算实际的路径：

(7)path.parse
解析对象的路径为组成其的片段：

root: 根路径。

dir: 从根路径开始的文件夹路径。

base: 文件名 + 扩展名

name: 文件名

ext: 文件扩展名

例如：

	require('path').parse('/users/test.txt')
	结果是：

{

  root: '/',

  dir: '/users',

  base: 'test.txt',

  ext: '.txt',

  name: 'test'

}

(8)path.relative()

接受 2 个路径作为参数。 基于当前工作目录，返回从第一个路径到第二个路径的相对路径

(9)path.resolve

可以使用 path.resolve() 获得相对路径的绝对路径计算：
绝对路径，就是从**盘符开始**的路径，例如：“c:\windows\system32\mfc42.dll”。 相对路径，就是从**当前路径开始**的路径，例如，当前路径是“c:\windows”，那么指定前面范例的文件，可以直接写“system32\mfc42.dll”。 注意，路径分隔符“\”在 c 语言里面是转义字符，所以表达路径分隔符需要用“\\”。


	path.resolve('joe.txt') //'/Users/joe/joe.txt' 
如果从主文件夹运行
通过指定第二个参数，resolve 会使用第一个参数作为第二个参数的基准：

	path.resolve('tmp', 'joe.txt') //'/Users/joe/tmp/joe.txt' 
如果从主文件夹运行
如果第一个参数以斜杠开头，则表示它是绝对路径：

	path.resolve('/etc', 'joe.txt') //'/etc/joe.txt'
