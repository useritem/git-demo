# 第八天日志
## 一、js回顾
###1.document
特点只能window，因为它是window.document是在浏览器中的全局对象window，所以不能在cmd中进行，如：
[![r0TaPP.png](https://s3.ax1x.com/2020/12/21/r0TaPP.png)](https://imgchr.com/i/r0TaPP)

###2.对象中的元素以及匿名函数
1.匿名函数
本例中hello函数不能直接调用（形如hello.set（2））,只能先构造

例如： 

	var a = new hello() ; a.set(2)就可以

[![r07aw9.png](https://s3.ax1x.com/2020/12/21/r07aw9.png)](https://imgchr.com/i/r07aw9)

2。对象中的函数元素本例中，aa这里相当于函数，其中函数可以用匿名函数，因为**a没啥用**，名字只能是aa

[![r07UeJ.png](https://s3.ax1x.com/2020/12/21/r07UeJ.png)](https://imgchr.com/i/r07UeJ)


## 二、 nodejs全局对象

###1.引出概念
而 Node.js 中的全局对象是 global，这里可以理解为JS中的windows全局对象，可以直接访问

在 Node.js 中你不可能在最外层定义变量，因为所有用户代码都是属于当前模块的， 而模块本身不是最外层上下文。
**
注意：** 最好不要使用 var 定义变量以避免引入全局变量，因为全局变量会污染命名空间，提高代码的耦合风险。

### 2.方法：
（1）_filename与 __dirname
输出文件所在位置的绝对路径,以及脚本文件的目录

	// 输出全局变量 __dirname 的值
	console.log( __dirname );

（2）setTimeout(cb, ms)
setTimeout(cb, ms) 全局函数在指定的毫秒(ms)数后执行指定函数(cb)。：setTimeout() 只执行一次指定函数。

	function printHello(){
  	 console.log( "Hello, World!");
	}
	// 两秒后执行以上函数
	setTimeout(printHello, 2000);
对应：setInterval(cb, ms)循环执行
用clearTimeout(t)停止，

	function printHello(){
 	  console.log( "Hello, World!");
	}
	// 两秒后执行以上函数
	var t = setTimeout(printHello, 2000);

	// 清除定时器
	clearTimeout(t);

（3）console方法

console.log相当于printf

	console.log('byvoid%diovyb', 1991); 
	byvoid1991iovyb 
###3.process

#<div style="color: red;">属性</div>

[![r0Oca4.png](https://s3.ax1x.com/2020/12/21/r0Oca4.png)](https://imgchr.com/i/r0Oca4)


#<div style="color: red;">方法</div>：

[![r0OgIJ.png](https://s3.ax1x.com/2020/12/21/r0OgIJ.png)](https://imgchr.com/i/r0OgIJ)


## 三、vue
### 1.attribute 和 property
attribute 是元素标签的属性，property 是元素对象的属性，例如：

	<input id="input" value="test value">
	<script>
	let input = document.getElementById('input');
	console.log(input.getAttribute('value')); // test value
	console.log(input.value); // test value
	</script>

input 的 value attribute 是通过**标签里的 value=“test value”** 定义的，可以通过input.getAttribute(‘value’) 获取，可以通过 input.setAttribute(‘value’, ‘New Value’) 更新


input 的 value property 可通过 **input.value（对象）** 获取和更新，初始值是与 attribute 中的赋值一致的