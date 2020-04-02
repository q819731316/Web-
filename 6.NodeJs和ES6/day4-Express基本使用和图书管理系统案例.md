# 大纲-day04

> Node.js的Web开发相关的内容：
> 1、Node.js不需要依赖第三方应用软件（Apache），可以基于api自己实现
> 2、实现静态资源服务器
> 3、路由处理
> 4、动态网站
> 5、模板引擎
> 6、get和post参数传递和处理
>
> Web开发框架：express

express框架就是对以上的一些底层api进行封装，形成一套新的api。对模板引擎也进行了整合，使用更加方便。

## 一、Express基本使用

### **1.静态服务器**

**1.安装**

**官方说明**

1.首先假定你已经安装了 [Node.js](https://nodejs.org/)，接下来为你的应用创建一个目录，然后进入此目录并将其作为当前工作目录。

```javasscript
$ mkdir myapp
$ cd myapp
```

2.通过 `npm init` 命令为你的应用创建一个 `package.json` 文件。欲了解 `package.json` 是如何起作用的，请参考 [Specifics of npm’s package.json handling](https://docs.npmjs.com/files/package.json).

```javascript
$ npm init
```

此命令将要求你输入几个参数，例如此应用的名称和版本。你可以直接按“回车”键接受大部分默认设置即可，下面这个除外：

```javascript
entry point: (index.js)
```

3.键入 `app.js` 或者你所希望的名称，这是当前应用的入口文件。如果你希望采用默认的 `index.js` 文件名，只需按“回车”键即可。接下来在 `myapp` 目录下安装 Express 并将其保存到依赖列表中。如下：

```javascfript
$ npm install express --save
```

如果只是临时安装 Express，不想将它添加到依赖列表中，可执行如下命令：

```javascript
$ npm install express --no-save
```

**简略**

1.新建一个目录myexpress，新建一个入口文件app.js。

2.通过 `npm init -y` 生成 `package.json` 文件。

3.安装express框架`npm install express --save`

**2.最简单的官方例子：hello world**

```javascript
/*
    Express之HelloWorld
*/
var express = require('express');       //引入包，返回的是一个函数
var app = express();				  //形成了一个对象，对象就包括了api

// 绑定路由
app.get('/', function(req, res) {		//调用app对象的get()API来监听路由  '/'根路径  只能监听到根路径的请求
    // 响应请求
    res.send('Hello World!');				//和end作用一样，对end做了一层包装
});

var server = app.listen(3000, "localhost", function() {
    // 监听的域名或者IP
    var host = server.address().address;
    // 监听的端口
    var port = server.address().port;
    console.log('Example app listening at http://%s:%s', host, port);
});
```

cmd中输入 node.运行。结果

```javascript
Example app listening at http://127.0.0.1:3000
```

这是基于express创建的服务器功能，之前我们是通过http模块来做。实际上，在它的内部，封装了我们之前写的代码。

 **3.express创建服务器**

对以上加强理解。新建一个test.js

```javascript
//const express = require('express');
//const app =express();
const app = require('express');

app.get('/', function(req, res) {	
    // 响应请求
    res.send('Hello World!');			
}).listen(3000,()=>{
  	console.log(running....);
})
```

应用服务器创建完成，启动`node test.js`。

网页localhost：3000刷新就是内容 Hello World!



还有等效的和之前http风格相似的。分两步。

```javascript
const app = require('express');

let server = app.get('/',(req, res)=>{
  	res.send('Hello World!');
})

server.listen(3000,()=>{
  	console.log(running....);
})
```

### 2.托管静态文件

利用 Express 托管静态（资源）文件，意思就是让图片、css、js、静态页面可以通过url地址来访问

官方文档：http://www.expressjs.com.cn/starter/static-files.html

static.js

```javascript
/*
    托管静态文件

    可以指定虚拟目录
    可以指定多个目录作为静态资源目录
*/
const express = require('express');     //引入包。(包和模块本质上说是一样的，使用都是通过require印记)
const app = express();

// 实现静态资源服务
//将public文件夹目录下的图片、CSS文件、Js文件对外开放访问
//use方法的第一个参数可以指定一个虚拟路径，添加后，访问的url得加上此虚拟目录
let server = app.use('/abc',express.static('public'));	

/*	要使用多个静态资源目录，请多次调用express.static中间件函数：
	express.static中间件函数会根据目录的添加顺序查找所需的文件
app.use('/nihao',express.static('hello'));
*/
server.listen(3000,()=>{
    console.log('running...');
});

//现在就可以访问 public 目录中的所有文件了。例如： http://localhost:3000/abc/images/kitten.jpg 
//注意由于加了虚拟目录的参数，所以得加上虚拟目录 

//进一步优化 监听也可以直接用app对象来监听
----------------------------------------- 
app.use('/abc',express.static('public'));
app.use('/nihao',express.static('hello'));
app.listen(3000,()=>{
    console.log('running...');
});
```

### 3.路由

除静态资源外，剩下的路径都是动态资源：服务器动态生成的内容。这样的路径请求，一般都不对应一个特定的文件名称，而是只有一个路径。这种路径的处理，有一种特定的称谓：**路由**

官方文档：http://www.expressjs.com.cn/starter/basic-routing.html

router.js

```javascript
/*
    路由（根据请求路径和请求方式进行路径分发处理）
    http的常用请求方式：四种对应增删改查。http协议的设计初衷就是支持增删改查
    post   添加
    get    查询
    put    更新
    delete 删除
	
	express支持上述四种，还有restful api(一种URL的格式)也支持上述四种。
	restful api一般不存在文件的后缀，直接是路径即可
*/

const express = require('express');
const app = express();
const router = require('./myrouter.js');


// 基本的路由处理:4种
app.get('/',(req,res)=>{
    res.send('get data');
});

app.post('/',(req,res)=>{
    res.send('post data');
});

app.put('/',(req,res)=>{
    res.send('put data');
});

app.delete('/',(req,res)=>{
    res.send('delete data');
});

//监听端口
app.listen(3000,()=>{
    console.log('running...');
});

//node router.js启动。表单只支持get和post，方便测试，可以用插件Postman来测试
---------------------------------------------------------------
  
// 除以上外，也可以直接使用use分发可以处理所有的路由请求   (所有)
app.use((req,res)=>{
    res.send('ok');
});

// all方法绑定的路由与请求方式无关
app.all('/abc',(req,res)=>{
    res.end('test router');
});

--------------------------------------
route方法可以指定特定的请求方式
app.route('/hello')
   .get((req,res)=>{
       res.send('get data');
   }).post((req,res)=>{
       res.send('post data');
   });
---------------------------------------
app.use('/admin',router);

//监听端口
app.listen(3000,()=>{
    console.log('running...');
});
```

将上面的单独拆到模块中。加载进去后，通过router的方式绑定路由。最后再导出

myrouter.js

```javascript
//加载
const express = require('express');
const router = express.Router();				//直接调用函数

//通过router的方式绑定路由
router.get('/hi',(req,res)=>{
    res.send('hi router');
});

router.get('/hello',(req,res)=>{
    res.send('hello router');
});

router.post('/abc',(req,res)=>{
    res.send('abc router');
});

//导出
module.exports = router;
```



### 4.中间件

**中间件**：处理过程中的环节。（本质上就是一个函数）

从请求开始到服务器返回内容间存在很多的节点。比如记录访问时间、记录访问日志、统计网站访问量，还有与业务相关的环节。

就像下面这个图

![](img/express.png)

中间件的功能：执行任何代码、修改请求和响应对象、终结请求-响应循环、调用堆栈中的下一个中间件

Express可应用以下几种中间件：

- 应用级中间件(比如前面的use)
- 路由级中间件
- 错误处理级中间件
- 内置中间件   (前面的静态服务：express.static)
- 第三方中间件  (cookie处理)

**1.应用级中间件**

```javascript
/*
    中间件：就是处理过程中的一个环节（本质上就是一个函数）
    可以访问请求对象req和响应对象res,第三个参数next：把请求传递到下一个环节
*/

const express = require('express');
const app = express();
let total = 0;

//应用级中间件

//全局，不管哪个路径，只要过来，就显示
app.use((req,res,next)=>{
    console.log('有人访问');
    // next方法的作用就是把请求传递到下一个中间件
    next();
});

app.use('/user',(req,res,next)=>{
    // 记录访问时间
    console.log(Date.now());
    // next方法的作用就是把请求传递到下一个中间件
    next();
});

app.use('/user',(req,res,next)=>{
    // 记录访问日志
    console.log('访问了/user');
    next();
});

app.use('/user',(req,res)=>{
    total++;
    console.log(total);
    res.send('result');
});

//上面有4个中间件，前一个向下一个传递需要next方式往下传递，否则后续中间件没法调用
app.listen(3000,()=>{
    console.log('running...');
});
```

node 01.js启动，然后通过url访问http://localhost:3000/user。cmd中就有了数据

**2.路由级中间件**

其实就是中间件的挂载方式

```javascript
/*
    中间件的挂载方式和执行流程
    1.use方法
    2.路由方式:get post put delete
    两种本质一回事，通过路由方式挂载中间件，不过是方法不同。一个use一个get、post...
*/
const express = require('express');
const app = express();

//第1个路由
app.get('/abc',(req,res,next)=>{
    console.log(1);
  	//next();如果没有next，又没有res.send内容，就会堵塞。原本结果是1、2 堵塞只有1
  	//next中还可以写route参数。代表跳转到下一个路由。这里中间件2就不会执行，而是跳到下一个路由，执行3了。
    next('route');
},(req,res)=>{
    console.log(2);
    res.send('abc');
});

//第2个路由
app.get('/abc',(req,res)=>{
    console.log(3);
    res.send('hello');
});

//下一个路由是指通过get或其他方式绑定的路由。上面的1、2其实是一个路由，只不过这个路由包含了多个中间件
```

路由的另一种做法：前面声明一组方法，然后直接通过get直接绑定一组。通过next向下传递，形成链式结构。
这个链式结构就相当于一系列的中间件将他们串起来

```javascript
var cb0 = function (req, res, next) {
  console.log('CB0');
  next();
}

var cb1 = function (req, res, next) {
  console.log('CB1');
  next();
}

var cb2 = function (req, res) {
  res.send('Hello from C!');
}

app.get('/example', [cb0, cb1, cb2]);     //通过get直接绑定一组

app.listen(3000,()=>{
    console.log('running...');
});
```

cmd后台中输出running...、CB0、CB1。浏览器localhost:3000//example显示的Hello from C!

**3.内置中间件**

就是上面的2.托管静态文件 内容

静态服务功能，通过use来加载

```javascript
app.use(express.static('public'));
app.use(express.static('uploads'));
app.use(express.static('files'));
```



补充与上面区分：算是框架？

express-static中间件

```javascript
const express = require('express');
const expressStatic = require("express-static");
const app = express();

//访问当前文件夹下的/www路径
app.use(expressStatic(__dirname + '/www')); 

app.use('/www', express.static('public'))

//区分
//app.set('views',path.join(__dirname,'views'));
```



**4.错误处理中间件**

错误处理中间件和其他类型中间件定义类似，只是使用4个参数不是3个。`(err, req, res, next)`

```javascript
app.use(function(err, req, res, next) {
  	console.error(err.stack);
	res.status(500).send('发生错误');
});
```

**5.第三方中间件**

比如中间件body-parser处理post参数 。安装`$ npm install body-parser --save`

比如中间件cookie-parser读取cookie。

使用见下方参数处理。

### 5.参数处理（第三方中间件body-parser）

使用第三方中间件body-parser处理post参数 。安装$ npm install body-parser --save

03.js

```javascript
/*
    应用中间件
*/
const express = require('express');
const app = express();
const bodyParser = require('body-parser')

// 挂载(启动)内置中间件
app.use(express.static('public'));
// 挂载(启动)参数处理中间件（处理的post提交）      解析默认表单提交：x-www-from-urlencoded格式的数据
app.use(bodyParser.urlencoded({ extended: false }));



// 处理get提交参数
app.get('/login',(req,res)=>{
    let data = req.query;
    console.log(data);
    res.send('get data');
});

// 处理post提交参数
app.post('/login',(req,res)=>{
    let data = req.body;
    // console.log(data);
    if(data.username == 'admin' && data.password == '123'){
        res.send('success');
    }else{
        res.send('failure');
    }
});

app.listen(3000,()=>{
    console.log('running...');
}); 
```

login.html

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <form action="http://localhost:3000/login" method="get">       //post就修改为post
        用户名：<input type="text" name="username" id="username"><br>
        密  码：<input type="password" name="password" id="password"><br>
        <input type="submit" id="btn" value="提交">
    </form>
</body>
</html>
```

**补充:**

​	body-parser中间件除了支持表单提交数据，也支持json形式的数据传递。json形式的数据我们可以借助ajax的方式来进行测试。而之前4种提交方式，除了用postman插件还可以使用ajax方法来测试。

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script type="text/javascript" src="jquery.js"></script>
    <script type="text/javascript">
    $(function(){
        $('#btn').click(function(){
            var obj = {			//对象并且得到表单中的值。传递数据的json方式用到
                username : $('#username').val(),
                password : $('#password').val()
            }
            $.ajax({
                type : 'delete',
                url : 'http://localhost:3000/login',
                contentType : 'application/json',		//处理json数据添加的指定格式
                dataType : 'text',
                // data : $('form:eq(0)').serialize(),	1.纯文本方式
              	//$('form:eq(0)')选中form表单      serialize()API得到全部数据
                data : JSON.stringify(obj),		//2.json方式 stringify将对象转为字符串，并且是json格式
                success : function(data){
                    console.log(data);
                }
            });
        });
    }); 
    </script>
</head>
<body>
    <form action="http://localhost:3000/login" method="get">
        用户名：<input type="text" name="username" id="username"><br>
        密  码：<input type="password" name="password" id="password"><br>
        <input type="button" id="btn" value="提交">			//没必要用submit，改为button
    </form>
</body>
</html>
```

04.js

```javascript
/*
    参数处理
*/
const express = require('express');
const app = express();
const bodyParser = require('body-parser')

// 挂载内置中间件
app.use(express.static('public'));

// 挂载参数处理中间件（post格式的参数）
app.use(bodyParser.urlencoded({ extended: false }));
// 处理json格式的参数
app.use(bodyParser.json());

// 处理get提交参数
app.get('/login',(req,res)=>{
    let data = req.query;
    console.log(data);
    res.send('get data');
});

// 处理post提交参数
app.post('/login',(req,res)=>{
    let data = req.body;
    // console.log(data);
    if(data.username == 'admin' && data.password == '123'){
        res.send('success');
    }else{
        res.send('failure');
    }
});

app.put('/login',(req,res)=>{
    res.end('put data');
});

app.delete('/login',(req,res)=>{
    res.end('delete data');
});

app.listen(3000,()=>{
    console.log('running...');
});
```

不同的请求，记得在ajax中修改

### 6.模板引擎整合（express-art-template）

express对模板引擎进行了整合处理。封装了一些方便的API。更灵活。

官方文档：http://www.expressjs.com.cn/guide/using-template-engines.html

通过app.set设置模板的路径

```javascript
/*
    模板引擎整合：art-template
*/
const express = require('express');
const path = require('path');
const template = require('art-template');
const app = express();

//设置模板引擎的三步
// 设置模板的路径
app.set('views',path.join(__dirname,'views'));      	//	views和view engine都是固定写法，后面是路径
// 设置模板引擎
app.set('view engine','art');						  //art 模板引擎后缀和下面的art对应
// 使express兼容art-template模板引擎
app.engine('art', require('express-art-template'));


//  '/list'绑定的路由，通过get请求  cmd中启动后，通过网址lcoalhost：3000/list 访问
app.get('/list',(req,res)=>{
    let data = {
        title : '水果',
        list : ['apple','orange','banana']
    }
    // 参数一：模板名称(路径和后缀在上面已经配置了)；参数二：渲染模板的数据
    res.render('list',data);		//render方法渲染数据
});

app.listen(3000,()=>{
    console.log('running...');
});
```

​	模板使用render()方法渲染数据

模板list.art

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>模板</title>
</head>
<body>
    <div>{{title}}</div>
    <div>
        <ul>
            {{each list}}									//循环遍历
                <li>{{$value}}</li>
            {{/each}}										//结束循环
        </ul>
    </div>
</body>
</html>
```

### 7.常用API基本使用

Express的API可以到文档中查看http://www.expressjs.com.cn/4x/api.html

**再补充一个Response 方法**

下表中响应对象（res）上的方法可以向客户端发送响应，并终止请求-响应周期。如果这些方法都不是从路由处理程序调用的，则客户端请求将保持挂起状态。

| 方法               | 描述                         |
| ---------------- | -------------------------- |
| res.download()   | 提示要下载的文件。                  |
| res.end()        | 结束响应过程。                    |
| res.json()       | 发送一个JSON格式的响应。             |
| res.jsonp()      | 发送一个支持JSONP的JSON格式的响应。     |
| res.redirect()   | 重定向请求。                     |
| res.render()     | 渲染视图模板。                    |
| res.send()       | 发送各种类型的响应。                 |
| res.sendFile()   | 以八位字节流的形式发送文件。             |
| res.sendStatus() | 设置响应状态代码并将其字符串表示形式作为响应体发送。 |

注意这里的end()和原生的end()有点区别。Express中的res.end()一般不传参。要传参用res.send和res.json。直接终结，不返回内容。

### 8.补充：文件上传下载

文档：[https://github.com/expressjs/multer#readme](https://github.com/expressjs/multer#readme)

我们使用form提交数据时，如果需要提交的数据中含有图片、音视频等，需要给form添加`enctype="multipart/form-data"`属性，如果不加，默认以`enctype="application/x-www-form-urlencoded"`编码传输。

body-parser组件只能处理`enctype="application/x-www-form-urlencoded"`编码的数据，并放到`req.body`中，所以如果含有图片等数据，需要借助中间件，使用`multer`中间件来完成文件的上传。

>官方文档有，上传单个就是single方法

#### 1.文件上传

`$ npm install multer --save`

upload.html

```javascript
<form action="/upload" method="post" enctype="multipart/form-data">
	<input type="file" name='file'>
	<input type="submit" value="上传">
</form>
 
```

**单文件**

demo

```javascript
var express = require('express');
var multer  = require('multer');
const fs=require('fs');
const pathLib=require('path');	
var app = express();

//var upload = multer({ dest: 'uploads/' });	


//接口	multer({ dest: 'uploads/' })是上传的文件名		single('file')表示接受name为file的上传数据
//注意这里是三个参数，如果要做抽离处理。按第三个参数抽离就是。对应的单独文件导使用到的对应的包
server.post('/upload',  multer({ dest: 'uploads/' }).single('file'),(req,res)=>{
	console.log(req.file);
  	fs.renameSync(req.file.path, `uploads/${req.file.originalname}`); 	//重新拼接文件名
  	res.send(req.file);		//返回去文件 注意不写此句也完成了，这只是返回页面页面显示文件查看数据
})

server.listen(8080);
```

写完后，可以通过postman测试。post请求，对应url。body选formdata 参数写对应的file ，将输入框右下text改为file

然后send会返回

```javascript
{
    "fieldname": "file",								 //字段名：表单的name属性值
    "originalname": "javaf`���D�.doc",			  	    //上传的图片原始文件名（含后缀）
    "encoding": "7bit",							 		 //文件编码
    "mimetype": "application/msword",			  		   //上传文件格式（MIME 类型）
    "destination": "uploads/",							  //上传后的相对图片路径（不包含文件）
    "filename": "29b26380c669df22484876f8701359bf",			//上传的图片新生成的文件名（不含后缀）
    "path": "uploads\\29b26380c669df22484876f8701359bf",	 //上传后的完整图片路径（包含文件）
    "size": 68096										  //上传大小	
}
```

此时文件已上传到文件夹，但是将我们的文件名改成了临时名，所以需要fs模块处理。

```javascript
//fs.renameSync(oldPath, newPath);异步  			同步fs.rename()
fs.renameSync(req.file.path, `uploads/${req.file.originalname}`);		
```

另外后面的不是路由名，而是存储路径。所以和创建的文件夹名也就是dest对应



`multer({dest: './uploads'})`表示将上传的图片传输到指定文件夹（这里以uploads为例，如果没有该文件夹，会自动生成）,如果不添加dest属性，这些文件将保存在内存中，永远不会写入磁盘。

`upload.any()`表示接收任何上传的数据，对应的有个`upload.single('user')`(**表示只接收name为user的上传数据**)，本例使用`upload.any()`接收任何上传的数据。

multer会将上传的文件信息写到`req.file`中，表单的文本域信息放到 `req.body`中（所以不需要再引入body-parser）

**多文件上传**

```javascript
var express = require('express');
var router = express.Router();

const multer = require('multer')
const fs = require('fs');

router.post('/upload', multer({
	dest: 'uploads'
}).array('file', 10), (req, res) => {
  	const files = req.files;
  	const fileList = [];
  	for (var i in files) {
    	var file = files[i];
    	fs.renameSync(file.path, `upload/${file.originalname}`);
    	file.path = `uploads/${file.originalname}`;
    	fileList.push(file);
  	}
  	//res.send(fileList);	这里可以重定向回上传页面而不是返回数据到页面
})

```

注意的是参数二也改变了。是array()方法，参数2是最大支持文件数

#### 2.文件下载

get方法，传参是url。然后

```javascript
var express = require('express');
var router = express.Router();

const multer = require('multer')
const fs = require('fs');

router.get('/upload', (req, res) => {
	//文件不存在则flase    uploads中找
	req.query.url ? res.download(`upload/${req.query.url}`) : res.send({
    	success: false
  	})
})
```

#### 3.文件删除

fs.unlink(path)

```javascript
fs.unlink('/uploads/'+deletefilename,function(error){
if(error){
	console.log(error);
	return false;
}
	console.log('删除文件成功');
})
```





## 二、Express图书管理案例

新建mybook包。新建index.js`npm init -y`生成package.json文件。安装需要的包(空格一次性安装所有包)`npm install express art-template body-parser --save`。安装后，package.json就多了三个包的依赖。另外还需要express-art-template包

### 1.入口文件index.js

```javascript
/*
    图书管理系统-入口文件
*/
const express = require('express');
const template = require('art-template');
const bodyParser = require('body-parser');
const router = require('./router.js');		//引入路由文件
const path = require('path');				//引入path模块
const app = express();		   				//app对象

// 启动静态资源服务		(public文件夹下有静态资源时才用到这句。如果此案例样式写成内部样式就不需要这句)
app.use('/www',express.static('public'));		//www虚拟路径，就是网页平时访问输入的

// 设置模板引擎的3行
// 设置模板的路径
app.set('views',path.join(__dirname,'views'));
// 设置模板引擎
app.set('view engine','art');
// 使express兼容art-template模板引擎
app.engine('art', require('express-art-template'));


// 处理请求参数的2行  （04.js的两行）   (注意，不能讲这两行放到下面启动服务器以后，否则数据处理得不到)
// 挂载参数处理中间件（post）
app.use(bodyParser.urlencoded({ extended: false }));
// 处理json格式的参数
app.use(bodyParser.json());


// 启动服务器功能
// 1.(设置)配置路由
app.use(router);
// 2.监听端口
app.listen(3000,()=>{
    console.log('running...');
});
```

### 2.路由模块router.js

```javascript
/*
    路由模块
*/

const express = require('express');
const router = express.Router();
const service = require('./service.js');		//加载业务模块 使得下方可以直接绑定

// 路由处理

// 渲染主页
router.get('/',service.showIndex);		//后面本是具体的回调函数。简化，将路由直接绑定到业务方法（函数）上
// 去添加图书(跳转到添加图书的页面)
router.get('/toAddBook',service.toAddBook);
// 添加图书(提交表单。注意是post，表单提交一般都是用post)
router.post('/addBook',service.addBook);
// 跳转到编辑图书信息页面
router.get('/toEditBook',service.toEditBook);
// 编辑图书提交表单(编辑图书信息页面点击提交后需要的新的路由)
router.post('/editBook',service.editBook);
// 删除图书信息
router.get('/deleteBook',service.deleteBook);

module.exports = router;		//导出路由，否则入口文件中app.use(router)得不到
```

简化：路由处理时，后面不再是具体的回调函数，而是绑定下面业务模块service.js中写好的函数。

还需要模板。单独创建文件夹views，里面放具体的页面，比如index.html。数据暂时没用数据库，用单独的页面去存data.json。

### 3.业务模块service.js

而业务模块，比如删除增加操作。就放在单独的页面service.js

函数要exports公开出去

```javascript
/*
    业务模块
*/
const data = require('./data.json');		//导入数据。应该用let,修改的是内容而不是变量本身也可以const？
const path = require('path');
const fs = require('fs');

// 自动生成图书编号（自增） (添加图书时用到)
let maxBookCode = ()=>{
    let arr = [];
    data.forEach((item)=>{			//数组进行遍历。ietm是个对象
        arr.push(item.id);
    });
    return Math.max.apply(null,arr);	//数组中计算数组最大值的一个最简单的方法。apply()传递的是arr数组
}
// 把内存数据写入文件	 (写入的是文本，JSON.stringify将data转为字符串)   null,4不加写入文件无空格。这是格式
let writeDataToFile = (res) => {
    fs.writeFile(path.join(__dirname,'data.json'),JSON.stringify(data,null,4),(err)=>{
        if(err){
            res.send('server error');		
        }
        // 文件写入成功之后重新跳转到主页面 	(express路由的一个API:res.redirect(跳转的路径))
        res.redirect('/');
    });
}

// 渲染主页面(渲染需要的数据在上方  )
exports.showIndex = (req,res) => {
  	//res.render('index',data);  或者起个名称(加个对象)data本身是数组，给数组起了名字list。方便对数据解析渲染
    res.render('index',{list : data});	 //处理具体模板。参数(模板名称，对应数据)
}

}
// 跳转到添加图书的页面
exports.toAddBook = (req,res) => {
    res.render('addBook',{});				//文件名称addBook。因为是添加，所以不需要数据。写个空对象即可
}
// 添加图书保存数据
exports.addBook = (req,res) => {
    // 获取到表单提交过来的数据
    let info = req.body;
    let book = {};			//空对象用来放表单数据(新添加的图书)
    for(let key in info){	//for in循环遍历对象。将所有info中的对象放在book中
        book[key] = info[key];
    }
  	book.id = maxBookCode() + 1;
  	data.push(book);		//注意data在内存中并没有写入文件。
  	//把内存中的数据写入文件(导入fs模块和path模块)
    writeDataToFile(res);
}
// 跳转编辑图书页面
exports.toEditBook = (req,res) => {
    let id = req.query.id;			//获取修改href传过来的id参数
    let book = {};
    data.forEach((item)=>{		//data中找到这条
        if(id == item.id){
            book = item;
            return;			//停止跳出循环
        }
    });
  	//找到后把信息返回到页面进行渲染。这里使用模板引擎就可以了
    res.render('editBook',book);		//模板名称和数据
}
// 编辑图书更新数据
exports.editBook = (req,res) => {
    let info = req.body;			//这个是表单编辑的数据
    data.forEach((item)=>{
        if(info.id == item.id){
            for(let key in info){
                item[key] = info[key];		//item是index中显示的
            }
            return;
        }
    });
    // 把内存中的数据写入文件
    writeDataToFile(res);
}
// 删除图书信息
exports.deleteBook = (req,res) => {
    let id = req.query.id;
    data.forEach((item,index)=>{
        if(id == item.id){
            // 删除数组的一项数据
            data.splice(index,1);
        }
        return;
    });
    // 把内存中的数据写入文件
    writeDataToFile(res);
}
```

### 4.数据data.json

```javascript
//对象数组。注意json都要用双引号""
[
    {
        "id": "1",
        "name": "三国演义",
        "author": "罗贯中",
        "category": "文学",
        "desc": "一个杀伐纷争的年代"
    },
    {
        "id": "2",
        "name": "水浒传",
        "author": "施耐庵",
        "category": "文学",
        "desc": "108条好汉的故事"
    },
    {
        "id": "3",
        "name": "西游记",
        "author": "吴承恩",
        "category": "文学",
        "desc": "佛教与道教的斗争"
    },
    {
        "id": "4",
        "name": "红楼梦",
        "author": "曹雪芹",
        "category": "文学",
        "desc": "一个封建王朝的缩影"
    },
    {
        "name": "天龙八部",
        "author": "金庸",
        "category": "文学",
        "desc": "武侠小说",
        "id": 5
    }
]
```

###5.模板页面index.art

index.art

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图书管理系统</title>
    <link rel="stylesheet" type="text/css" href="/www/style.css">	//定制 路径名称www(存放在public下)
</head>
<body>
    <div class="title">图书管理系统<a href="/toAddBook">添加图书</a></div>
    <div class="content">
        <table cellpadding="0" cellspacing="0">
            <thead>
                <tr>
                    <th>编号</th>
                    <th>名称</th>
                    <th>作者</th>
                    <th>分类</th>
                    <th>描述</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                {{each list}}
                <tr>
                    <td>{{$value.id}}</td>						//注意每个value都是对象
                    <td>{{$value.name}}</td>
                    <td>{{$value.author}}</td>
                    <td>{{$value.category}}</td>
                    <td>{{$value.desc}}</td>
                    <td><a href="/toEditBook?id={{$value.id}}">修改</a>|<a href="/deleteBook?id={{$value.id}}">删除</a></td>
                    //后面加的参数 ?id={{$value.id} }是告诉服务器要修改哪个数据
                </tr>
                {{/each}}
   
            </tbody>
        </table>
    </div>
</body>
</html>
```

模板引擎处理数据。

each**遍历**list => {{each list}}        结束each=> {{/each}}

然后中间将对应的数据替换进行**数据的填充 **=>{{ $value }}。注意每个value都是对象,然后点出属性。



### 6.public文件与其他补充

样式存储在public文件夹下，引入时定制了www路径。然后在入口文件index.js中启动静态资源服务。

注意可以添加www虚拟路径www，就是网页平时访问输入的。



增删改查操作。href属性写路径`<a href="/toAddBook">添加图书</a></div>`，路径有了再在router.js添加路由。`router.get('toAddBook',service.toAddBook);`，再在业务模块service.js中准备toAddBook方法。

```javascript
exports.toAddBook = (req,res) => {
	res.render('addBook',{});
}
```

**注意：**路由router.js中的`router.get('/toAddBook',service.toAddBook);`前面是**网页访问url**。

而上面的`res.render('addBook',{});`是views中的**文件名称**。

接下来准备添加图书的页面。views文件夹下新建addBook.art

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>添加图书</title>
</head>
<body>
    <div>添加图书</div>
    <form action="/addBook" method="post">			//提交路径action。表单一般都是post提交	
      	//编辑时，需要用到的一个隐藏域。编辑时告诉服务器编辑的是哪条数据
      	<input type="hidden" name="id" value="{{id}}">
        名称：<input type="text" name="name"><br>
        作者：<input type="text" name="author"><br>
        分类：<input type="text" name="category"><br>
        描述：<input type="text" name="desc"><br>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```

修改类似。在跳转时，要把值填充进去。所以后面加个value=" ''

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>修改图书</title>
</head>
<body>
    <div>修改图书</div>
    <form action="/editBook" method="post">
        <input type="hidden" name="id" value="{{id}}">
        名称：<input type="text" name="name" value="{{name}}"><br>
        作者：<input type="text" name="author" value="{{author}}"><br>
        分类：<input type="text" name="category" value="{{category}}"><br>
        描述：<input type="text" name="desc" value="{{desc}}"><br>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```

然后再在处理路由`router.get('/toEditBook',service.toEditBook);`

其他同理

## 三、Node.js操作数据库

以上是将数据存在了文件中。但是有缺点，每次对数据的操作都要去写文件。数据量大时，性能开销是很大的。

为了解决问题，我们应该讲数据存在数据库。

### 1.Mysql环境准备

以前安装的Wamp提供了三个环境Apache、PHP、MYSQL。操作数据库我们借助图形界面的终端Navicat。

1.先打开Wamp。打开Navicat，创建连接，随便起个名称。点击测试，显示连接成功。点击确定。

2.然后双击打开，新建一个数据库。字符选通用的`utf8 -- UTF-8 Unicode`，规则选`utf8_bin`就可以了。

3.创建完成后双击打开。新建表，对应数据。注意id选择自增。这样数据库就帮我们维护id，并且不重复自增。这样插入数据时就没必要加id这个字段了。注意最后一个字段desc在数据库中不能叫这个名字，因为是数据库的关键字(降序排列  oreder by id desc)。然后保存。

4.打开表可以直接填充数据。初始化脚本，点击查询，新建查询。然后输入sql语句`insert into book (name,author,category,description) values ('','','','');`

我们也可以利用data.json生成sql脚本，然后再将脚本初始化。

注意：
​	必须打开Wamp，数据库才可以连接成功。

### 2.数据初始化操作

**把data.json文件中的数据拼接成insert语句**

在项目包中创建一个test文件夹。用来测试。

initsql.js

```javascript
/*
    把data.json文件中的数据拼接成insert语句
    (实际上就是文件操作：将data.json文件读取出来，进行数据的解析，然后拼接字符串，生成sql脚本就可以了)
*/
//引入文件相关模块
const path = require('path');
const fs = require('fs');

//fs.readFile读取文件
fs.readFile(path.join(__dirname,'../','data.json'),'utf8',(err,content)=>{
    if(err) return;
  	//读到的是字符串，解析要将他转为对象。实际就是个对象数组
    let list = JSON.parse(content);
    let arr = [];		//存放所有的sql语句 
    list.forEach((item)=>{			//item就是具体的一个对象
      	//拼接字符串 这里是读取的文件，所以是desc而不是数据库的sescrition
        let sql = `insert into book (name,author,category,description) values ('${item.name}','${item.author}','${item.category}','${item.desc}');`;
        arr.push(sql);
    });
  	//fs.writeFile写文件   (data.sql是写的文件的名称(路径) , 写入文件的内容arr.join('')拼接成整体的字符串)
    fs.writeFile(path.join(__dirname,'data.sql'),arr.join(''),'utf8',(err)=>{
        console.log('init data finished!');
    });
});
```

cmd输入node initsql.js。完成生成data.sql，里面是一系列的sql语句。打开后复制到Navicat的新建查询里，再运行即可。

## 四、mysql第三方包基本使用（nodeJS进行数据库的操作）

### 1.操作数据库基本步骤

使用nodeJS进行数据库的增删改查操作，还需要用到一个第三方包mysql。(这类第三方包都是去npm官网搜索进入github有文档说明)

安装`npm install mysql`,可以输入`npm install mysqljs/mysql`安装最新修复bug的更稳定。

mybook外新建一个mydb。用来进行演示数据库的操作。

**操作数据库的基本步骤**

入口文件index.js

```javascript
/*
    操作数据库基本步骤
*/

// 加载数据库驱动(操作数据库的具体api)
const mysql = require('mysql');
// 创建数据库连接
const connection = mysql.createConnection({
    host: 'localhost',    // 数据库所在的服务器的域名或者IP地址
    user: 'root',         // 登录数据库的账号
    password: '',         // 登录数据库的密码
    database: 'book'      // 数据库名称(注意不是连接的名称)
});
// 执行连接操作
connection.connect();
// 操作数据库	total是自己起的别名
connection.query('select count(*) as total from book', function(error, results, fields) {
    if (error) throw error;
    console.log('表book中共有', results[0].total + '条数据');
});
// 关闭数据库
connection.end();
```

`$ node . `会显示表book中共有5条数据。上面就是初步完成了数据库的查询操作，其他修改删除操作类似，只不过这里的sql脚本不同。

### 2.实现增删改查基本操作

查询返回的是一个集合，而插入删除修改返回的都是一个对象。里面有个常用的属性就是results.affectedRows。results.affectedRows == 1说明操作成功。或者是大于1，比如在修改数据时，选择性别是男或女的时候。影响的行数就大于1。

`update book set category = '作品' where category = '文学'; `受影响就是多行。可以设置results.affectedRows > 0 

#### 1.数据库的插入操作

只是操作数据库sql语句不一样。这里简化后与原生sql相比更简单，只要设置一个问号？，然后通过data的方式往里传参就可以了。而且id是自增的。

insert.js

```javascript
/*
    插入数据
*/
// 加载数据库驱动
const mysql = require('mysql');
// 创建数据库连接
const connection = mysql.createConnection({
    host: 'localhost', // 数据库所在的服务器的域名或者IP地址
    user: 'root', // 登录数据库的账号
    password: '', // 登录数据库的密码
    database: 'book' // 数据库名称
});
// 执行连接操作
connection.connect();

//使用第三方包时。sql可以这样写，简化处理了。问号？填充数据需要一个对象data，然后将data传到下面操作数据库里就可以
let sql = 'insert into book set ?'	 
let data = {
    name : '明朝那些事',
    author : '当年明月',
    category : '文学',
    description : '明朝的历史'
}
// 操作数据库
connection.query(sql,data, function(error, results, fields) {
    if (error) throw error;
    // console.log(results);	results中有一个属性affectedRows影响的行数。当等于1，说明插入成功
    if(results.affectedRows == 1){
        console.log('数据插入成功');
    }
});
// 关闭数据库
connection.end();
```

`$ node insert.js `。然后Navicat刷新会发现插入成功。

#### 2.更新操作

修改按条件来修改，比如修改id是几的数据。

这里API还是一样的`connection.query`。修改sql语句

```javascript
/*
    更新数据
*/
// 加载数据库驱动
const mysql = require('mysql');
// 创建数据库连接
const connection = mysql.createConnection({
    host: 'localhost', // 数据库所在的服务器的域名或者IP地址
    user: 'root', // 登录数据库的账号
    password: '', // 登录数据库的密码
    database: 'book' // 数据库名称
});
// 执行连接操作
connection.connect();

//问号将数据先填充，然后data传参  根据条件 where id=? 来修改
let sql = 'update book set name=?,author=?,category=?,description=? where id=?';
let data = ['浪潮之巅','吴军','计算机','IT巨头的兴衰史',8];

// 操作数据库
connection.query(sql,data, function(error, results, fields) {
    if (error) throw error;
    // console.log(results);
    if(results.affectedRows == 1){
        console.log('更新成功');
    }
});
// 关闭数据库
connection.end();
```

`$ node update.js  `运行

#### 3.删除操作

删除更简单，传参只需要传一个id就可以了。要删除的哪条。

```javascript
/*
    删除数据
*/
// 加载数据库驱动
const mysql = require('mysql');
// 创建数据库连接
const connection = mysql.createConnection({
    host: 'localhost', // 数据库所在的服务器的域名或者IP地址
    user: 'root', // 登录数据库的账号
    password: '', // 登录数据库的密码
    database: 'book' // 数据库名称
});
// 执行连接操作
connection.connect();

//删除的传一个id9就可以了，但是也要用数组包住。
let sql = 'delete from book where id = ?';
let data = [9];

// 操作数据库
connection.query(sql,data, function(error, results, fields) {
    if (error) throw error;
    // console.log(results);
    if(results.affectedRows == 1){
        console.log('删除成功');
    }
});
// 关闭数据库
connection.end();
```

#### 4.查询操作

查询所有的，data等于null就行.console.log(results);会输出所有的，

```javascript
let sql = 'select * from book';
let data = null;

// 操作数据库
connection.query(sql,data, function(error, results, fields) {
    if (error) throw error;
    console.log(results);
});
//
```

因为results是个对象数组，可以通过results[0]获取到第一数据对象。然后可以点出属性。比如results[2].name

但是，当我通过id查询具体的某条数据。results是个只有一条数据的对象数组。只有results[0]而没有results[1]...

所以当通过id查询具体时，如果console.log(results[2].name); 会报错

```javascript
/*
    查询数据
*/
// 加载数据库驱动
const mysql = require('mysql');
// 创建数据库连接
const connection = mysql.createConnection({
    host: 'localhost', // 数据库所在的服务器的域名或者IP地址
    user: 'root', // 登录数据库的账号
    password: '', // 登录数据库的密码
    database: 'book' // 数据库名称
});
// 执行连接操作
connection.connect();

let sql = 'select * from book where id = ?';
let data = [6];

// 操作数据库
connection.query(sql,data, function(error, results, fields) {
    if (error) throw error;
    console.log(results[0].name);
    // console.log(results);
});
// 关闭数据库
connection.end();
```

### 3.封装

增删改查的代码我们可以知道是有规律的。不管哪种操作，步骤基本是类似的。只不过sql语句和数据参数以及处理的结果不一样。我们可以将其封装成一个通用的API。使用更方便。

sql和data通过参数传递的方式传递进去。处理的结果resultst通过回调函数callback

db.js

```javascript
/*
    封装操作数据库的通用api
*/
const mysql = require('mysql');

exports.base = (sql,data,callback) => {
    // 创建数据库连接
    const connection = mysql.createConnection({
        host: 'localhost', // 数据库所在的服务器的域名或者IP地址
        user: 'root', // 登录数据库的账号
        password: '', // 登录数据库的密码
        database: 'book' // 数据库名称
    });
    // 执行连接操作
    connection.connect();

    // 操作数据库(数据库操作也是异步的，对于异步操作，不能通过返回值的方式处理，只能通过回调函数)
  	//这也是nodeJS异步编程风格的通用做法，之前的文件操作，网络通信的操作都是通过回调函数的方式来编码
    connection.query(sql,data, function(error, results, fields) {
        if (error) throw error;
      	//调用callback回调函数并传递results参数
        callback(results);
    });
    // 关闭数据库
    connection.end();
}
```

然后新建一个dbtest.js。运行时直接`$ node dbtest.js`即可

```javascript
/*
    测试通用api
*/
const db = require('./db.js');

//插入操作
let sql = 'insert into book set ?';
let data = {
    name : '笑傲江湖',
    author : '金庸',
    category : '文学',
    description : '武侠小说'
}

db.base(sql,data,(result)=>{
    console.log(result);
  	//刷新Navicat。已插入
});

// 更新操作
let sql = 'update book set name=?,author=?,category=?,description=? where id=?';
let data = ['天龙八部','金庸','文学','武侠小说',11];

db.base(sql,data,(result)=>{
    console.log(result);
});

//删除操作
let sql = 'delete from book where id = ?';
let data = [11];

db.base(sql,data,(result)=>{
    console.log(result);
});
//查询操作
let sql = 'select * from book where id = ?';
let data = [8];

db.base(sql,data,(result)=>{
    console.log(result[0].name);
});
```

注意封装的result就是原生的results。是个对象数组。本质是数组，所以可以通过result.length判断有无结果。也可以通过result[0]，获取到查询的数据。比如result[0].name。

另外还有	result.affectedRows == 1这种用法。

## 五、基于数据库实现登录功能

修改之前的登录验证案例(之前是假数据)

安装需要的包。

`$ npm install express body-parser --save`     不同的项目包，需要的第三方包需要在项目包下重新安装。

Navicat新建user表

```javascript
/*
    登录验证（前端+后端+数据库）
*/
const express = require('express');
const bodyParser = require('body-parser');
const db = require('./db.js');					//加载封装好的api
const app = express();

// 处理请求参数   （图书管理中index.js的两行）  
// 挂载参数处理中间件（post）      这里可以只用一行，处理正常的表单提交。不用json的数据
app.use(bodyParser.urlencoded({ extended: false }));
// 启动静态资源服务		 (public文件夹下有静态资源时才用到这句。)
app.use(express.static('public'));

//绑定路径(路由) 注意这里的是check 那么在login.html中的表单提交路径也是check
app.post('/check',(req,res)=>{
  	//获取到表单的数据
    let param = req.body;
	
  	//上面dbtest的三句。调用db.js
    let sql = 'select count(*) as total from user where username=? and password=?';
    let data = [param.username,param.password];

    db.base(sql,data,(result)=>{
      	//查得到 total=1，说明数据库中有。登陆成功
        if(result[0].total == 1){
            res.send('login success!');
        }else{
            res.send('login failure!');
        }
    });
  
});

app.listen(3000,()=>{
    console.log('running...');
});

```

public文件夹下的index.html

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>登录</title>
</head>
<body>
    <form action="http://localhost:3000/check" method="post">
        用户名：<input type="text" name="username"><br>
        密码：<input type="password" name="password"><br>
        <input type="submit" value="登录">
    </form>
</body>
</html>
```

`$ node login.js` 浏览器输入http://localhost:3000/check.html。 输入账户密码 登录成功

以上就是 登录验证（前端+后端+数据库） 。

但是数据库操作的代码，在真实的项目中，应该写在一个独立的文件中。这里放一起演示效果。