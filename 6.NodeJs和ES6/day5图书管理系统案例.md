# 大纲-day05
## 一、图书管理系统整合数据库

之前是data.json文件存储数据。现在我们把数据存储在数据库中。首先将db.js复制到mybook包。同时安装mysql包。

service.js中导入db.js。将方法中的data修改。**总结就是将数据替换**

另外注意，插入方法修改后，由于连接的是数据库，而我们之前写的是desc。修改之前的addBook.art、editBook.art、index.art的表单提交和数据填充里的desc都得改为数据库中的description。否则不匹配报错。



另外这里之所以可以在另一个页面渲染数据。是使用的表单提交配合res.render('模板'，数据)。然后跳转渲染的页面通过模板的语法填充。

```javascript
/*
    业务模块
*/
const data = require('./data.json');
const path = require('path');
const fs = require('fs');
const db = require('./db.js');


// 渲染主页面
exports.showIndex = (req,res) => {
    let sql = 'select * from book';
    db.base(sql,null,(result)=>{
        res.render('index',{list : result});	//方法放在内部。因为数据库获取的数据只能通过回调函数来得到
    });
}
// 跳转到添加图书的页面
exports.toAddBook = (req,res) => {
    res.render('addBook',{});
}
// 添加图书保存数据
exports.addBook = (req,res) => {
    // 获取表单数据
    let info = req.body;
    let book = {};
    for(let key in info){
        book[key] = info[key];
    }
    let sql = 'insert into book set ?';
  	//插入的data就是上面遍历的book赋值得到的
    db.base(sql,book,(result)=>{
        if(result.affectedRows == 1){
            res.redirect('/');
        }
    });
}
// 跳转编辑图书页面
exports.toEditBook = (req,res) => {
    let id = req.query.id;
    let sql = 'select * from book where id=?';
    let data = [id];
    db.base(sql,data,(result)=>{
      	//result是个数组，得用result[0]
        res.render('editBook',result[0]);
    });
}
// 编辑图书更新数据
exports.editBook = (req,res) => {
  	//info是提交过来的数据，通过info进行填充
    let info = req.body;
    let sql = 'update book set name=?,author=?,category=?,description=? where id=?';
    let data = [info.name,info.author,info.category,info.description,info.id];
    db.base(sql,data,(result)=>{
        if(result.affectedRows == 1){
            res.redirect('/');
        }
    });
}
// 删除图书信息
exports.deleteBook = (req,res) => {
    let id = req.query.id;
    let sql = 'delete from book where id=?';
    let data = [id];
    db.base(sql,data,(result)=>{
        if(result.affectedRows == 1){
            res.redirect('/');
        }
    });
}
```

`$ node .`启动后，刷新会发现数据连的是数据库的。而不是data.json

(有一点要注意，这个项目所有的页面都是后台生成的，是后端渲染。也就是说浏览器发送请求，得到的内容就是一个完整的页面。views文件夹下的模板都是在后台进行解析和填充。里面的循环结构都运行在服务器端。)

这里的添加和编辑不是同一个表单。添加无hidden的id隐藏域。而编辑有。

## 二、后台接口开发

以上这种开发模式就是后端渲染

另一种方式：**基于后台接口的前端渲染**

这两种在项目中很多是结合使用的，也有只用后台接口，将数据拿到后所有工作前端进行的前后端分离的开发模式。

主流json接口，跨域jsonp接口。还有一种restful接口

### 1.json接口

对于json接口，express也提供了比较方便的API。官方文档的路由下的方法列表中可以看到。res.json。

`res.json() 发送一个JSON格式的响应。`

1.新建一个文件夹API(API：应用程序间的接口)。里面只提供数据，不提供完整的页面。

2.新建入口文件index.js·`$ npm init -y`。然后安装epress和数据库`$ npm install express mysqljs/mysql --save `

3.将之前封装的模块db.js拷过来。然后就可以开发一个接口形式的模式

index.js

```javascript
/*
    后台接口开发: json接口
*/
const express = require('express');
const db = require('./db.js');
const app = express();

// 指定api路径(绑定路由) allBooks           
app.get('/allBooks',(req,res)=>{
    let sql = 'select * from book';
  	//这里的result是个数组
    db.base(sql,null,(result)=>{
        res.json(result);
    });
});

app.listen(3000,()=>{
    console.log('running...');
});
```

直接`$ node index.js` 然后浏览器输入localhost:3000/allBooks 就可以看到后台返回的数据。

(可以整个拷到编辑器中 快捷`ctrl+shift+p`输入`set js `设置文件类型，然后右键JsFormat(格式化，得下载插件)。就可以看到清晰的格式化好的标准格式的json数据。)

### 2.jsonp接口

`res.jsonp() 发送一个支持JSONP的JSON格式的响应`

jsonp接口需要传递一个回调函数的键(默认就是callback)。注意这个不是回调函数的名称，而是传递回调函数的参数的键叫callback。

比如`callback=foo` 。等号前面的键callback，等号后面的回调函数的名称foo

```javascript
/*
    后台接口开发:jsonp接口
*/
const express = require('express');
const db = require('./db.js');
const app = express();

//如果修改默认名称，不叫callback。才用到下面这行。
// 修改json回调函数传递参数的key
app.set('jsonp callback name', 'cb');		
//注意修改默认名称后，如果传参时不是cb比如还是callback不报错而是得到json形式的数据

// 指定api路径 allBooks 
app.get('/allBooks',(req,res)=>{
    let sql = 'select * from book';
    db.base(sql,null,(result)=>{
        res.jsonp(result);
    });
});

app.listen(3000,()=>{
    console.log('running...');
});
```

调用时要传递一个参数。浏览器输入localhost:3000/allBooks?callback=foo，得到的前面多了一步。

/**/ typeof foo == 'function' && foo()

后面的是函数调用，json形式的数据作为参数传递给了回调函数。foo()括号中就是json形式的参数。

所以关于json和jsonp接口从后台来说，开发起来差距不大。仅仅是改变了API。jsonp解决跨域问题。

### 3.restful接口

这种形式的接口不是像json接口、jsonp接口从响应的数据格式来表述的，而是从url的风格来表述的。

```javascript
/*
    restful api  是从URL的格式来表述的  
    可以与传统的对比。等效
    get     http://localhost:3000/books
    //添加书
    get     http://localhost:3000/books/book		
    post    http://localhost:3000/books/book		路径一样，但是提交的方式不同
    //编辑
    get     http://localhost:3000/books/book/1		编辑哪本书的id
    //编辑图书后的提交(提交表单)
    put     http://localhost:3000/books/book		put就是http协议更新数据的，但是路径和上面添加完全相同
    //删除
    delete  http://localhost:3000/books/book/2
	规则：提倡一个url对应一个资源。每个资源与请求方式有关系。每个请求方式会对资源产生影响。请求方式+路径就用来共同	表示一个特定的资源。路径参数book复数表示获取到多个，单数表示操作单个的。传参也是通过斜杠而不是问号。
	
    传统的URL风格
    http://localhost:3000/
    //去添加(跳转到添加图书的页面)
    get		http://localhost:3000/toAddBook				
    //添加图书(提交表单)
    post	http://localhost:3000/addBook
    // 跳转到编辑图书信息页面
   		http://localhost:3000/toEditBook?id=1
    //编辑图书提交表单(编辑图书信息页面点击提交后需要的新的路由)
    	http://localhost:3000/editBook
    //删除
    		http://localhost:3000/deleteBook?id=2
*/
const express = require('express');
const db = require('./db.js');
const app = express();
                         
app.get('/books',(req,res)=>{
    let sql = 'select * from book';
    db.base(sql,null,(result)=>{
        res.json(result);		//restful返回的很自由。json和jsonp都可以使用
    });
});
                              
//请求路径 http://localhost:3000/books/book/1     :id传参
app.get('/books/book/:id',(req,res)=>{
  	//获取id。 官方文档API下的Request中可以看到这个api。url参数如果是name那么这里就是req.params.name
    let id = req.params.id;		
    let sql = 'select * from book where id=?';
    let data = [id];
    db.base(sql,data,(result)=>{
        res.json(result[0]);
    });
});

app.listen(3000,()=>{
    console.log('running...');
});
```

以下是restful接口的案例。如果使用前两个接口的化，没有太大区别，主要是路径url的对应修改。

## 三、基于Ajax与后台接口的前端渲染案例

- 前端渲染与后端渲染分析

基于以上，我们来做使用restful接口的前端渲染的图书管理系统。使用后台接口来做。

**前端渲染**：后台只提供数据，所有页面相关的都放在前端来做。

##图书管理系统

### 1.后台接口

新建包mybook-restful，新建入口文件。`$ npm init -y`生成package.json文件。然后安装express、body-parser和mysql`$ npm install express body-parser mysqljs/mysql --save`，这里不需要安装模板引擎。因为后台只提供数据，不处理页面。

#### 1.入口文件index.js

```javascript
/*
    实现图书管理系统后台接口
*/
const express = require('express');
const bodyParser = require('body-parser');
const router = require('./router.js');		//导入单独隔离的路由模块
const app = express();

// 启动静态资源服务
app.use('/www',express.static('public'));
// 挂载参数处理中间件（post提交的数据处理）
app.use(bodyParser.urlencoded({ extended: false }));

// (设置)配置路由
app.use(router);

//监听端口
app.listen(3000,()=>{
    console.log('running...');
});
```

#### 2.路由模块router.js

```javascript
/*
    get     http://localhost:3000/books
    get     http://localhost:3000/books/book
    post    http://localhost:3000/books/book
    get     http://localhost:3000/books/book/1
    put     http://localhost:3000/books/book
    delete  http://localhost:3000/books/book/2
*/
const express = require('express');
const service = require('./service.js');
const router = express.Router();

// 提供所有的图书信息
router.get('/books',service.allBooks);
// 添加图书信息时提交数据
router.post('/books/book',service.addBook);
// 编辑图书时根据id查询相应信息
router.get('/books/book/:id',service.getBookById);
// 提交编辑的数据(修改数据)
router.put('/books/book',service.editBook);
// 删除图书信息
router.delete('/books/book/:id',service.deleteBook);


module.exports = router;
```

#### 3.业务模块service.js

```javascript
const db = require('./db.js');
//天气接口
//const weather = require('./api.js');

exports.allBooks = (req,res) => {
    let sql = 'select * from book';
    db.base(sql,null,(result)=>{
      	//后端渲染这里是res.render('index',{list : result}); 而前端渲染的后台接口只需要返回数据就行了
        res.json(result);
    });
};

exports.addBook = (req,res) => {
    let info = req.body;
    let sql = 'insert into book set ?';
    db.base(sql,info,(result)=>{
        if(result.affectedRows == 1){
            res.json({flag : 1});			//反回一个标记 falg=1成功
        }else{
            res.json({flag : 2});
        }  
    });
};

// 编辑图书时根据id查询相应信息
exports.getBookById = (req,res) => {
    let id = req.params.id;
    let sql = 'select * from book where id=?';
    let data = [id];
    db.base(sql,data,(result)=>{
        res.json(result[0]);
    });
};

// 提交编辑的数据(修改数据)
exports.editBook = (req,res) => {
    let info = req.body;
    let sql = 'update book set name=?,author=?,category=?,description=? where id=?';
    let data = [info.name,info.author,info.category,info.description,info.id];
    db.base(sql,data,(result)=>{
        if(result.affectedRows == 1){
            res.json({flag : 1});
        }else{
            res.json({flag : 2});
        }  
    });
};

exports.deleteBook = (req,res) => {
    let id = req.params.id;
    let sql = 'delete from book where id=?';
    let data = [id];
    db.base(sql,data,(result)=>{
        if(result.affectedRows == 1){
            res.json({flag : 1});
        }else{
            res.json({flag : 2});
        } 
    });
};
```

#### 4.操作数据库db.js

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

    // 操作数据库(数据库操作也是异步的)
    connection.query(sql,data, function(error, results, fields) {
        if (error) throw error;
        callback(results);
    });
    // 关闭数据库
    connection.end();
}
```

以上。所有后台接口完成。提供详细的api，前端就可以通过接口来开发。

没有表单与页面，我们可以通过postman测试。

### 2.前端渲染

基于接口实现前端的渲染工作

先准备静态资源目录public。在index.js中启用静态服务，监控路径让他成为静态资源`app.use('/www',express.static('public'));`      (上方后台接口已补充)

在public下新建子目录。css、js、img。

还需要一些文件。做这里的前端的页面渲染操作，要使用jquery并且借助模板引擎，后台用的art-template也可以用到浏览器端。将这些拷到public下的js目录下。（dialog.js弹窗用）

#### 1.获取数据列表

1.在public根目录下创建index.html。将以前的index.art拷到index.html。导入依赖的库文件jquery、模板、弹窗

2.与之前不同，我们将tbody标签中的模板提取写在head标签`<script type="text/template" id="indexTpl"></script>`中中(起个id)，再给tbody标签起个id。我们可以通过模板的id获取模板，模板渲染后就通过tbody的id获取到元素填充到tbody位置。

3.js目录下新建一个index.js。用来请求后台的接口数据，拿到后做页面的渲染工作。

```javascript
$(function(){
  	function initList(){
		//调用$.ajax()发送ajax请求数据
		$.ajax({
			type : 'get',
			url : '/books',
			dataType : 'json',
			success : function(data){
				// 渲染数据列表。直接调用模板引擎的api 
				// template(模板的id，数据)      注意数据要起个名，这个list和模板的list一致
				var html = template('indexTpl',{list : data});
				//得到数据填充到对应的位置(选择器选的tbody)
				$('#dataList').html(html);
			}
		})
    }
  	initList();
})
```

index.html头部引入index.js

`$ node .`启动。浏览器输入localhost:3000/www/index.html。获取数据列表完成。数据是数据库中的内容。

####2.添加

还是采用后台接口，前端渲染的方式。所以没必要进行页面的跳转。我们可以借助弹窗的方式来实现功能。借助引入的原生js写的库文件dialog.js实现弹窗。使用只需要传递四个参数

- cWidth : 弹窗宽度
- cHeight : 弹窗高度
- title : 弹窗标题
- info : 弹窗内容（原生DOM对象）

给添加图书的按钮绑定一个事件。把默认的跳转清掉，并加一个id方便后面绑定事件。

```javascript
 <div class="title">图书管理系统<a id="addBookId" href="javascript:;">添加图书</a>
```

另外在最后添加一个表单。这个是弹窗方法的第四个参数要用到的dom对象。给一个id便于获取。

```javascript
<form class="form" id="addBookForm">
	名称：<input type="text" name="name"><br>
	作者：<input type="text" name="author"><br>
	分类：<input type="text" name="category"><br>
	描述：<input type="text" name="description"><br>
	<input type="button" value="提交">
</form>
```

注意：提交按钮的type必须是button，如果是subit，会点击就提交表单。是表单提交。

是用ajax的方式提交。另外，添加样式。要注意一个就是要隐藏起来`display:  none;`弹窗时再显示。

然后在index.js处理逻辑。

```javascript
// 添加图书信息
//添加方法封装起来。只在页面初始化的执行一次，编辑后无法在绑定。所以封装起来，方便调用
function addBook(){
	$('#addBookId').click(function(){
      	//注意这里的form是jquery对象。所以要用form.get(0)或者form[0]的方式获取原生dom对象（两个对象的互相转换）
		var form = $('#addBookForm');
		// 实例化弹窗对象（dialog.js的方法） 第四个参数是dom对象。操作的表单，需要一个表单
		var mark = new MarkBox(600,400,'添加图书',form.get(0));
      	//显示弹窗
		mark.init();
      	
      	//找到提交按钮绑定事件
      	//unbind是因为修改时用到表单绑定事件重复先解绑再绑定。但是编辑完再添加也出现重复。所以这里也先解绑再绑定
		form.find('input[type=button]').unbind('click').click(function(){、
        	//$.ajax()向后台提交数据
			$.ajax({
				type : 'post',
				url : '/books/book',
				data : form.serialize(),			//jquery提供的serialize()获取全部的表单数据
				dataType : 'json',
				success : function(data){
					if(data.flag == '1'){
						// 关闭弹窗
						mark.close();
						// 添加图书成功之后重新渲染数据列表 （上面的渲染函数）
						initList();
					}
				}
			});
		});
	});
}
```

在做这些时，不需要重启服务。因为后台接口没动。这些都是前端的工作，所以刷新页面就可以。以前我们都是做的后端操作，所以才要重启。

####3.修改

修改比较麻烦，分两步走。首先根据id把数据库的数据查取出来填充到表单中。然后修改完后再提交。

同样将修改的默认跳转清掉

```javascript
<td><a href="javascript:;">修改</a>|<a href="javascript:;">删除</a></td>
```

注意写代码回到获取列表的数据渲染那。因为操作列表中的标签必须数据列表渲染加载完成后才可以进行

```javascript
$(function(){
  	function initList(){
		//调用$.ajax()发送ajax请求
		$.ajax({
			type : 'get',
			url : '/books',
			dataType : 'json',
			success : function(data){
				// 渲染数据列表。直接调用模板引擎的api 
				// template(模板的id，数据)      注意数据要起个名，这个list和模板的list一致
				var html = template('indexTpl',{list : data});
				//得到数据填充到对应的位置(选择器选的tbody)
				$('#dataList').html(html);
              	 
				//从这开始。也不可以写在回调函数外，这样可能服务端的数据还没返回
				//在模板中，通过获取tbdoy，找到下面的tr，然后遍历。element是tr
				$('#dataList').find('tr').each(function(index,element){
					//element是原生dom对象，所以要转为jquery对象。第五个td是删除修改列
					var td = $(element).find('td:eq(5)');	
					//每行的第一个就是id，所以可以这样获取
					var id = $(element).find('td:eq(0)').text();
					// 绑定编辑图书的单击事件
					td.find('a:eq(0)').click(function(){
						//在外面封装方法。这里调用
						editBook(id);
					});
					// 绑定删除图书的单击事件
					td.find('a:eq(1)').click(function(){
						deleteBook(id);
					});

					// 绑定添加图书信息的单击事件(编辑完成渲染后，调用添加的，会将绑定添加表单的事件重新绑定)
					addBook();
					// 重置表单（编辑完成后，表单的数据还在。为了让添加时表单是空的，这里重置清空表单）
					var form = $('#addBookForm');
					form.get(0).reset();
					//上面的并不能将隐藏域的id清空。所以还需要下面一步
					form.find('input[type=hidden]').val('');
                });
              
			}
		})
    }
  	initList();
  	// 编辑图书信息
    function editBook(id){
        var form = $('#addBookForm');
        // 先根据数据id查询最新的数据
        $.ajax({
            type : 'get',
            url : '/books/book/' + id,
            dataType : 'json',
            success : function(data){
                //初始化弹窗(新的弹窗，然后放表单)
                var mark = new MarkBox(600,400,'编辑图书',form.get(0));
                mark.init();
                // 填充表单数据
                form.find('input[name=id]').val(data.id);
                form.find('input[name=name]').val(data.name);
                form.find('input[name=author]').val(data.author);
                form.find('input[name=category]').val(data.category);
                form.find('input[name=description]').val(data.description);
                // 对表单提交按钮重新绑定单击事件(
              	//注意在添加时已经为这个按钮绑定过了，绑定事件重复了。所以要先解绑再绑定
                form.find('input[type=button]').unbind('click').click(function(){
                    // 编辑完成数据之后重新提交表单
                    $.ajax({
                        type : 'put',
                        url : '/books/book',
                        data : form.serialize(),
                        dataType : 'json',
                        success : function(data){
                            if(data.flag == '1'){
                                // 隐藏弹窗
                                mark.close();
                                // 重新渲染数据列表
                                initList();
                            }
                        }
                    });
                });
            }
        });
    }
})
```

注意细节。事件要每次先解绑再绑定。其次，清空表单。

#### 4.删除

```javascript
// 删除图书信息
function deleteBook(id){
	$.ajax({
		type : 'delete',
		url : '/books/book/' + id,
		dataType : 'json',
		success : function(data){
			if(data.flag == '1'){
              	 // 删除图书信息之后重新渲染数据列表
				initList();
			}
		}
	});
}
```

这就是前端渲染。关于这两种方式，在很多项目中都是结合使用的，一般比较复杂的都是在后端进行渲染。渲染完后，对页面的一部分，可能是通过前端渲染的方式，后端提供接口数据。拿到数据后，前端做局部的渲染和解析工作。

下面是具体的代码。

#### 5.index.html

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图书管理系统</title>
    <link rel="stylesheet" type="text/css" href="/www/css/style.css">
    <script type="text/javascript" src="/www/js/jquery.js"></script>
    <script type="text/javascript" src="/www/js/template-web.js"></script>
    <script type="text/javascript" src="/www/js/dialog.js"></script>
    <script type="text/javascript" src="/www/js/index.js"></script>   //渲染文件
    //模板  起id以获取模板
    <script type="text/template" id="indexTpl">
        {{each list}}
        <tr>
            <td>{{$value.id}}</td>
            <td>{{$value.name}}</td>
            <td>{{$value.author}}</td>
            <td>{{$value.category}}</td>
            <td>{{$value.description}}</td>
            <td><a href="javascript:;">修改</a>|<a href="javascript:;">删除</a></td>
        </tr>
        {{/each}}
    </script>
</head>
<body>
    <div class="title">图书管理系统<a id="addBookId" href="javascript:;">添加图书</a> 
    </div>
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
        	//渲染后模板填充的位置
            <tbody id="dataList">
                
            </tbody>
        </table>
    </div>

    <form class="form" id="addBookForm">
        <input type="hidden" name="id">
        名称：<input type="text" name="name"><br>
        作者：<input type="text" name="author"><br>
        分类：<input type="text" name="category"><br>
        描述：<input type="text" name="description"><br>
        <input type="button" value="提交">
    </form>
</body>
</html>
```

#### 6.index.js

```javascript
$(function(){
    // 初始化数据列表
    function initList(){
      	//调用$.ajax()发送ajax请求
        $.ajax({
            type : 'get',
            url : '/books',
            dataType : 'json',
            success : function(data){
                // 渲染数据列表
              	//直接调用模板引擎的api  (模板的id，数据(注意数据要起个名，这个list和模板的list一致))
                var html = template('indexTpl',{list : data});
              	//得到数据填充到对应的位置
                $('#dataList').html(html);
                // 必须在渲染完成内容之后才可以操作DOM标签

                $('#dataList').find('tr').each(function(index,element){
                    var td = $(element).find('td:eq(5)');
                    var id = $(element).find('td:eq(0)').text();
                    // 绑定编辑图书的单击事件
                    td.find('a:eq(0)').click(function(){
                        editBook(id);
                    });
                    // 绑定删除图书的单击事件
                    td.find('a:eq(1)').click(function(){
                        deleteBook(id);
                    });

                    // 绑定添加图书信息的单击事件
                    addBook();
                    // 重置表单
                    var form = $('#addBookForm');
                    form.get(0).reset();
                    form.find('input[type=hidden]').val('');
                });
            }
        });
    }
    initList();
    // 删除图书信息
    function deleteBook(id){
        $.ajax({
            type : 'delete',
            url : '/books/book/' + id,
            dataType : 'json',
            success : function(data){
                // 删除图书信息之后重新渲染数据列表
                if(data.flag == '1'){
                    initList();
                }
            }
        });
    }

    // 编辑图书信息
    function editBook(id){
        var form = $('#addBookForm');
        // 先根据数据id查询最新的数据
        $.ajax({
            type : 'get',
            url : '/books/book/' + id,
            dataType : 'json',
            success : function(data){
                // 初始化弹窗
                var mark = new MarkBox(600,400,'编辑图书',form.get(0));
                mark.init();
                // 填充表单数据
                form.find('input[name=id]').val(data.id);
                form.find('input[name=name]').val(data.name);
                form.find('input[name=author]').val(data.author);
                form.find('input[name=category]').val(data.category);
                form.find('input[name=description]').val(data.description);
                // 对表单提交按钮重新绑定单击事件
                form.find('input[type=button]').unbind('click').click(function(){
                    // 编辑完成数据之后重新提交表单
                    $.ajax({
                        type : 'put',
                        url : '/books/book',
                        data : form.serialize(),
                        dataType : 'json',
                        success : function(data){
                            if(data.flag == '1'){
                                // 隐藏弹窗
                                mark.close();
                                // 重新渲染数据列表
                                initList();
                            }
                        }
                    });
                });
            }
        });
    }

    // 添加图书信息
    function addBook(){
        $('#addBookId').click(function(){
            var form = $('#addBookForm');
            // 实例化弹窗对象
            var mark = new MarkBox(600,400,'添加图书',form.get(0));
            mark.init();
            form.find('input[type=button]').unbind('click').click(function(){
                $.ajax({
                    type : 'post',
                    url : '/books/book',
                    data : form.serialize(),
                    dataType : 'json',
                    success : function(data){
                        if(data.flag == '1'){
                            // 关闭弹窗
                            mark.close();
                            // 添加图书成功之后重新渲染数据列表
                            initList();
                        }
                    }
                });
            });
        });
    }
});
```



#### 7.后续补充：

后面测试发现存在一个bug。因为一开始添加的表单不需要id。而我们编辑用到了id。两个公用一个表单的话，在添加时就会把id也加入插入语句。出问题。所以多加一个表单即可。对于

## 四、从服务器主动发送请求

我们之前是**客户端-服务器模式**，和**从服务器主动发送请求**的本质是有区别的。但是性质差不多。

之前我们是客户端主动发请求，服务器是被动的，接到请求后响应返回数据给客户端。服务器始终是被动的。

Nodejs也提供了另外的功能，让服务器主动去发请求，但是服务器一般发送的请求是**发送到另外一个服务器**。别的服务器提供服务，从而得到别的服务器的数据。

![23232](D:\用户Users\chenh\Desktop\笔记\Node.js\img\23232.PNG)

### 1.基于核心模块主动发送请求

从Nodejs官方文档下的http模块的最后可以找到一个API：`http.request(options[, callback])`。第一个参数options对象包括了一些属性，用来设置发送之前的配置信息。相关属性可以拼接一个完整的url。从而可以发送请求得到结果。比如host域名、protocol协议等等。详细可见官方文档。

链接：[http://nodejs.cn/api/http.html#http_http_request_options_callback](http://nodejs.cn/api/http.html#http_http_request_options_callback)

index.js

```javascript
/*
    从服务器主动发送请求
    http.request(options[, callback])
*/

const http = require('http');		//引入http核心模块
//这个示例将数据写入文件中。所以引入核心模块
const path = require('path');
const fs = require('fs');

let options = {
    hostname : 'www.baidu.com',
    port : 80
}

//调用。回调函数里面只有一个参数：respon对象
//http调用了request返回了一个对象叫requset对象。然后可以通过req.write()可以在发送请求时传递数据：向请求体中写入
let req = http.request(options,(res)=>{
    let info = '';
	
  //接到的数据是通过事件的方式来处理的(和之前文件处理一样)。因为数据量可能很大。
    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        fs.writeFile(path.join(__dirname,'baidu.html'),info,(err)=>{
            console.log('已经获取到百度主页的内容');
        });
    });
});

//req.write(data);		向请求体写入数据
req.end(); //请求结束
```

然后通过`$ node index.js`启动。

这个就是服务器主动向外发送请求得到内容。

### 2.从服务器主动发送请求调用后台接口

除此外，我们还可以请求后台接口，从而得到接口数据。

先将之前我们所写好的后台接口的服务启动。然后我们就可以访问图书管理相关的接口。

**1.请求调用查询数据接口**

创建一个新文件 	01.js

```javascript
/*
    从服务器主动发送请求调用接口-查询数据
    （将所有图书信息展示出来）
*/

const http = require('http');

//相关属性可以拼接一个完整的url。从而可以发送请求得到结果。
let options = {
    protocol : 'http:',
    hostname : 'localhost',
    port : 3000,
    path : '/books'
}

let req = http.request(options,(res)=>{
    let info = '';

    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        console.log(info);
    });
});

req.end();

```

再开一个cmd，执行`$ node 01.js` ，就得到了json格式的所有的列表数据。通过这种方式我们也可以测试后台的接口，和之前插件postman一样。

**2.请求调用添加数据接口**

添加数据时要指定请求头headers，约定提交的数据格式是什么。

02.js  

```javascript
/*
    从服务器主动发送请求调用接口-添加数据
*/

const http = require('http');
const querystring = require('querystring');			//引入核心模块将数据转为标准的参数格式

let options = {
    protocol : 'http:',
    hostname : 'localhost',
    port : 3000,
    path : '/books/book',
    method : 'post',			//默认请求是get请求。post的话要加这个参数
    headers : {
      	//表单数据格式。ajax中post提交时也要设置
        'Content-Type': 'application/x-www-form-urlencoded',
    }
}

let req = http.request(options,(res)=>{
    let info = '';

    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        console.log(info);
    });
});

//将数据转为标准的参数格式
let data = querystring.stringify({
    name : 'adafa',
    author : 'asdfa',
    category : 'afdasf',
    description : 'adsfa'
});


req.write(data);
req.end();
```

`$ node 02.js` 数据库就多了一条数据

**2.请求调用修改数据的接口**

(1)根据id去查询数据

03.js

```javascript
/*
    从服务器主动发送请求调用接口-根据id去查询数据
*/

const http = require('http');
const querystring = require('querystring');

let options = {
    protocol : 'http:',
    hostname : 'localhost',
    port : 3000,
    path : '/books/book/2',					//查询id为2的数据
    method : 'get'
}

let req = http.request(options,(res)=>{
    let info = '';

    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        console.log(info);
    });
});

req.end();

```

`node 03.js` ，终端cmd返回{flag : 1}。查询到

(2)编辑数据

```javascript
/*
    从服务器主动发送请求调用接口-编辑数据
*/

const http = require('http');
const querystring = require('querystring');

let options = {
    protocol : 'http:',
    hostname : 'localhost',
    port : 3000,
    path : '/books/book',
    method : 'put',
    headers : {
        'Content-Type': 'application/x-www-form-urlencoded',
    }
}

let req = http.request(options,(res)=>{
    let info = '';

    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        console.log(info);
    });
});

let data = querystring.stringify({
    id : 2		  				 //编辑时要提供id编辑哪条数据
    name : 'ddddddddd',
    author : 'sssssssss',
    category : 'eeeeeeeee',
    description : 'gggggggg'
});
req.write(data);
req.end();

```

`$ node 04.js`，终端cmd返回{flag : 1}。刷新数据库，已编辑修改。

**4.请求调用删除数据的接口**

05.js

```javascript
/*
    从服务器主动发送请求调用接口-删除数据
*/

const http = require('http');
const querystring = require('querystring');

let options = {
    protocol : 'http:',
    hostname : 'localhost',
    port : 3000,
    path : '/books/book/2',
    method : 'delete'
}

let req = http.request(options,(res)=>{
    let info = '';

    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        console.log(info);
    });
});

req.end();

```

`$ node 05.js `，终端cmd返回{flag : 1}。删除成功

这就是从服务器主动发送请求调用另一服务器接口。不过以上是服务器接口在一台设备上，真实场景，接口要部署到不同的服务器中。以上的脚本可以做接口调试工作。

## 五、 调用第三方接口

除此外，也可以调用一些第三方的接口。比如查询天气，调用别的服务器的开放的查询天气接口。

```javascript
//3个url都可以查询天气 末尾的数字是城市编码
http://www.weather.com.cn/data/sk/101010100.html
http://www.weather.com.cn/data/cityinfo/101010100.html
http://m.weather.com.cn/data/101010100.html


城市编码：
北京:101010100朝阳:101010300顺义:101010400怀柔:101010500通州:101010600昌平:101010700延庆:101010800丰台:101010900石景山:101011000大兴:101011100房山:101011200密云:101011300门头沟:101011400平谷:101011500八达岭:101011600佛爷顶:101011700汤河口:101011800密云上甸子:101011900斋堂:101012000霞云岭:101012100北京城区:101012200海淀:101010200天津:101030100宝坻:101030300东丽:101030400西青:101030500北辰:101030600蓟县:101031400汉沽:101030800静海:101030900津南:101031000塘沽:101031100大港:101031200武清:101030200宁河:101030700上海:101020100宝山:101020300嘉定:101020500南汇:101020600浦东:101021300青浦:101020800松江:101020900奉贤:101021000崇明:101021100徐家汇:101021200闵行:101020200金山:101020700石家庄:101090101张家口:101090301承德:101090402唐山:101090501秦皇岛:101091101沧州:101090701衡水:101090801邢台:101090901邯郸:101091001保定:101090201廊坊:101090601郑州:101180101新乡:101180301许昌:101180401平顶山:101180501信阳:101180601南阳:101180701开封:101180801洛阳:101180901商丘:101181001焦作:101181101鹤壁:101181201濮阳:101181301周口:101181401漯河:101181501驻马店:101181601三门峡:101181701济源:101181801安阳:101180201合肥:101220101芜湖:101220301淮南:101220401马鞍山:101220501安庆:101220601宿州:101220701阜阳:101220801亳州:101220901黄山:101221001滁州:101221101淮北:101221201铜陵:101221301宣城:101221401六安:101221501巢湖:101221601池州:101221701蚌埠:101220201杭州:101210101舟山:101211101湖州:101210201嘉兴:101210301金华:101210901绍兴:101210501台州:101210601温州:101210701丽水:101210801衢州:101211001宁波:101210401重庆:101040100合川:101040300南川:101040400江津:101040500万盛:101040600渝北:101040700北碚:101040800巴南:101040900长寿:101041000黔江:101041100万州天城:101041200万州龙宝:101041300涪陵:101041400开县:101041500城口:101041600云阳:101041700巫溪:101041800奉节:101041900巫山:101042000潼南:101042100垫江:101042200梁平:101042300忠县:101042400石柱:101042500大足:101042600荣昌:101042700铜梁:101042800璧山:101042900丰都:101043000武隆:101043100彭水:101043200綦江:101043300酉阳:101043400秀山:101043600沙坪坝:101043700永川:101040200福州:101230101泉州:101230501漳州:101230601龙岩:101230701晋江:101230509南平:101230901厦门:101230201宁德:101230301莆田:101230401三明:101230801兰州:101160101平凉:101160301庆阳:101160401武威:101160501金昌:101160601嘉峪关:101161401酒泉:101160801天水:101160901武都:101161001临夏:101161101合作:101161201白银:101161301定西:101160201张掖:101160701广州:101280101惠州:101280301梅州:101280401汕头:101280501深圳:101280601珠海:101280701佛山:101280800肇庆:101280901湛江:101281001江门:101281101河源:101281201清远:101281301云浮:101281401潮州:101281501东莞:101281601中山:101281701阳江:101281801揭阳:101281901茂名:101282001汕尾:101282101韶关:101280201南宁:101300101柳州:101300301来宾:101300401桂林:101300501梧州:101300601防城港:101301401贵港:101300801玉林:101300901百色:101301001钦州:101301101河池:101301201北海:101301301崇左:101300201贺州:101300701贵阳:101260101安顺:101260301都匀:101260401兴义:101260906铜仁:101260601毕节:101260701六盘水:101260801遵义:101260201凯里:101260501昆明:101290101红河:101290301文山:101290601玉溪:101290701楚雄:101290801普洱:101290901昭通:101291001临沧:101291101怒江:101291201香格里拉:101291301丽江:101291401德宏:101291501景洪:101291601大理:101290201曲靖:101290401保山:101290501呼和浩特:101080101乌海:101080301集宁:101080401通辽:101080501阿拉善左旗:101081201鄂尔多斯:101080701临河:101080801锡林浩特:101080901呼伦贝尔:101081000乌兰浩特:101081101包头:101080201赤峰:101080601南昌:101240101上饶:101240301抚州:101240401宜春:101240501鹰潭:101241101赣州:101240701景德镇:101240801萍乡:101240901新余:101241001九江:101240201吉安:101240601武汉:101200101黄冈:101200501荆州:101200801宜昌:101200901恩施:101201001十堰:101201101神农架:101201201随州:101201301荆门:101201401天门:101201501仙桃:101201601潜江:101201701襄樊:101200201鄂州:101200301孝感:101200401黄石:101200601咸宁:101200701成都:101270101自贡:101270301绵阳:101270401南充:101270501达州:101270601遂宁:101270701广安:101270801巴中:101270901泸州:101271001宜宾:101271101内江:101271201资阳:101271301乐山:101271401眉山:101271501凉山:101271601雅安:101271701甘孜:101271801阿坝:101271901德阳:101272001广元:101272101攀枝花:101270201银川:101170101中卫:101170501固原:101170401石嘴山:101170201吴忠:101170301西宁:101150101黄南:101150301海北:101150801果洛:101150501玉树:101150601海西:101150701海东:101150201海南:101150401济南:101120101潍坊:101120601临沂:101120901菏泽:101121001滨州:101121101东营:101121201威海:101121301枣庄:101121401日照:101121501莱芜:101121601聊城:101121701青岛:101120201淄博:101120301德州:101120401烟台:101120501济宁:101120701泰安:101120801西安:101110101延安:101110300榆林:101110401铜川:101111001商洛:101110601安康:101110701汉中:101110801宝鸡:101110901咸阳:101110200渭南:101110501太原:101100101临汾:101100701运城:101100801朔州:101100901忻州:101101001长治:101100501大同:101100201阳泉:101100301晋中:101100401晋城:101100601吕梁:101101100乌鲁木齐:101130101石河子:101130301昌吉:101130401吐鲁番:101130501库尔勒:101130601阿拉尔:101130701阿克苏:101130801喀什:101130901伊宁:101131001塔城:101131101哈密:101131201和田:101131301阿勒泰:101131401阿图什:101131501博乐:101131601克拉玛依:101130201拉萨:101140101山南:101140301阿里:101140701昌都:101140501那曲:101140601日喀则:101140201林芝:101140401台北县:101340101高雄:101340201台中:101340401海口:101310101三亚:101310201东方:101310202临高:101310203澄迈:101310204儋州:101310205昌江:101310206白沙:101310207琼中:101310208定安:101310209屯昌:101310210琼海:101310211文昌:101310212保亭:101310214万宁:101310215陵水:101310216西沙:101310217南沙岛:101310220乐东:101310221五指山:101310222琼山:101310102长沙:101250101株洲:101250301衡阳:101250401郴州:101250501常德:101250601益阳:101250700娄底:101250801邵阳:101250901岳阳:101251001张家界:101251101怀化:101251201黔阳:101251301永州:101251401吉首:101251501湘潭:101250201南京:101190101镇江:101190301苏州:101190401南通:101190501扬州:101190601宿迁:101191301徐州:101190801淮安:101190901连云港:101191001常州:101191101泰州:101191201无锡:101190201盐城:101190701哈尔滨:101050101牡丹江:101050301佳木斯:101050401绥化:101050501黑河:101050601双鸭山:101051301伊春:101050801大庆:101050901七台河:101051002鸡西:101051101鹤岗:101051201齐齐哈尔:101050201大兴安岭:101050701长春:101060101延吉:101060301四平:101060401白山:101060901白城:101060601辽源:101060701松原:101060801吉林:101060201通化:101060501沈阳:101070101鞍山:101070301抚顺:101070401本溪:101070501丹东:101070601葫芦岛:101071401营口:101070801阜新:101070901辽阳:101071001铁岭:101071101朝阳:101071201盘锦:101071301大连:101070201锦州:101070701 
```

比如http://www.weather.com.cn/data/sk/101010100.html，复制到浏览器。就会得到北京的天气。不过是乱码，可以打开控制台查看文件可以看到。

接下来就调用这个第三方接口。

新建一个独立文件。06.js

```javascript
/*
    调用第三方接口-获取天气信息
*/

const http = require('http');
const querystring = require('querystring');

let cityCode = '101010300';
let options = {
    protocol : 'http:',
    hostname : 'www.weather.com.cn',
    port : 80,
    path : '/data/sk/'+cityCode+'.html',
    method : 'get'
}

let req = http.request(options,(res)=>{
    let info = '';

    res.on('data',(chunk)=>{
        info += chunk;
    });
    res.on('end',()=>{
        console.log(info);
    });
});

req.end(); 
```

`$ node 06.js`就获取到了天气信息。

### 1.针对第三方接口进行二次封装

weather.js

```javascript
/*
    调用第三方接口-获取天气信息-封装通用的天气查询模块
*/

const http = require('http');
const querystring = require('querystring');
	
//nodeJs主要是异步的方式得到资源，所以借助回调函数来获取天气信息。而不能通过返回值
//对外提供API
exports.queryWeather = (cityCode,callback) => {
    let options = {
        protocol : 'http:',
        hostname : 'www.weather.com.cn',
        port : 80,
        path : '/data/sk/'+cityCode+'.html',
        method : 'get'
    }

    let req = http.request(options,(res)=>{
        let info = '';

        res.on('data',(chunk)=>{
            info += chunk;
        });
        res.on('end',()=>{
          	//info是字符串格式，转为对象更方便
            info = JSON.parse(info);
          	//调用回调函数
            callback(info);
        });
    });

    req.end();
}
```

07.js

```javascript
/*
    测试天气查询模块
*/
const weather = require('./weather.js');/**/

weather.queryWeather('101020100',(data)=>{
  	//console.log(data);
    console.log(data.weatherinfo.WD);
});
```

`$ node 07.js`，就获取到了对象，然后可以点出属性打印出具体的。

### 2.图书管理系统扩展天气查询功能

首先封装api。其实就是上面的weather.js

api.js

```javascript
/*
    调用第三方接口-获取天气信息-封装通用的天气查询模块
*/

const http = require('http');
const querystring = require('querystring');

exports.queryWeather = (cityCode,callback) => {
    let options = {
        protocol : 'http:',
        hostname : 'www.weather.com.cn',
        port : 80,
        path : '/data/sk/'+cityCode+'.html',
        method : 'get'
    }

    let req = http.request(options,(res)=>{
        let info = '';

        res.on('data',(chunk)=>{
            info += chunk;
        });
        res.on('end',()=>{
            info = JSON.parse(info);
            callback(info);
        });
    });

    req.end();
}
```

路由文件router.js下添加

```javascript
// 查询天气
router.get('/weather/:id',service.queryWeather);
```

业务模块service.js添加

```javascript
//开头引入封装好的模块
const weather = require('./api.js');



exports.queryWeather = (req,res) => {
    let cityCode = req.params.id;
    weather.queryWeather(cityCode,(data)=>{
        res.json({info : data.weatherinfo});
    });
}
```

模板index.html添加

```javascript
<script type="text/template" id="weatherTpl">
	<ul>
		<li>城市：{{city}}</li>
		<li>风向：{{WD}}</li>
		<li>级别：{{WS}}</li>
		<li>温度：{{temp}}</li>
		<li>时间：{{time}}</li>
</ul>
</script>


//下拉框
<select>
        <option value="101010100">北京</option>
        <option value="101020100">上海</option>
        <option value="101280101">广州</option>
        <option value="101280601">深圳</option>
</select>
<input type="button" value="查询" id="weather">
```

index.js在function主体最后添加

```javascript
// 查询天气
$('#weather').click(function(){
	var cityCode = $('select option:selected').val();
  	$.ajax({
    	type : 'get',
    	url : '/weather/' + cityCode,
    	dataType : 'json',
    	success : function(data){
          	 // template(模板的id，数据)  
      		var html = template('weatherTpl',data.info);
      		var mark = new MarkBox(600,400,'天气信息',$(html).get(0));
      		mark.init();
    	}
  	});
});
```







 