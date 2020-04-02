

# Vue.js - Day3

## 一、Vue中的动画

​	Vue中的动画文档手册：  		https://cn.vuejs.org/v2/guide/transitions.html

和CSS3的炫酷动画不同(比如翻转、3D变化等等)，只有很基础的动画，比如淡入淡出和渐变(opacity从0-1)，位置的简单平移(上下左右) 等。

为什么要有动画：动画能够提高用户的体验，帮助用户更好的理解页面中的功能；

### 1.使用过渡类名实现动画

​				                 入场 									离开

![day2-过渡类名](D:\用户Users\chenh\Desktop\笔记\Vue\img\day2-过渡类名.PNG)

入场或离开为半场动画。      具体动画其他查文档手册。

需求： 点击按钮，让 h3 显示，再点击，让 h3 隐藏

过程：

1. 使用 transition 元素，把 需要被动画控制的元素，包裹起来
2. 自定义两组样式，来控制 transition 内部的元素实现动画

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  	<meta charset="UTF-8">
  	<meta name="viewport" content="width=device-width, initial-scale=1.0">
  	<meta http-equiv="X-UA-Compatible" content="ie=edge">
  	<title>Document</title>
  	<script src="./lib/vue-2.4.0.js"></script>
  	//2. 自定义两组样式，来控制 transition 内部的元素实现动画
  	<style>
    	/* v-enter 【这是一个时间点】 是进入之前，元素的起始状态，此时还没有开始进入 */
    	/* v-leave-to 【这是一个时间点】 是动画离开之后，离开的终止状态，此时，元素 动画已经结束了 */
    	.v-enter,
    	.v-leave-to {
      		opacity: 0;
      		transform: translateX(150px);
    	}

    	/* v-enter-active 【入场动画的时间段】 */
    	/* v-leave-active 【离场动画的时间段】 */
    	.v-enter-active,
    	.v-leave-active{
      		transition: all 0.8s ease;
    	}
  	</style>
</head>

<body>
  	<div id="app">
    	<input type="button" value="toggle" @click="flag=!flag">
    	//需求： 点击按钮，让 h3 显示，再点击，让 h3 隐藏
    	//1. 使用 transition 元素，把 需要被动画控制的元素，包裹起来
    	//transition 元素，是 Vue 官方提供的
    	<transition>
      		<h3 v-if="flag">这是一个H3</h3>
    	</transition>
  	</div>

  	<script>
    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {
        		flag: false
      		},
      		methods: {}
    	});
  	</script>
</body>

</html>
```

当多组又不想共用动画时，可以给标签加一个name属性

代码：

1. HTML结构：

```javascript
<div id="app">
    <input type="button" value="动起来" @click="myAnimate">
    //使用 transition 将需要过渡的元素包裹起来
    <transition name="fade">        //与上面不同，我增加了一个name属性.同时样式的前缀也修改了
      	<div v-show="isshow">动画哦</div>
	</transition>
</div>
```

2. VM 实例：

```javascript
// 创建 Vue 实例，得到 ViewModel
var vm = new Vue({
	el: '#app',
	data: {
		isshow: false
	},
	methods: {
		myAnimate() {
			this.isshow = !this.isshow;
		}
	}
});
```

3. 定义两组类样式：

```javascript
//v-enter-acitve根据我修改的name：fade就变成了fade-enter-acitve
/* 定义进入和离开时候的过渡状态 */
<style>
	.fade-enter-active,
    .fade-leave-active {
		transition: all 0.2s ease;
		position: absolute;
	}
    
    /* 定义进入过渡的开始状态 和 离开过渡的结束状态 */
	.fade-enter,
    .fade-leave-to {
		opacity: 0;
		transform: translateX(100px);
    }
</style>
```

### （未学）2.使用第三方 CSS 动画库

​	使用第三方 CSS 动画库:	https://cn.vuejs.org/v2/guide/transitions.html#自定义过渡类名

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  	<meta charset="UTF-8">
  	<meta name="viewport" content="width=device-width, initial-scale=1.0">
  	<meta http-equiv="X-UA-Compatible" content="ie=edge">
  	<title>Document</title>
  	<script src="./lib/vue-2.4.0.js"></script>
    //导入第三方CSS动画库
  	<link rel="stylesheet" href="./lib/animate.css">
  	//入场 bounceIn    离场 bounceOut
</head>

<body>
  	<div id="app">
    	<input type="button" value="toggle" @click="flag=!flag">
    	//需求： 点击按钮，让 h3 显示，再点击，让 h3 隐藏
    	/*
    	<transition enter-active-class="animated bounceIn" leave-active-class="animated bounceOut">
 			<h3 v-if="flag">这是一个H3</h3>
		</transition> */

		//使用 :duration="毫秒值" 来统一设置 入场 和 离场 时候的动画时长 -->
		/*
		<transition enter-active-class="bounceIn" leave-active-class="bounceOut" :duration="200">
			<h3 v-if="flag" class="animated">这是一个H3</h3>
    	</transition>*/

		//使用  :duration="{ enter: 200, leave: 400 }"  来分别设置 入场的时长 和 离场的时长
		<transition 
			enter-active-class="bounceIn" 
			leave-active-class="bounceOut" 
    		:duration="{ enter: 200, leave: 400 }">
      		<h3 v-if="flag" class="animated">这是一个H3</h3>
    	</transition> 
	</div>

	<script>
 		// 创建 Vue 实例，得到 ViewModel
		var vm = new Vue({
			el: '#app',
			data: {
				flag: false
			},
			methods: {}
		});
	</script>
</body>

</html>
```



1. 导入动画类库：

```javascript
<link rel="stylesheet" type="text/css" href="./lib/animate.css">
```

1. 定义 transition 及属性：

```javascript
<transition
	enter-active-class="fadeInRight"
	leave-active-class="fadeOutRight"
	:duration="{ enter: 500, leave: 800 }">
  	<div class="animated" v-show="isshow">动画哦</div>
</transition>
```

### 3.使用动画钩子函数

使用前两种都无法完成半场动画：点按钮只有动画的一半(只有入场没有离开)，再点不会回到原位置。

比如添加购物车，点第二次，动画还是添加到购物车而不是从购物车离开回去。

一个完整的动画共有8个钩子函数，只需要设置半场的4个就可以实现半场动画。

![day3-钩子函数](D:\用户Users\chenh\Desktop\笔记\Vue\img\day3-钩子函数.PNG)

1. 定义 transition 组件以及三个钩子函数：

```javascript
<div id="app">
    <input type="button" value="切换动画" @click="isshow = !isshow">
    <transition
    @before-enter="beforeEnter"
    @enter="enter"
    @after-enter="afterEnter">
      	<div v-if="isshow" class="show">OK</div>
    </transition>
</div>
```

2. 定义三个 methods 钩子方法：

```javascript
methods: {
        beforeEnter(el) { // 动画进入之前的回调
          	el.style.transform = 'translateX(500px)';
        },
        enter(el, done) { // 动画进入完成时候的回调
          	el.offsetWidth;
          	el.style.transform = 'translateX(0px)';
          	done();
        },
        afterEnter(el) { // 动画进入完成之后的回调
          	this.isshow = !this.isshow;
        }
      }
```

3. 定义动画过渡时长和样式：

```javascript
.show{
      transition: all 0.4s ease;
}
```

4. 完整代码

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
    	.ball {
      		width: 15px;
      		height: 15px;
      		border-radius: 50%;
      		background-color: red;
    	}
  	</style>
</head>

<body>
  	<div id="app">
    	<input type="button" value="快到碗里来" @click="flag=!flag">
    	//1. 使用 transition 元素把 小球包裹起来
    	<transition
      		@before-enter="beforeEnter"
      		@enter="enter"
      		@after-enter="afterEnter">
      		<div class="ball" v-show="flag"></div>
    	</transition>
	</div>

  	<script>

    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {
        		flag: false
      		},
      		methods: {
	        	// 大家可以认为 ， el 是通过 document.getElementById('') 方式获取到的原生JS DOM对象
        		beforeEnter(el){
          			// beforeEnter 表示动画入场之前，此时，动画尚未开始，可以 在 beforeEnter 中，设置元素开始动画之前的起始样式
          			// 设置小球开始动画之前的，起始位置
          			el.style.transform = "translate(0, 0)";
        		},
        		enter(el, done){
          			//这句话，没有实际的作用，但是如果不写，出不来动画效果.可认为el.offsetWidth会强制动画刷新
          			//不一定写offsetWidth，offsetHeight、offsetLeft等等也行
          			el.offsetWidth;
          			// enter 表示动画 开始之后的样式，这里，可以设置小球完成动画之后的，结束状态
          			el.style.transform = "translate(150px, 450px)";
          			el.style.transition = 'all 1s ease';

          			// 这里的 done， 起始就是 afterEnter 这个函数，也就是说：done 是 afterEnter 函数的引用
          			done();
        		},
        		afterEnter(el){
          			// 动画完成之后，会调用 afterEnter
          			// 这句话， 第一个功能，是控制小球的显示与隐藏
          			// 第二个功能： 直接跳过后半场动画，让 flag 标识符直接变为 false
          			// 当第二次再点击按钮的时候， flag: false  ->    true
          			this.flag = !this.flag;
					//使用el.style.opacity = 0.5;会出问题。为什么？
					// Vue 把一个完整的动画，使用钩子函数，拆分为了两部分：
          			 // 我们使用 flag 标识符，来表示动画的切换；
          			 // 刚一开始，flag = false  ->   true   ->   false  
					// 半场动画只是上半场      上半场       下半场        
					//如果不修改flag值，只把隐藏，点第一次 flag从flase->true  点第二次 true->flase就是后半						场动画了。就出问题                
        		}
      		}
    	});
	</script>
</body>

</html>
```

### 4.v-for 的列表过渡：transition-group

v-for 的列表过渡：		https://cn.vuejs.org/v2/guide/transitions.html#列表的进入和离开过渡

在实现列表过渡的时候，如果需要过渡的元素，是通过 v-for 循环渲染出来的，不能使用 transition 包裹，需要使用 transition-group

**(1)添加过渡**

1. 定义过渡样式：

```javascript
<style>
    .list-enter,
    .list-leave-to {
      	opacity: 0;
      	transform: translateY(10px);
    }
    
    .list-enter-active,
    .list-leave-active {
      	transition: all 0.3s ease;
    }
</style>
```

2. 定义DOM结构，其中，需要使用 transition-group 组件把v-for循环的列表包裹起来：

> 在实现列表过渡的时候，如果需要过渡的元素，是通过 v-for 循环渲染出来的，不能使用 transition 包裹，需要使用 transitionGroup
>
> 如果要为 v-for 循环创建的元素设置动画，必须为每一个元素设置 :key 属性
>
> 给 transition-group 添加 appear 属性，实现页面刚展示出来时候，入场时候的效果
>
> 通过 为 transition-group 元素，设置 tag 属性，指定 transition-group 渲染为指定的元素，如果不指定 tag 属性，默认，渲染为 span 标签

```javascript
<div id="app">

	<div>
		<label>
			Id:
			<input type="text" v-model="id">
		</label>

		<label>
			Name:
			<input type="text" v-model="name">
		</label>

		<input type="button" value="添加" @click="add">
	</div>

	<!-- <ul> -->  
         //在实现列表过渡的时候，如果需要过渡的元素，是通过v-for循环渲染出来的，不能使用transition包裹，需要使用 transitionGroup
	//如果要为v-for循环创建的元素设置动画，必须为每一个元素设置 :key 属性
	//给 transition-group 添加 appear 属性，实现页面刚展示出来时候，入场时候的效果
	//通过 为 transition-group 元素，设置 tag 属性，指定 transition-group 渲染为指定的元素，如果不指定 tag 属性，默认，渲染为 span 标签
		<transition-group appear tag="ul">
			<li v-for="(item, i) in list" :key="item.id" @click="del(i)">
				{{item.id}} --- {{item.name}}
			</li>
		</transition-group>            
	<!-- </ul> -->
            
 </div>           
```

3. 定义 VM中的结构：

```javascript
// 创建 Vue 实例，得到 ViewModel
var vm = new Vue({
	el: '#app',
	data: {
		id: '',
		name: '',
	list: [
		{ id: 1, name: '赵高' },
		{ id: 2, name: '秦桧' },
		{ id: 3, name: '严嵩' },
		{ id: 4, name: '魏忠贤' }
	]
	},
	methods: {
		add() {
			this.list.push({ id: this.id, name: this.name })
			this.id = this.name = ''
		},
        del(i) {
          this.list.splice(i, 1)
        }
	}
});
```

#### 列表的排序过渡

`<transition-group>` 组件还有一个特殊之处。不仅可以进入和离开动画，**还可以改变定位**。要使用这个新功能只需了解新增的 `v-move` 特性，**它会在元素的改变定位的过程中应用**。

- `v-move` 和 `v-leave-active` 结合使用，能够让列表的过渡更加平缓柔和：

```javascript
/*.v-move 和 .v-leave-active 配合使用，能够实现列表后续的元素，渐渐地漂上来的效果 */
.v-move{
  	transition: all 0.8s ease;
}
.v-leave-active{
  	position: absolute;
}
```

## 二、定义Vue组件
什么是组件： 组件的出现，就是为了拆分Vue实例的代码量的，能够让我们以不同的组件，来划分不同的功能模块，将来我们需要什么样的功能，就可以去调用对应的组件即可；
组件化和模块化的不同：
+ 模块化： 是从代码逻辑的角度进行划分的；方便代码分层开发，保证每个功能模块的职能单一；
+ 组件化： 是从UI界面的角度进行划分的；前端的组件化，方便UI组件的重用；
### 1.全局组件定义的三种方式

1. **使用 Vue.extend 配合 Vue.component 方法：**

HTML结构

```javascript
<div id="app">
    //如果要使用组件，直接，把组件的名称，以 HTML 标签的形式，引入到页面中，即可
    // 如果使用 Vue.component 定义全局组件的时候，组件名称使用了 驼峰命名，则在引用组件的时候，需要把 大写的驼峰改为小写的字母，同时，两个单词之前，使用 - 链接；		myCom1 --> my-com1
	//如果组件名称不使用驼峰,则直接拿名称来使用即可;   mycom1 --> mycom1
    <my-com1></my-com1>
</div>
```
定义全局组件

```javascript
//1.1 使用 Vue.extend 来创建全局的Vue组件
var com1 = Vue.extend({
      template: '<h3>这是使用 Vue.extend 创建的组件</h3>';  // 通过 template 属性，指定了组件要展示的HTML结构
});
//1.2 使用 Vue.component('组件的名称', 创建出来的组件模板对象)         组件名称自己起
Vue.component('myCom1', com1);
```
>Vue.component 是用来全局注册组件的方法，其作用是将通过 Vue.extend 生成的扩展实例构造器注册（命名）为一个组件.



注意:**不论是哪种方式创建出来的组件,组件的 template 属性指向的模板内容,必须有且只能有唯一的一个根元素

例如

 template: '<h3>这是使用 Vue.extend 创建的组件</h3><span>123</span>'就会报错

 template: '<div><h3>这是直接使用 Vue.component 创建出来的组件</h3><span>123</span></div>'就只有一个根元素

也可将上面两步合为一步

```javascript
// Vue.component 第一个参数:组件的名称,将来在引用组件的时候,就是一个 标签形式 来引入 它的
// 第二个参数: Vue.extend 创建的组件  ,其中 template 就是组件将来要展示的HTML内容
Vue.component('mycom1', Vue.extend({
	template: '<h3>这是使用 Vue.extend 创建的组件</h3>'
}))
```
 注意:不论是哪种方式创建出来的组件,组件的 template 属性指向的模板内容,必须有且只能有唯一的一个根元素

2. **直接使用 Vue.component 方法：**

更简化。参数二直接传一个对象字面量。

```javascript
Vue.component('register', {
      template: '<h1>注册</h1>'
});
```
3. **通过 template 元素,在外部定义的组件结构**

   在写如template: '<div><h3>这是直接使用 Vue.component 创建出来的组件</h3><span>123</span></div>'时。没有智能提示和高亮，容易写漏。

   可以用第三种方式：使用 template 元素定义组件的HTML模板结构。将 template 的内容通过template标签脱离出去。
```javascript
//在 被控制的 #app 外面,使用 template 元素,定义组件的HTML模板结构
<template id="tmpl">
	<div>
		<h1>这是通过 template 元素,在外部定义的组件结构,这个方式,有代码的智能提示和高亮</h1>
		<h4>好用,不错!</h4>
	</div>
</template>

/*   ?    
将模板字符串，定义到script标签中：        
<script id="tmpl" type="x-template">
	<div><a href="#">登录</a> | <a href="#">注册</a></div>
</script>
*/
```
同时，需要使用 Vue.component 来定义组件：
```javascript
Vue.component('account', {
	template: '#tmpl';           //id
});
```

> 注意： 组件中的DOM结构，有且只能有唯一的根元素（Root Element）来进行包裹！

### 2.使用`components`属性定义局部子组件

1. 组件实例定义方式：

```javascript
<script>
	// 创建 Vue 实例，得到 ViewModel
	var vm = new Vue({
		el: '#app',
		data: {},
		methods: {},
		components: { 	// 定义实例内部私有组件
			account: { 	// account 组件
				// 在这里使用定义的子组件
				template: '<div><h1>这是Account组件{{name}}</h1><login></login></div>', 
				components: { 	// 定义子组件的子组件
					login: { 	// login 组件
						template: "<h3>这是登录组件</h3>"
					}
			}
		}
	}
});
</script>
```

2. 引用组件：

```javascript
<div id="app">
	<account></account>
</div>
```

### 3.组件中的数据data和响应事件methods

**组件中的data**

>1. 组件可以有自己的 data 数据
>2. 组件的 data 和 实例的 data 有点不一样,实例中的 data 可以为一个对象,但是**组件中的 data 必须是一个方法**
>3. 组件中的 data 除了必须为一个方法之外,这个**方法内部,还必须返回一个对象才行;**
>4. 组件中 的data 数据,使用方式,和实例中的 data 使用方式完全一样!!!

1. 在组件中，`data`需要被定义为一个方法，例如：
```javascript
Vue.component('account', {
	template: '<h1>这是全局组件 --- {{msg}}</h1>',
	data() {
		return {
			msg: '这是组件的中data定义的数据'
		}
	},
	methods:{
		login(){
			alert('点击了登录按钮');
		}
	}
});
```
2. 在子组件中，如果将模板字符串，定义到了script标签中，那么，要访问子组件身上的`data`属性中的值，需要使用`this`来访问；

### 4.【重点】为什么组件中的data属性必须定义为一个方法并返回一个对象

data属性不为方法会报错。而为什么返回一个对象？

通过计数器案例演示

如果返回的不是一个对象，而是一个对象引用。 那么我们引用了多个相同的组件：计数器之间本应不相干。但是点第一个的按钮， 后面的组件计数器也会跟着改变。如果返回是对象就没这问题。

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
      	//引用三次组件，三个计数器
    	<counter></counter>
    	<hr>
    	<counter></counter>
    	<hr>
    	<counter></counter>
  	</div>


  	<template id="tmpl">
    	<div>
      		<input type="button" value="+1" @click="increment">
      		<h3>{{count}}</h3>
    	</div>
  	</template>

  	<script>
        //外部定义一个对象 
    	var dataObj = { count: 0 }

    	// 这是一个计数器的组件, 身上有个按钮,每当点击按钮,让 data 中的 count 值 +1
    	Vue.component('counter', {
      		template: '#tmpl',
      		data: function () {  //注意组件中的data必须是一个function，并且返回的是对象
        		// return dataObj;     返回对象引用而不是对象。三个本应不相干的组件就会一起增加
        		return { count: 0 }
      		},
      		methods: {
        		increment() {
          			this.count++
        		}
      		}
    	})

    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {},
      		methods: {}
    	});
  	</script>
</body>

</html>
```

### 5.组件切换：使用`flag`标识符结合`v-if`和`v-else`

例子：点击切换注册登录界面

1. 页面结构：
```javascript
<div id="app">
    <a href="" @click.prevent="flag=true">登录</a>
    <a href="" @click.prevent="flag=false">注册</a>

    <login v-if="flag"></login>
    <register v-else="flag"></register>

</div>
```
2. Vue实例定义：
```javascript
<script>
	Vue.component('login', {
		template: '<h3>登录组件</h3>'
    })

	Vue.component('register', {
		template: '<h3>注册组件</h3>'
    })

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
		el: '#app',
		data: {
			flag: false
		},
		methods: {}
    });
</script>
```

### 6.组件切换：使用`:is`属性来切换不同的子组件，添加组件过渡

**1.使用Vue提供的component切换组件**

1. 组件实例定义方式：
```javascript
<script>
// 登录组件
const login = Vue.extend({
	template: `<div>
				<h3>登录组件</h3>
			</div>`
    });
Vue.component('login', login);

// 注册组件
const register = Vue.extend({
	template: `<div>
				<h3>注册组件</h3>
			/div>`
    });
Vue.component('register', register);

// 创建 Vue 实例，得到 ViewModel
var vm = new Vue({
	el: '#app',
	data: { 
      comName: 'login';     // 当前 component 中的 :is 绑定的组件的名称 
    }, 
	methods: {}
});]
</script>
```
2. 使用`component`标签，来引用组件，并通过`:is`属性来指定要加载的组件：
```javascript
<div id="app">
  	// 组件名称是 字符串 别忘了''
    <a href="#" @click.prevent="comName='login'">登录</a>
    <a href="#" @click.prevent="comName='register'">注册</a>

	//Vue提供了 component ,来展示对应名称的组件
    //component 是一个占位符, :is 属性,可以用来指定要展示的组件的名称
    <component :is="comName"></component>
    
</div>
```
**2.添加组件过渡**

多个组件的过渡很简单，我们不需要使用key属性，只需要使用动态组件。

1. 将要过渡的component用transition包起来就是

```javascript
<div id="app">
    <a href="" @click.prevent="comName='login'">登录</a>
    <a href="" @click.prevent="comName='register'">注册</a>

    //通过 mode 属性,设置组件切换时候的 模式  (使得不会在第一个组件还没出去时，第二个组件就出来了)
    <transition mode="out-in">
      	<component :is="comName"></component>
    </transition>

  </div>
```

2. 添加切换样式：

```javascript
<style>
    .v-enter,
    .v-leave-to {
		opacity: 0;
      	transform: translateX(30px);
    }

    .v-enter-active,
    .v-leave-active {
      	position: absolute;
      	transition: all 0.3s ease;
    }

    h3{
      	margin: 0;
    }
</style>
```

总结:当前学习了几个 Vue 提供的标签了?

```javascript
component,  template,  transition,  transitionGroup
```

### 7.父组件向子组件传值

默认情况下，子组件无法访问到父组件中的data上的数据和methods 中的方法。

> 父组件，可以在引用子组件的时候， 通过 属性绑定（v-bind:） 的形式, 把 需要传递给 子组件的数据，以属性绑定的形式，传递到子组件内部，供子组件使用。 

注意:当父组件通过属性绑定传递给子组件数据时，如果想用传过来的数据，一定要**使用`props`属性**来定义父组件传递过来的数据。

1. 组件实例定义方式
```javascript
<script>
	// 创建 Vue 实例，得到 ViewModel      (vm想象成大的组件)
	var vm = new Vue({
		el: '#app',
		data: {
			msg: '这是父组件中的消息'
		},
		components: {       //子组件
			data: {},	// 注意：子组件中的data数据，并不是通过父组件传递过来的，而是子组件自身私有的，比如： 							子组件通过 Ajax ，请求回来的数据，都可以放到 data 身上；
            			// data 上的数据，都是可读可写的；
			son: {
				template: '<h1>这是子组件 --- {{finfo}}</h1>',
				//注意： 组件中的 所有 props 中的数据，都是通过父组件传递给子组件的
				// props 中的数据，都是只读的，无法重新赋值
				props: ['parentmsg'];   //把父组件传递过来的parentmsg 属性，先在props数组中，定义一下，这										样，才能使用这个数据
			}
		}
	});
</script>
```
2. 使用`v-bind`或简化指令，将数据传递到子组件中：
```javascript
<div id="app">
    <son :parentmsg="msg"></son>
</div>
```

### 8.子组件向父组件传值

> 父组件向子组件 传递 方法，使用的是 事件绑定机制； v-on, 当我们自定义了 一个 事件属性之后，那么，子组件就能够，通过某些方式，来调用 传递进去的 这个 方法了

1. 原理：父组件将方法的引用，传递到子组件内部，子组件在内部调用父组件传递过来的方法，同时把要发送给父组件的数据，在调用方法的时候当作参数传递进去；
2. 父组件将方法的引用传递给子组件，其中，`getMsg`是父组件中`methods`中定义的方法名称，`func`是子组件调用传递过来方法时候的方法名称
```javascript
<son @func="getMsg"></son>
```
3. 子组件内部通过`this.$emit('方法名', 要传递的数据)`方式，来调用父组件中的方法，同时把数据传递给父组件使用
```javascript
<div id="app">
	//引用父组件     (son是自定义的组件名称) 
    <son @func="getMsg"></son>

    //组件模板定义
    /*
    <script type="x-template" id="son">
      	<div>
        	<input type="button" value="向父组件传值" @click="sendMsg" />
      	</div>
    </script>*/
    
    <template id="tmpl">
    	<div>
      		<h1>这是子组件</h1>
      		<input type="button" value="点击子组件中的按钮，触发父组件传递过来的func方法" @click="sendMsg">
    	</div>
  	</template>
</div>

<script>
	// 子组件的定义方式
    Vue.component('son', {
      	template: '#son', 			// 组件模板Id
        data() {
        	return {
          		sonmsg: { name: '小头儿子', age: 6 }
        	}
      	},      	
      	methods: {
        	sendMsg() { // 按钮的点击事件
              	 // 当点击子组件的按钮的时候，如何拿到父组件传递过来的func方法，并调用这个方法？？？
          		//  emit 英文原意： 是触发，调用、发射的意思
          		//this.$emit('func', '123'); 调用父组件传递过来的getMsg方法，同时把数据传递出去.传参给getMsg
              	this.$emit('func', this.sonmsg);   	//点击时把子组件里的data传给父组件
        	}										
      	}
    });

    // 创建 Vue 实例，得到 ViewModel （父组件）
    var vm = new Vue({
      	el: '#app',
      	data: {
        	datamsgFormSon: null;         //在父组件中定义一个用来接收从子组件接收到的数据  
      	},
      	methods: {
        	getMsg(val){ 		// 子组件中，通过 this.$emit() 实际调用的方法，在此进行定义
          		// console.log('调用了父组件身上的 show 方法: --- ' + val)
          		this.datamsgFormSon = data;         //将子组件中的数据保存到父组件
        	}
      	}     	
      	components: {
        	com2
        	// com2: com2
      }
    });
</script>
```

### 评论列表案例
目标：主要练习父子组件之间传值

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
</head>

<body>
  	<div id="app">


    	<cmt-box @func="loadComments"></cmt-box>

		//使用bootsteap的list-group渲染假数据为列表
    	<ul class="list-group">
      		<li class="list-group-item" v-for="item in list" :key="item.id">
        		<span class="badge">评论人： {{ item.user }}</span>
        		{{ item.content }}
      		</li>
    	</ul>


  	</div>

	//发表评论定义为组件，抽离出来，方便复用
  	<template id="tmpl">
    	<div>

      	<div class="form-group">
        	<label>评论人：</label>
        	<input type="text" class="form-control" v-model="user">
      	</div>

      	<div class="form-group">
        	<label>评论内容：</label>
        	<textarea class="form-control" v-model="content"></textarea>
      	</div>

      	<div class="form-group">
        	<input type="button" value="发表评论" class="btn btn-primary" @click="postComment">
      	</div>

    	</div>
  	</template>

  	<script>

    	var commentBox = {
      		data() {
        		return {
          			user: '',
          			content: ''
        		}
      		},
      		template: '#tmpl',
      		methods: {
        		postComment() { // 发表评论的方法
          // 分析：发表评论的业务逻辑
          // 1. 评论数据存到哪里去？？？   存放到了 localStorage 中  localStorage.setItem('cmts', '')
          // 2. 先组织出一个最新的评论数据对象
          // 3. 想办法，把 第二步中，得到的评论对象，保存到 localStorage 中：
          //  3.1 localStorage 只支持存放字符串数据， 要先调用 JSON.stringify 
          //  3.2 在保存 最新的 评论数据之前，要先从 localStorage 获取到之前的评论数据（string）， 转换为 一个  数组对象， 然后，把最新的评论， push 到这个数组
          //  3.3 如果获取到的 localStorage 中的 评论字符串，为空不存在， 则  可以 返回一个 '[]'  让 JSON.parse 去转换
          //  3.4  把 最新的  评论列表数组，再次调用 JSON.stringify 转为  数组字符串，然后调用 localStorage.setItem()

          			var comment = { id: Date.now(), user: this.user, content: this.content }

         		 	// 从 localStorage 中获取所有的评论
          			var list = JSON.parse(localStorage.getItem('cmts') || '[]')
          			list.unshift(comment)
          			// 重新保存最新的 评论数据
          			localStorage.setItem('cmts', JSON.stringify(list))

          			this.user = this.content = ''

          			// this.loadComments() // ???父组件vm实例的方法，子组件不能直接调用，得通过绑定传子组件
          			this.$emit('func')
        		}
      		}
    	}

    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {
        		list: [
          			{ id: Date.now(), user: '李白', content: '天生我材必有用' },
          			{ id: Date.now(), user: '江小白', content: '劝君更尽一杯酒' },
          			{ id: Date.now(), user: '小马', content: '我姓马， 风吹草低见牛羊的马' }
        		]
      		},
      		beforeCreate(){ // 注意：这里不能调用 loadComments 方法，因为在执行这个钩子函数的时候，data 和 								methods 都还没有被初始化好
      		},
      		created(){
        		this.loadComments()
      		},
     		methods: {
        		loadComments() { // 从本地的 localStorage 中，加载评论列表
          			var list = JSON.parse(localStorage.getItem('cmts') || '[]')
          			this.list = list
        		}
      		},
      		components: {
        		'cmt-box': commentBox
      		}
    	});
  	</script>
</body>

</html>
```



### 9.(未看)使用 `this.$refs` 来获取元素和组件
```javascript
  <div id="app">
    <div>
      <input type="button" value="获取元素内容" @click="getElement" />
      <!-- 使用 ref 获取元素 -->
      <h1 ref="myh1">这是一个大大的H1</h1>

      <hr>
      <!-- 使用 ref 获取子组件 -->
      <my-com ref="mycom"></my-com>
    </div>
  </div>

  <script>
    Vue.component('my-com', {
      template: '<h5>这是一个子组件</h5>',
      data() {
        return {
          name: '子组件'
        }
      }
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {
        getElement() {
          // 通过 this.$refs 来获取元素
          console.log(this.$refs.myh1.innerText);
          // 通过 this.$refs 来获取组件
          console.log(this.$refs.mycom.name);
        }
      }
    });
  </script>
```

## Vue.js - Day4

## 一、什么是路由

1. **后端路由：**对于普通的网站，所有的超链接都是URL地址，**所有的URL地址都对应服务器上对应的资源**；
2. **前端路由：**对于单页面应用程序来说，主要通过URL中的hash(#号)来实现不同页面之间的切换，同时，hash有一个特点：HTTP请求中不会包含hash相关的内容；所以，**单页面程序中的页面跳转主要用hash实现**(锚点)；
3. 在单页面应用程序中，这种**通过hash改变来切换页面的方式(不涉及页面的刷新)**，称作前端路由（区别于后端路由）；

切换组件

## 二、在 vue 中使用 vue-router

[安装以及手册：https://router.vuejs.org/zh/installation.html](https://router.vuejs.org/zh/installation.html)

1. 导入 vue-router 组件类库：
```javascript
//1. 导入 vue-router 组件类库。注意在此之前导入了Vue.js
<script src="./lib/vue-router-2.7.0.js"></script>
```
2. 使用 router-link 组件来导航
```javascript
//2. 使用 router-link 组件来导航
//router-link 默认渲染为一个a 标签，如果想渲染为其他的。如span标签，在后面加一属性 tag="span"
<router-link to="/login" tag="span">登录</router-link>
<router-link to="/register">注册</router-link>

//如果没用router-link 而是使用普通的a标签，地址前加一个# <a href="#/login">登录</a>
```
3. 使用 router-view 组件来显示匹配到的组件

   放组件的容器，显示组件

```javascript
//3. 使用 router-view 组件来显示匹配到的组件
//这是 vue-router 提供的元素，专门用来当作占位符的，将来，路由规则，匹配到的组件，就会展示到这个 router-view 中去 
//所以： 我们可以把 router-view 认为是一个占位符
<router-view></router-view>
```
4. 创建使用`Vue.extend`创建组件
```javascript
// 4.1 使用 Vue.extend 来创建登录组件的模板对象
var login = Vue.extend({
	template: '<h1>登录组件</h1>'
});

// 4.2 创建注册组件的模板对象
var register = {
	template: '<h1>注册组件</h1>'
};
```
5. 创建一个路由 router 实例，通过 routers 属性来定义路由匹配规则
```javascript
//创建一个路由对象， 当 导入vue-router包之后，在window全局对象中，就有了一个路由的构造函数，叫做VueRouter
// 在 new 路由对象的时候，可以为 构造函数，传递一个配置对象
var routerObj = new VueRouter({
	routes: [         // 这个配置对象中的route表示 【路由匹配规则】 的意思  （注意route 不带r）
		// 每个路由规则，都是一个对象，这个规则对象身上，有两个必须的属性：
		//属性1 是 path， 表示监听 哪个路由链接地址；
		//属性2 是 component， 表示，如果 路由是前面匹配到的 path ，则展示 component 属性对应的那个组件
			// 注意： component 的属性值，必须是一个 组件的模板对象， 不能是 组件的引用名称；
		{ path: '/login', component: login },         //必须是上面创建的模板对象，而不是名称'login'
		{ path: '/register', component: register }
		{ path: '/', redirect: '/login' },   //前面是根路径，默认进入就展示登录组件
	]      									// 这里的redirect和Node 中的redirect完全是两码事
});
```
6. 使用 router 属性来使用路由规则
```javascript
// 6. 创建 Vue 实例，得到 ViewModel
var vm = new Vue({
	el: '#app',
  	data: {},	
  	method: {},
	router: routerObj      // 将路由规则对象，注册到 vm 实例上，用来监听 URL 地址的变化，然后展示对应的组件
    });
});
```

## 三、设置选中路由高亮显示

检查元素，知切换组件时，正显示的组件带有特殊的class类`router-link-active`，切换后，没显示的组件的类就消失到正显示的组件上。可以给这个类设置样式来实现选中路由的高亮显示。

这是vue-router提供的默认的类，我们也可以自定义。

设置链接激活时所使用的CSS类名，默认值可以通过路由的构造选项`linkActiveClass`来全局配置。

```javascript
var routerObj = new VueRouter({
      routes: [
        { path: '/', redirect: '/login' }, // 这里的 redirect 和 Node 中的 redirect 完全是两码事
        { path: '/login', component: login },
        { path: '/register', component: register }
      ],
      linkActiveClass: 'myactive'
    })
```

## 四、设置路由切换动画

1. 将router-view用transition包裹起来

   ```javascript
   <transition mode="out-in">        //mode过渡模式 先out再in
   	<router-view></router-view>
   </transition>
   ```

2. 自定义动画类

   ```javascript
   .v-enter,
   .v-leave-to {
   	opacity: 0;
   	transform: translateX(140px);
   }

   .v-enter-active,
   .v-leave-active {
   	transition: all 0.5s ease;
   }
   ```

## 五、在路由规则中定义参数

两种方法

1. 通过 `this.$route.params`来获取路由中的参数：

```javascript
var register = Vue.extend({
      template: '<h1>注册组件 --- {{this.$route.params.id}}</h1>'
});
```

2. 在规则中定义参数：

```javascript
{ path: '/register/:id', component: register }
```

详细看下方

**1.使用query方法传递参数**

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  	<meta charset="UTF-8">
  	<meta name="viewport" content="width=device-width, initial-scale=1.0">
  	<meta http-equiv="X-UA-Compatible" content="ie=edge">
  	<title>Document</title>
  	<script src="./lib/vue-2.4.0.js"></script>
  	<script src="./lib/vue-router-3.0.1.js"></script>
</head>

<body>
  	<div id="app">

    	//如果在路由中，使用查询字符串，给路由传递参数，则 不需要修改 路由规则的 path 属性
    	<router-link to="/login?id=10&name=zs">登录</router-link>
    	<router-link to="/register">注册</router-link>

    	<router-view></router-view>

	</div>

  	<script>

    	var login = {
      		template: '<h1>登录 --- {{ $route.query.id }} --- {{ $route.query.name }}</h1>', //this省略了
      		data(){
        		return {
          			msg: '123'
        		}
      		},
      		created(){ 		// 组件的生命周期钩子函数
        		// console.log(this.$route);  输入发现存储在query对象中
        		// console.log(this.$route.query.id);    通过query.id拿到传递的数据
      		}
    	}

    	var register = {
      		template: '<h1>注册</h1>'
    	}

    	var router = new VueRouter({
      		routes: [
        		{ path: '/login', component: login },
        		{ path: '/register', component: register }
      		]
    	})

    	// 创建 Vue 实例，得到 ViewModel
   	 	var vm = new Vue({
      		el: '#app',
      		data: {},
      		methods: {},
      		// router: router
      		router
    	});
  	</script>
</body>

</html>
```

**2.使用params方法传递参数**

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  	<meta charset="UTF-8">
  	<meta name="viewport" content="width=device-width, initial-scale=1.0">
  	<meta http-equiv="X-UA-Compatible" content="ie=edge">
  	<title>Document</title>
  	<script src="./lib/vue-2.4.0.js"></script>
  	<script src="./lib/vue-router-3.0.1.js"></script>
</head>

<body>
  	<div id="app">

    	//跳转时跟在url后面的解析成就是路由规则的path 属性的id
    	<router-link to="/login/12/ls">登录</router-link>       
    	<router-link to="/register">注册</router-link>

    	<router-view></router-view>

  	</div>

  	<script>

    	var login = {
      		template: '<h1>登录 --- {{ $route.params.id }} --- {{ $route.params.name }}</h1>',
      		data(){
        		return {
          			msg: '123'
        		}
      		},
      		created(){ // 组件的生命周期钩子函数
        		console.log(this.$route.params.id);     //此时query是空对象{}
      		}
    	}

    	var register = {
      		template: '<h1>注册</h1>'
    	}

    	var router = new VueRouter({
      		routes: [
        		{ path: '/login/:id/:name', component: login },  //: 占位符 ：id 当成id解析出来
        		{ path: '/register', component: register }
      		]
    	})

    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {},
      		methods: {},
      		// router: router
      		router
    	});
  	</script>
</body>

</html>
```



1. 在规则中定义参数：
```javascript
{ path: '/register/:id', component: register }
```
2. 通过 `this.$route.params`来获取路由中的参数：
```javascript
var register = Vue.extend({
      template: '<h1>注册组件 --- {{this.$route.params.id}}</h1>'
});
```

## 六、使用 `children` 属性实现路由嵌套
```javascript
<div id="app">
	<router-link to="/account">Account</router-link>

	<router-view></router-view>
</div>

<template id="tmpl">
	<div>
      	<h1>这是 Account 组件</h1>

      	<router-link to="/account/login">登录</router-link>
      	<router-link to="/account/register">注册</router-link>

      	<router-view></router-view>
    	</div>
</template>
        
<script>
    // 父路由中的组件
    const account = Vue.extend({
		template: '#tmp1';
    });

    // 子路由中的 login 组件
    const login = Vue.extend({
      	template: '<div>登录组件</div>'
    });

    // 子路由中的 register 组件
    const register = Vue.extend({
      	template: '<div>注册组件</div>'
    });

    // 路由实例
    var router = new VueRouter({
      	routes: [
        	{ path: '/', redirect: '/account/login' },    // 使用 redirect 实现路由重定向
        	{
          		path: '/account',
          		component: account,
          		children: [ 		// 通过 children 数组属性，来实现路由的嵌套
                  			   // 注意：子路由的path 前面不要带 / ，否则永远以根路径开始请求，这样不方便我们
            				{ path: 'login', component: login },		//用户去理解URL地址
            				{ path: 'register', component: register }
          		]
        	}
      	]
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      	el: '#app',
      	data: {},
      	methods: {},
      	components: {
        	account
      	},
      	router: router
    });
</script>
```

## 七、命名视图实现经典布局



1. 标签代码结构：
```javascript
<div id="app">
    <router-view></router-view>
    <div class="content">
      	<router-view name="left"></router-view>   //给router-view一个name属性，去router对象的path找对应名称
      	<router-view name="main"></router-view>
    </div>
</div>
```
2. JS代码：
```javascript
<script>
    var header =  Vue.component('header', {
        template: '<div class="header">Header头部区域</div>'
    });

    var sidebar =  Vue.component('sidebar', {
      	template: '<div class="sidebar">Left侧边栏区域</div>'
    };

    var mainbox = Vue.component('mainbox', {
      	template: '<div class="mainbox">mainBox主体区域</div>'
    });

    // 创建路由对象
    var router = new VueRouter({
      	routes: [
        	{
          /* 这种不行，因为只有一个url，匹配只能匹配到一个组件，当匹配到一个，上面的三个router-view就会重复3次
          	{ path: '/', component: header },
        	{ path: '/left', component: leftBox },
        	{ path: '/main', component: mainBox }
           	3选1      */

          		path: '/', components: {
            		'default': header,         //匹配到，默认放 
            		'left': sidebar,
            		'main': mainBox
          		}
        	}
      	]
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      router
    });
  </script>
```
3. CSS 样式：
```javascript
<style>
    .header {
      	border: 1px solid red;
    }

    .content{
      	display: flex;
    }
    .sidebar {
      	flex: 2;
      	border: 1px solid green;
      	height: 500px;
    }
    .mainbox{
      	flex: 8;
      	border: 1px solid blue;
      	height: 500px;
    }
</style>
```

## 八、`watch`属性监听数据改变
考虑一个问题：想要实现 `名` 和 `姓` 两个文本框的内容改变，则全名的文本框中的值也跟着改变；（用以前的知识如何实现？？？		@keyup  但是路由没有事件，是个元素，无法绑定事件，这时就只能通过watch来实现监听数据改变）



1. **监听`data`中属性的改变：**
```javascript
<div id="app">
    <input type="text" v-model="firstName"> +
    <input type="text" v-model="lastName"> =
    <span>{{fullName}}</span>
</div>

	<script>
    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {
        		firstName: 'jack',
        		lastName: 'chen',
        		fullName: 'jack - chen'
      		},
      		methods: {},
      		watch: {   //使用这个属性，可以监视data中指定数据的变化，然后触发这个watch中对应的function处理函数
        		'firstName': function (newVal, oldVal) {  // 第一个参数是新数据，第二个参数是旧数据
          			this.fullName = newVal + ' - ' + this.lastName;
        		},
        		'lastName': function (newVal, oldVal) {    //lastName改变了，触发后面的function
          			this.fullName = this.firstName + ' - ' + newVal;
       		 	}
      		}
    	});
	</script>
```
2. **监听路由对象(地址)的改变：**
```javascript
<div id="app">
    <router-link to="/login">登录</router-link>
    <router-link to="/register">注册</router-link>

    <router-view></router-view>
</div>

	<script>
    	var login = Vue.extend({
      		template: '<h1>登录组件</h1>'
    	});

    	var register = Vue.extend({
      		template: '<h1>注册组件</h1>'
    	});

    	var router = new VueRouter({
      		routes: [
        		{ path: "/login", component: login },
        		{ path: "/register", component: register }
      		]
    	});

    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {},
      		methods: {},
      		router: router,
      		watch: {
        			//  this.$route.path
        			'$route.path': function (newVal, oldVal) {
          			// console.log(newVal + ' --- ' + oldVal)
          			if (newVal === '/login') {
            			console.log('欢迎进入登录页面');
          			} else if (newVal === '/register') {
            			console.log('欢迎进入注册页面');
          			}
        	}
    	});
  	</script>
```

## 九、`computed`计算属性的使用

1. 默认只有`getter`的计算属性：
```javascript
<div id="app">
    <input type="text" v-model="firstName"> +
    <input type="text" v-model="lastName"> =
    <span>{{fullName}}</span>
</div>

    <script>
    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {
        		firstName: 'jack',
        		lastName: 'chen'
      		},
      		methods: {},
      		computed: { // 在 computed 中，可以定义一些 属性，这些属性，叫做 【计算属性】， 计算属性的本质，就是一个方法，只不过，我们在使用 这些计算属性的时候，是把它们的名称，直接当作属性来使用的；并不会把计算属性，当作方法去调用；
        // 注意1：计算属性，在引用的时候，一定不要加 () 去调用，直接把它 当作 普通 属性去使用就好了；
        // 注意2：只要 计算属性，这个 function 内部，所用到的任何 data 中的数据发送了变化，就会立即重新计算这个 计算属性的值
        // 注意3：计算属性的求值结果，会被缓存起来，方便下次直接使用； 如果计算属性方法中，所以来的任何数据，都没有发生过变化，则，不会重新对 计算属性求值；
              // 计算属性； 特点：当计算属性中所以来的任何一个 data 属性改变之后，都会重新触发 本计算属性 的重新计算，从而更新 fullName 的值
              	'fullName': function() {     //fullName不在data中定义而是在computed中定义
                	return this.firstname + '-' + this.lastname;  	
              	}
              	/*等同
        		fullName() {     
          			return this.firstName + '-' + this.lastName;
        		}*/
      		}
    	});
  	</script>
```
2. 定义有`getter`和`setter`的计算属性：
```javascript
<div id="app">
    <input type="text" v-model="firstName">
    <input type="text" v-model="lastName">
    //点击按钮重新为 计算属性 fullName 赋值
    <input type="button" value="修改fullName" @click="changeName">

    <span>{{fullName}}</span>
</div>

  	<script>
    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {
        		firstName: 'jack',
        		lastName: 'chen'
      		},
      		methods: {
        		changeName() {
          			this.fullName = 'TOM - chen2';
        		}
      		},
      		computed: {
        		fullName: {
          			get: function () {
            			return this.firstName + ' - ' + this.lastName;
          			},
          			set: function (newVal) {
            			var parts = newVal.split(' - ');
            			this.firstName = parts[0];
            			this.lastName = parts[1];
          			}
        		}
      		}
    	});
    </script>
```

## 十、`watch`、`computed`和`methods`之间的对比
1. `computed`属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用；
2. `methods`方法表示一个具体的操作，主要书写**业务逻辑**；
3. `watch`一个对象，键是需要观察的表达式，值是对应回调函数。主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作；可以看作是`computed`和`methods`的结合体；

使用computed的计算属性，必须要return返回。而watch不用

## 十一、工具`nrm`的安装使用
作用：提供了一些最常用的NPM包镜像地址，能够让我们快速的切换安装包时候的服务器地址；
什么是镜像：原来包刚一开始是只存在于国外的NPM服务器，但是由于网络原因，经常访问不到，这时候，我们可以在国内，创建一个和官网完全一样的NPM服务器，只不过，数据都是从人家那里拿过来的，除此之外，使用方式完全一样；
1. 运行`npm i nrm -g`全局安装`nrm`包；

2. 使用`nrm ls`查看当前所有可用的镜像源地址以及当前所使用的镜像源地址；

3. 使用`nrm use npm`或`nrm use taobao`切换不同的镜像源地址；

   > 注意： nrm 只是单纯的提供了几个常用的 下载包的 URL地址，并能够让我们在 这几个 地址之间，很方便的进行切换，但是，我们每次装包的时候，使用的 装包工具，都是  npm

## 相关文件
1. [URL中的hash（井号）](http://www.cnblogs.com/joyho/articles/4430148.html)