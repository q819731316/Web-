# 大纲-day01
## 1.1初识Nodejs

Nodejs的设计核心思想：

​	1.异步IO

​	2.基于事件

- JavaScript是什么？ 
- JavaScript可以运行在哪里？ 

![](img/1.png)

| 浏览器     | 内核      |
| ------- | ------- |
| IE      | Trident |
| FireFox | Gecko   |
| Chrome  | WebKit  |
| Safari  | WebKit  |
| Opera   | Presto  |
| Edge    | Chakra  |

## 1.2Node.js的诞生
![image](img/2.png)

- 作者Ryan Dahl 瑞恩·达尔
    + 2004 纽约 读数学博士 
    + 2006 退学到智利 转向开发 
    + 2009.5对外宣布node项目，年底js大会发表演讲 
    + 2010 加入Joyent云计算公司 
    + 2012 退居幕后

> Node.js 是一种建立在Google Chrome’s v8 engine上的 non-blocking (非阻塞）, event-driven （基于事件的） I/O平台. 
> Node.js平台使用的开发语言是JavaScript，平台提供了操作系统低层的API，方便做服务器端编程，具体包括文件操作、进程操作、通信操作等系统模块

## 1.3Node.js可以用来做什么？

- 具有复杂逻辑的动态网站 
- WebSocket服务器 
- 命令行工具 
- 带有图形界面的本地应用程序 
- ......

## 一、终端基本使用
cmd窗口打开后执行特定命令

### 1.打开应用

window + R

-  notepad 打开记事本

-  mspaint 打开画图

-  calc 打开计算机

-  write 写字板

-  sysdm.cpl 打开环境变量设置窗口

      环境变量：帮助我们通过命令行的方式找到具体的可执行文件。

      ​	比如将php集成环境的wampmanager.exe的路径加到环境变量path种，就可以直接在cmd输入执行，默认路径C盘下就可执行而不需要进入到具体的所在路径。
### 2.cmd常用命令
- md：创建目录
- rmdir(rd)：删除目录，目录内没有文档。
- echo on a.txt：创建空文件
  - echo 123 > a.txt       写入123 （覆盖）
    - echo 456 > a.txt    追加456
- del: 删除文件
- rm 文件名： 删除文件
- cat 文件名： 查看文件内容
  - cat > 文件名：向文件中写上内容。
  - cat >> 文件名：追加

## 二、Node.js开发环境准备

1. 普通安装方式[官方网站](https://nodejs.org/zh-cn/)

2. 多版本安装方式
    - 卸载已有的Node.js
    - 下载[nvm](https://github.com/coreybutler/nvm-windows)：管理不同版的node.js
    - 在C盘创建目录dev
    - 在dev目中中创建两个子目录nvm和nodejs
    - 并且把nvm包解压进去nvm目录中
    - 在install.cmd文件上面右键选择【以管理员身份运行】
    - 打开的cmd窗口直接回车会生成一个settings.txt文件，修改文件中配置信息
    - 配置nvm和Node.js环境变量
        + NVM_HOME:C:\dev\nvm
        + NVM_SYMLINK:C:\dev\nodejs
    - 把配置好的两个环境变量加到Path中
## 三、nvm常用的命令
- nvm list 查看当前安装的Node.js所有版本
- nvm install 版本号 安装指定版本的Node.js
- nvm uninstall 版本号 卸载指定版本的Node.js
- nvm use 版本号 选择指定版本的Node.js

## 四、Node.js之HelloWorld
### 1. 运行方式

1. 命令行方式

   cmd 输入node  然后直接输入代码运行。在REPL环境执行。

   REPL：read-eval-print-loop	  读取代码-执行-打印结果-循环这个过程

   - 在REPL环境中，下划线_表示最后一次执行结果
   - .exit可以退出REPL环境

2. 运行文件方式

    **cmd中 输入 node xx.js 运行js文件**

### 2. 全局对象概览

Node.js全局提供了很多的API，比如内置的对象、全局函数。但是和浏览器很大的一个区别，Node.js中不存在window对象。但是Node中有个类似的概念，叫Global

Node.js提供了很多开发环境相应的模块，包含了大量的API

全局成员概述：

```javascript
//global相当于浏览器中的window，可省略     全局对象，其他隶属于global
global.console.log(123);

//包含文件名称的全路径
console.log(__filename);
//文件路径(不包括文件名)
console.log(__dirname);

exports、module和require三者是nodeJS的核心：模块化开发
还有一个比较特别的process(进程)对象,提供了很多相关属性
比如
//argv是一个数组，默认情况下 前两项数据分别是：当前Node.js环境的路径     当前执行的js文件的全路径
//从第三个参数开始表示命令行参数   比如node xx.js 123 234   后面的会接受并添加到argv中 然后下行会打印出
console.log(process.argv);   
//打印当前系统的架构（系统位数64/32）
console.log(process.argv);   
```

![globle](D:\用户Users\chenh\Desktop\笔记\Node.js\img\globle.PNG)

## 五、服务器端模块化

-   服务器端模块化规范CommonJS与实现Node.js

    模块化开发：

    ​	传统非模块化开发有如下缺点：1.命名冲突     2.文件依赖(引入很多js，js还依赖其他js，有先后顺序)

    ​

    **前端标准的模块化规范：**

    1、AMD - requirejs

    2、CMD - seajs        ( 前几年tb和阿里巴巴用的，进来有其他方案)

    **服务器端的模块化规范：**

    1、Common.JS  - Node.js

    ​

    **模块化相关规则**

    1、如何定义模块化：一个js文件就是一个模块化，<u>模块内部的成员都是相互独立的</u>

    2、模块成员的导出和引入

-   模块导出与引入

```javascript
/*01.js*/
var sum =  function(a, b){
      	return parseInt(a) + parseInt(b);
}
//导出模块成员 
//如果不导出，02.js报错:sum不是一个方法。  因为得不到01.js的方法 因为默认情况下，模块间的成员是相互隔离的
exports.sum = sum;
-----------------------------------------------------   
/*新建一个模块02.js*/
//引入模块
var moduel = require('./01.js');   

var ret = moduel.sum(4,7);
console.log(ret);

=======================================================
导出成员的另一种方式：moduel属性
/*01.js*/
var sum =  function(a, b){
	return parseInt(a) + parseInt(b);
}
moduel.exports = sum;   //导出的是一个单独的方法
------------------------------
/*02.js*/
//引入模块
var moduel = require('./01.js');       //更合理：var sum = require('./01.js');

var ret = moduel(4,7);                 //var ret = sum(4,7);  
console.log(ret);

=================================================
导出其实还有一种，通过global
模块成员导出：global      (不常用)
/*01.js*/
var a = 123;
global.a = a;
-----------------------------------
/*02.js*/
//var moduel = require('./01.js');
//console.log(moduel);      //空的,得不到01中的成员
require('./01.js');
console.log(global.a);      
```

注意：

​	Node.js内部实现的机制：已经加载的模块会缓存，如果再引入require('./01.js')，如果路径一样，引入模块多次，还是用的第一次加载后就缓存的内容。不会再重新的加载，提高了性能。

​        另外，引入时路径把后缀.JS去掉也是可以的。但是有一个细节。不加扩展名的时候会按照如下后缀顺序进行查找  

 .js   ->   .json  ->   .node


-   模块导出机制分析

-   模块加载规则

                                        模块文件的后缀3种情况：  .js 	.json	.node

    + 模块查找 ：不加扩展名的时候会按照如下后缀顺序进行查找   .js   ->   .json  ->   .node

      ​

      也就是说有同名01.js、01.json和01.node文件。当我require('./01')。 找到的是.js文件，如果没有.js，找到的是.json文件

-   模块分类
    + 自定义模块
    + 系统核心模块
        * fs 文件操作
        * http 网络操作
        * path 路径操作
        * querystring 查询参数解析
        * url url解析
        * ......

## 六、ES6常用语法
### 1.变量声明两关键字：let与const

```javascript
与以前var不同

1.let声明的变量不存在预解析
console.log(flag);  //报错：is not defined
//var flag = 123;      //var有预解析 结果是undefined
let flag = 123;
-----------------------------------------------------
2.let声明的变量不允许重复(同一个作用域内)
let flag = 123;
let flag = 456;      //var会覆盖掉  结果是456
console.log(flag);      //报错:has alreday been declared
------------------------------------
3.ES6引入了块级作用域(在块级作用域内，变量只能先声明再使用)
if(true) {
  	//var flag = 123;    外部访问得到
  	let flag = 123; //let声明：块内部声明，外部访问不到
}
console.log(flag);   //flag is not defined     

//以下也是块级作用域
{
  	let flag =111;
  	console.log(flag);
}

for(let i =0; i < 3; i++) {
  	//for循环括号中声明的变量只能在循环体中使用
  	console.log(i);
}
console.log(i);    //报错   Assignment to constant variable
=============================================
const对以上规则同样适用。同时const还有另外两点。
1.const用来声明常量，不允许重新赋值。
const n = 1;
n = 2;       //报错
2.const声明的常量必须初始化。
const abc;      //报错 Missing initalizer in const declaration
```
let和const区别之一：const用来声明常量，不允许重新赋值。

###  2.变量的解构赋值

+ 数组解构赋值

  ```javascript
  以前的赋值
  //var a = 1,b = 2,c = 3;

  数组的解构赋值
  //var [a,b,c] = [1,2,3];     //1	2	3
  //let [a,b,c] = [,123,];     //undefined	123		undefined
  //数组的解构赋值指定默认值
  let [a=111,b,c] = [,123,];     //111(默认值)		123		undefined
  console.log(a,b,c)  
  ```

+ 对象解构赋值

  ```javascript
  let {foo,bar} = {foo: 'hello',bar: 'hi'};      // hello     hi
  let {foo,bar} = {bar: 'hi',foo: 'hello'};	 // hello     hi        对象顺序无影响，根据名称赋值
  console.log(foo,bar);

  //对象属性别名(如果有了别名，那么原来的名称就无效了)
  let {foo:abc,bar} = {bar: 'hi',foo: 'hello'};	 // foo别名abc
  //对象的解构赋值指定默认值
  let {foo:abc='hello',bar} = {bar: 'hi'};       
  console.log(abc,bar);       // hello     hi
  console.log(foo,bar);      //报错 foo undfined
  -------------------------------------------------
  内置对象直接赋值 直接将对象中的所有属性和前面对应的名称绑定
  let {cos,sin,random} = Math;    //cos,sin,random都是Math对象中的方法

  console.log(typeof cos);   //function
  console.log(typeof sin);   //function
  console.log(typeof random);  //function
  ```

+ 字符串解构赋值

  ```javascript
        变量         值
  let [a,b,c,d,e] = "hello";
  console.log(a,b,c,d,e);     //h e l l o

  这种方式不可以得到字符串的长度
  //let [a,b,c,d,e，length] = "hello";
  //console.log("hello".length);   //undefined

  可以用对象的方式
  let {length} = "hi";
  console.log(length);     //2
  ```


###  3.字符串扩展

+ includes()    

  判断字符串中是否包含指定的字符串（有的话返回true，否则返回false）

  ​	参数一：匹配的字符串

  ​	参数二：从第几个字符开始匹配

  ```javascript
  console.log('hello world'.includes('world'));     //true
  console.log('hello world'.includes('world1111'));     //false

  console.log('hello world'.includes('world',6));      //第二个参数作用：从字符串某个索引开始进行判断
  ```

+ startsWith()

  判断字符串中是否以特定的字符串开始

  ```javascript
  let url = 'admin/index.php';
  console.log(url.startsWith('admin'));      //true
  ```

+ endsWith()

  判断字符串中是否以特定的字符串结束

  ```javascript
  判断后缀
  let url = 'admin/index.php';
  console.log(url.endsWith('php'));      //true
  ```

+ 模板字符串

  ```javascript
  let obj ={
    	username: 'list',
    	age: '12',
    	gender: 'male'
  }

  //原方法
  let tag = '<div><span>'+obj.username+'</span><span>'+obj.age+'</span><span>'+obj.gender+'</span></div>';
  console.log(tag);
  ----------------------------------------------
  //模板``    let tpl=``;    反引号``表示模板，``中间就是模板，模板中的内容可以有格式。通过${}方式填充数据
  let tpl = `
  	<div>
      	<span>${obj.username}</span>
          <span>${obj.age}</span>
          <span>${obj.gender}</span>
      </div>
  `;
  console.log(tpl);
  -------------------
  另外${}还可以进行表达式运算、函数调用
  let fn = function(info) {
    	return ingo;
  }
  let tpl = `
  	<div>
      	<span>${1+1}</span>
          <span>${fn('nihao')}</span>
      </div>
  `;
  console.log(tpl);
  ```

### 4.函数扩展

+ 参数默认值

  ```javascript
  //以前做法
  function foo(param){
    	let p = param || "hello";
    	console.log(p);
  }
  foo();      //hello
  foo('hi');	//hi
  --------------
  //es6
  function(param = 'hello'){
    	console.log(param);
  }
  foo();      //hello
  foo('hi');	//hi
  ```

+ 参数解构赋值

  ```javascript
  //默认参数赋值
  function foo(uname='ch',age=22){
    	console.log(uname,age);
  }
  foo();      //ch 22
  foo('lq',21);	//lq 21
  ---------------------------------------
  //解构赋值 
  function foo([uname,age]){
  	console.log(uname,age);
  }
  foo();  //注意 报错 cannot match against 'undefined' or 'null'   也就是说解构赋值时必须有对应的对象传给它
  foo({});   //结果 undefined  undefined

  或者将传递对象写在括号中
  function foo([uname,age]={}){
  	console.log(uname,age);
  }
  foo();     //结果 undefined  undefined
  ------------------------------------------
  function foo([uname='ch',age=22]={}){
  	console.log(uname,age);
  }
  foo({uname:"lq",age:22});
  ```


+ rest参数（剩余参数） ...rest

  ```javascript
  function(a,b,c){
    		
  }
  foo(1,2,3);
  ---------------------
  //rest参数，也可以写成其他的 比如param： ...param
  function(a,...rest){
    	console.log(a,rest);	//1 [2,3]
  }
  foo(1,2,3);

  function(a,b,...rest){
    	console.log(rest);	 // [3,4,5]
  }
  foo(1,2,3,4,5);
  ```

+ 扩展运算符   ...

  简单来说，与上面rest一起。两个方法其实可以说是互逆的。一个把多个参数变成单个的数组，一个把数组拆散变成单个的参数

  ```javascript
  fucntion foo(a,b,c,d,e){
    	console.log(a + b + c + d + e);
  }
  foo(1,2,3,4,5);      //常规传递
  //如果数据是数组传递
  let arr = [1,2,3,4,5];
  foo.apply(null,arr);
  ------------------------------
  扩展运算符简化
  foo(...arr);        //...arr -->1,2,3,4,5
  ```

  应用：合并数组

  ```javascript
  //合并数组
  let arr1 = [1,2,3];
  let arr2 = [4,5,6];
  let arr3 = [...arr1,...arr2];   //把两个数组拆开，合并到一个新数组中
  console.log(arr3);				//[1,2,3,4,5,6];
  ```

+ 箭头函数

  ```javascript
  function foo(){
    	console.log('hello');
  }
  foo();
  //等价箭头函数
  let foo = () => console.log('hello');
  foo();
  ----------------------------
  function foo(v){
    	return v;
  }
  //等价箭头函数
  let foo = v => v;       //单个参数括号省略，多个参数必须用小括号包住
  let ret = foo(111);
  console.log(ret);
  ---------------------------
  //多个参数必须用小括号包住
  let foo = (a,b) => {let c = 1;console.log(a + b + c);}   //代码体多行必须用大括号包起，否则报错
  foo(1,2);
  ==================================
  //匿名函数
  let arr = [123,456,789];
  arr.forEach(function(element,index){
    	console.log(element,index);
  });
  //等价箭头函数
  arr.forEach((element,index)=>{
    	console.log(element,index);
  })
  ```

  箭头函数注意事项：

  ​	1、箭头函数的this取决于函数的定义，而不是调用

  ​	(在定时器中用箭头函数就可以解决this指向问题，箭头函数内部的this与外部的this保持一致)

  ```javascript
  function foo(){
    	console.log(this);      //this不是window  在node.js中打印中一大串东西，里面有什么process、global等等
  }						//是nodejs生成的，我们也用不到。只需知道与浏览器有差异
  foo();

  //改变this 也是用之前内容就可以  
  //没什么特别之处。唯一就是this在nodeJS中默认不是window了
  foo.call({num:1});         //结果 {num:1} 
  ---------------------------------------
  function foo(){
    	//使用call调用foo时，这里的this就是call的第一个参数
    	setTimeOut(()=>{
        	console.log(this.num);
    	});     
  }					
  foo.call({num:1}); 
  ```
  ​	2、箭头函数不可以new

  ```javascript
  let foo = () => {this.num = 123;};
  new foo();     //报错 foo is not a constructor
  ```

  ​	3、箭头函数不可以使用arguments获取参数列表，可以使用rest参数代替

  ```javascript
  /*
  let foo = (a,b) => {
    	//console.log(a,b);
    	console.log(arguments);        //不是参数列表，而是一个对象 获取不到实参列表
  }
  foo(123,456);
  */
  let foo = (..。param) => {
    	console.log(param);       //打印所有实参列表 [123,456]
  }
  foo(123,456);
  ```

### 5.对象的扩展

- 属性的简洁表示

  ES6允许直接写入变量和函数，作为

- ​

### 6.类与继承

```javascript
//以前
function Animal(name){
	this.name = name;
}
Animal.prototype.showName = function(){   //方法加到原型上
    console.log(this.name);
}
var a = new Animal('Tom');
a.showName();
var a1 = new Animal('Jerry');
a1.showName();

/*其实静态方法在以前也遇到过
Animal.abc = function(){};  给函数加普通的属性或者方法
$.ajax = function(){}; 			
*/


-------------------------
//用类的方式重构以上
class Animal{
    // es6引入：静态方法(静态方法只能通过类名调用，不可以使用实例对象调用)
    static showInfo(){
        console.log('hi');
    }
    // 构造函数
    constructor(name){
        this.name = name;
    }

    showName(){
        console.log(this.name);
    }
}
//实例化
let a = new Animal('spike');
a.showName();
//a.showInfo();       不能通过实例的方式调用静态方法
Animal.showInfo(); 	  //类来调用
------------------------------
  
// 类的继承引入关键字： extends
class Dog extends Animal{
    constructor(name,color){
        super(name);		//super用来调用父类
        this.color = color;
    }

    showColor(){
        console.log(this.color);
    }
}

let d = new Dog('doudou','yellow');
d.showName();		//doudou
d.showColor();		//yellow
// d.showInfo();     静态方法也可以继承，但是是通过类来调用
Dog.showInfo();
```
### 补充：7.promise

异步操作的两个问题。

​	当主程序跳到封装好的方法里去执行，但是进入后发现fs.readFile()是个异步方法，主程序不会执行而是**放在事件队列中**就不管了。(让子程序去执行)。然后主程序就跳出这个异步方法。所以没有return dataStr。所以就拿不到封装好的方法内部的异步函数的返回值。如果用变量接受就是undefined。return不行，我们就用回调函数callback。

​	return不生效

#### 1.引言：

 promise用来干嘛的？	就是单纯的**为了解决回调地狱问题**；并不能帮我们减少代码量。



在文件夹files下创建一些文件。然后在文件夹files同目录创建一个js文件。

```javascript
// 需求：你要封装一个方法，我给你一个要读取文件的路径，你这个方法能帮我读取文件，并把内容返回给我

const fs = require('fs')
const path = require('path')

// 这是普通读取文件的方式		第一个参数文件的路径。第二个参数编码
/* fs.readFile(path.join(__dirname, './files/1.txt'), 'utf-8', (err, dataStr) => {
  	if (err) throw err
  	console.log(dataStr)
}) */

// 封装方法的初衷： 给定文件路径，返回读取到的内容。而上面我们不可以通过return dataStr将内容返回

// 我们可以规定一下， callback 中，有两个参数，第一个参数，是 失败的结果；第二个参数是成功的结果；
//如果路径不合法或者什么导致错误。不thorw掉err而是交给用户处理。所以我们把失败的结果也通过callback交个调用
// 同时，我们规定了:如果成功后，返回的结果，应该位于 callback 参数的第二个位置，此时，第一个位置 由于没有出错，所以，放一个 null;如果失败了，则 第一个位置放 Error对象，第二个位置放置一个 undefined
function getFileByPath(fpath, callback) {
  	fs.readFile(fpath, 'utf-8', (err, dataStr) => {
    	// 如果报错了，进入if分支后，if后面的代码就没有必要执行了。所以return
    	if (err) return callback(err)
    	// console.log(dataStr)
    	// return dataStr
    	callback(null, dataStr)
  	})
}


/* 
var result = getFileByPath(path.join(__dirname, './files/1.txt'))
console.log(result); 		//结果是undefined	

当主程序跳到封装好的方法里去执行，但是进入后发现fs.readFile()是个异步方法，主程序不会执行而是放在事件队列中就不管了。(让子程序去执行)。然后主程序就跳出这个异步方法。所以没有return dataStr。所以就拿不到封装好的方法内部的异步函数的返回值。如果用变量接受就是undefined。return不行，我们就用回调函数callback。

*/


getFileByPath(path.join(__dirname, './files/11.txt'), (err, dataStr) => {
  	// console.log(dataStr + '-----')
  	if (err) return console.log(err.message)
    //如果没有失败
  	console.log(dataStr)
})
```

这种上面callback传一个参数，下面传两个参数的。共用一个callback如果不理解。可以看看下面这种改造的。

```javascript
// 需求：你要封装一个方法，我给你一个要读取文件的路径，你这个方法能帮我读取文件，并把内容返回给我

const fs = require('fs')
const path = require('path')

//第一个放成功的回调，第二个放失败的回调
function getFileByPath(fpath, succCb, errCb) {
  	fs.readFile(fpath, 'utf-8', (err, dataStr) => {
    	if (err) return errCb(err)
    	succCb(dataStr)
  	})
}

getFileByPath(path.join(__dirname, './files/11.txt'), function (data) {
  	console.log(data + '娃哈哈，成功了！！！')
}, function (err) {
  	console.log('失败的结果，我们使用失败的回调处理了一下：' + err.message)
})

-----------------------------------------------------------------------
//换一种调用方式
// 需求： 先读取文件1，再读取文件2，最后再读取文件3		这是异步读取，我们来一个异步嵌套就可以了
// 如果层级很深的话。	回调地狱
// 使用 ES6 中的 Promise，来解决 回调地狱的问题；
// 问： Promise 的本质是要干什么的：就是单纯的为了解决回调地狱问题；并不能帮我们减少代码量；
getFileByPath(path.join(__dirname, './files/1.txt'), function (data) {
  	console.log(data)

  	getFileByPath(path.join(__dirname, './files/2.txt'), function (data) {
    	console.log(data)

    	getFileByPath(path.join(__dirname, './files/3.txt'), function (data) {
      		console.log(data)
    	})
  	})
})
```

使用 ES6 中的 Promise，来解决 回调地狱的问题；

Promise 的本质：就是单纯的为了解决回调地狱问题；并不能帮我们减少代码量；

#### 2.概念：

1. Promise 是一个 构造函数，既然是构造函数， 那么，我们就可以  new Promise() 得到一个 Promise 的实例；


2. 在 Promise 上，有两个函数，分别叫做 resolve（成功之后的回调函数） 和 reject（失败之后的回调函数）


3. 在 Promise 构造函数的 Prototype 属性上，有一个 .then() 方法，也就说，只要是 Promise 构造函数创建的实例，都可以访问到 .then() 方法


4. Promise 表示一个 异步操作；每当我们 new 一个 Promise 的实例，这个实例，就表示一个具体的异步操作；


5. 既然 Promise 创建的实例，是一个异步操作，那么，这个 异步操作的结果，只能有两种状态：

   ​5.1 状态1： 异步执行成功了，需要在内部调用 成功的回调函数 resolve 把结果返回给调用者；
    5.2 状态2： 异步执行失败了，需要在内部调用 失败的回调函数 reject 把结果返回给调用者；
   ​5.3 由于 Promise 的实例，是一个异步操作，所以，内部拿到 操作的结果后，无法使用 return 把操作的结果返回给调用者； 这时候，只能使用回调函数的形式，来把 成功 或 失败的结果，返回给调用者；

6. 我们可以在 new 出来的 Promise 实例上，调用 .then() 方法，【预先】 为 这个 Promise 异步操作，指定 成功（resolve） 和 失败（reject） 回调函数；

```javascript
形式上的异步操作
var promise = new Promise()
// 注意：这里 new 出来的 promise， 只是代表 【形式上】的一个异步操作；
	// 什么是形式上的异步操作：就是说，我们只知道它是一个异步操作，但是做什么具体的异步事情，目前还不清楚

具体的异步操作
// 这是一个具体的异步操作，其中，使用 function 指定一个具体的异步操作
var promise = new Promise(function(){
	// 这个 function 内部写的就是具体的异步操作！！！比如我写下面这个，就是读文件的异步操作
  	fs.readFile();
})
---------------------------------------------------------
const fs = require('fs')

/*
var promise = new Promise(function () {
  	fs.readFile('./files/2.txt', 'utf-8', (err, dataStr) => {
    	if (err) throw err
    	console.log(dataStr)
  	})
}) 
// 每当 new 一个 Promise 实例的时候，就会立即 执行这个 异步操作中的代码
// 也就是说，new 的时候，除了能够得到 一个 promise 实例之外，还会立即调用 我们为 Promise 构造函数传递的那个 function，执行这个 function 中的 异步操作代码；
*/


//在js中，只有function会按需执行。其他的代码都会立即执行。我们要他调用时才执行,写在funciton中

// 初衷： 给路径，返回读取到的内容
function getFileByPath(fpath) {
  //如果不return这个promise。函数内部的变量，外部无法访问(也就是调用时)
  return new Promise(function (resolve, reject) {
    fs.readFile(fpath, 'utf-8', (err, dataStr) => {
		//注意下面的reject和resolve是两个函数。得传递，不能直接调用
      	if (err) return reject(err)
      	resolve(dataStr)
    })
  })
}

//getFileByPath('./files/2.txt');				//按需执行


//传递的reject和resolve实参是上面第6点说的。通过实例身上的.then()方法指定成功和失败的实参
//在 new 出来的 Promise 实例上，调用 .then() 方法，【预先】 为 这个 Promise 异步操作，指定 成功（resolve） 和 失败（reject） 回调函数；
getFileByPath('./files/2.txt').then(function (data) {
	console.log(data + '-------')
}, function (err) {
	console.log(err.message)
}) 
```

Promise执行步骤：1.定义函数 2. 调用方法 3、进入方法new一个Promise实例，并立即执行后面括号中的function.

4.function中的异步读文件操作还没执行完。就将promise return出去了。5.拿到promise实例 6.通过.then执行两个回调

7.function中的异步读文件操作执行完。访问reject()或者resolve()函数

#### 3.使用promise解决回调地狱问题

```javascript
// 先读取文件1，在读取2，最后读取3
const fs = require('fs')

function getFileByPath(fpath) {
  	return new Promise(function (resolve, reject) {
    	fs.readFile(fpath, 'utf-8', (err, dataStr) => {

      	if (err) return reject(err)
      	resolve(dataStr)
    })
  })
}

/
// 注意： 通过 .then 指定 回调函数的时候，成功的 回调函数，必须传，但是，失败的回调，可以省略不传

// 这是一个 错误的示范，千万不要这么用!因为和之前的地狱回调没区别！！
/* getFileByPath('./files/1.txt')
  .then(function (data) {
    console.log(data)

    getFileByPath('./files/2.txt')
      .then(function (data) {
        console.log(data)

        getFileByPath('./files/3.txt')
          .then(function (data) {
            console.log(data)
          })
      })
  }) */


// 在上一个 .then 中，返回一个新的 promise 实例，可以继续用下一个 .then 来处理
// 如果 ，前面的 Promise 执行失败，我们不想让后续的Promise 操作被终止，可以为 每个 promise 指定 失败的回调

// 读取文件1
getFileByPath('./files/11.txt')
	.then(function (data) {
    	console.log(data)

    	// 读取文件2		在第一个.then()里面return了一个新的promise对象
    	return getFileByPath('./files/2.txt')
  	}, function (err) {
    	console.log('这是失败的结果：' + err.message)
    	// return 一个 新的 Promise
    	return getFileByPath('./files/2.txt')
  	})//第一个.then()后面可以继续.then()。这个.then()操作的就是上一个.then()中返回的promise
  	.then(function (data) {
    	console.log(data)

    	return getFileByPath('./files/3.txt')
  	})
  	.then(function (data) {
    	console.log(data)
  	}).then(function (data) {
    	console.log(data)
  	}) 

// console.log('OKOKOK')
//如果不写错误的回调，后面写一句不相干的代码，然后第一个.then()文件路径错误。执行后，会先输出okokok再报错。
//当第一次调用方法，读文件路径。异步的方式，主程序已经把promise创建完了。return pomise后主程序去执行这不相干的一行。当执行完这一行。可能文件还没读取完。所以后报错。然后promise异常终止。后面的.then也不执行了。
```

将回调嵌套格式改成了串联形式。

#### 4.捕获异常的两种方式

```javascript
1.每个promise中指定失败的回调，防止后续promise不能正常执行的问题
// 当 我们有这样的需求： 哪怕前面的 Promise 执行失败了，但是，不要影响后续 promise 的正常执行，此时，我们可以单独为 每个 promise，通过 .then 指定一下失败的回调；

getFileByPath('./files/11.txt')
	.then(function (data) {
    	console.log(data)

    	// 读取文件2		在第一个.then()里面return了一个新的promise对象
    	return getFileByPath('./files/2.txt')
  	}, function (err) {
    	console.log('这是失败的结果：' + err.message)
    	// return 一个 新的 Promise
    	return getFileByPath('./files/2.txt')
  	})
  	.then(function (data) {
    	console.log(data)

    	return getFileByPath('./files/3.txt')
  	})
  	.then(function (data) {
    	console.log(data)
  	}).then(function (data) {
    	console.log(data)
  	})


2.捕获错误并防止后面.then()执行。
// 有时候，我们有这样的需求，和上面的需求刚好相反：如果后续的Promise执行，依赖于前面 Promise 执行的结果，如果前面的失败了，则后面的就没有继续执行下去的意义了，此时，我们想要实现，一旦有报错，则立即终止所有 Promise的执行；
//捕获错误并防止后面.then()执行。不要在每一个.then()里失败回调，在最后面用.catch()
getFileByPath('./files/1.txt')
  .then(function (data) {
    console.log(data)

    // 读取文件2
    return getFileByPath('./files/22.txt')
  })
  .then(function (data) {
    console.log(data)

    return getFileByPath('./files/3.txt')
  })
  .then(function (data) {
    console.log(data)
  })
  .catch(function (err) { // catch 的作用： 如果前面有任何的 Promise 执行失败，则立即终止所有 promise 的执行，并 马上进入 catch 去处理 Promise中 抛出的异常；
    console.log('这是自己的处理方式：' + err.message)
  })
```

### 5.jQuery中Ajax使用Promise

jQuery中Ajax也支持promise



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

  	<input type="button" value="获取数据" id="btn">
	//cnpm i jquery -s
  	<script src="./node_modules/jquery/dist/jquery.min.js"></script>

    <script>
    	$(function () {
      		$('#btn').on('click', function () {
        		$.ajax({
          			url: './data.json',
          			type: 'get',
          			dataType: 'json',
                  	/*常规操作
                  	 success : function(data){
                      	console.log(data);
                  	}*/
        	})
          	.then(function (data) {
            	console.log(data)
          	})
      	})
    });
  	</script>
</body>

</html>
```

.then()也可以拿到数据。前面返回一个promise，后面通过.then()指定成功回调。

