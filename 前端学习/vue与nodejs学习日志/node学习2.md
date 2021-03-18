##1、JSON操作

###1、数组对象变为字符串

	JSON.string(obj)
	序列化操作·

###2、字符串变为对象

	JSON.parse(string)
	反序列化操作

但是注意string得符合对象规则，得是这样的：
	
	var dso = '{"1":"sad","2":"das"}'

**npm可以理解为一种包的管理工具**

##2、数据丢失问题

###>1、样例：

	var arr = ['1','w','ad'];
	for(var i = 0;i<arr.length;i++){
	 setTimeout(() => {
            console.log(arr[i])
        },1000);
	}
此时由于执行console.log(arr[i])时，for循环已经执行完毕，所以造成i丢失

所以执行错误，可以用闭包解决

            var arr = ['1','w','ad'];            

            // 闭包解决数据丢失的问题
            for(var i = 0;i<arr.length;i++){
                (function(i){
                    setTimeout(() => {
                        console.log(arr[i])
                    },1000);
                }(i)) //这个（i）表示i的调用
            //    道理在于：当循环执行的时候，自调用的匿名函数会执行，i会传入，
            //    而此时的i是匿名函数的实参，所以可以传入，保证i不会丢失
            }            

法二：其实也可以吧var改为let也可以

###3>异步造成的数据丢失问题：

 	 for(let i = 0 ;i<data.length;i++){
       // var obj ={}
        data_arr[i] = {}
        // 调用let解决异步问题：
     
            fs.stat(data[i],function(err,da){
                //  注意函数为异步操作,当循环结束之后，回调函数才会执行
                // 由于闭包的特点是保存变量，所以采用闭包解决此问题
                cont++
                data_arr[i].name = data[i]
                data_arr[i].size = da.size
                data_arr[i].ntime = moment(da.mtime).format('YYYY-MM-DD hh-mm-ss')
                                                                // 注意这个字母不可以改变
                //    console.log(data[i])

                // 判读文件类型：da中的自带方法da.isFile()
                if(da.isFile() === true){
                    // 代表时文本文件
                    data_arr[i].type = 'f'
                }else{
                    data_arr[i].type = 'd'
                }

                if(cont ===data.length){
                    console.log(data_arr)
                }
                })

       
    //    注意不能在外边进行赋值或者打印，因为当打印进行时，函数还没有开始执行，（异步原因）
       // data_arr.push(obj)
    }

所以特别注意异步函数造成的执行顺序变化的问题

##3、package.json作用

1、记录项目的有关信息，会自动进行更新

<p style="color: red;">牛逼的来了：：：：：</p>

由于package.json中记录了所有的第三方模块，所以一旦在控制台进行npm install操作之后

文件**自动下载所有**的第三方模块的相关信息


所以：

2、package.json方便进行项目管理！！！！！

##package-lock.json作用

1、在于记录有关于模块与项目的详细信息

##4、npm进行路径的修改

通过淘宝的路径来进行下载(指定路径)

	npm install jquery --registry=https://registry.npm.taobao.org

当然了，也可以npm改变为淘宝静态路由cnpm

	npm config set registry https://.npm.taobao.org
