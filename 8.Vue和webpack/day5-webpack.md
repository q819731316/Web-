

# Vue.js - Day5 - Webpack

## 一、

### 在网页中会引用哪些常见的静态资源？
+ JS
  +  .js  .jsx  .coffee  .ts（TypeScript  类 C# 语言） 后两种是中间语言，需要通过编译器编译成js
+ CSS
  + .css  .less   .sass(无用，scss的老版本)   .scss


+ Images
  + .jpg   .png   .gif   .bmp   .svg


+ 字体文件（Fonts）
  + .svg   .ttf   .eot   .woff   .woff2


+ 模板文件
  + .ejs   .jade  .vue【这是在webpack中定义组件的方式，推荐这么用】


### 网页中引入的静态资源多了以后有什么问题？？？
1. 网页加载速度慢， 因为 我们要发起很多的二次请求；
2. 要处理错综复杂的依赖关系


### 如何解决上述两个问题
1. 问题1：合并、压缩、精灵图、图片的Base64编码(通过编码请求时下载，不用二次请求，不过只适合小图片 )
2. 问题2：可以使用之前学过的requireJS、也可以使用webpack可以解决各个包之间的复杂依赖关系；

## 二、什么是webpack?
webpack 是前端的一个项目构建工具，它是基于 Node.js 开发出来的一个前端工具；


### 如何完美实现上述的2种解决方案
1. 使用Gulp， 是基于 task 任务的；(只能处理Glup任务周边的，然后将任务相互关联起来来完成项目的构建)
2. 使用Webpack， 是基于整个项目进行构建的；
+ 借助于webpack这个前端自动化构建工具，可以完美实现资源的合并、打包、压缩、混淆等诸多功能。
+ 根据官网的图片介绍webpack打包的过程
+ [webpack官网](http://webpack.github.io/)

### 1.webpack安装的两种方式
1. 运行`npm i webpack -g`全局安装webpack，这样就能在全局使用webpack的命令
2. 在项目根目录中运行`npm i webpack --save-dev`安装到项目依赖中

### 2.初步使用webpack打包构建列表隔行变色案例

列表隔行变色案例

1. 运行`npm init -y`初始化项目，使用npm管理项目中的依赖包
2. 创建项目基本的目录结构
3. 使用`cnpm i jquery --save`安装jquery类库
4. 创建`main.js`并书写各行变色的代码逻辑：
```javascript
// 导入jquery类库
import $ from 'jquery'

// 设置偶数行背景色，索引从0开始，0是偶数
$('#list li:even').css('backgroundColor','lightblue');
// 设置奇数行背景色
$('#list li:odd').css('backgroundColor','pink');
```
5. 直接在页面上引用`main.js`会报错，因为浏览器不认识`import`这种高级的JS语法，需要使用webpack进行处理，webpack默认会把这种高级的语法转换为低级的浏览器能识别的语法；
6. 运行`webpack 入口文件路径 输出文件路径`对`main.js`进行处理：
```javascript
webpack src/js/main.js dist/bundle.js
```
首页的index.html中导入main.js。在main.js中导入所需要的第三方包和资源。

以使得index.html不需要进行二次请求。就是不推荐在首页index.js导入包，否则又是上面说的静态资源拖慢速度的问题。让

main.js

```javascript
/* 
	这是 main.js 是我们项目的JS入口文件
*/

// 1. 导入 Jquery
// import *** from *** 是ES6中导入模块的方式			从node_moduels中导入jquery包，再用变量$来接受
import $ from 'jquery'		
// 由于 ES6的代码，太高级了，浏览器解析不了，所以，这一行执行会报错。
// const $ = require('jquery');       也不行，浏览器也不支持node语法
/*	
	解决：通用webpack对文件做一层处理
	刚才运行的命令格式：    webpack  要打包的文件的路径  打包好的输出文件的路径
	在编译器终端输入： webpack .\src\main.js  .\dist\bundle.js     
	通过webpack处理生成浏览器能识别的文件。(将识别不了的es6的mainjs处理成可以的bundle.js )  
*/

$(function () {
  $('li:odd').css('backgroundColor', 'yellow')
  $('li:even').css('backgroundColor', function () {
    return '#' + 'D97634'
  })
})


// 经过刚才的演示，Webpack 可以做什么事情？？？
// 1. webpack 能够处理 JS 文件的互相依赖关系；
// 2. webpack 能够处理JS的兼容问题，把 高级的、浏览器不识别的语法，转为 低级的，浏览器能正常识别的语法
```

main.js中的代码，涉及到了ES6的新语法，但是浏览器不识别。通过webpack这么一个前端构建工具。把main.js做了一下处理，生成一个bundle.js的文件。而如果我们在main.js进行了修改。需要重新打包。

如果我们不想每次打包都输入完整的要打包的命令格式：`webpack  要打包的文件的路径  打包好的输出文件的路径`，而是希望输入webpack就实现。就可以如下操作。 

### 3.使用webpack的配置文件简化打包时候的命令

1. 在项目根目录中创建`webpack.config.js`
2. 由于运行webpack命令的时候，webpack需要指定入口文件和输出文件的路径，所以，我们需要在`webpack.config.js`中配置这两个路径：
   webpack.config.js

```javascript
// 导入处理路径的模块
var path = require('path');

// 导出一个配置对象，将来webpack在启动的时候，会默认来查找webpack.config.js，并读取这个文件中导出的配置对象，来进行打包处理
//这个配置文件，其实就是一个js文件，通过node中的模块操作，向外暴露了一个配置对象
module.exports = {
  	//入口，表示要用webpack打包哪个文件
	entry: path.resolve(__dirname, 'src/js/main.js'), 	// 项目入口文件
  	//出口
	output: {										 // 配置输出选项
      	//path:path.join(__dirname, './dist')
		path: path.resolve(__dirname, 'dist'), 			// 配置输出的路径
		filename: 'bundle.js' 						   // 配置输出的文件名
	}
}

// 当我们在 控制台，直接输入 webpack 命令执行的时候，webpack 做了以下几步：
//  1. 首先，webpack 发现，我们并没有通过命令的形式，给它指定入口和出口
//  2. webpack 就会去 项目的 根目录中，查找一个叫做 `webpack.config.js` 的配置文件
//  3. 当找到配置文件后，webpack 会去解析执行这个 配置文件，当解析执行完配置文件后，就得到了 配置文件中，导出的配置对象
//  4. 当 webpack 拿到 配置对象后，就拿到了 配置对象中，指定的 入口  和 出口，然后进行打包构建；
```

### 4.实现webpack的实时打包构建

如果我们想写一行就能看到最新效果。需要新装一个工具实现实时打包编译。

node		nodemon				监听代码文件的变动，当代码改变之后，自动重启。

webpack		webpack-dev-server 	 	监听代码文件的变动，当代码改变之后，自动打包。

实现实时监听

```javascript
使用 webpack-dev-server 这个工具，来实现自动打包编译的功能

1. 运行 npm i webpack-dev-server -D 把这个工具安装到项目的本地开发依赖
2. 安装完毕后，这个 工具的用法， 和 webpack 命令的用法，完全一样
3. 由于，我们是在项目中，本地安装的 webpack-dev-server ， 所以，无法把它当作 脚本命令，在powershell 终端中直接运行；（只有那些 安装到 全局 -g 的工具，才能在 终端中正常执行）
4. 注意： webpack-dev-server 这个工具，如果想要正常运行，要求，在本地项目中，必须安装 webpack
5. webpack-dev-server 帮我们打包生成的 bundle.js 文件，并没有存放到 实际的 物理磁盘上；而是，直接托管到了 电脑的内存中，所以，我们在 项目根目录中，根本找不到这个打包好的 bundle.js;
6. 我们可以认为， webpack-dev-server 把打包好的 文件，以一种虚拟的形式，托管到了 咱们项目的 根目录中，虽然我们看不到它，但是，可以认为， 和 dist  src   node_modules  平级，有一个看不见的文件，叫做 bundle.js
```



1. 由于每次重新修改代码之后，都需要手动运行webpack打包的命令，比较麻烦，所以使用`webpack-dev-server`来实现代码实时打包编译，当修改代码之后，会自动进行打包构建。
2. 运行`cnpm i webpack-dev-server --save-dev`安装到开发依赖
3. 安装完成之后，在命令行直接运行`webpack-dev-server`来进行打包，发现报错，此时需要借助于`package.json`文件中的指令，来进行运行`webpack-dev-server`命令，在`scripts`节点下新增`"dev": "webpack-dev-server"`指令，发现可以进行实时打包，但是dist目录下并没有生成`bundle.js`文件，这是因为`webpack-dev-server`将打包好的文件放在了内存中


+ 把`bundle.js`放在内存中的好处是：由于需要实时打包编译，所以放在内存中速度会非常快
+ 这个时候访问webpack-dev-server启动的`http://localhost:8080/`网站，发现是一个文件夹的面板，需要点击到src目录下，才能打开我们的index首页，此时引用不到bundle.js文件，需要修改index.html中script的src属性为:`<script src="../bundle.js"></script>`
+ 为了能在访问`http://localhost:8080/`的时候直接访问到index首页，可以使用`--contentBase src`指令来修改dev指令，指定启动的根目录：
```javascript
 "dev": "webpack-dev-server --contentBase src"
```
 同时修改index页面中script的src属性为`<script src="bundle.js"></script>`



补充：

在package.json的在`scripts`节点下新增`"dev": "webpack-dev-server"`，然后通过`npm run dev`运行 

### 5.实现自动打开浏览器、热更新和配置浏览器的默认端口号
**注意：热更新在JS中表现的不明显，可以从一会儿要讲到的CSS身上进行介绍说明！**
#### 方式1：
+ 修改`package.json`的script节点如下，其中`--open`表示自动打开浏览器，`--port 4321`表示打开的端口号为4321，`--hot`表示启用浏览器热更新：`--contenBase src`contenBase修改根路径为src。(根目录打开不是项目文件夹的显示页面)。`--hot`实现页面无刷新(异步)。(js无，样式修改有效：不需要刷新就可以看到样式修改)
```javascript
"dev": "webpack-dev-server --hot --port 4321 --open"
```

#### 方式2：

相对麻烦些

1. 修改`webpack.config.js`文件，新增`devServer`节点如下：
```javascript
//配置dev-server命令参数的第二种方式
devServer:{
	open:true,			//自动打开浏览器	
	port:4321,			//设置启动时的运行端口				
	contentBase:'src',		//指定托管的根目录
     hot:true,				//启用热更新
}
```
2. 在`webpack.config.js`文件头部引入`webpack`模块：
```javascript
var webpack = require('webpack');
```
3. 在webpack.config.js下新增`plugins`节点，节点下新增：
```javascript
plugins：[
	new webpack.HotModuleReplacementPlugin();		//new一个热更新的模块对象
]
```

### 6.使用`html-webpack-plugin`插件配置启动页面：

上面已经将bundle.js存在内存。我们也可以使用`html-webpack-plugin`插件**在内存中根据模块生成index.html页面**

>这个插件的两个作用：
>
>1. 自动在内存中根据指定页面生成一个内存的页面
>2. 自动，把打包好的 bundle.js 追加到页面中去



由于使用`--contentBase`指令的过程比较繁琐，需要指定启动的目录，同时还需要修改index.html中script标签的src属性，所以推荐大家使用`html-webpack-plugin`插件配置启动页面.

1. 运行`cnpm i html-webpack-plugin --save-dev`安装到开发依赖
2. 修改`webpack.config.js`配置文件如下：

```javascript
const path = require('path')
// 启用热更新的 第2步
const webpack = require('webpack')
// 导入在内存中生成 HTML 页面的 插件
// 只要是插件，都一定要 放到 plugins 节点中去

const htmlWebpackPlugin = require('html-webpack-plugin')

//这个配置文件，起始就是一个 JS 文件，通过 Node 中的模块操作，向外暴露了一个 配置对象
module.exports = {
  	entry: path.join(__dirname, './src/main.js'),			// 入口，表示，要使用 webpack 打包哪个文件
  	output: { 		// 输出文件相关的配置
    	path: path.join(__dirname, './dist'), 				// 指定 打包好的文件，输出到哪个目录中去
    	filename: 'bundle.js'								 // 这是指定 输出的文件的名称
  	},
  	devServer: { // 这是配置 dev-server 命令参数的第二种形式，相对来说，这种方式麻烦一些
    	//  --open --port 3000 --contentBase src --hot
    	open: true, 						// 自动打开浏览器
    	port: 3000, 						// 设置启动时候的运行端口
    	contentBase: 'src', 				// 指定托管的根目录
    	hot: true 							// 启用热更新 的 第1步
  	},
  	plugins: [ 								// 配置插件的节点
    	new webpack.HotModuleReplacementPlugin(), 	//new一个热更新的 模块对象，这是 启用热更新的第 3 步
    	new htmlWebpackPlugin({ 					//创建一个 在内存中 生成HTML页面的插件
      		template: path.join(__dirname, './src/index.html'),	 //指定模板页面，将来会根据指定的页面路径，去生成内存中的 页面
      		filename: 'index.html' // 指定生成的页面的名称
    	})
  	],
}
```

3. 修改`package.json`中`script`节点中的dev指令如下：

```javascript
"dev": "webpack-dev-server"
```

4. 将index.html中script标签注释掉，因为`html-webpack-plugin`插件会自动把bundle.js注入到index.html页面中！

>当使用 html-webpack-plugin 之后，我们不再需要手动处理 bundle.js 的引用路径了，因为 这个插件，已经帮我们自动 创建了一个 合适的 script , 并且，引用了 正确的路径
>  <!-- <script src="/bundle.js"></script> -->



##三、使用webpack打包其他文件

注意： webpack, 默认只能打包处理 JS 类型的文件，无法处理 其它的非 JS 类型的文件；
如果要处理 非JS类型的文件，我们需要手动安装一些 合适 第三方 loader 加载器；

### 1.使用webpack打包css文件

```javascript
// 这是 main.js 是我们项目的JS入口文件

// 1. 导入 Jquery
// import *** from *** 是ES6中导入模块的方式
// 由于 ES6的代码，太高级了，浏览器解析不了，所以，这一行执行会报错
import $ from 'jquery'
// const $ = require('jquery')

import './css/index.css'
import './css/index.scss'
// 注意： 如果要通过路径的形式，去引入 node_modules 中相关的文件，可以直接省略 路径前面的 node_modules 这一层目录，直接写 包的名称，然后后面跟上具体的文件路径
// 不写 node_modules 这一层目录 ，默认 就会去 node_modules 中查找
import 'bootstrap/dist/css/bootstrap.css'


// 注意： webpack, 默认只能打包处理 JS 类型的文件，无法处理 其它的非 JS 类型的文件；
// 如果要处理 非JS类型的文件，我们需要手动安装一些 合适 第三方 loader 加载器；
// 1. 如果想要打包处理 css 文件，需要安装 cnpm i style-loader css-loader -D
// 2. 打开 webpack.config.js 这个配置文件，在 里面，新增一个 配置节点，叫做 module, 它是一个对象；在 这个 module 对象身上，有个 rules 属性，这个 rules 属性是个 数组；这个数组中，存放了，所有第三方文件的 匹配和 处理规则；
```

1. 运行`cnpm i style-loader css-loader --save-dev` 安装 合适 第三方 loader 加载器
2. 修改`webpack.config.js`这个配置文件：
```javascript
module: { // 用来配置第三方loader模块的
        rules: [ // 文件的匹配规则(使用正则表达式来匹配)
          	//匹配所有.css结尾的文件    \. 转义就是个.    $符结尾 
            { test: /\.css$/, use: ['style-loader', 'css-loader'] },     //处理css文件的规则
        ]
    }
```
3. 注意：`use`表示使用哪些模块来处理`test`所匹配到的文件；`use`中相关loader模块的调用顺序是从后向前调用的；

```javascript
// 注意： webpack 处理第三方文件类型的过程：
// 1. 发现这个 要处理的文件不是JS文件，然后就去 配置文件中，查找有没有对应的第三方 loader 规则
// 2. 如果能找到对应的规则， 就会调用 对应的 loader 处理 这种文件类型；
// 3. 在调用loader 的时候，是从后往前调用的；
// 4. 当最后的一个 loader 调用完毕，会把 处理的结果，直接交给 webpack 进行 打包合并，最终输出到  bundle.js 中去
```

### 2.使用webpack打包less文件
1. 运行`cnpm i less-loader less -D`
2. 修改`webpack.config.js`这个配置文件：
```javascript
{ test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] },
```

### 3.使用webpack打包sass文件
1. 运行`cnpm i sass-loader node-sass --save-dev`

   node-sass使用npm是装不下的，一般都要用cnpm来装

2. 在`webpack.config.js`中添加处理sass文件的loader模块：
```javascript
{ test: /\.scss$/, use: ['style-loader', 'css-loader', 'sass-loader'] }
```

### 4.使用webpack处理css中的路径
1. 运行`cnpm i url-loader file-loader --save-dev`
2. 在`webpack.config.js`中添加处理url路径的loader模块：
```javascript
{ test: /\.(png|jpg|gif)$/, use: 'url-loader' }
```
3. 可以通过`limit`指定进行base64编码的图片大小；只有小于指定字节（byte）的图片才会进行base64编码：
```javascript
{ test: /\.(png|jpg|gif)$/, use: 'url-loader?limit=43960' },
```

limit给定的值是图片大小，单位是byte。如果我们引用的图片大于或等于给定的limit值。则不会被转为base64格式的字符串

这里得到的是hash值的名称(防止重名)，如果要得到原来的图片名。就需要加参数[name]之前什么名打包后还是什么名。.[ext]之前什么后缀名，打包后还是什么后缀。

```javascript
{ test: /\.(png|jpg|gif)$/, use: 'url-loader?limit=43960&name=[name].[ext]' },
```

补充：

要注意的是。如果在项目两个不同的文件夹放两个同名的不同图片，再显示在同一个页面。使用webpack处理，尽管在css中写了两个的路径名不同。但是打包后得到的页面两张图片还是一样。执行时，执行到第一个将它复制到根目录去。执行到第二个，也复制到根目录下。但是名字一样，所以替换了。

解决办法:

通过-链接hash值，让图片文件名前面加上前8位的hash值(hash值32位)。这样图片原来名称一样，但是打包后前面带了hash值，文件名就不一样了。

```javascript
{ test: /\.(png|jpg|gif)$/, use: 'url-loader?limit=43960&name=[hash:8]-[name].[ext]' },
```

### 5.使用url-loader处理字体

当我们引入bootstrap时。里面引入了字体。字体文件也要处理。否则报错。

```javascript
{ test: /\.(tff|eot|svg|woff|woff2)$/, use: 'url-loader' },
```



## 四、使用babel处理高级JS语法

1. 运行`cnpm i babel-core babel-loader babel-plugin-transform-runtime --save-dev`安装babel的相关loader包
2. 运行`cnpm i babel-preset-env babel-preset-stage-0 --save-dev`安装babel转换的语法
3. 在`webpack.config.js`中添加相关loader模块，其中需要注意的是，一定要把`node_modules`文件夹添加到排除项：

```javascript
{ test: /\.js$/, use: 'babel-loader', exclude: /node_modules/ }
```

4. 在项目根目录中添加`.babelrc`文件，并修改这个配置文件如下：

```javascript
{
    "presets":["env", "stage-0"],
    "plugins":["transform-runtime"]
}
```

5. **注意：语法插件`babel-preset-es2015`可以更新为`babel-preset-env`，它包含了所有的ES相关的语法；**

```javascript
// 项目的JS入口文件
console.log('ok')

import './css/index.css'
import './css/index.scss'
import 'bootstrap/dist/css/bootstrap.css'


// class 关键字，是ES6中提供的新语法，是用来 实现 ES6 中面向对象编程的方式
//使用class关键字，定义一个构造函数。然后可以new Person得到person实例
class Person {
  // 使用 static 关键字，可以定义静态属性。
  // 所谓的静态属性，就是 可以直接通过 类名， 直接访问的属性 Person.info
  // 和静态属性对立的 实例属性： 只能通过类的实例，来访问的属性，叫做实例属性
  static info = { name: 'zs', age: 20 }
}

//和Java  C#  实现面向对象的方式完全一样了，   class 是从后端语言中借鉴过来的， 来实现面向对象
var p1 = new Person()


//没有es6前的面向对象方式。只是人为定义了，并没有真正实现面向对象的方式。本质还是一个function
function Animal(name){
  this.name  = name
}
Animal.info = 123

var a1 = new Animal('小花')

// 这是静态属性：
console.log(Animal.info)
// 这是实例属性：
console.log(a1.name)



// 访问 Person 类身上的  info 静态属性
console.log(Person.info); 			 报错

// 在 webpack 中，默认只能处理 一部分 ES6 的新语法，一些更高级的ES6语法或者 ES7 语法，webpack 是处理不了的；这时候，就需要 借助于第三方的 loader，来帮助webpack 处理这些高级的语法，当第三方loader 把 高级语法转为 低级的语法之后，会把结果交给 webpack 去打包到 bundle.js 中

// 通过 Babel ，可以帮我们将 高级的语法转换为 低级的语法
// 1. 在 webpack 中，可以运行如下两套 命令，安装两套包，去安装 Babel 相关的loader功能：
// 1.1 第一套包： cnpm i babel-core babel-loader babel-plugin-transform-runtime -D
// 1.2 第二套包： cnpm i babel-preset-env babel-preset-stage-0 -D
// 2. 打开 webpack 的配置文件，在 module 节点下的 rules 数组中，添加一个 新的 匹配规则：
// 2.1 { test:/\.js$/, use: 'babel-loader', exclude:/node_modules/ }
// 2.2 注意： 在配置 babel 的 loader规则的时候，必须 把 node_modules 目录，通过 exclude 选项排除掉：原因有俩：
// 2.2.1 如果 不排除 node_modules， 则Babel 会把 node_modules 中所有的 第三方 JS 文件，都打包编译，这样，会非常消耗CPU，同时，打包速度非常慢；
// 2.2.2 哪怕，最终，Babel 把 所有 node_modules 中的JS转换完毕了，但是，项目也无法正常运行！
// 3. 在项目的 根目录中，新建一个 叫做 .babelrc  的Babel 配置文件，这个配置文件，属于JSON格式，所以，在写 .babelrc 配置的时候，必须符合JSON语法规范： 不能写注释，字符串必须用双引号
// 3.1 在 .babelrc 写如下的配置：  大家可以把 preset 翻译成 【语法】 的意思
        {
          "presets": ["env", "stage-0"],
          "plugins": ["transform-runtime"]
        }
// 4. 了解： 目前，我们安装的 babel-preset-env, 是比较新的ES语法， 之前， 我们安装的是 babel-preset-es2015, 现在，出了一个更新的 语法插件，叫做 babel-preset-env ，它包含了 所有的 和 es***相关的语法
```



有时候使用`$ npm i node-sass -D`装不上，这时候，就必须使用 `$ cnpm i node-sass -D`

cnpm安装很简单。`$ npm install -g cnpm --registry=https://registry.npm.taobao.org`

## 相关文章
[babel-preset-env：你需要的唯一Babel插件](https://segmentfault.com/p/1210000008466178)
[Runtime transform 运行时编译es6](https://segmentfault.com/a/1190000009065987)