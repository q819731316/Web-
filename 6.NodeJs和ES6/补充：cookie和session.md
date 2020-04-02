# cookie和session

和服务器的交互：http

但是http有不足的是它是**无状态的**。也就是两次请求间，服务器无法识别是否是同一人。每刷新一次，登陆一次。

## cookie和session的区别：

**cookie：**在客户端(浏览器)保存一些数据，并且每次向服务器发起请求时都会带过来。登录状态什么的都是通过cookie完成的。第一次访问时，cookie可能是空的，就可以设置。下次再访问，通过带过来的cookie就可以识别。

​	**缺陷**：保存在客户端(浏览器)的信息都是不安全和不可信的。用户可以轻松的看到和修改所有的cookie。像用户余额这种，如果存在cookie问题就大了。另外，cookie大小有限(4k)。

**session：**在服务端保存一些数据。相对cookie来说安全不少，和数据库一个级别。而session由于存储在服务器，服务器多大，就可以存多少。

## 一、cookie

那既然这样，为什么还要cookie呢？因为**session是基于cookie来实现的。**

第一次客户端(浏览器)访问服务器。cookie是空的`cookie: {}`  ，服务器就会给客户端种一个cookie：`cookie: {session_id:xxxx}`。

当再一次访问服务器，客户端就会将cookie带给服务器，而且cookie里面的这个sess_id不变。服务器就可以通过读取session id相关的文件来得到session相关的信息



cookie中有一个session的ID，服务器利用sessionid找到session文件、读取、写入。 

但是也存在隐患：**session劫持**。

​	sessionid存储在cookie，那别人在你的客户端拿到你的id，然后去访问，就是你的身份。这些就需要一些处理了。比如加密等等





cookie:

​	1.发送——

​	2.读取——cookie-parser

session：

​	cookie-seesion

​	1.写入cookie

​	2.读取



安装 `npm install express express-static cookie-parser cookie-session`   初始化，然后运行。

### 1.发送cookie

以服务器来看。响应，回去的都是response。给客户端发送、添加一个cooie，所以是res.cookie()

request请求、读取客户端的cookie。所以是读取req.cookies

添加一个cookie

server1.js

```javascript
const express = require('express');
var server = express();

//cookie
server.use('/aaa/index.html',function (req,res){
	//发送cookie(名字user,值ch)：res.cookie('user','ch');    设置的其他东西。比如path、过期时间需要第三个参数 
  	//path路径，通俗说就是在哪个目录下才可以读取cookie。这里就必须localhost：8080/aaa/index.html
  	//maxAge。比如30天过期 （ms为单位） 
  	res.cookie('user','ch',{path: '/aaa',maxAge: 30*24*60*60*1000});
  	res.send('test');
})

server.listen(8080,(req,res)=>{
	console.log('running');
});
```

在浏览器控制台的network后的application下的storage下的cookie就可以看到



注意这里如果use第一个路径是根路径`servr.use('/', )`。就是localhost：8080访问。

而改成`servr.use('/aaa/index.html', )`。访问就是localhost：8080/aaa/index.html。

这里没有写html文件。

### 2.读取cookie(cookie-parser)

>发送cookie仅仅res.cookie()就好了。而读取cookie就需要用到cookie-parser中间件。

server2.js

```javascript
const express = require('express');
const cookieParser = require('cookie-parser');		//引入读取cookie的中间件
var server = express();

//cookie
server.use(cookieParser());  	//加了后才有req.cookies属性

server.use('/',function (req,res){
  	console.log(req.cookies);			// { user : ch } 		就是我们上面发送的的cookie
  	res.send('test');
})

server.listen(8080,(req,res)=>{
	console.log('running');
});
```

注意这里访问不到。

​	cookie只可以作用**当前目录和当前目录的子目录**. 但不能作用于当前目录的上一级目录。

cookie是可以往下访问的。比如/aaa/bbb/ccc 。我在bbb种了一个cookie，那么它的下一层/ccc和当前目录都可以访问到。而上一层/aaa(包括根目录)不行。

#### cookie签名

应该叫签名，值还在里面，而且这种%开头的明显是urlencoded。也可以`decodeURIComponet('')`通过解码。得到

s:值.sdsafasfasfsd   (.后的全是签名)  。

必须提供签名的密钥，而且值只有自己知道，因为放在了后台。

```javascript
const express = require('express');
const cookieParser = require('cookie-parser');		//引入读取cookie的中间件
var server = express();

//cookie
server.use(cookieParser('safa34sgrg'));  	//加了后才有req.cookies属性

server.use('/',function (req,res){
  	req.secret = 'safa34sgrg';		    //签名的密钥，比如我随便写一个
  	res.cookie('user','ch',{signed: true});				//设置signed的签名，默认false
  	res.send('test');
  	//console.log(req.cookies);		
})

server.listen(8080);
```

然后在控制台看到的是 user : s%3Ach.962Yw%2BcDZvQqB90sxZGosVth5BkaHqCqDdooG3JHzjo

被签名了。原文看得见，但是可以杜绝修改：修改了可以看出来。	(把值改了，后面的签名对不上)



签名后，得到的是这种。而我要值，该怎么办？

在`server.use(cookieParser());  `中加上签名，和他说一声。`server.use(cookieParser('safa34sgrg'))`



但是，上面如果输出`console.log(req.cookies);	`，得到的是空。因为存储到了另一个属性

```javascript
console.log('带签名的cookie：', req.signedCookies);		//存储签名过的cookie
console.log('无签名的cookie：', req.cookies);			//无签名的cookie
```


并不是所有的cookie都签名，因为签了名的和原始的比特别长。而cookie本来容量就有限。如果对所有的cookie都签名会非常浪费空间。

标志：s什么的开头，说明带签名的。如果不是，普通的就不需要解析。



总结cookie使用事项：

​	cookie：1.空间非常小——省着用，精打细算(能不签就不签)

​			2.安全性非常差——校验cookie(至少校验cookie是否被篡改过)

### 3.删除

也是response。因为实际是对浏览器发送要求，删除cookie

res.clearCookie(名字)

```javascript
const express = require('express');
const cookieParser = require('cookie-parser');		//引入读取cookie的中间件
var server = express();

//cookie
server.use(cookieParser());  	//加了后才有req.cookies属性

server.use('/',function (req,res){
    res.clearCookie('user');
  	res.send('test');	
})

server.listen(8080);
```



### 总结

1. 发送/添加cookie	

   ​	`req.secret = '字符串签名';	`

   ​	`res.cookie(名字，值，{path : '/', maxAge : ms , signed : true});`

2. 读取cookie

   ​	用了中间件cookie-parser			`const cookieParser = require('cookie-parser');	`

   ​	`server.use(cookeParser('字符串签名'));`

   ​	两属性

   ​		 req.cookies			未签名的cookie

   ​		req.signedCookies 	 带签名的cookie

3. 删除cookie

   ​	res.clearCookie(名字)



cookie由于在客户端，再怎么加密，费工夫还是可以。所以cookie加密看自己，没有意义。如果实在要加密。可以用到cookie-encryter中间件。专门用来加密cookie。不过建议加密的放在session。

## 二、sessioncookie-session

解析好的session在属性req.session上

```javascript
const express = require('express');
const cookieParser = require('cookie-parser');		//引入读取cookie的中间件
const cookieSession = require('cookie-session');		//引入读取cookie的中间件
var server = express();

//session

server.use(cookieParser());

//cookie-session强制必须要一个keys用来加密，否则报错。而且不在req和res。
//数组个数不定，而且会循环使用数组中的密钥来加密
server.use(cookieSession({
	keys: ['aaa','bbb', 'ccc']
}));	//一般放cookie-parser后，因为只有解析了cookie后才能用里面的sessionid

server.use('/',function (req,res){
  	console.log(req.session);	
})

server.listen(8080);
```

比如记录访问次数。比如保存登录状态

```javascript
const express = require('express');
const cookieParser = require('cookie-parser');		//引入读取cookie的中间件
const cookieSession = require('cookie-session');		//引入读取cookie的中间件
var server = express();

//session

server.use(cookieParser());

//cookie-session强制必须要一个keys用来加密，否则报错。而且不在req和res。数组越长越安全
//数组个数不定，而且会循环使用数组中的密钥来加密
server.use(cookieSession({
	keys: ['aaa','bbb', 'ccc']
}));	//一般放cookie-parser后，因为只有解析了cookie后才能用里面的sessionid

server.use('/',function (req,res){
  	if(req.session['count'] == null){		//第一次访问
      	req.session['count'] == 1;
  	}else {
      	req.session['count']++;
  	}
  	console.log(req.session);	
  	res.send(test);
})

server.listen(8080);
```

运行访问后不断刷新，后台数字就一直在增加。



### session的参数#

​	和session相关的cookie有两个。name为session的后面值就是sessionid，name为session.sjg是签名，保护防止session被篡改。每刷新一次，这个session.sjg都会变化？

```javascript
server.use(cookieSession({
  	name: 'dsa',			//可以自己修改name
	keys: ['aaa','bbb', 'ccc'],
  	maxAge：	2*3600*1000			//session有效期。默认20分钟？
}));	

```

比如长时间不登录，会自动把session销毁，就让你登录状态掉了。如果session一直有效。攻击者就有无限的时间去劫持你的session。而且sesion存在服务器上，时间越长，占用服务器。



session其实只是一个逻辑上的东西。代码里并没有session。session其实还是cookie，只不过里面只存一个id

所以不存在独立的session一说，不能独立存在，基于cookie。

**session总结：**

​	中间件cookie-session

```javascript
server.use(cookieParser());					//首先需要cookie-parser解析
server.use(cookieSession({
	keys: ['...','..', '...',...],
}));	

           
server.use('/',function (req,res){
  	console.log(req.session['xxx']);	
    //删除直接delete就行。就是js中删除属性的delete
    delete req.session['xxx']			//删除的是服务器上的node上的
})           
```





​	