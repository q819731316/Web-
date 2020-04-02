# Vue.js - Day1

## 课程介绍
学习Vue基本的语法和概念；打包工具 Webpack , Gulp

<!-- 1. MVC 和 MVVM 的区别 -->

<!-- 2. 学习了Vue中最基本代码的结构 -->

<!-- 3. 插值表达式   v-cloak   v-text   v-html   v-bind（缩写是:）   v-on（缩写是@）   v-model   v-for   v-if     v-show -->

<!-- 4. 事件修饰符  ：  .stop   .prevent   .capture   .self     .once -->

<!-- 5. el  指定要控制的区域    data 是个对象，指定了控制的区域内要用到的数据    methods 虽然带个s后缀，但是是个对象，这里可以自定义了方法 -->

 <!-- 6. 在 VM 实例中，如果要访问 data 上的数据，或者要访问 methods 中的方法， 必须带 this -->

<!-- 7. 在 v-for 要会使用 key 属性 （只接受 string / number） -->

<!-- 8. v-model 只能应用于表单元素 -->

<!-- 9. 在vue中绑定样式两种方式  v-bind:class   v-bind:style -->

## 一、入门介绍


### 1.什么是Vue.js

+ Vue.js 是目前最火的一个前端框架，React是最流行的一个前端框架（React除了开发网站，还可以开发手机App， Vue语法也是可以用于进行手机App开发的，需要借助于Weex）

+ <u>Vue.js</u> 是前端的**主流框架之一**，和<u>Angular.js</u>、<u>React.js</u> 一起，并成为前端三大主流框架！

+ Vue.js 是一套构建用户界面的框架，**只关注视图层**，它不仅易于上手，还便于与第三方库或既有项目整合。（Vue有配套的第三方类库，可以整合起来做大型项目的开发）

+ 前端的主要工作？主要负责MVC中的V这一层；主要工作就是和界面打交道，来制作前端页面效果；



### 2.为什么要学习流行框架
+ 企业为了提高开发效率：在企业中，时间就是效率，效率就是金钱；
- 企业中，使用框架，<u>能够提高开发的效率</u>；



+  提高开发效率的发展历程：原生JS -> Jquery之类的类库 -> 前端模板引擎 -> Angular.js / Vue.js

   Angular.js / Vue.js框架：能够帮助我们减少不必要的DOM操作；提高渲染效率；提供了**双向数据绑定**的概念【通过框架提供的指令，我们前端程序员只需要关心数据的业务逻辑，不再关心DOM是如何渲染的了】

+  在Vue中，一个核心的概念，就是让用户不再操作DOM元素，解放了用户的双手，让程序员可以多的时间去关注业务逻辑；



+ 增强自己就业时候的竞争力
- 人无我有，人有我优
- 你平时不忙的时候，都在干嘛？

### 3.框架和库的区别

+ 框架：是一套完整的解决方案(功能完善)；对项目的侵入性较大，项目如果需要更换框架，则需要重新架构整个项目。

   - 比如选择node 中的 express作为后端开放框架，撇弃express而选择原生的HTTP模块来开发，项目就需要重新再来，侵入性较大。
+ 库（插件）：提供某一个小功能，对项目的侵入性较小，如果某个库无法完成某些需求，可以很容易切换到其它库实现需求。
   - 1. 从Jquery 切换到 Zepto
   - 2. EJS 切换到 art-template






## 二、Node（后端）中的 MVC 与 前端中的 MVVM 之间的区别

+ MVC 是后端的分层开发概念；
  - M：Model 主要处理数据的CLUD		
  - V：View看成前端页面
  - C：业务逻辑层
+ MVVM是前端视图层的概念，主要关注于 **视图层分离**，也就是说：MVVM把前端的视图层，分为了 三部分 Model, View , VM ViewModel


![01.MVC和MVVM的关系图解](img/01.MVC和MVVM的关系图解.png)

+ 为什么有了MVC还要有MVVM






## 三、Vue.js 基本代码 和 MVVM 之间的对应关系

Vue之 - `基本的代码结构`和`插值表达式`

```javascript
<head>
	<meta charset="UTF-8">
  	<meta name="viewport" content="width=device-width, initial-scale=1.0">
  	<meta http-equiv="X-UA-Compatible" content="ie=edge">
 	 <title>Document</title>
  	<!-- 1. 导入Vue的包 -->
  	<script src="./lib/vue-2.4.0.js"></script>
     // 当我们导入包之后，在浏览器的内存中，就多了一个 Vue 构造函数
</head>

<body>
	<!-- 将来 new 的Vue实例，会控制这个 元素中的所有内容 -->
V	<!-- Vue 实例所控制的这个元素区域，就是我们的 V  -->
	<div id="app">
    	<p>{{ msg }}</p>          <!-- 插值表达式，不用写data.msg而是直接写msg -->
  	</div>

  	<script>
          <!-- 2. 创建一个Vue的实例 -->
VM 	  	//  注意：我们 new 出来的这个 vm 对象，就是我们 MVVM中的 VM调度者
           
        //Vue()里面传一个配置对象{}
    	var vm = new Vue({
      		el: '#app',                  //表示，当前我们 new 的这个 Vue 实例，要控制页面上的哪个区域       
M    		// 这里的 data 就是 MVVM中的 M，专门用来保存 每个页面的数据的
      		data: {                         // data 属性中，存放的是el(element) 中要用到的数据
        		msg: '欢迎学习Vue'
             // 通过 Vue 提供的指令，很方便的就能把数据渲染到页面上，程序员不再手动操作DOM元素了
             // 【前端的Vue之类的框架，不提倡我们去手动操作DOM元素了】
      		}
    	})
    </script>
</body>
```





## 四、Vue指令

<!-- 1. 如何定义一个基本的Vue代码结构 -->
<!-- 2. 插值表达式 和  v-cloak   -->
<!-- 3. v-text-->
<!-- 4. v-html -->
<!-- 5. v-bind   Vue提供的属性绑定机制   缩写是 : -->
<!-- 6. v-on     Vue提供的事件绑定机制   缩写是 @ -->

为了方便观看，将本应是<!--  -->格式的注释修改为 //

### 1.插值表达式和 `v-cloak`、`v-text`和`v-html`

v-cloak 能够解决插值表达式闪烁的问题。

v-cloak正常显示元素原本内容，而v-text和v-html都会覆盖元素中原本内容。另外v-html的变量内容的标签格式内容会解析成html格式

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  	<meta charset="UTF-8">
  	<meta name="viewport" content="width=device-width, initial-scale=1.0">
  	<meta http-equiv="X-UA-Compatible" content="ie=edge">
  	<title>Document</title>
  	<style>
      	// 当网速慢时，刷新时会将内容闪烁出原本的插值表达式，使用 v-cloak 能够解决插值表达式闪烁的问题
    	[v-cloak] {        
      		 display: none;
    	}
  	</style>
</head>

<body>
  	<div id="app">
      	
    	<p v-cloak>++++++++ {{ msg }} ----------</p>
    	<h4 v-text="msg">==================</h4>    
        
    	//两个效果一样，但默认 v-text 是没有闪烁问题的 
   	 	//而且v-text会覆盖元素中原本的内容，而插值表达式只会替换自己的这个占位符，不会把整个元素的内容清空
        	//v-cloak正常显示msg外的其他内容，而v-text不显示msg外内容，解析到v-text会将msg完全将标签中内容覆				盖。		v-html也会覆盖，msg内容的标签格式内容会解析成html格式
        
		//v-cloak和v-text都会将内容当做文本输出到网页，不会解析成为标签(msg2中的h1)。使用v-html输出为html格式 
        
    	<div>{{msg2}}</div>
    	<div v-text="msg2"></div>
    	<div v-html="msg2">1212112</div>

  	<script src="./lib/vue-2.4.0.js"></script>

  	<script>
    	var vm = new Vue({
        	el: '#app',
              	data: {
                  	msg: '123',
                  	msg2: '<h1>哈哈，我是一个大大的H1， 我大，我骄傲</h1>',
          		}
    	})

	</script>
</body>

</html>
```

### 2.`v-bind`属性绑定

v-bind用来绑定**属性**。

v-bind的三种用法:

1. 直接使用指令`v-bind`
2. 使用简化指令`:`
3. 在绑定的时候，**拼接**绑定内容：`:title="btnTitle + '这是追加的内容'"`


```javascript
<body>
    <div id="app">

		//v-bind: 是 Vue中，提供的用于绑定属性的指令。 
              //告诉绑定的属性值mytitle是一个变量，去寻找变量的内容替换掉mytitle
      
         //将mytitle的内容作为title的属性值，不绑定的话只显示mytitle，不显示实际内容
		<input type="button" value="按钮" v-bind:title="mytitle">  
          
         //注意： v-bind: 指令可以被简写为 :要绑定的属性 -->           
         <input type="button" value="按钮" v-bind:title="mytitle + '123'">             
         // v-bind 中，可以写合法的JS表达式。
         		//v-bind会将引号里的内容当做js代码(表达式)去解析，所以可以实现变量+字符串完成内容的追加 
           
  	</div>


  	<script src="./lib/vue-2.4.0.js"></script>

 	<script>
    	var vm = new Vue({
      		el: '#app',
      		data: {
          		msg: '123',
        		msg2: '<h1>哈哈，我是一个大大的H1， 我大，我骄傲</h1>',
        		mytitle: '这是一个自己定义的title'
      		},
    	})

  	</script>
</body>
```

### 3.`v-on`事件绑定

需求：点击按钮，输出一句话

```javascript
/* 不用vue的话，给按钮定义一个id，然后  
document.getElementById('btn').onclick = function(){
	alert('Hello')
} 
*/
```
v-on用来绑定事件：触发按钮的点击事件，会调用vue的事件绑定机制，执行show方法，show方法去msg中method中找。

简化  @

在Vue中，使用事件绑定机制，为元素指定处理函数的时候，如果加了小括号，就可以给函数传参。

```javascript
<body>
    <div id="app">
		
       	// Vue 中提供了 v-on: 事件绑定机制 
      
    		//<input type="button" value="按钮" v-on:click="alert('hello')">  错误✖
         	//报错，alert未定义    原因；和v-bind一样，alert被当做一个变量，msg找不到变量 
          
		<input type="button" value="按钮" v-on:click="show">
         //其他事件，如mouseover
         <input type="button" value="按钮" v-on:mouseover="show">
          //@简化
         <input type="button" value="按钮" @click="show">
  	</div>


  	<script src="./lib/vue-2.4.0.js"></script>

 	<script>
    	var vm = new Vue({
      		el: '#app',
      		data: {
          		msg: '123',
        		msg2: '<h1>哈哈，我是一个大大的H1， 我大，我骄傲</h1>',
        		mytitle: '这是一个自己定义的title'
      		},
      		methods: { 	    // 这个 methods属性中定义了当前Vue实例所有可用的方法    (methods是个对象) 
        		show: function () {
          			alert('Hello');
        		}	
      		}
    	})

  	</script>
</body>
```

#### 实例：跑马灯效果

分析：

1. 给 【开始】 按钮，绑定一个点击事件   v-on   @
2. 在按钮的事件处理函数中，写相关的业务逻辑代码：拿到 msg 字符串，然后 调用 字符串的 substring 来进行字符串的截取操作，把 第一个字符截取出来，放到最后一个位置即可；
3. 为了实现点击下按钮，自动截取的功能，需要把 2 步骤中的代码，放到一个定时器中去；

```javascript
<!-- 1. 导入Vue包 -->
<script src="./lib/vue-2.4.0.js"></script>
```

1. HTML结构：

```javascript
<div id="app">
  	<!-- 2. 创建一个要控制的区域 -->
    <p>{{info}}</p>
    <input type="button" value="开启" v-on:click="go">
    <input type="button" value="停止" v-on:click="stop">
</div>
```

2. Vue实例：

```javascript
// 创建 Vue 实例，得到 ViewModel
// 注意：在 VM实例中，如果想要获取 data 上的数据，或者想要调用 methods 中的方法，必须通过 this.数据属性名  或  this.方法名 来进行访问，这里的this，就表示我们new出来的VM实例对象
var vm = new Vue({
	el: '#app',
  	data: {
		info: '猥琐发育，别浪~~！',
		intervalId: null         // 在data上定义 定时器Id
	},
	methods: {
		go() {
			// 如果当前有定时器在运行，则直接return
			if (this.intervalId != null) {     //防止点几次go按钮就开启多个定时器
				return;
			}
			// 开始定时器    注意定时器中的this指向问题，这里通过es6的箭头函数解决：内部this指向外部this
 			//箭头函数内部的this与外部的this保持一致
			this.intervalId = setInterval(() => {
              	 //重新拼接字符串：获取到 后面的所有字符 + 获取到第一个字符  
				this.info = this.info.substring(1) + this.info.substring(0, 1);
			}, 500);
		},
       	// 注意： VM实例，会监听自己身上data中所有数据的改变，只要数据一发生变化，就会自动把最新的数据，从				         data上同步到页面中去；【好处：程序员只需要关心数据，不需要考虑如何重新渲染DOM页面】
		stop() {     // 停止定时器
			clearInterval(this.intervalId);            
          	 // 每当清除了定时器之后，需要重新把 intervalId 置为 null
             this.intervalId = null;
		}
	}
});
```

### 4.`事件修饰符`

事件绑定机制：v-on，v-on也提供了些事件修饰符

+ .stop       阻止冒泡
+ .prevent    阻止默认事件
+ .capture    添加事件侦听器时使用<u>事件捕获</u>模式

+ .self       只当事件在该元素本身（比如不是子元素）触发时触发回调  （冒泡、捕获不触发）
+ .once       事件只触发一次


```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
  	<title>Document</title>
  	<script src="./lib/vue-2.4.0.js"></script>
  	<style>
    	.inner {
      		height: 150px;
      		background-color: darkcyan;
    	}

    	.outer {
      		padding: 40px;
      		background-color: red;
    	}
  	</style>
</head>

<body>
  	<div id="app">

    	//1.使用  .stop  阻止冒泡 
    	<div class="inner" @click="div1Handler">
      		<input type="button" value="戳他" @click.stop="btnHandler">
    	</div>

    	//2.使用 .prevent 阻止默认行为 
    	<a href="http://www.baidu.com" @click.prevent="linkClick">有问题，先去百度</a>

    	//3.使用  .capture 实现捕获触发事件的机制(由外到里)
    	<div class="inner" @click.capture="div1Handler">
      		<input type="button" value="戳他" @click="btnHandler">
    	</div> 

   		//4.使用 .self 实现只有点击当前元素时候，才会触发事件处理函数
    	<div class="inner" @click.self="div1Handler">
      		<input type="button" value="戳他" @click="btnHandler">
    	</div> 

        //5.使用 .once 只触发一次事件处理函数 -->
    	<a href="http://www.baidu.com" @click.prevent.once="linkClick">有问题，先去百度</a>


    	//演示： .stop 和 .self 的区别     .stop阻止所有冒泡，.self只阻止当前元素的冒泡
        // .stop阻止所有冒泡，只有按钮有，div1和div2都没触发事件了
    	<div class="outer" @click="div2Handler">
      		<div class="inner" @click="div1Handler">
        		<input type="button" value="戳他" @click.stop="btnHandler">
      		</div>
    	</div>

    	//.self只会阻止自己身上冒泡行为的触发，并不会真正阻止冒泡的行为 
        //按钮有，div1自己没触发，通过自己.self阻止了按钮冒到自己身上的，但是div2触发了
    	<div class="outer" @click="div2Handler">
      		<div class="inner" @click.self="div1Handler">
        		<input type="button" value="戳他" @click="btnHandler">
      		</div>
    	</div>

  </div>

    <script>
    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {},
      		methods: {
        		div1Handler() {
          			console.log('这是触发了 inner div 的点击事件')
        		},
        		btnHandler() {
          			console.log('这是触发了 btn 按钮 的点击事件')
        		},
        		linkClick() {
          			console.log('触发了连接的点击事件')
        		},
        		div2Handler() {
          			console.log('这是触发了 outer div 的点击事件')
        		}
      		}
    	});
  	</script>
</body>

</html>
```

### 5.`v-model`和`双向数据绑定`

唯一只有`v-model`可以实现双向数据绑定，之前的`v-bind`、`v-on`都不行，都是单向的。

修改M层，`vm.msg = '修改了数据'`，会将V层的修改。但是反之不行。而`v-model`可以将V层的表单修改后，将M层的也修改。双向绑定。

注意： v-model 只能运用在表单元素中   input(radio, text, address, email....)    select     checkbox    textarea

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  	<meta charset="UTF-8">
  	<meta name="viewport" content="width=device-width, initial-scale=1.0">
  	<meta http-equiv="X-UA-Compatible" content="ie=edge">
  	<title>Document</title>
  	<script src="./lib/vue-2.4.0.js"></script>
</head>

<body>
  	<div id="app">
    	<h4>{{ msg }}</h4>

    	//v-bind 只能实现数据的单向绑定，从 M 自动绑定到 V， 无法实现数据的双向绑定
    	<input type="text" v-bind:value="msg" style="width:100%;">

    	//使用  v-model 指令，可以实现 表单元素和 Model 中数据的双向数据绑定
    	/*注意： v-model 只能运用在表单元素中
          input(radio, text, address, email....)   select    checkbox   textarea*/
    	<input type="text" style="width:100%;" v-model="msg">
  	</div>

  	<script>
    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {
        		msg: '大家都是好学生，爱敲代码，爱学习，爱思考，简直是完美，没瑕疵！'
      		},
      		methods: {
      		}
    	});
  	</script>
</body>

</html>
```

#### v-model简易计算器案例

1. HTML 代码结构

```javascript
<div id="app">
	<input type="text" v-model="n1">

	<select v-model="opt">
		<option value="+">+</option>
		<option value="-">-</option>
      	<option value="*">*</option>
      	<option value="/">/</option>
    </select>

	<input type="text" v-model="n2">
	<input type="button" value="=" @click="calc">

    <input type="text" v-model="result">
</div>
```

2. Vue实例代码：

```javascript
// 创建 Vue 实例，得到 ViewModel
var vm = new Vue({
	el: '#app',
	data: {
		n1: 0,
		n2: 0,
		result: 0,
		opt: '+'
	},
	methods: {
		calc() { // 计算器算数的方法  
          	// 逻辑：
			switch (this.opt) {
            	case '+':
              		this.result = parseInt(this.n1) + parseInt(this.n2)
              		break;
            	case '-':
              		this.result = parseInt(this.n1) - parseInt(this.n2)
              		break;
            	case '*':
              		this.result = parseInt(this.n1) * parseInt(this.n2)
              		break;
            	case '/':
              		this.result = parseInt(this.n1) / parseInt(this.n2)
              		break;
          	} 

          // 法2：注意：下面这是投机取巧的方式，正式开发中，尽量少用
          var codeStr = 'parseInt(this.n1) ' + this.opt + ' parseInt(this.n2)'
          this.result = eval(codeStr);    //eval()能将字符串解析执行拿到结果。可计算某个字符串，并执行代码
        }
      }
    });
```

### 6.迭代`v-for`和`key`属性

1. 迭代数组

   (1)for循环普通数组

```javascript
<div id="app">
  /*
    <p>{{list[0]}}</p>
    <p>{{list[1]}}</p>
    <p>{{list[2]}}</p>
    <p>{{list[3]}}</p>
    <p>{{list[4]}}</p> 
  */
    <p v-for="item in list">{{item}}</p>       //等效上面，另外还有一个索引可以添加
    <p v-for="(item, i) in list">索引值：{{i}} --- 每一项：{{item}}</p>
    
</div>
    
<script>
	// 创建 Vue 实例，得到 ViewModel
	var vm = new Vue({
      	el: '#app',
      	data: {
        	list: [1, 2, 3, 4, 5, 6]
      	},
      	methods: {}
    });
</script>
```

​	(2)for循环对象数组

```javascript
<ul>
  	<li v-for="user in list">ID：{{user.id}} --- 名字：{{user.age}}</li>
	<li v-for="(user, i) in list">索引：{{i}} --- ID：{{user.id}} --- 名字：{{user.age}}</li>
</ul>
  
<script>
	// 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      	el: '#app',
      	data: {
        	list: [
          		{ id: 1, name: 'zs1' },
          		{ id: 2, name: 'zs2' },
          		{ id: 3, name: 'zs3' },
          		{ id: 4, name: 'zs4' }
        	]
      	},
      	methods: {}
    });
 </script>
```

1. 迭代对象中的**属性**

   数据是对象时可以通过key和val遍历

```javascript
//循环遍历对象身上的属性
<div v-for="(val, key) in user">值：{{val}} --- 键：{{key}}</div>
//注意：在遍历对象身上的键值对的时候， 除了 有  val  key  ,在第三个位置还有一个索引 
<div v-for="(val, key, i) in user">值：{{val}} --- 键：{{key}} --- 索引：{{i}}</div>

<script>
    // 创建 Vue 实例，得到 ViewModel
	var vm = new Vue({
      	el: '#app',
      	data: {
        	user: {
          		id: 1,
          		name: 'ch',
          		gender: '男'
        	}
      	},
      	methods: {}
    });
  </script>
```

2. 迭代数字

```javascript
// in 后面我们放过 普通数组，对象数组，对象， 还可以放数字
// 注意：如果使用 v-for 迭代数字的话，前面的count值从 1 开始(而不是0)
<p v-for="i in 10">这是第 {{i}} 个P标签</p>          

<script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      	el: '#app',
      	data: {},
      	methods: {}
    });
</script>
```

**key属性**

注意不是上面遍历循环用到的键key

> 2.2.0+ 的版本里，**当在组件中使用** v-for 时，**<u>key 现在是必须的</u>**。
>
> (没用key，没有指定默认选中是哪个。若原本勾选第五项，点击按钮在开头添加一项后，勾就成上一个了(从头数还是第5)。选中记录的是项数，而不是对应的哪个)



当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用 “**就地复用**” 策略。如果数据项的顺序被改变，Vue将**不是移动 DOM 元素来匹配数据项的顺序**， 而是**简单复用此处每个元素**，并且确保它在特定索引下显示已被渲染过的每个元素。



为了给 Vue 一个提示，**以便它能跟踪每个节点的身份，从而重用和重新排序现有元素**，你需要为每项提供一个唯一 key 属性。

```javascript
<body>
    <div id="app">

    <div>
      	<label>Id:
        	<input type="text" v-model="id">				//与data中的id双向绑定
      	</label>

      	<label>Name:
        	<input type="text" v-model="name">
      	</label>

      	<input type="button" value="添加" @click="add">
    </div>

    //注意： v-for 循环的时候，key属性只能使用 number或string 
    //注意： key 在使用的时候，必须使用 v-bind 属性绑定的形式，指定 key 的值 -->
    //在组件中，使用v-for循环的时候，或者在一些特殊情况中，如果 v-for 有问题，必须在使用 v-for 的同时，指定唯一的 字符串/数字      类型   :key 值 
    	<p v-for="item in list" :key="item.id">            //key绑定
      		<input type="checkbox">{{item.id}} --- {{item.name}}
    	</p>
	</div>

	<script>
    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {
        		id: '',
        		name: '',
        		list: [
          			{ id: 1, name: '李斯' },
          			{ id: 2, name: '嬴政' },
          			{ id: 3, name: '赵高' },
          			{ id: 4, name: '韩非' },
          			{ id: 5, name: '荀子' }
        		]
      		},
      		methods: {
        		add() { // 添加方法
          			this.list.unshift({ id: this.id, name: this.name })
        		}
      		}
    	});
	</script>
</body>
```

### 7.`v-if`和`v-show`切换显示

`v-if`和`v-show`切换显示

```javascript
<body>
    <div id="app">

    	//<input type="button" value="toggle" @click="toggle">
    	<input type="button" value="toggle" @click="flag=!flag">

    	//v-if 的特点：每次都会重新删除或创建元素。有较高的切换性能消耗
    	//v-show 的特点： 每次不会重新进行DOM的删除和创建操作，只是切换了元素的 display:none 样式。有较高的初始						渲染消耗

  
    	//如果元素涉及到频繁的切换，最好不要使用 v-if, 而是推荐使用 v-show
    	//如果元素可能永远也不会被显示出来被用户看到，则推荐使用 v-if
    	<h3 v-if="flag">这是用v-if控制的元素</h3>
    	<h3 v-show="flag">这是用v-show控制的元素</h3>

  	</div>

  	<script>
    	// 创建 Vue 实例，得到 ViewModel
   	 	var vm = new Vue({
      		el: '#app',
      		data: {
        		flag: false
      		},
      		methods: {
        		/* 方法1
        		toggle() {
          			this.flag = !this.flag
        		} */
      		}
    	});
  	</script>
</body>
```

> 一般来说，v-if 有**更高的切换消耗**而 v-show 有**更高的初始渲染消耗**。因此，如果需要频繁切换 v-show 较好，如果在运行时条件不大可能改变 v-if 较好。

**v-if 的特点：**每次都会重新删除或创建元素。有较高的切换性能消耗

**v-show 的特点：** 每次不会重新进行DOM的删除和创建操作，只是切换了元素的 display:none 样式。有较高的初始						渲染消耗


如果元素涉及到频繁的切换，最好不要使用 v-if, 而是推荐使用 v-show
如果元素可能永远也不会被显示出来被用户看到，则推荐使用 v-if






## 五、在Vue中使用样式

### 1.使用class样式

需要使用  v-bind 做数据绑定

1. 数组
```javascript
//第一种使用方式，直接传递一个数组，注意： 这里的 class 需要使用  v-bind 做数据绑定
<h1 :class="['red', 'thin']">这是一个邪恶的H1</h1>              //里面是字符串形式，不加''是变量形式找不到
```

2. 数组中使用三元表达式
```javascript
<h1 :class="['red', 'thin', isactive?'active':'']">这是一个邪恶的H1</h1>
//isactive在data中定义为boolean形式，true就是'active'，false就是'' 
```

3. 数组中嵌套对象
```javascript
//在数组中使用 对象来代替三元表达式，提高代码的可读性
<h1 :class="['red', 'thin', {'active': isactive}]">这是一个邪恶的H1</h1>
//active类名 isactive为true时，就应用此样式
```

4. 直接使用对象
```javascript
//在为 class 使用 v-bind 绑定对象的时候，对象的属性是类名，由于对象的属性可带引号，也可不带引号，所以这里没写引号；  属性的值 是一个标识符
<h1 :class="{red:true, italic:true, active:true, thin:true}">这是一个邪恶的H1</h1>
```

### 2.使用内联样式style

1. 直接在元素上通过 `:style` 的形式，书写样式对象
```javascript
//对象就是无序键值对的集合
<h1 :style="{color: 'red', 'font-size': '40px'}">这是一个善良的H1</h1>
```

2. 将样式对象，定义到 `data` 中，并直接引用到 `:style` 中
+ 在data上定义样式：
```javascript
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' }
}
```
+ 在元素中，通过属性绑定的形式，将样式对象应用到元素中：
```javascript
<h1 :style="h1StyleObj">这是一个善良的H1</h1>
```

3. style在 `:style` 中通过数组，引用多个 `data` 上的样式对象
+ 在data上定义样式：
```javascript
data: {
        h1StyleObj: { color: 'red', 'font-size': '40px', 'font-weight': '200' },
        h1StyleObj2: { fontStyle: 'italic' }
}
```
+ 在元素中，通过属性绑定的形式，将样式对象应用到元素中：
```javascript
<h1 :style="[h1StyleObj, h1StyleObj2]">这是一个善良的H1</h1>
```



# Vue.js - Day2

## 一、品牌管理案例

1. 导包：bootstrap-3.3.7、vue-resource-1.3.4、vue-2.4.0


2. 创建控制元素


3. 创建控制实例

### 1.完整代码

HTML代码结构(用了bootstrap)

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  	<meta charset="UTF-8">
  	<meta name="viewport" content="width=device-width, initial-scale=1.0">
  	<meta http-equiv="X-UA-Compatible" content="ie=edge">
  	<title>Document</title>
  	<script src="./lib/vue-2.4.0.js"></script>
  	<link rel="stylesheet" href="./lib/bootstrap-3.3.7.css">
  	//需要用到Jquery吗？？？Jquery封装了一些dom操作，vue能让我们免除操作dom过程。所以vue中尽量不要用jquery -->
</head>

<body>
  	<div id="app">

    	<div class="panel panel-primary">
      		<div class="panel-heading">
        		<h3 class="panel-title">添加品牌</h3>
      		</div>
      		<div class="panel-body form-inline">
        		<label>
          			Id:
          			<input type="text" class="form-control" v-model="id">
        		</label>

        		<label>
          			Name:
         		 	<input type="text" class="form-control" v-model="name">
        		</label>

        		//在Vue中，使用事件绑定机制，为元素指定处理函数的时候，如果加了小括号，就可以给函数传参了
        		<input type="button" value="添加" class="btn btn-primary" @click="add()">

        		<label>
          			搜索名称关键字：
          			<input type="text" class="form-control" v-model="keywords">
        		</label>
      		</div>      
		</div>


		<table class="table table-bordered table-hover table-striped">
			<thead>
        		<tr>
          			<th>Id</th>
          			<th>Name</th>
          			<th>Ctime</th>
          			<th>Operation</th>
        		</tr>
      		</thead>
      		<tbody>
        		//之前，v-for中的数据，都是直接从data的list中直接渲染过来的。不写死list，否则永远都是固定的列表
        		//现在，我们自定义了一个 search 方法，同时，把所有的关键字，通过传参的形式，传递给了search方法
        		//在 search 方法内部，通过执行for循环， 把所有符合搜索关键字的数据，保存到一个新数组中，返回
        		<tr v-for="item in search(keywords)" :key="item.id"> 
          			<td>{{ item.id }}</td>						 
          			<td v-text="item.name"></td>
          			<td>{{ item.ctime }}</td>
          			<td>
            			<a href="" @click.prevent="del(item.id)">删除</a>       //阻止  默认行为：刷新页面
          			</td>
        		</tr>
      		</tbody>
		</table>



	</div>

	<script>
    // 创建 Vue 实例，得到 ViewModel
	var vm = new Vue({
		el: '#app',
		data: {
        	id: '',
        	name: '',
       	 	keywords: '',             // 搜索的关键字
        	list: [
          		{ id: 1, name: '奔驰', ctime: new Date() },
          		{ id: 2, name: '宝马', ctime: new Date() }
        	]
      	},
      	methods: {
        	add() { // 添加的方法
          		var car = { id: this.id, name: this.name, ctime: new Date() }
          		this.list.push(car)
          		this.id = this.name = ''
        	},
        	del(id) { // 根据Id删除数据
          		var index = this.list.findIndex(item => {
            		if (item.id == id) {
              			return true;
            		}
          		})
          		this.list.splice(index, 1)
        	},
        	search(keywords) { 		// 根据关键字，进行数据的搜索
          		return this.list.filter(item => {
            		if (item.name.includes(keywords)) {
              			return item;
            		}
          		})
        	}
      	}
	});
	</script>
</body>

</html>
```



### 2.添加新品牌

分析：

1. 获取到 id 和 name ,直接从 data 上面获取 

2. 将获取的id和name组织出一个对象

3. 把这个对象，调用数组的相关方法，添加到当前 data 上的list 中

4. 注意：在Vue中，已经实现了数据的双向绑定，每当我们修改了 data 中的数据，Vue会默认监听到数据的改动，自动把最新的数据，应用到页面上；

   ​

5. 当我们意识到上面的第四步的时候，就证明大家已经入门Vue了，我们更多的是在进行 VM中 Model 数据的操作，同时，在操作Model数据的时候，指定的业务逻辑操作；

```javascript
add() { // 添加的方法
	var car = { id: this.id, name: this.name, ctime: new Date() };       //组织出对象
	this.list.push(car);		//新对象添加到数组中的list上
	this.id = this.name = '';     //添加后清空输入栏
},
```



### 3.删除品牌

分析：
1. 如何根据Id，找到要删除这一项的索引 			（如果是数据库，找到直接写sql语句就可以了）
2. 如果找到索引了，直接调用 数组的 splice 方法

```javascript
del(id) { // 根据Id删除数据
	/* 方法1：
	this.list.some((item, i) => {       //some()还是forEach()?
		if (item.id == id) {
			this.list.splice(i, 1);      //删除
			// 在 数组的 some 方法中，如果 return true，就会立即终止这个数组的后续循环
			return true;
		}
	}) */
  
	//方法2：数组新增findIndex()方法。专门用来查找索引的，而法1的some()可以在内部做任何事情
	var index = this.list.findIndex(item => {         
		if (item.id == id) {
			return true;
		}
	})
	// console.log(index)
	this.list.splice(index, 1);
},
```

### 4.根据条件筛选品牌

```javascript
search(keywords) { 		// 根据关键字，进行数据的搜索
	 /* 方法1；
	var newList = []
	this.list.forEach(item => {       //遍历每一项
		if (item.name.indexOf(keywords) != -1) {
			newList.push(item);    //存在就push进新数组
      	}
	})
	return newList */

  	//法2：filter
  	// 注意：  forEach   some   filter   findIndex   这些都属于数组的新方法，
	//  都会对数组中的每一项，进行遍历，执行相关的操作；
	return this.list.filter(item => {
		//if(item.name.indexOf(keywords) != -1)            用过的indexOf方法 

	// 注意 ： ES6中，为字符串提供了一个新方法，叫做  String.prototype.includes('要包含的字符串')
	// 如果包含，则返回 true ，否则返回 false
    //Jquery有个类似的contain  $(':contain(keywords)').forEach
		if (item.name.includes(keywords)) {
			return item;
		}
	})
	// return newList
}
```



在2.x版本中**手动实现筛选的方式**：                https://cn.vuejs.org/v2/guide/list.html#显示过滤-排序结果

+ 筛选框绑定到 VM 实例中的 `searchName` 属性：

```
<hr> 输入筛选名称：
<input type="text" v-model="searchName">
```

+ 在使用 `v-for` 指令循环每一行数据的时候，不再直接 `item in list`，而是 `in` 一个 过滤的methods 方法，同时，把过滤条件`searchName`传递进去：

```javascript
<tbody>
	<tr v-for="item in search(searchName)">
		<td>{{item.id}}</td>
		<td>{{item.name}}</td>
		<td>{{item.ctime}}</td>
		<td>
			<a href="#" @click.prevent="del(item.id)">删除</a>
		</td>
	</tr>
</tbody>
```

+ `search` 过滤方法中，使用 数组的 `filter` 方法进行过滤：

```javascript
search(name) {
	return this.list.filter(x => {
    	return x.name.indexOf(name) != -1;
  	});
}
```



**(已废除)**1.x 版本中的filterBy指令，在2.x中已经被废除：

[filterBy - 指令]：          https://v1-cn.vuejs.org/api/#filterBy)

```
<tr v-for="item in list | filterBy searchName in 'name'">
  	<td>{{item.id}}</td>
  	<td>{{item.name}}</td>
  	<td>{{item.ctime}}</td>
  	<td>
    	<a href="#" @click.prevent="del(item.id)">删除</a>
  	</td>
</tr>
```



### 补充：Vue调试工具`vue-devtools`的安装步骤和使用

Vue.js devtools - 翻墙安装方式 :      https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=zh-CN

M上的数据在控制台都可以实时看到。

## 三、过滤器

概念：Vue.js 允许你自定义过滤器，**可被用作一些常见的文本格式化**。过滤器可以用在两个地方：**mustache 插值和 v-bind 表达式**。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符 '' | '' 指示；

对数据在输出前做进一步的处理。过滤器只是做输出前最后一层的处理，M中的原数据并没有被修改。就像自拍加滤镜，自己人没变，只是照片滤镜(过滤器)后改变了。

比如上面案例，我们可以通过过滤器将时间格式化成我们要的格式。具体见day2代码：02.品牌列表案例2也可以看下方过滤器。

```javascript
过滤器调用时格式		{{ name | 过滤器的名称}}    

//过滤器的定义语法。			要对传过来的数据做怎样的处理
// 过滤器中的 function ，第一个参数，已经被规定死了，永远都是 过滤器 管道符前面 传递过来的数据
Vue.filter('过滤器名称'，function(data){
  	return data + 123;       //只要调用过滤器，传过来的data就作处理，再返回给管道符前
})
```

一个简单的例子：过滤器的基本使用

过滤器调用时可以传参,而且也可以在调用时传递多个参数。而且过滤器可以多次来调用。

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  	<meta charset="UTF-8">
  	<meta name="viewport" content="width=device-width, initial-scale=1.0">
  	<meta http-equiv="X-UA-Compatible" content="ie=edge">
  	<title>Document</title>
  	<script src="./lib/vue-2.4.0.js"></script>
</head>

<body>
    <div id="app">
    	<p>{{ msg | msgFormat('疯狂', '123') | test }}</p>      //过滤器调用时可以传参，传要修改成的数据
  	</div>

  	<script>
    	// 定义一个 Vue 全局的过滤器，名字叫做msgFormat
    	Vue.filter('msgFormat', function (msg, arg, arg2) {     //调用时传递多个参数
      		// 字符串的  replace 方法，第一个参数，除了可写一个字符串之外，还可以定义一个正则表达式
          	//如果第一个参数直接写要替换的字符"单纯"，只会替换第一个。后面不替换
      		return msg.replace(/单纯/g, arg + arg2);     //替换为 arg + arg2
    	})
		
        //过滤器可以多次来调用。
    	Vue.filter('test', function (msg) {
      		return msg + '测试第二次调用过滤器';
    	})


    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {
        		msg: '曾经，我也是一个单纯的少年，单纯的我，傻傻的问，谁是世界上最单纯的男人'
      		},
      		methods: {}
    	});
  	</script>
</body>

</html>
```

### 1.全局过滤器

全局过滤器：所有的vm实例都可以共享的过滤器。filter，不带s。			(全局不带s，私有带) 

1. HTML元素

```javascript
//如果不传格式
<td>{{ item.ctime | dateFormat() }}</td>		//	{{ item.ctime | dataFormat }}	
//传入格式
<td>{{ item.ctime | dataFormat('yyyy-MM-DD') }}</td>
```

2.  <script>标签中定义全局过滤器

```javascript
// 定义一个全局过滤器							//在参数列表中 通过 pattern="" 来指定形参默认值，防止报错
Vue.filter('dataFormat', function (dateStr, pattern = '') {	//ES6优化：参数默认值，或如下方注释掉的所写
  													
  	//根据给定的事件字符串，进行时间的格式化
    var dt = new Date(dateStr);
  	// 获取年月日  yyyy-mm-dd
  	var y = dt.getFullYear();
  	var m = (dt.getMonth() + 1).toString().padStart(2, '0');
  	var d = dt.getDate().toString().padStart(2, '0');

  	// 如果 传递进来的字符串类型，转为小写之后，等于 yyyy-mm-dd，那么就返回 年-月-日
  	// 否则，就返回  年-月-日 时：分：秒
  	//if (pattern && pattern.toLowerCase() === 'yyyy-mm-dd') {
  	if (pattern.toLowerCase() === 'yyyy-mm-dd') {
    	return `${y}-${m}-${d}`;
  	} else {
    	// 获取时分秒
    	var hh = dt.getHours().toString().padStart(2, '0');
    	var mm = dt.getMinutes().toString().padStart(2, '0');
    	var ss = dt.getSeconds().toString().padStart(2, '0');

    	return `${y}-${m}-${d} ${hh}:${mm}:${ss}`;
  	}
});
```



> 注意：当有局部和全局两个名称相同的过滤器时候，会以就近原则进行调用，即：私有过滤器优先于全局过滤器被调用！



padStart()方法补充：

不用判断，通过padStart将时间中小于10的补足。如8补为08。

> 使用ES6中的字符串新方法 **String.prototype.padStart(maxLength, fillString='')** 或 **String.prototype.padEnd(maxLength, fillString='')**来在**头部**和**尾部**填充字符串；
>
> 第一个参数：补充完后字符串长度，第二个参数：用什么来补充
>
> 使用时，记得将数字通过toString方法转为字符串，再调用padStart()



### 2.私有过滤器

私有局部过滤器：**只能在当前VM对象所控制的 View 区域进行使用**。filters，带s。

​	注意：和methods一样，虽然filters也带了s。但是也是对象。 (js中，一般带s的是数组)

1. HTML元素：

```javascript
<td>{{item.ctime | dataFormat('yyyy-mm-dd')}}</td>
```

2. 私有 `filters` 定义方式：

```javascript
//自定义一个私有的过滤器。在VM对象中添加
var vm2 = new Vue({
      el: '#app2',
      data: {
        dt: new Date()
      },
      methods: {},
      filters: {     // 私有局部过滤器，过滤器有两个 条件  【过滤器名称 和 处理函数】
          dataFormat(input, pattern = "") {    // 在参数列表中 通过 pattern="" 来指定形参默认值，防止报错
              var dt = new Date(input);
              // 获取年月日
              var y = dt.getFullYear();
              var m = (dt.getMonth() + 1).toString().padStart(2, '0');
              var d = dt.getDate().toString().padStart(2, '0');

              // 如果 传递进来的字符串类型，转为小写之后，等于 yyyy-mm-dd，那么就返回 年-月-日
              // 否则，就返回  年-月-日 时：分：秒
              if (pattern.toLowerCase() === 'yyyy-mm-dd') {
                  return `${y}-${m}-${d}`;
              } else {
                  // 获取时分秒
                  var hh = dt.getHours().toString().padStart(2, '0');
                  var mm = dt.getMinutes().toString().padStart(2, '0');
                  var ss = dt.getSeconds().toString().padStart(2, '0');

                  //return y + '-' + m + '-' + d + ' ' + hh + ':' + mm + ':' + ss
                  return `${y}-${m}-${d} ${hh}:${mm}:${ss}`;        //模板字符串 (看起来不乱) 
              }
          }
      }
})
```

## 四、键盘修饰符以及自定义键盘修饰符

### 1. 【了解即可】Vue 1.x 中 自定义键盘修饰符

```javascript
Vue.directive('on').keyCodes.f2 = 113;
```

### 2. Vue 2.x 中 自定义键盘修饰符

Vue中自定义键盘修饰符(全部看手册)：        https://cn.vuejs.org/v2/guide/events.html#按键码

1. 使用自定义的按键修饰符：

```javascript
<input type="text" v-model="name" @keyup="add">    

//抬起enter键，触发事件
<input type="text" v-model="name" @keyup.enter="add">    
  
//没提供的自定义键盘修饰符就无法通过这种方式使用，但是可以使用ASCII码实现
<input type="text" v-model="name" @keyup.f2="add">          //无效，f2的ASCII码是113
<input type="text" v-model="name" @keyup.113="add">         //有用
```

2. 通过`Vue.config.keyCodes.名称 = 按键值`来自定义按键修饰符的别名：

```javascript
Vue.config.keyCodes.f2 = 113;
//然后就可以直接使用f2
<input type="text" v-model="name" @keyup.f2="add"> 
```
## 五、自定义指令
### 1. Vue 1.x 中 自定义元素指令【已废弃,了解即可】
```javascript
Vue.elementDirective('red-color', {
  bind: function () {
    this.el.style.color = 'red';
  }
});
```
使用方式：
```javascript
<red-color>1232</red-color>
```

### 2. Vue 2.x 中 自定义元素指令

自定义指令手册:                       https://cn.vuejs.org/v2/guide/custom-directive.html

1. 自定义全局和私有的自定义指令：

   (1)全局自定义指令

   使用  **Vue.directive()** 定义全局的指令

   参数1 ： 指令的**名称**。             

   ​	注意：在定义的时候，指令的名称前面，不需要加 v- 前缀。但是在调用的时候，必须在指令名称前加上 v- 前缀来进行调用

   参数2： 是一个**对象**，这个对象身上，有一些指令相关的函数，这些函数可以在特定的阶段，执行相关的操作。

```javascript
//定义全局的指令   v-focus，为绑定的元素自动获取焦点：
Vue.directive('focus', {
	bind: function (el) {          // 每当指令绑定到元素上的时候，会立即执行这个 bind 函数，只执行一次
        // 注意:在每个函数中，第一个参数，永远是 el ，表示被绑定了指令的那个元素，这个el参数，是一个原生的JS对象
        // 在元素刚绑定了指令的时候，还没有插入到DOM中去，这时候，调用focus方法没有作用
        // 因为，一个元素，只有插入DOM之后，才能获取焦点
        // el.focus();      和样式相关的
	},
	inserted: function (el) {   // inserted表示元素插入到DOM中的时候，会执行inserted 函数【触发1次】
		el.focus()
		// 和JS行为有关的操作，最好在 inserted 中去执行，放置 JS行为不生效
	},
	updated: function (el) {    // 当VNode更新的时候，会执行 updated， 可能会触发多次
		    
	}
  	//还有两个可以看手册查阅
})
```
**例子**：自定义一个 设置字体颜色的 指令

​	样式，只要通过指令绑定给了元素，不管这个元素有没有被插入到页面中去，这个元素肯定有了一个内联的样式。将来元素肯定会显示到页面中，这时候，浏览器的渲染引擎必然会解析样式，应用给这个元素

**和样式相关的操作，一般都可以在 bind 执行**

```javascript
Vue.directive('color', {
	bind: function (el, binding) {
		// el.style.color = 'red';
		// console.log(binding.name);

		//也可以传第二个参数通过传入来决定
      	//console.log(binding.value)
		// console.log(binding.expression)
		el.style.color = binding.value
	}
})
```

​	(2)私有自定义指令

```javascript
//自定义私有自定义指令。在VM对象中添加
//自定义私有指令 v-color 和 v-font-weight，为绑定的元素设置指定的字体颜色 和 字体粗细：
var vm2 = new Vue({
      el: '#app2',
      data: {
        dt: new Date()
      },
      methods: {},
      directives: {      //自定义私有自定义指令，有两个参数:
          color: {         // 为元素设置指定的字体颜色
              bind(el, binding) {      
                  // el.style.color = 'red';
                  el.style.color = binding.value;
              }
          },
          'font-weight': function (el, binding2) {         //  自定义指令的简写形式，等同于定义了 bind 和 
				el.style.fontWeight = binding2.value;		  //  update 两个钩子函数         
          }
      }
})
```

2. 自定义指令的使用方式：

```javascript
<input type="text" v-model="searchName" v-focus v-color="'red'" v-font-weight="900">
```

3. 函数简写

大多数情况下，我们可能想在 bind 和 update 钩子上做重复动作，并且不关心其他的钩子函数。可以这样写。

```javascript
Vue.directive('color',function(el,param){
  	el.style.backgroundColor = param.value;
})
//比如上面的'font-weight'就是简写
```

相当于在bind和update上都写了一份

## 六、vue实例的生命周期

vue实例的生命周期手册：           https://cn.vuejs.org/v2/guide/instance.html#实例生命周期

+ 什么是生命周期：从Vue实例创建、运行、到销毁期间，总是伴随着各种各样的事件，这些事件，统称为生命周期！
+ [生命周期钩子](https://cn.vuejs.org/v2/api/#选项-生命周期钩子)：就是生命周期事件的别名而已；
+ 生命周期钩子 = 生命周期函数 = 生命周期事件
+ 主要的生命周期函数分类(根据函数的不同分3类)：
-   创建期间的生命周期函数：
    + beforeCreate：实例刚在内存中被创建出来，此时，还没有初始化好 data 和 methods 属性
    + created：实例已经在内存中创建OK，此时 data 和 methods 已经创建OK，此时还没有开始 编译
    + beforeMount：此时已经完成了模板的编译，但是还没有挂载到页面中
    + mounted：此时，已经将编译好的模板，挂载到了页面指定的容器中显示

-   运行期间的生命周期函数：
    + beforeUpdate：状态更新之前执行此函数， 此时 data 中的状态值是最新的，但是界面上显示的 数据还是旧的，因为此时还没有开始重新渲染DOM节点
    + updated：实例更新完毕之后调用此函数，此时 data 中的状态值 和 界面上显示的数据，都已经完成了更新，界面已经被重新渲染好了！

-   销毁期间的生命周期函数：
    + beforeDestroy：实例销毁之前调用。在这一步，实例仍然完全可用。
    + destroyed：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。


![day2-lifecycle](img/day2-lifecycle.png)




8个生命周期钩子函数

```javascript
<body>
	<div id="app">
    	<input type="button" value="修改msg" @click="msg='No'">
    	<h3 id="h3">{{ msg }}</h3>
  	</div>

  	<script>
    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      	el: '#app',
      	data: {
        	msg: 'ok'
      	},
      	methods: {
        	show() {
          		console.log('执行了show方法')
        	}
      	},
      	beforeCreate() { // 这是我们遇到的第一个生命周期函数，表示实例完全被创建出来之前，会执行它
        	// console.log(this.msg)
        	// this.show()
        	// 注意： 在 beforeCreate 生命周期函数执行的时候，data 和 methods 中的 数据都还没有没初始化
      	},
      	created() { // 这是遇到的第二个生命周期函数
        	// console.log(this.msg)
        	// this.show()
        	//  在 created 中，data 和 methods 都已经被初始化好了！
        	// 如果要调用 methods 中的方法，或者操作 data 中的数据，最早，只能在 created 中操作
      	},
      	beforeMount() { // 这是遇到的第3个生命周期函数，表示 模板已经在内存中编辑完成了，但是尚未把 模板渲染到 页面中
        	// console.log(document.getElementById('h3').innerText)
        	// 在 beforeMount 执行的时候，页面中的元素，还没有被真正替换过来，只是之前写的一些模板字符串
      	},
      	mounted() { // 这是遇到的第4个生命周期函数，表示，内存中的模板，已经真实的挂载到了页面中，用户已经可以看到渲染好的页面了
        	// console.log(document.getElementById('h3').innerText)
        	// 注意： mounted 是 实例创建期间的最后一个生命周期函数，当执行完 mounted 就表示，实例已经被完全创建好了，此时，如果没有其它操作的话，这个实例，就静静的 躺在我们的内存中，一动不动
      	},


      	// 接下来的是运行中的两个事件
      	beforeUpdate() { // 这时候，表示 我们的界面还没有被更新【数据被更新了吗？  数据肯定被更新了】
        	/* console.log('界面上元素的内容：' + document.getElementById('h3').innerText)
        	console.log('data 中的 msg 数据是：' + this.msg) */
        	// 得出结论： 当执行 beforeUpdate 的时候，页面中的显示的数据，还是旧的，此时 data 数据是最新的，页面尚未和 最新的数据保持同步
      	},
      	updated() {
        	console.log('界面上元素的内容：' + document.getElementById('h3').innerText)
        	console.log('data 中的 msg 数据是：' + this.msg)
        	// updated 事件执行的时候，页面和 data 数据已经保持同步了，都是最新的
      	}
    });
</script>
```



## 七、vue-resource 第三方包实现 get, post, jsonp请求

内容较多，详细文档手册： https://github.com/pagekit/vue-resource

除了 vue-resource 之外，还可以使用 `axios` 的第三方包实现实现数据的请求。

1. 之前的学习中，如何发起数据请求？

   jquery发ajax

2. 常见的数据请求类型？  get  post  jsonp

3. 测试的URL请求资源地址：

   - get请求地址： http://vue.studyit.io/api/getlunbo

   - post请求地址：http://vue.studyit.io/api/post

   - jsonp请求地址：http://vue.studyit.io/api/jsonp
```javascript
//全局Vue对象
Vue.http.get('/someUrl', [config]).then(successCallback, errorCallback);
Vue.http.post('/someUrl', [body], [config]).then(successCallback, errorCallback);

//在Vue实例中
this.$http.get('/someUrl', [config]).then(successCallback, errorCallback);
this.$http.post('/someUrl', [body], [config]).then(successCallback, errorCallback);

//[config]是可选项，具体有哪些在文档查看。和.then里的errorCallback都可省去。
//get和jsonp比较像，都是url地址和.then即可。通过.then获取到服务器返回的数据。看到.then知前面的方法this.$http.xxxx是promise来封装的
get(url, [config])
post(url, [body], [config])    //post参数：    地址，提交给服务器的数据，提交格式（可选选项选提交格式）
jsonp(url, [config])
```
**小例子：vue-resource的基本使用** 

```javascript
//vue-resource的基本使用 顺序操作可看后面5-8小点
<!DOCTYPE html>
<html lang="en">

<head>
  	<meta charset="UTF-8">
  	<meta name="viewport" content="width=device-width, initial-scale=1.0">
  	<meta http-equiv="X-UA-Compatible" content="ie=edge">
  	<title>Document</title>
  	<script src="./lib/vue-2.4.0.js"></script>
  	//注意：vue-resource 依赖于 Vue，所以先后顺序要注意:先导入vue再导入vue-resource
    //导入vue包向window对象暴露出Vue,导入vue-resouse包在Vue上挂载了$http属性。
  	<script src="./lib/vue-resource-1.3.4.js"></script>
    //通过this.$http可以点出方法，比如 this.$http.get 、 this.$http.post 、 this.$http.jsonp
</head>

<body>
  	<div id="app">
    	<input type="button" value="get请求" @click="getInfo">
    	<input type="button" value="post请求" @click="postInfo">
    	<input type="button" value="jsonp请求" @click="jsonpInfo">
  	</div>

  	<script>
    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {},
      		methods: {
        		getInfo() {		 // 发起get请求
          			//  当发起get请求之后， 通过 .then 来设置成功的回调函数
          			this.$http.get('http://vue.studyit.io/api/getlunbo').then(function (result) {
            			// 通过 result.body 拿到服务器返回的成功的数据。(data也可用，我们用body) 
            			console.log(result.body);
          			})
        		},
        		postInfo() { 	// 发起 post 请求   表单类型提交格式application/x-wwww-form-urlencoded
          			//  手动发起的 Post 请求，默认没有表单格式，所以，有的服务器处理不了
          			//  通过post方法的第三个参数，{ emulateJSON: true } 设置提交的内容类型为 普通表单数据格式
            		this.$http.post('http://vue.studyit.io/api/post', {}, { emulateJSON: true }).then(result => {           
            			console.log(result.body);
          			})
        		},
        		jsonpInfo() { 	// 发起JSONP 请求
          			this.$http.jsonp('http://vue.studyit.io/api/jsonp').then(result => {
            			console.log(result.body);
          			})
        		}
      		}
    	});
    </script>
</body>

</html>
```

4. JSONP的实现原理
+   由于浏览器的安全性限制，不允许AJAX访问 **协议不同、域名不同、端口号不同**的 数据接口，浏览器认为这种访问不安全；

    > 回顾：
    >
    > ajax跨域中的**同源策略**：浏览器的一种安全策略，所谓同源指的是请求url地址中的协议、域名、端口都相同，只要其中之一不相同就是跨域。
    >
    > ​	主要是为了保证浏览器的安全性，在同源策略下，浏览器不允许ajax跨域获取服务器数据。
    >
    > 解决方案：**<u>jsonp</u>**、domain+iframe、location、hash+frame、window.name+iframe、window.postMessage、flash等第三方插件	

+   可以通过动态创建script标签的形式，把script标签的src属性，指向数据接口的地址，因为script标签不存在跨域限制，这种数据获取方式，称作JSONP   （注意：根据JSONP的实现原理，知晓，**JSONP只支持Get请求**）；

+   具体实现过程：
    - 先在客户端定义一个回调方法，预定义对数据的操作；
    - 再把这个回调方法的名称，通过URL传参的形式，提交到服务器的数据接口；
    - 服务器数据接口组织好要发送给客户端的数据，再拿着客户端传递过来的回调方法名称，拼接出一个调用这个方法的字符串，发送给客户端去解析执行；
    - 客户端拿到服务器返回的字符串之后，当作Script脚本去解析执行，这样就能够拿到JSONP的数据了；

+   通过 Node.js ，来手动实现一个JSONP的请求例子；
    ```javascript
      const http = require('http');
      // 导入解析 URL 地址的核心模块
      const urlModule = require('url');

      const server = http.createServer();
      // 监听 服务器的 request 请求事件，处理每个请求
      server.on('request', (req, res) => {
      	const url = req.url;

        	// 解析客户端请求的URL地址
        	var info = urlModule.parse(url, true);

        	// 如果请求的 URL 地址是 /getjsonp ，则表示要获取JSONP类型的数据
        	if (info.pathname === '/getjsonp') {
          	// 获取客户端指定的回调函数的名称
          	var cbName = info.query.callback;
          	// 手动拼接要返回给客户端的数据对象
          	var data = {
            		name: 'zs',
            		age: 22,
            		gender: '男',
              	hobby: ['吃饭', '睡觉', '运动']
          	}
          	// 拼接出一个方法的调用，在调用这个方法的时候，把要发送给客户端的数据，序列化为字符串，作为参数传递给这个调用的方法：
          	var result = `${cbName}(${JSON.stringify(data)})`;
          	// 将拼接好的方法的调用，返回给客户端去解析执行
          	res.end(result);
        	} else {
          	res.end('404');
        	}
      });

      server.listen(3000, () => {
        	console.log('server running at http://127.0.0.1:3000');
      });
    ```

5. vue-resource 的配置步骤：
+ 直接在页面中，通过`script`标签，引入 `vue-resource` 的脚本文件；
+ 注意：引用的先后顺序是：先引用 `Vue` 的脚本文件，再引用 `vue-resource` 的脚本文件；
6. 发送get请求：
```javascript
getInfo() { // get 方式获取数据
  	this.$http.get('http://127.0.0.1:8899/api/getlunbo').then(res => {
    	console.log(res.body);
  	})
}
```
7. 发送post请求：
```javascript
postInfo() {
  	var url = 'http://127.0.0.1:8899/api/post';
  	// post 方法接收三个参数：
  	// 参数1： 要请求的URL地址
  	// 参数2： 要发送的数据对象
  	// 参数3： 指定post提交的编码类型为 application/x-www-form-urlencoded
  	this.$http.post(url, { name: 'zs' }, { emulateJSON: true }).then(res => {
    	console.log(res.body);
  	});
}
```
8. 发送JSONP请求获取数据：
```javascript
jsonpInfo() { // JSONP形式从服务器获取数据
    var url = 'http://127.0.0.1:8899/api/jsonp';
    	this.$http.jsonp(url).then(res => {
    	console.log(res.body);
  	});
}
```





## 八、配置本地数据库和数据接口API

1. 先解压安装 `PHPStudy`;
2. 解压安装 `Navicat` 这个数据库可视化工具，并激活；
3. 打开 `Navicat` 工具，新建空白数据库，名为 `dtcmsdb4`;
4. 双击新建的数据库，连接上这个空白数据库，在新建的数据库上`右键` -> `运行SQL文件`，选择并执行 `dtcmsdb4.sql` 这个数据库脚本文件；如果执行不报错，则数据库导入完成；
5. 进入文件夹 `vuecms3_nodejsapi` 内部，执行 `npm i` 安装所有的依赖项；
6. 先确保本机安装了 `nodemon`, 没有安装，则运行 `npm i nodemon -g` 进行全局安装，安装完毕后，进入到 `vuecms3_nodejsapi`目录 -> `src`目录 -> 双击运行 `start.bat`
7. 如果API启动失败，请检查 PHPStudy 是否正常开启，同时，检查 `app.js` 中第 `14行` 中数据库连接配置字符串是否正确；PHPStudy 中默认的 用户名是root，默认的密码也是root

## 九、品牌管理改造

### 1.展示品牌列表

 获取所有的品牌列表 

分析：
1. 由于已经导入了 Vue-resource这个包，所以 ，可以直接通过  this.$http 来发起数据请求
2. 根据接口API文档，知道，获取列表的时候，应该发起一个 get 请求
3. this.$http.get('url').then(function(result){})
4. 当通过 then 指定回调函数之后，在回调函数中，可以拿到数据服务器返回的 result
5. 先判断 result.status 是否等于0，如果等于0，就成功了，可以 把 result.message 赋值给 this.list ; 如果不等于0，可以弹框提醒，获取数据失败！

```javascript
getAllList() { // 获取所有的品牌列表 
	this.$http.get('http://www.liulongbin.top:3005/api/getprodlist').then(result => {
		// 注意： 通过 $http 获取到的数据，都在 result.body 中放着
		var result = result.body;
		if (result.status === 0) {
			// 成功了
			this.list = result.message;
		} else {
			// 失败了
			alert('获取数据失败！');
		}
	})
},
```

注意，这个显示全部的方法只是写在了vue实例中的method中。也没其他的按钮绑定，刷新网页就显示，需要调用这方法。我们写在vue生命周期的created() 中，一初始化完数据，就执行此函数。

```javascript
// 创建 Vue 实例，得到 ViewModel
var vm = new Vue({
	el: '#app',
	data: {
		name: '',
		list: [ // 存放所有品牌列表的数组
		]
	},
	created() { // 当 vm 实例 的 data 和 methods 初始化完毕后，vm实例会自动执行created 这个生命周期函数
		this.getAllList()
	},
  	methods: {
		getAllList(){
           
		},
    	add(){
          
    	},
      	de(){
          
      	}
  	}
});  
```

### 2.添加品牌数据

分析：

1. 经过查看 数据API接口，发现，要发送一个 Post 请求，  this.$http.post
2. this.$http.post() 中接收三个参数：
    2.1 第一个参数： 要请求的URL地址
    2.2 第二个参数： 要提交给服务器的数据 ，要以对象形式提交给服务器 { name: this.name }
    2.3 第三个参数： 是一个配置对象，要以哪种表单数据类型提交过去， { emulateJSON: true }, 以普通表单格式，将数据提交给服务器 application/x-www-form-urlencoded
3. 在 post 方法中，使用 .then 来设置成功的回调函数，如果想要拿到成功的结果，需要 result.body

```javascript
add() {  // 添加品牌列表到后台服务器
	this.$http.post('api/addproduct', { name: this.name }, { emulateJSON: true }).then(result => {
		if (result.body.status === 0) {
			// 成功了！
			// 添加完成后，只需要手动，再调用一下 getAllList 就能刷新品牌列表了
			this.getAllList();
			// 清空 name 
			this.name = '';
		} else {
			// 失败了
			alert('添加失败！')
		}
}) 		

//第3个参数 { emulateJSON: true } 可以在全局启用，然后在add()方法中省去。
    
// 全局启用 emulateJSON 选项
Vue.http.options.emulateJSON = true;
```

### 3.删除品牌数据

第一步：

​	为每一个删除链接注册点击事件(事件修饰符阻止跳转的默认行为)，点击将id传过去，以知道删除哪个

```javascript
del(id) { // 删除品牌
	this.$http.get('http://www.liulongbin.top:3005/api/delproduct/' + id).then(result => {
		if (result.body.status === 0) {
			// 删除成功
			this.getAllList()
		} else {
			alert('删除失败！')
		}
	})
}
```

另外，可以通过vue-resource的全局配置。使用全局配置来设置一些默认值。比如请求地址的根域名，当url的域名修改时，整个项目不用去一个个改地址。而是修改根地址(域名)就行。方法内部的不用修改

```javascript
// 如果我们通过全局配置了请求的数据接口 根域名，则在每次单独发起http请求的时候，请求的 url 路径，应该以相对路径开头，前面不能带 /  ，否则不会启用根路径做拼接；
Vue.http.options.root = 'http://www.liulongbin.top:3005/';

//然后方法内部的this.$http.get将地址改为url相对地址。比如 this.$http.get('api/delproduct/' + id)
```

### 4.完整代码

代码段较长，关注js代码而不是V部分的内容

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
  	<meta http-equiv="X-UA-Compatible" content="ie=edge">
  	<title>Document</title>
  	<script src="./lib/vue-2.4.0.js"></script>
  	<script src="./lib/vue-resource-1.3.4.js"></script>
  	<link rel="stylesheet" href="./lib/bootstrap-3.3.7.css">
</head>

<body>
  	<div id="app">

    	<div class="panel panel-primary">
      		<div class="panel-heading">
        		<h3 class="panel-title">添加品牌</h3>
      		</div>
      	<div class="panel-body form-inline">

        	<label>
          		Name:
          		<input type="text" v-model="name" class="form-control">
        	</label>

        	<input type="button" value="添加" @click="add" class="btn btn-primary">
      	</div>
    </div>



    <table class="table table-bordered table-hover table-striped">
		<thead>
        	<tr>
          		<th>Id</th>
          		<th>Name</th>
          		<th>Ctime</th>
          		<th>Operation</th>
        	</tr>
		</thead>
		<tbody>
        	<tr v-for="item in list" :key="item.id">
          		<td>{{item.id}}</td>
          		<td>{{item.name}}</td>
          		<td>{{item.ctime}}</td>
          		<td>
            		<a href="" @click.prevent="del(item.id)">删除</a>
          		</td>
        	</tr>
		</tbody>
	</table>


	</div>

	<script>
    // 如果我们通过全局配置了请求的数据接口 根域名，则在每次单独发起http请求的时候，请求的 url 路径，应该以相对路径开头，前面不能带 /  ，否则不会启用根路径做拼接；
                      
    // 全局配置请求的数据接口 根域名              
	Vue.http.options.root = 'http://www.liulongbin.top:3005/';
	// 全局启用 emulateJSON 选项				
    Vue.http.options.emulateJSON = true;			//设置提交的内容类型为 普通表单数据格式

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
		el: '#app',
		data: {
			name: '',
			list: [ // 存放所有品牌列表的数组
			]
		},
		created() { // 当 vm 实例 的 data 和 methods 初始化完毕后，vm实例会自动执行created 这个生命周期函数
			this.getAllList()
		},
		methods: {
			getAllList() { // 获取所有的品牌列表 
          // 分析：
          // 1. 由于已经导入了 Vue-resource这个包，所以 ，可以直接通过  this.$http 来发起数据请求
          // 2. 根据接口API文档，知道，获取列表的时候，应该发起一个 get 请求
          // 3. this.$http.get('url').then(function(result){})
          // 4. 当通过 then 指定回调函数之后，在回调函数中，可以拿到数据服务器返回的 result
          // 5. 先判断 result.status 是否等于0，如果等于0，就成功了，可以 把 result.message 赋值给 this.list ; 如果不等于0，可以弹框提醒，获取数据失败！

				this.$http.get('api/getprodlist').then(result => {
					// 注意： 通过 $http 获取到的数据，都在 result.body 中放着
					var result = result.body;
					if (result.status === 0) {
						// 成功了
						this.list = result.message;
					} else {
 						// 失败了
						alert('获取数据失败！');
					}
				})
			},
			add() {  // 添加品牌列表到后台服务器
          // 分析：
          // 1. 经过查看 数据API接口，发现，要发送一个 Post 请求，  this.$http.post
          // 2. this.$http.post() 中接收三个参数：
          //   2.1 第一个参数： 要请求的URL地址
          //   2.2 第二个参数： 要提交给服务器的数据 ，要以对象形式提交给服务器 { name: this.name }
          //   2.3 第三个参数： 是一个配置对象，要以哪种表单数据类型提交过去， { emulateJSON: true }, 以普通表单格式，将数据提交给服务器 application/x-www-form-urlencoded
          // 3. 在 post 方法中，使用 .then 来设置成功的回调函数，如果想要拿到成功的结果，需要 result.body
				this.$http.post('api/addproduct', { name: this.name }).then(result => {
					if (result.body.status === 0) {
						// 成功了！
						// 添加完成后，只需要手动，再调用一下 getAllList 就能刷新品牌列表了
						this.getAllList();
						// 清空 name 
						this.name = '';
					} else {
						// 失败了
						alert('添加失败！');
					}
				})
			},
			del(id) { // 删除品牌
				this.$http.get('api/delproduct/' + id).then(result => {
					if (result.body.status === 0) {
						// 删除成功
						this.getAllList();
					} else {
						alert('删除失败！');
					}
				})
			}
		}
	});
    </script>
</body>

</html>
```



## 相关文章
1. [vue.js 1.x 文档](https://v1-cn.vuejs.org/)
2. [vue.js 2.x 文档](https://cn.vuejs.org/)
3. [String.prototype.padStart(maxLength, fillString)](http://www.css88.com/archives/7715)
4. [js 里面的键盘事件对应的键码](http://www.cnblogs.com/wuhua1/p/6686237.html)
5. [pagekit/vue-resource](https://github.com/pagekit/vue-resource)
6. [navicat如何导入sql文件和导出sql文件](https://jingyan.baidu.com/article/a65957f4976aad24e67f9b9b.html)
7. [贝塞尔在线生成器](http://cubic-bezier.com/#.4,-0.3,1,.33)
