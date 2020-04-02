# 大纲-day03

## 一、Web开发概述

```javascript
Node.js的Web开发相关的内容：
1、Node.js不需要依赖第三方应用软件（Apache），可以基于api自己实现
2、实现静态资源服务器
3、路由处理
4、动态网站
5、模板引擎
6、get和post参数传递和处理

Web开发框架：express
```
- Node.js服务器模型与php服务器模型的区别

  传统的动态网站开发需要应用软件

  PHP ： Apache + php模块

  java ：Tomcat 、Weblogic

  Node.js  : 不需要应用软件（可以自己实现）

![](img/server.png)

## 二、Node.js实现静态网站功能
- 使用**http模块**初步实现服务器功能
- 实现静态服务器功能

### 1.创建服务器

http.createServer([requestListener])

参数可选，并且返回是一个http.server的实例

```javascript
  /*
      初步实现服务器功能
  */
  const http = require('http');         //引入http核心模块，然后就可以使用API

  //一个最简单的服务器的请求响应的处理方式

  // 创建服务器实例对象
  let server = http.createServer();             //返回值是一个server对象，包含一系列的方法
  // 给server对象绑定请求事件  'request'也是文档提供的事件
  server.on('request',(req,res)=>{	    //两对象  req:(request)请求的信息 	res:(respon)响应的信息
  	res.end('hello');				  //res.end方式把客户端进行响应  （启动访问后页面内容是hello）
  });
  // (绑定)监听端口：服务器的端口，绑定时指定3000，否则访问不到   （正常的业务一般用的是80，端口才可以省略）
  server.listen(3000);
  -----------------------------
  简写
  http.createServer((req,res)=>{192.168.0.106:3000成功，ip地址默认是localhost或者127.0.0.1
      res.end('ok');
  }).listen(3000,'192.168.0.106',()=>{	
      console.log('running...');		  
  });	
  //listen不止一个参数，比如域名、回调函数   第2参数:监控的ip地址
  //192.168.0.106:3000成功，ip地址默认是localhost或者127.0.0.1
  //此时，localhost:3000访问失败。因为修改为了本地ip'192.168.0.106'
```

  启动

```javascript
  node xxx.js
  成功没任何提示和报错，但是一直在提供服务
```

  通过窗口访问

```javascript
  浏览器输入 localhost:3000 回车
```

  但是有个问题，不管请求的地址是什么，它返回的内容都是一样的。localhost:3000/index.html也是返回的hello。

  应该根据url的不同，返回的内容不同

### 2. 处理请求路径的分发

```javascript
/*
      处理请求路径的分发
      1、req对象是Class: http.IncomingMessage的实例对象
      2、res对象是Class: http.ServerResponse的实例对象
*/
const http = require('http');
http.createServer((req,res)=>{
	// req.url可以获取URL中的路径（端口之后部分） 所以我们可以根据此属性来实现
	// res.end(req.url);
	if(req.url.startsWith('/index')){					//如果req.url是以/index开始
		// write向客户端响应内容,可以写多次
		res.write('hello');
		res.write('hi');
		res.write('nihao');
		//而end方法用来完成响应，只能执行一次
		res.end();
	}else if(req.url.startsWith('/about')){
		res.end('about');
	}else{
		res.end('no content');
	}
}).listen(3000,'192.168.0.106',()=>{
	console.log('running...');
});
```

  以上完成返回的都是字符串，对于url请求请求文件，应该返回一个完整的页面。

### 3.响应完整的页面信息

 以上完成返回的都是字符串，对于url请求请求文件，应该返回一个完整的页面。涉及到了文件的读取操作，用到之前的文件操作与目录操作。

单独的目录www来存放完整的页面，www下新建index.html和about.html

```javascript
  /*
      响应完整的页面信息
  */
const http = require('http');
const path = require('path');
const fs = require('fs');

// 根据路径读取文件的内容，并且响应到浏览器端
//代码重用，提取出来声明方法readFile，原本未单独提取出来成方法直接写在下面时，res可以获取到，但这里需要传入res
let readFile = (url,res) => {
	fs.readFile(path.join(__dirname,'www',url),'utf8',(err,fileContent)=>{
		if(err){
			res.end('server error');
		}else{
             res.end(fileContent);
         }
     });
}

http.createServer((req,res)=>{
	// 处理路径的分发
	if(req.url.startsWith('/index')){
		readFile('index.html',res);
	}else if(req.url.startsWith('/about')){
		readFile('about.html',res);
	}else if(req.url.startsWith('/list')){
		readFile('list.html',res);
	}else{
		//writeHead设置相应类型和编码，否则容易乱码
		res.writeHead(200,{
			'Content-Type':'text/plain; charset=utf8'
		});
		res.end('页面被狗狗叼走了');
	}
}).listen(3000,'192.168.0.106',()=>{
	console.log('running...');
});
```

上面是通过分支的方式去判断不同的请求，这样做不太灵活。随着静态资源越来越多，分支也越来越多。

我们应该**根据url来获取对应的文件**，一般静态资源url中的文件名称req.url与真实的文件名称是一样的。所以我们可以根据这个特征来读取文件。

```javascript
/*响应完整的页面信息*/

/*以上的替代，简洁很多
const http = require('http');
const path = require('path');
const fs = require('fs');

http.createServer((req,res)=>{
    fs.readFile(path.join(__dirname,'www',req.url),(err,fileContent)=>{
        if(err){
            // 没有找到对应文件
            res.writeHead(404,{
                'Content-Type':'text/plain; charset=utf8'
            });
            res.end('页面被狗狗叼走了');
        }else{
            res.end(fileContent);		
        }
    });
}).listen(3000,()=>{
    console.log('running...');
});*/

//但是还有个问题，这仅仅是文本文件，如果是图片也可以，但是控制台打开networl会发现没有给具体的响应类型。如果没用类型，有时候会有问题。有一个办法区分请求的是哪种格式的文件，从而找到对应的响应内容类型。这样浏览器就可以识别不同的文件风格，更为严谨。
const http = require('http');
const path = require('path');
const fs = require('fs');
const mime = require('./mime.json');    //导入映射表：根据不同的文件后缀找到对应的响应内容类型

http.createServer((req,res)=>{
    fs.readFile(path.join(__dirname,'www',req.url),(err,fileContent)=>{
        if(err){
            // 没有找到对应文件
            res.writeHead(404,{
                'Content-Type':'text/plain; charset=utf8'
            });
            res.end('页面被狗狗叼走了');
        }else{
          	//默认类型
            let dtype = 'text/html';
            // 获取请求文件的后缀
            let ext = path.extname(req.url);
            // 如果请求的文件后缀合理，就获取到标准的响应格式
            if(mime[ext]){		//如果在响应表中是存在的，就覆盖掉
                dtype = mime[ext];
            }
            // 如果响应的内容是文本，就设置成utf8编码
            if(dtype.startsWith('text')){
                dtype += '; charset=utf8'
            }
            // 设置响应头信息
            res.writeHead(200,{
                'Content-Type':dtype
            });
            res.end(fileContent);
        }
    });
}).listen(3000,()=>{
    console.log('running...');
});
```

### 4.静态服务抽离成独立的模块

静态资源服务是一个非常独立的功能他，可以抽取成一个单独的模块。

```javascript
//http不需要，只用req,res
const path = require('path');
const fs = require('fs');
const mime = require('./mime.json');

//暴露出方法
exports.staticServer = (req,res,root) => {        //root静态资源根路径
    fs.readFile(path.join(root,req.url),(err,fileContent)=>{
        if(err){
            // 没有找到对应文件
            res.writeHead(404,{
                'Content-Type':'text/plain; charset=utf8'
            });
            res.end('页面被狗狗叼走了');
        }else{
            let dtype = 'text/html';
            // 获取请求文件的后缀
            let ext = path.extname(req.url);
            // 如果请求的文件后缀合理，就获取到标准的响应格式
            if(mime[ext]){
                dtype = mime[ext];
            }
            // 如果响应的内容是文本，就设置成utf8编码
            if(dtype.startsWith('text')){
                dtype += '; charset=utf8'
            }
            // 设置响应头信息
            res.writeHead(200,{
                'Content-Type':dtype
            });
            res.end(fileContent);
        }
    });
}
```

测试服务器功能，单独再创建一个服务器。

```javascript
const http = require('http');
const ss = require('./06.js');      	//引入模块
const path = require('path');

http.createServer((req,res)=>{
    // ss.staticServer(req,res,path.join(__dirname,'www'));
    ss.staticServer(req,res,path.join('C:\\Users\\www\\Desktop','test'));  //静态资源根路径，最后是文件夹名
}).listen(3000,()=>{
    console.log('running....');
})
```



## 三、参数传递与获取

动态网站涉及到参数的传递与获取

### 1.get参数获取

url核心模块

```javascript
/*
    get参数处理-url核心模块
*/

const url = require('url');

// parse方法的作用就是  把URL字符串转化为对象
let str = 'http://www.baidu.com/abc/qqq?flag=123&keyword=java';
let ret = url.parse(str,true);  
// 第二个参数加上容易获取到属性值，比如
console.log(ret.query.keyword);

----------------------------------------------
//format的作用就是  把对象转化为标准的URL字符串
let obj = {
   protocol: 'http:',
   slashes: true,
   auth: null,
   host: 'www.baidu.com',								
   port: null,
   hostname: 'www.baidu.com',						//不包括端口，如果没指定值与host是一样的
   hash: null,									   //锚点，#后面的
   search: '?flag=123&keyword=java',				//所有参数，？后面的
   query: 'flag=123&keyword=java',					//去掉问号后的
   pathname: '/abc/qqq',						
   path: '/abc/qqq?flag=123&keyword=java',
   href: 'http://www.baidu.com/abc/qqq?flag=123&keyword=java' 
};
let ret1 = url.format(obj);
console.log(ret1);  //http://www.baidu.com/abc/qqq?flag=123&keyword=java
```

### 2.获取get请求的参数

```javascript
/*
    get参数解析：参数从浏览器端获取到后台
*/
const http = require('http');
const path = require('path');
const url = require('url');			//引入get的核心模-url

http.createServer((req,res)=>{			//创建服务器
    let obj = url.parse(req.url,true);			//通过url的格式化方式获取到具体的数据
    res.end(obj.query.username + '=========' + obj.query.password);
}).listen(3000,()=>{
    console.log('running....');
})

浏览器输入localhost:3000?username=ch&passpassword=123，回车获取到
页面显示ch=========123
```

### 3.post参数获取

post数据不包括在url中。

querystring核心模块。模块中只有4个API，而且querystring.escape(str)和querystring.unescape(str)分别是被querystring.stringify()和querystring.parse()内部调用的，所以用不到。分别是转义处理和反转义处理。

```javascript
/*
    post参数处理-querystring核心模块
*/
const querystring = require('querystring');
const http = require('http');

// parse方法的作用就是 把字符串转成对象
let param = 'username=lisi&password=123';         // {username: 'lisi', password: '123'}
//如果参数名称一样的话，值会放在数组中
let param = 'foo=bar&abc=xyz&abc=123';			//  {foo: 'bar', abc: ['xyz', '123',] }
let obj = querystring.parse(param);
console.log(obj);
  
----------------------------------------------
// stringify的作用就是把对象转成字符串
let obj1 = {
     flag : '123',
     abc : ['hello','hi']
}
let str1 = querystring.stringify(obj1); 	
console.log(str1);			// flag=123&abc=hello&abc=hi
```

### 4.获取post请求的参数

```javascript
/*
   post参数解析：参数从浏览器端获取到后台
*/
const querystring = require('querystring');
const http = require('http');

http.createServer((req,res)=>{
    if(req.url.startsWith('/login')){		//如果以开头，传递过来参数
        let pdata = '';
      	//不能用传统的方式而是事件的方式
        req.on('data',(chunk)=>{
            // 每次获取一部分数据,调用一次拼接一次
            pdata += chunk;
        });

        req.on('end',()=>{			//绑定一个end结束事件
            // 这里才能得到完整的数据
            console.log(pdata);
            let obj = querystring.parse(pdata);
            res.end(obj.username+'-----'+obj.password);			//数据返回到浏览器端
        });
    }
}).listen(3000,()=>{
    console.log('running...');
})
```

测试可以用postman插件测试

### 5.登录验证案例

login.html

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <form action="http://localhost:3000/login" method="post">
        用户名：<input type="text" name="username"><br>
        密码：<input type="password" name="password"><br>
        <input type="submit" value="提交">
    </form>
</body>
</html>
```

后台 11.js

```javascript
/*
    登录验证功能
*/
const http = require('http');
const url = require('url');
const querystring = require('querystring');
const ss = require('./06.js');             //引入静态资源服务

http.createServer((req,res)=>{
    // 启动静态资源服务
    if(req.url.startsWith('/www')){
        ss.staticServer(req,res,__dirname);
    }
    console.log(req.url);
    // 动态资源
    if(req.url.startsWith('/login')){
        // get请求
        if(req.method == 'GET'){
            let param = url.parse(req.url,true).query;
            if(param.username == 'admin' && param.password == '123'){
                res.end('get success');
            }else{
                res.end('get failure');
            }
        }
        // post请求
        if(req.method == 'POST'){
            let pdata = '';
            req.on('data',(chunk)=>{
                pdata += chunk;       //每次获取一部分数据
            });
            req.on('end',()=>{      //得到完整的数据
                let obj = querystring.parse(pdata);  
                if(obj.username == 'admin' && obj.password == '123'){       //登录成功
                    res.end('post success');
                }else{
                    res.end('post failure');
                }
            });
        }
    }

}).listen(3000,()=>{
    console.log('running....');
});
```

06.js

```javascript
const path = require('path');
const fs = require('fs');
const mime = require('./mime.json');

exports.staticServer = (req,res,root) => {
    fs.readFile(path.join(root,req.url),(err,fileContent)=>{
        if(err){
            // 没有找到对应文件
            res.writeHead(404,{
                'Content-Type':'text/plain; charset=utf8'
            });
            res.end('页面被狗狗叼走了');
        }else{
            let dtype = 'text/html';
            // 获取请求文件的后缀
            let ext = path.extname(req.url);
            // 如果请求的文件后缀合理，就获取到标准的响应格式
            if(mime[ext]){
                dtype = mime[ext];
            }
            // 如果响应的内容是文本，就设置成utf8编码
            if(dtype.startsWith('text')){
                dtype += '; charset=utf8'
            }
            // 设置响应头信息
            res.writeHead(200,{
                'Content-Type':dtype
            });
            res.end(fileContent);
        }
    });
}
```



## 四、动态网站开发
- 创建服务器实现动态网站效果

index.tpl

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>查询成绩</title>
</head>
<body>
    <form action="http://localhost:3000/score" method="post">
        请输入考号：<input type="text" name="code">
        <input type="submit" value="查询">
    </form>
</body>
</html>
```

result.tpl

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>成绩结果</title>
</head>
<body>
    <div>
        <ul>
            <li>语文：$$chinese$$</li>
            <li>数学：$$math$$</li>
            <li>外语：$$english$$</li>
            <li>综合：$$summary$$</li>
        </ul>
    </div>
</body>
</html>
```

12.js

```javascript
/*
    动态网站开发

    成绩查询功能
*/

const http = require('http');
const path = require('path');
const fs = require('fs');
const querystring = require('querystring');
const scoreData = require('./scores.json');

http.createServer((req,res)=>{
    // 路由（请求路径+请求方式）
    // 查询成绩的入口地址 /query
    if(req.url.startsWith('/query') && req.method == 'GET'){
        fs.readFile(path.join(__dirname,'view','index.tpl'),'utf8',(err,content)=>{
            if(err){
                res.writeHead(500,{
                    'Content-Type':'text/plain; charset=utf8'
                });
                res.end('服务器错误，请与管理员联系');
            }
            res.end(content);
        });
    }else if(req.url.startsWith('/score') && req.method == 'POST'){
        // 获取成绩的结果 /score
        let pdata = '';
        req.on('data',(chunk)=>{
            pdata += chunk;
        });
        req.on('end',()=>{
            let obj = querystring.parse(pdata);
            let result = scoreData[obj.code];
            fs.readFile(path.join(__dirname,'view','result.tpl'),'utf8',(err,content)=>{
                if(err){
                    res.writeHead(500,{
                        'Content-Type':'text/plain; charset=utf8'
                    });
                    res.end('服务器错误，请与管理员联系');
                }
                // 返回内容之前要进行数据渲染
                content = content.replace('$$chinese$$',result.chinese);
                content = content.replace('$$math$$',result.math);
                content = content.replace('$$english$$',result.english);
                content = content.replace('$$summary$$',result.summary);

                res.end(content);
            });
        });
    }
    

}).listen(3000,()=>{
    console.log('running....');
});
```

score.json

```javascript
{
    "no123" : {
        "chinese" : "100",
        "math" : "140",
        "english" : "149",
        "summary" : "289"
    },
    "no124" : {
        "chinese" : "120",
        "math" : "120",
        "english" : "119",
        "summary" : "239"
    },
    "no125" : {
        "chinese" : "130",
        "math" : "110",
        "english" : "139",
        "summary" : "269"
    }
}
```



## 五、模板引擎（art-template）
- 理解模板引擎本质

  模板引擎是一个将**页面模板**和要**显示的数据**结合生成HTML页面的工具。

  模板的诞生是为了将显示与数据分离，模板技术多种多样，但其本质是将模板文件和数据通过模板引擎生成最终的HTML代码。模板引擎就是利用正则表达式识别模板标识，并利用数据替换其中的标识符。

  通过模板，我们可以将逻辑写在html页面里。只要更换数据源，就能输出不同的页面代码了。

- 引擎基本使用

  (具体可看下面案例以及express图书馆案例中也清除展示了模板引擎)

新建mydemo文件夹，创建index.js作为入口文件。cmd中`\mydemo\npm init-y`生成pacakage.json文件来描述包。

安装模板引擎`\mydemo\npm install art-templagte --save`

案例：

准备模板

​	mytpl.art

```javascript
{{if user}}
	<h2>{{user.name}}</h2>
{{/if}}
```

入口文件

​	index.js

```javascript
/*
    模板引擎
*/

//基本使用
let template = require('art-template');
let html = template(__dirname + '/mytpl.art', {
	user: {
		name: 'lisi'
	}
});
console.log(html);
----------------------------------
//模板引擎提供的核心方法API  
template(filename, data);					//基于模板名渲染模板
template.compile(source, options);		 	 //将模板代码编译成函数
template.render(source, data, options);		 //将模板代码编译成函数并立即执行

let tpl = '<ul>{{each list as value}}<li>{{value}}</li>{{/each}}</ul>';   //  {{/each}}循环结尾
let render = template.compile(tpl);		//将模板代码编译成函数：渲染函数render
let ret = render({
	list : ['apple','orange','banana']
});
console.log(ret);
-----------------------------------
//let tpl = '<ul>{{each list as value}}<li>{{value}}</li>{{/each}}</ul>'; 	下面也可以
let tpl = '<ul>{{each list}}<li>{{$index}}-------------{{$value}}</li>{{/each}}</ul>';
let ret = template.render(tpl,{			//将模板代码编译成函数并立即执行
	list : ['apple','orange','banana','pineapple']
});
console.log(ret);
```

**修改案例**(未用模板引擎)

模板score.art

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>成绩结果</title>
</head>
<body>
    <div>
        <ul>
            <li>语文：{{chinese}}</li>				//将$$符替掉就可以了   原样：$$chinese$$
            <li>数学：{{math}}</li>
            <li>外语：{{english}}</li>
            <li>综合：{{summary}}</li>
        </ul>
    </div>
</body>
</html>
```

入口文件index.js

```javascript
html = template(__dirname + '/score.at', {
	chinese : '120',
	math : '130',
	english : '146',
	summary : '268'
});
console.log(html);
```

(未看)**模板引擎修改成绩查询案例**




