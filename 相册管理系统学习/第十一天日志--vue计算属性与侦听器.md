# 第十一天日志
## 一、计算属性
### 1.引入目的
由于模板内表达式，一般仅用于简单运算，对于复杂逻辑，如：

	<div id="example">
  	{{ message.split('').reverse().join('') }}
	</div>

这里表示显示变量message的翻转字符串，当涉及到多处包含时，会难以处理

所以 **复杂逻辑 ** --- 计算属性

### 2.样例及引入

 	<p>Reversed message: "{{ reversedMessage() }}"</p>
	// 在组件中
	methods: {
  	reversedMessage: function () {
    return this.message.split('').reverse().join('')
  	}
	}

定义为属性时，计算属性时基于它的响应式依赖进行缓存的，只有相应式依赖发生改变时，才会重新求值，所以只要message没有改变，多次访问reversedmessage会立刻返回一样的结果。

### 3.计算属性与监听属性的对比

监听属性：watch函数的回调作用

样例：

	<div id="demo">{{ fullName }}</div>

监听属性：（代码命令较为重复）

	var vm = new Vue({
  	el: '#demo',
 	 data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
 	 },
 	 watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
 	 }
	})

计算属性：

	var vm = new Vue({
 	 el: '#demo',
 	 data: {
    firstName: 'Foo',
    lastName: 'Bar'
  	},
  	computed: {                         //fullname表示声明的计算属性
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  	}
	})

### 4.监听函数

	<div id="watch-example">
  	<p>
   	 Ask a yes/no question:
   	 <input v-model="question">
  	</p>
  	<p>{{ answer }}</p>
	</div>

JS

	<!-- 因为 AJAX 库和通用工具的生态已经相当丰富，Vue 核心代码没有重复 -->
	<!-- 提供这些功能以保持精简。这也可以让你自由选择自己更熟悉的工具。 -->
	<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
	<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
	<script>
	var watchExampleVM = new Vue({
	  el: '#watch-example',
  	data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  	},
  	watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  	},
  	created: function () {
    // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
    // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
    // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
    // 请参考：https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  	},
  	methods: {
    getAnswer: function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)'
        return
      }
      this.answer = 'Thinking...'
      var vm = this
      axios.get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error
        })
    }
 	 }
	})
	</script>

在这个示例中，使用 watch 选项允许我们执行异步操作 (访问一个 API)，限制我们执行该操作的频率，并在我们得到最终结果前，设置中间状态(指的是文本框内数据的变化中的状态)。这些都是计算属性无法做到的。