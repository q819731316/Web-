# Vue.js - day6

vscode中.vue文件的小图标和提示什么的。可以安装扩展包Vetur和Vue 2 Snippers

## 一、普通页面中使用render函数渲染组件与传统方式渲染

### 1.页面中渲染基本的组件(传统方式：通过components注册组件)

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
    	<p>33333</p>
    	<login></login>
  	</div>

  	<script>
		//字面量类型模板对象
    	var login = {
      		template: '<h1>这是登录组件</h1>'
    	}

    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {},
      		methods: {},
      		components: {
        		login
      		}
    	});
  	</script>
</body>

</html>
```



### 2.普通页面中使用render函数渲染组件

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
    	<p>444444</p>
  	</div>

  	<script>
		//字面量类型模板对象
    	var login = {
      		template: '<h1>这是登录组件</h1>'
    	}
	
    	// 创建 Vue 实例，得到 ViewModel
    	var vm = new Vue({
      		el: '#app',
      		data: {},
      		methods: {},
          	//render是一个属性，属性值是一个方法。形参createElements
      		render: function (createElements) { 
              	// createElements 是一个方法，调用它，能够把指定的组件模板，渲染为 html 结构
              	//将login组件模板传给createElements
        		return createElements(login);
        		// 注意：这里 return 的结果，会 替换页面中el 指定的那个容器(也就是删掉id为app的容器)
      		}
    	});
  	</script>
</body>

</html>
```

在页面中渲染基本的组件是标签放在el指定的容器中，而render函数渲染是直接替换页面中el 指定的那个容器(也就是删掉id为app的容器)为刚渲染的login组件.

使用components注册组件，就相当于插值表达式 {{msg}}，只替换当前位置。不会把之前内容清空。

而render属性渲染出来的组件，相当于v-text。它将div中的所有内容都清空了。把自己放在那位置。

## 二、如何在 webpack 构建的项目中，使用 Vue 进行开发?

**在使用webpack构建的Vue项目中使用模板对象？**

复习：

### 1.**普通网页中如何使用(导入)vue？**

1. 使用script标签，引入vue包

   ​	注意：这个Vue包是最全的。而如果`import Vue from 'vue'`导入的不全，只提供了一个runtime-only的方式，并没有提供网页中的那样使用方式

2. 在index页面，创建一个id为app的容器

3. 通过new Vue得到一个vm实例

   ​

### 2.**如何在webpack构建的项目中使用(导入)vue？**

**在webpack 中尝试使用 Vue：**

方式一：还是使用原来网页版

```javascript
// 在webpack 中尝试使用 Vue：

// 注意： 在 webpack 中， 使用 import Vue from 'vue' 导入的 Vue 构造函数，功能不完整，只提供了 runtime-only 的方式，并没有提供 像网页中那样的使用方式；

//import Vue from 'vue';			再使用下面的网页版的通过new Vue得到一个vm实例   发现会报错
// 回顾 包的查找规则：
// 1. 找 项目根目录中有没有 node_modules 的文件夹
// 2. 在 node_modules 中 根据包名，找对应的 vue 文件夹
// 3. 在 vue 文件夹中，找 一个叫做 package.json 的包配置文件
// 4. 在 package.json 文件中，查找 一个 main 属性【main属性指定了这个包在被加载时候，的入口文件】
	//webpack中导入的并不是vue.js而是vue.runtime.commmon.js
//想使用就得手动导入最全的vue.js
import Vue from '../node_modules/vue/dist/vue.js'

var vm = new Vue({
	el: '#app',
  	data: {
    	msg: '123'
  	},
})


// 1. 导入 login 组件
import login from './login.vue'
// 默认，webpack 无法打包 .vue 文件，需要安装 相关的loader： 
//  cnpm i vue-loader vue-template-compiler -D
//  在配置文件中，新增loader哦配置项 { test:/\.vue$/, use: 'vue-loader' }

//如果想使用这种网页的方式。必须导vue.js的包而不是vue.runtime.commmon.js
var vm = new Vue({
  el: '#app',
  data: {
    msg: '123'
  },
})
```

方式二：

还是导入`import Vue from 'vue';`	。但是我们可以在webpack.config.js中加一个节点。

在`webpack.config.js`中添加`resolve`属性：

```javascript
resolve: {
	alias: {	// 修改 Vue 被导入时候的包的路径
      	'vue$': 'vue/dist/vue.esm.js'
    }
}
```

`webpack.config.js`全部代码

```javascript
// 由于 webpack 是基于Node进行构建的，所有，webpack的配置文件中，任何合法的Node代码都是支持的
var path = require('path')
// 在内存中，根据指定的模板页面，生成一份内存中的首页，同时自动把打包好的bundle注入到页面底部
// 如果要配置插件，需要在导出的对象中，挂载一个 plugins 节点
var htmlWebpackPlugin = require('html-webpack-plugin')

// 当以命令行形式运行 webpack 或 webpack-dev-server 的时候，工具会发现，我们并没有提供 要打包 的文件的 入口 和 出口文件，此时，他会检查项目根目录中的配置文件，并读取这个文件，就拿到了导出的这个 配置对象，然后根据这个对象，进行打包构建
module.exports = {
  entry: path.join(__dirname, './src/main.js'), // 入口文件
  output: { // 指定输出选项
    path: path.join(__dirname, './dist'), // 输出路径
    filename: 'bundle.js' // 指定输出文件的名称
  },
  plugins: [ // 所有webpack  插件的配置节点
    new htmlWebpackPlugin({
      template: path.join(__dirname, './src/index.html'), // 指定模板文件路径
      filename: 'index.html' // 设置生成的内存页面的名称
    })
  ],
  module: { // 配置所有第三方loader 模块的
    rules: [ // 第三方模块的匹配规则
      { test: /\.css$/, use: ['style-loader', 'css-loader'] }, // 处理 CSS 文件的 loader
      { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] }, // 处理 less 文件的 loader
      { test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] }, // 处理 scss 文件的 loader
      { test: /\.(jpg|png|gif|bmp|jpeg)$/, use: 'url-loader?limit=7631&name=[hash:8]-[name].[ext]' }, // 处理 图片路径的 loader
      // limit 给定的值，是图片的大小，单位是 byte， 如果我们引用的 图片，大于或等于给定的 limit值，则不会被转为base64格式的字符串， 如果 图片小于给定的 limit 值，则会被转为 base64的字符串
      { test: /\.(ttf|eot|svg|woff|woff2)$/, use: 'url-loader' }, // 处理 字体文件的 loader 
      { test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ }, // 配置 Babel 来转换高级的ES语法
      { test: /\.vue$/, use: 'vue-loader' } // 处理 .vue 文件的 loader
    ]
  },
  resolve: {
    alias: { // 修改 Vue 被导入时候的包的路径
      // "vue$": "vue/dist/vue.js"
    }
  }
}
```

## 三、使用网页形式使用组件

### 1.导入完整vue.js使用组件

在index中放组件。比如login组件
index.html的容器中添加一个`<login></login>`

然后在main.js

```javascript
import Vue from '../node_modules/vue/dist/vue.js'

var vm = new Vue({
	el: '#app',
  	data: {
    	msg: '123'
  	},
})

var login = {
  template: '<h1>这是login组件，是使用网页中形式创建出来的组件</h1>'
}

var vm = new Vue({
  el: '#app',
  data: {
    msg: '123'
  },
  //注册组件
  components: {
    login
  }
})

```
是正常的，但是如果是import Vue from 'vue'; 导入是runtime-only的vue包就报错。

那runtime-only中如何渲染一个组件？

### 2.在`import Vue from 'vue'; `导入的runtime-only的vue包中渲染一个组件

不定义一个字面量类型模板对象，也不在vm实例中用 components。

而是单独在src下定义一个login.vue。这个 .vue文件就是一个纯粹的组件。里面有三部分。

这个通过.vue创建的文件，webpack不识别。

login.vue

```javascript
//第一部分：写html代码模板
<template>
	<div>
    	<h1>这是登录组件，使用 .vue 文件定义出来的 --- {{msg}}</h1>
  	</div>
</template>

//第二部分：写业务逻辑
<script>
export default {
	data() {	// 注意：组件中的 data 必须是 function
   		return {
      		msg: "123"
    	};
  	},
  	methods: {
    	show() {
      		console.log("调用了 login.vue 中的 show 方法");
    	}
  	}
};
</script>

//写样式
<style>

</style>
```

main.js

```javascript
import Vue from 'vue';


// 1. 导入 login 组件
import login from './login.vue'
// 默认，webpack 无法打包 .vue 文件，需要安装 相关的loader： 
//  cnpm i vue-loader vue-template-compiler -D				(vue-loader内部又依赖vue-template-compiler)
//  在配置文件webpack.config.js中，新增loader配置项 { test:/\.vue$/, use: 'vue-loader' }


var vm = new Vue({
  	el: '#app',
  	data: {
    	msg: '123'
  	},
  	//注册组件  		仅仅这样webpack打包还是不行
  	//components: {
    //	login
  	//}
 
  /*   在 webpack 中，如果想要通过 vue， 把一个组件放到页面中去展示，vm 实例中的 render 函数可以实现
  	render: function (createElements) { 
    	return createElements(login)
  	} 
  */
  	render: c => c(login)
})

```

index.html

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>

<body>
  	<!-- 这是容器 -->
  	<div id="app">
    	<p>{{msg}}</p>
        
    	<login></login>
  	</div>
</body>

</html>
```

总结梳理： webpack 中如何使用 vue :

1. 安装vue的包： ` cnpm i vue -S`

2. 由于 在 webpack 中，推荐使用 .vue 这个组件模板文件定义组件，所以，需要安装 能解析这种文件的 loader   

   ` cnpm i vue-loader vue-template-complier -D`

3. 在 main.js 中，导入 vue 模块  `import Vue from 'vue'`

4. 定义一个 .vue 结尾的组件，其中，组件有三部分组成： template   script     style

5. 使用` import login from './login.vue' `导入这个组件

6. 创建 vm 的实例 `var vm = new Vue({ el: '#app', render: c => c(login) })`

7. 在页面中创建一个 id 为 app 的 div 元素，作为我们 vm 实例要控制的区域；

### 3.补充：export default和export的使用方式

如何在vue文件定义自己的data和method？

login.vue中

```javascript
//第二部分：写业务逻辑
<script>
export default {
	data() {	// 注意：组件中的 data 必须是 function
   		return {
      		msg: "123"
    	};
  	},
  	methods: {
    	show() {
      		console.log("调用了 login.vue 中的 show 方法");
    	}
  	}
};
</script>
```

test.js

```javascript
// 这是 Node 中向外暴露成员的形式：
// module.exports = {}

// 在 ES6中，也通过规范的形式，规定了ES6中如何 导入 和 导出 模块
//  ES6中导入模块，使用   import 模块名称 from '模块标识符'    import '表示路径'

// 在 ES6 中，使用 export default 和 export 向外暴露成员：
1. export default
//1.直接export default这个成员
export default {
 	name: 'zs',
  	age: 20  
}
//2.或者是用一个变量来接受，再export default这个变量
var info = {
  name: 'zs',
  age: 20
}

export default info

// 注意： export default 向外暴露的成员，可以使用任意的变量来接收
// 注意： 在一个模块中，export default 只允许向外暴露1次
// 注意： 在一个模块中，可以同时使用 export default 和 export 向外暴露成员
-------------------------------------------------
  
2.export
export var title = '小星星'
export var content = '哈哈哈'

// 注意： 使用 export 向外暴露的成员，只能使用 { } 的形式来接收，这种形式，叫做 【按需导出】
// 注意： export 可以向外暴露多个成员， 同时，如果某些成员，我们在 import 的时候，不需要，则可以 不在 {}  中定义
// 注意： 使用 export 导出的成员，必须严格按照 导出时候的名称，来使用  {}  按需接收；
// 注意： 使用 export 导出的成员，如果 就想 换个 名称来接收，可以使用 as 来起别名；



// 在Node中 使用 var 名称 = require('模块标识符')
// module.exports 和 exports 来暴露成员
```

然后在main.js中就可以通过导入获得

```javascript
import Vue from 'vue';


// 1. 导入 login 组件
import login from './login.vue'


var vm = new Vue({
  	el: '#app',
  	data: {
    	msg: '123'
  	},
  	render: c => c(login)
})



import m222, { title as title123, content } from './test.js';

console.log(m222)
console.log(title123 + ' --- ' + content)
```



### 4.总结：在webpack中配置.vue组件页面的解析

1. 运行`cnpm i vue -S`将vue安装为运行依赖；

2. 运行`cnpm i vue-loader vue-template-compiler -D`将解析转换vue的包安装为开发依赖；

3. 运行`cnpm i style-loader css-loader -D`将解析转换CSS的包安装为开发依赖，因为.vue文件中会写CSS样式；

4. 在`webpack.config.js`中，添加如下`module`规则：

```javascript
module: {
	rules: [
      	{ test: /\.css$/, use: ['style-loader', 'css-loader'] },
      	{ test: /\.vue$/, use: 'vue-loader' }
    ]
}
```

5. 创建`App.js`组件页面：

```javascript
<template>
    <!-- 注意：在 .vue 的组件中，template 中必须有且只有唯一的根元素进行包裹，一般都用 div 当作唯一的根元素 -->
	<div>
		<h1>这是APP组件 - {{msg}}</h1>
       	 <h3>我是h3</h3>
    </div>
</template>

<script>
    //注意:在.vue的组件中，通过script标签来定义组件的行为，需要使用ES6中提供的export default方式，导出一个vue实例对象
	export default {
		data() {
        	return {
         		msg: 'OK'
        	}
      	}
    }
</script>


<style scoped>
	h1 {
      	color: red;
    }
</style>
```

6. 创建`main.js`入口文件：

```javascript
// 导入 Vue 组件
import Vue from 'vue'

// 导入 App组件
import App from './components/App.vue'

// 创建一个 Vue 实例，使用 render 函数，渲染指定的组件
var vm = new Vue({
      el: '#app',
      render: c => c(App)
});
```

## 
## 四、ES6中语法使用总结

1. 使用 `export default` 和 `export` 导出模块中的成员; 对应ES5中的 `module.exports` 和 `export`

2. 使用 `import ** from **` 和 `import '路径'` 还有 `import {a, b} from '模块标识'` 导入其他模块

3. 使用箭头函数：`(a, b)=> { return a-b; }`



## 五、在vue组件页面中，集成vue-router路由模块

[vue-router官网](https://router.vuejs.org/)

普通网页中就是通过scirpt标签引入vue-router.js就会自动的把vue路由安装到vue上。现在引用了webpack这个模块化的工具。在模块化工程中使用，必须通过Vue.use()明确地安装路由功能。如果使用全局的scirpt标签，则无需手动安装。

### 1.平级路由

main.js

```javascript
import Vue from 'vue'
// 1. 导入 vue-router 包
import VueRouter from 'vue-router'
// 2. 手动安装 VueRouter 
Vue.use(VueRouter)

// 导入 app 组件
import app from './App.vue'
// 导入 Account 组件
import account from './main/Account.vue'
import goodslist from './main/GoodsList.vue'

// 3. 创建路由对象
var router = new VueRouter({
  	routes: [
    	// account  goodslist  定义路由
    	{ path: '/account', component: account },
    	{ path: '/goodslist', component: goodslist }
  	]
})

var vm = new Vue({
	el: '#app',
  	render: c => c(app), 
  //render会把el指定的容器中所有的内容都清空覆盖，所以不要把路由的router-view和router-link直接写到el所控制的元素中。不起作用。而是写在app组件中
  	router // 4. 将路由对象挂载到 vm 上
})

// 注意： App 这个组件，是通过 VM 实例的 render 函数，渲染出来的， render 函数如果要渲染 组件， 渲染出来的组件只能放到 el: '#app' 所指定的 元素中；
// Account 和 GoodsList 组件， 是通过 路由匹配监听到的，所以这两个组件，只能展示到 属于 路由的 <router-view></router-view> 中去；
```

app.vue

```javascript
<template>
	<div>
    	<h1>这是 App 组件</h1>

  
    	<router-link to="/account">Account</router-link>
    	<router-link to="/goodslist">Goodslist</router-link>

    	<router-view></router-view>
  	</div>
</template>

<script>
</script>

<style>

</style>
```

创建一个文件夹。里面放两个组件 Account.vue和 GoodsList.vue 组件

index.html

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>

<body>
  <!-- 这是容器 -->
  <div id="app">
    
  </div>
</body>

</html>
```

我们也可以抽离出

### 2.结合webpack实现children子路由

上面创建了两个平级路由Account和Goodlist。我们可以再在Account下创建两个子组件。首先同样创建一个文件夹比如subcom。里面放两个**子**组件register.vue和login.vue组件。

然后在main.js中的routers下再创建子组件。添加如下

```javascript
// 导入Account的两个子组件
import login from './subcom/login.vue'
import register from './subcom/register.vue'

// 3. 创建路由对象
var router = new VueRouter({
  	routes: [
    	// account  goodslist  定义路由
    	{ 
          path: '/account',
          component: account,
          children: [
          		{path: 'login',component: account}	
            	{path: 'register',component: account}
          ]
        },
    	{ path: '/goodslist', component: goodslist }
  	]
})

```

完整如下

main.js

```javascript
import Vue from 'vue'
// 1. 导入 vue-router 包
import VueRouter from 'vue-router'
// 2. 手动安装 VueRouter 
Vue.use(VueRouter)

// 导入 app 组件
import app from './App.vue'
// 导入 Account 组件
import account from './main/Account.vue'
import goodslist from './main/GoodsList.vue'

// 导入Account的两个子组件
import login from './subcom/login.vue'
import register from './subcom/register.vue'


// 3. 创建路由对象
var router = new VueRouter({
  	routes: [
    	// account  goodslist  定义路由
    	{ 
          path: '/account',
          component: account,
          children: [
          		{path: 'login',component: account}	
            	{path: 'register',component: account}
          ]
        },
    	{ path: '/goodslist', component: goodslist }
  	]
})

var vm = new Vue({
	el: '#app',
  	render: c => c(app), 
  //render会把el指定的容器中所有的内容都清空覆盖，所以不要把路由的router-view和router-link直接写到el所控制的元素中。不起作用。而是写在app组件中
  	router // 4. 将路由对象挂载到 vm 上
})

// 注意： App 这个组件，是通过 VM 实例的 render 函数，渲染出来的， render 函数如果要渲染 组件， 渲染出来的组件只能放到 el: '#app' 所指定的 元素中；
// Account 和 GoodsList 组件， 是通过 路由匹配监听到的，所以这两个组件，只能展示到 属于 路由的 <router-view></router-view> 中去；
```

Account.vue修改

```javascript
<template>
  <div>
    <h1>这是 App 组件</h1>
  
    <router-link to="/account/login">登录</router-link>
    <router-link to="/account/register">注册</router-link>

    <router-view></router-view>			//路由展示的位置
  </div>
</template>

<script>
</script>

<style>

</style>
```

我们也可以将路由抽离

1. 导入路由模块：

```javascript
import VueRouter from 'vue-router'
```

2. 安装路由模块：

```javascript
Vue.use(VueRouter);
```

3. 导入需要展示的组件:

```javascript
import login from './components/account/login.vue'
import register from './components/account/register.vue'
```

4. 创建路由对象:

```javascript
var router = new VueRouter({
  	routes: [
    	{ path: '/', redirect: '/login' },
    	{ path: '/login', component: login },
    	{ path: '/register', component: register }
  	]
});
```

5. 将路由对象，挂载到 Vue 实例上:

```javascript
var vm = new Vue({
    el: '#app',
  	// render: c => { return c(App) }
  	render(c) {
    	return c(App);
  	},
  	router // 将路由对象，挂载到 Vue 实例上
});
```

6. 改造App.vue组件，在 template 中，添加`router-link`和`router-view`：

```javascript
<router-link to="/login">登录</router-link>
<router-link to="/register">注册</router-link>
<router-view></router-view>
```



### 3.组件中的css作用域问题

如果我只在登录子组件login.vuw中写了样式

```javascript
<template>
  <div>
    <h3>这是Account的登录子组件</h3>
  </div>
</template>

<script>
</script>


//<style>
<style scoped>
div {
  color: red;
}
</style>
```

在之前一切正常。但是当我点了登录这个子组件。login.vue本身以及所有的父组件全部应用了样式。本来应该是只是子组件改变。在style标签后加一个 scoped属性即可。

另外，普通的标签只支持普通的样式。如果要启用scss或者less。需要为style元素设置lang属性。

比如我在Acoount组件定义。

```javascript
<style lang="scss" scoped>
// 只要咱们的 style 标签， 是在 .vue 组件中定义的，那么，推荐都为 style 开启 scoped 属性
body{
	div {
  		font-style: italic;
	}
}
</style>
```

注意只要自己应用样式，也要加scoped。样式的scoped是通过css的属性选择器实现的。添加了scoped的检查元素会发现样式是这样的: 	div[data-v-44a2eb03]  {  font-style: italic;  }

### 4.抽离路由为单独的模块

将创建路由对象的过程和导入路由组件

main.js

```javascript
import Vue from 'vue'
// 1. 导入 vue-router 包
import VueRouter from 'vue-router'
// 2. 手动安装 VueRouter 
Vue.use(VueRouter)

// 导入 app 组件
import app from './App.vue'

// 导入 自定义路由模块
import router from './router.js'

var vm = new Vue({
	el: '#app',
  	render: c => c(app), //render会把el指定的容器中，所有的内容都清空覆盖，所以不要把路由的router-view和router-link 直接写到 el 所控制的元素中
  	router 			// 4. 将路由对象挂载到 vm 上
})

// 注意： App 这个组件，是通过 VM 实例的 render 函数，渲染出来的， render 函数如果要渲染 组件， 渲染出来的组件，只能放到 el: '#app' 所指定的 元素中；
// Account 和 GoodsList 组件， 是通过 路由匹配监听到的，所以， 这两个组件，只能展示到 属于 路由的 <router-view></router-view> 中去；
```

router.js

```javascript
import VueRouter from 'vue-router'

// 导入 Account 组件
import account from './main/Account.vue'
import goodslist from './main/GoodsList.vue'

// 导入Account的两个子组件
import login from './subcom/login.vue'
import register from './subcom/register.vue'

// 3. 创建路由对象
var router = new VueRouter({
	routes: [
    	// account  goodslist
    	{
      		path: '/account',
      		component: account,
      		children: [
        		{ path: 'login', component: login },
        		{ path: 'register', component: register }
      		]
    	},
    	{ path: '/goodslist', component: goodslist }
  	]
})

// 把路由对象暴露出去
export default router
```


