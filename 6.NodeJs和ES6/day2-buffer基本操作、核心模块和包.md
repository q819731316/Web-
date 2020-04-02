# 大纲-day02
## 一、Buffer基本操作
> Buffer对象是Node**处理二进制数据的一个接口**。它是Node原生提供的全局对象，可以直接使用，不需要require(‘buffer’)。

Buffer本质上就是**字节数组**。API分为三类。

1. 构造方法（类）
2. 静态方法
3. 实例方法

-   **实例化**

    实例化buf对象

    -   Buffer.alloc(size)

        ```javascript
        // let buf = new Buffer(5); //<Buffer a8 61 40 00> 。
        // console.log(buf);	将二进制的数据用16进制的表示方式显示出来，两字符一个字节，共5个字节。内容是随机生成的
        //new的方法已经不推荐使用了，使用下面这种替代

        let buf = Buffer.alloc(5);    //只是实例化了一个buffer对象，并未初始化
        console.log(buf);     //<Buffer 00 00 00 00 00> 更合理，创建空的buffer时没必要初始化，全部置0就行了。
        ```

    -   Buffer.from(string)

        ```javascript
                let buf = Buffer.from('hello','utf8'); //传入字符串。
                //第二个参数编码处理，可无，默认是utf8。不同编码内容有区别，比如base64
                console.log(buf);    //<Buffer 68 65 6c 6c 6f>
        ```

    -   Buffer.from(array)

        ```javascript
                let buf = Buffer.from([0x62, 0x75, 0x66, 0x66, 0x65, 0x72]);   //传入8位数组
                console.log(buf.toString());     //buffer
        ```

-   **功能方法**

            静态方法。都是通过类名Buffer来调用，不可以通过实例对象来调用。


    -   Buffer.isEncoding() 判断是否支持该编码

        ```javascript
        console.log(Buffer.isEncoding('utf8'));
        console.log(Buffer.isEncoding('gbk'));
        ```
    
    -   Buffer.isBuffer() 判断是否为Buffer
    
        ```javascript
        let buf = Buffer.from('hello');
        console.log(Buffer.isBuffer(buf));   //true
        console.log(Buffer.isBuffer({}));	//false
        ```
    
    -   Buffer.byteLength() 返回指定编码的字节长度，默认utf8
    
        ​	buffer存的东西与编码有关。


        ```javascript
        let buf = Buffer.from('中国');
        console.log(Buffer.byteLength(buf));  	//6   utf8对汉字来说是3个字节一个字符
        ----------------------------------------
        let buf = Buffer.from('中国','ascii');
        console.log(Buffer.byteLength(buf));	//2   一个字节表示一个字符
    
        console.log(buf.toString());     //乱码 结果只有 -   所以汉字不允许使用ascii
        ```
    
    -   Buffer.concat() 将一组Buffer对象合并为一个Buffer对象
    
        ​	Buffer.concat(list[,totalLength])   		 list是个数组，第2参数可选：总长度


        ```javascript
            let buf1 = Buffer.alloc(3);
            let buf2 = Buffer.alloc(5);
    
            let buf3 = Buffer.concat([buf1,buf2]);
            console.log(Buffer.byteLength(buf3));      // 8
            ------------------------------------------------
            let buf1 = Buffer.from('tom');
            let buf2 = Buffer.from('jerry');
    
            let buf3 = Buffer.concat([buf1,buf2]);
            console.log(Buffer.byteLength(buf3));	//8
                             
            console.log(buf3.toString());      //tomjerry
        ```



-   **实例方法**
    + write() 向buffer对象中写入内容

      ```javascript
      buf.write(string,[,offset[,length]][,encoding])   
      //offset：从哪开始，默认0   length：写入多少字节，默认是对象的长度buf.length  encoding：编码，默认utf8

      let buf = Buffer.alloc(5);  	//准备一个buf对象
      buf.write('hello',2,2);  //从第2开始写，只能写入3个字符hel。  只写入2个， 最后一个是空 
      console.log(buf);		//<Buffer 00 00 68 65 00>
      ```

    + slice() 截取新的buffer对象

      ```javascript
      buf.slice(start[,end])   //省去第二个就是截取全部

      let buf = Buffer.from('hello');
      let buf1 = buf.slice(2,3);		//开始位置2  结束位置3
      console.log(buf === buf1);		//false    不同的buf对象
      console.log(buf1.toString());  	// l
      ```

    + toString() 把buf对象转成字符串

      ```javascript
      let buf = Buffer.from('hello');
      let buf1 = buf.slice(2,3);

      console.log(buf1.toString());
      ```

    + toJson() 把buf对象转成json形式的字符串

      ```javascript
      // toJSON方法不需要显式调用，当JSON.stringify方法调用的时候会自动调用toJSON方法
      //const buf = Buffer.from([0x1, 0x2, 0x3, 0x4, 0x5]);  		{"type":"Buffer", "data":[1,2,3,4,5]}
      const buf = Buffer.from('hello');
      const json = JSON.stringify(buf);       //自动调用toJSON方法 结果就是json形式的字符串
      console.log(json);		//字符串转为对应的十进制数组{"type":"Buffer", "data":[104,101,108,108,111]		
      ```

      ​

## 二、核心模块API

路径在不同的系统间是有差异的。比如斜杠/，在windows中是右斜杠，而在linux中

### 1.路径操作
- 路径基本操作API

  - path.basename()获取路径的最后一部分：文件的名称

    ```javascript
    // 引入核心模块，引入后，可以使用此模块的相关API
    const path = require('path');

    console.log(path.basename('/foo/bar/baz/asdf/quux.html'));   //quux.html      文件名+扩展名 
    //第二个参数可选 后缀: 去掉后缀
    console.log(path.basename('/foo/bar/baz/asdf/quux.html', '.html'));  //quux   
    //注意这种形式的路径都是linux中的路径写法。window中都是带盘符的
    ```

  - path.dirname()获取路径

    ```javascript
     //之前所学的属性  __dirname：当前文件的路径
    console.log(__dirname); 

    console.log(path.dirname('/abc/qqq/www/abc.txt'));	//  /abc/qqq/www    只有路径没有名称
    //没有扩展名也是一样的
    console.log(path.dirname('/abc/qqq/www/abc'));     //	/abc/qqq/www	
    ```

  - path.extname()获取扩展名称

    ```javascript
    console.log(path.extname('index.html'));       // .html
    //如果没扩展名，只有. 结果就是.    完全没有就是空
    ```

  - 路径的格式化处理path.format()和 path.parse() 

    ```javascript
    path.format() 将对象转为字符串	obj->string
    path.parse()  将字符串转为对象	string->obj

    let obj = path.parse(__filename);
    console.log(obj);
    /*结果是个对象
    { root: 'E:\\', 							   文件的根路径
      dir: 'E:\\node\\day02\\02-code',				文件的全路径
      base: '02.js',							   文件的名称
      ext: '.js',								   扩展名
      name: '02' 								   文件名称
    }*/

    //获取文件某部分就很简单，比如
    console.log(obj.base);
    ```
    path.format()则相反，先给个对象，再转为

    ```javascript
    let objpath = {
    	root : 'd:\\',
    	dir : 'd:\\qqq\\www',
    	base : 'abc.txt',
    	ext : '.txt',
    	name : 'abc'
    };
    let strpath = path.format(objpath);
    console.log(strpath);	// d:\qqq\www\abc.txt
    ```

  - path.isAbsolute()判断是否为绝对路径

    ​	windows中除了盘符开头为绝对路径，还有双斜杠`//`和四斜杠`\\\\`开始的也是绝对路径。不过用的不多。

    ​	linux中绝对路径就是`/`开始的

    ```javascript
    console.log(path.isAbsolute('/foo/bar'));
    console.log(path.isAbsolute('C:/foo/..'));
    ```

  - path.join()拼接路径

    ​	两个点'..'表示上一层路径，一个点'.'表示当前路径。在连接路径时会做一个格式化处理，没有写斜杠的会自动加上。

    ```JAVASCRIPT
    //上两层路径，所以后面两个路径没了(注意不是后两个参数，而是路径)
    console.log(path.join('/foo', 'bar', 'baz/asdf', 'quux', '../../'));  //结果： /foo/bar/baz
    ```

  - path.normalize()规范化路径

    ​把一个不太标准的路径规范化为标准的路径

  ```JAVASCRIPT
  console.log(path.normalize('/foo/bar//baz/asdf/quux/..'));   	//结果：    /foo/bar/baz/asdf
  console.log(path.normalize('C:\\temp\\\\foo\\bar\\..\\'));   	//结果：    C:\temp\foo
  ```

  - path.relative()计算相对路径

    ​path.relative(from,to)从第一个路径到第二个路径怎么走

  ```javascript
  linux：
  console.log(path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb'));
  //结果:	../../impl/bbb
  windows：
  console.log(path.relative('C:\\orandea\\test\\aaa', 'C:\\orandea\\impl\\bbb'));
  //结果:	..\..\impl\\bbb
  ```

  - path.resolve()解析路径

    就像cmd中的cd一样

  ```javascript
    先进入，再进入
    console.log(path.resolve('/foo/bar', './baz');			//结果：	 /foo/bar/baz
    console.log(path.resolve('/foo/bar', '/temp/file/');	//结果：	  /temp/file
    //如果不是根路径。以当前路径为出发点(文件位置)
    console.log(path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif'));
  ```

  -   两个特殊属性path.delimiter和path.sep	

      就像cmd中的cd一样

  ```javascript
    console.log(path.delimiter);	//结果： 	 \   表示路径分隔符（windows是\ Linux是/）
    console.log(path.sep);		    //结果：    ;  环境变量分隔符(windows中使用分号;  linux中使用冒号:)
  ```


### 2.文件操作

- 异步IO编程概念分析:

  > ​	异步I/O input/output
  >
  > Node中IO最常见的操作：
  >
  > 1. 文件操作
  > 2. 网络操作
  >
  > 在浏览器中也存在异步操作：
  >
  > 1. 定时任务
  > 2. 事件处理
  > 3. Ajax回调处理

> js的运行是单线程的，但是单线程一旦遇到一个耗时的任务。会导致UI的堵塞。为了解决问题，在单线程的基础上，引入事件队列机制。

​	Node.js中的事件模型与浏览器中的事件模型类似： 单线程+事件队列

​	能进入队列的任务						Node.js中异步执行的任务

​		1. 文件读写操作				  		1. 文件I/O  

​		 2. 网络请求和相应的处理               		2. 网络I/O 

> 注意：js的运行是单线程的。但是node的环境和浏览器的环境是多线程的。
>
> Node.js中,代码的风格主要是异步的。基于回调函数的编码风格。

- 文件信息获取

  - fs.stat(path,callback)   		参数一：访问文件路径		参数二：回调函数
  - fs.statSync(path)            带Sync结尾是同步的方法，不带的是异步的方法。

  Node.js虽然设计是异步的，但是也支持同步来做。

  ```javascript
  //引入核心模块
  const fs = require('fs');       //fs：file system

  //异步操作
  console.log(1);
  fs.stat('./abc',(err,stat) => {    //箭头函数形式
      // 一般回调函数的第一个参数是错误对象，如果err为null,表示没有错误，否则表示报错了
      if(err) return;			//如果err,return
    	//第2个参数是一个特殊的对象fs.stat，对象中有一些方法。具体看文档
      if(stat.isFile()){			    //判断是否为文件
           console.log('文件');
      }else if(stat.isDirectory()){	 //判断是否为目录
           console.log('目录');
       }
      console.log(stat);
      /*具体的相关属性
      atime：文件访问时间
      ctime：文件的状态信息发生变化的时间（比如文件的权限）
      mtime：文件数据发生变化的时间
      birthtime：文件创建的时间
      */
      console.log(2);
  });
  console.log(3);
  //结果：1  3  2    异步的任务会进入队列中，所以2最后

  // 同步操作  
  console.log(1);
  let ret = fs.statSync('./data.txt');
  console.log(ret);
  console.log(2);
  //结果：先1  ret对象 然后2
  ```

- 读文件操作

  - fs.readFile(file[,options],callback)
  - fs.readFileSync()(file[,options])

  ```javascript
  const fs = require('fs');
  const path = require('path');

  //异步操作
  let strpath = path.join(__dirname,'data.txt');       //拼接路径
  fs.readFile(strpath,(err,data)=>{		//data:文件读取到的内容
  	if(err) return;
     	console.log(data.toString());
  });

  // 如果有第二个参数并且是编码，那么回调函数获取到的数据就是字符串
  // 如果没有第二个参数，那么得到的就是Buffer实例对象，想看到就得调用toString方法
  fs.readFile(strpath,'utf8',(err,data)=>{
  	if(err) return;
       // console.log(data.toString());
      console.log(data);
  });

  // 同步操作
  let ret = fs.readFileSync(strpath,'utf8');
  console.log(ret);
  ```

- 写文件操作

  ```javascript
  const fs = require('fs');
  const path = require('path');

  //异步操作
  let strpath = path.join(__dirname,'data.txt');
  fs.writeFile(strpath,'hello nihao','utf8',(err)=>{		//路径，内容，编码，回调函数
  	if(!err){
  		console.log('文件写入成功');
  	}
  });

  let buf = Buffer.from('hi');
  fs.writeFile(strpath,buf,'utf8',(err)=>{
  	if(!err){
  		console.log('文件写入成功');
  	}
  });

  // 同步操作
  fs.writeFileSync(strpath,'tom and jerry');
  ```

  上面的读写操作只能处理内容量比较少的空间，因为readFile()和writeFile()这两个API会把所有文件内容加载到内存。当文件内容比较大，可能会导致内存消耗比较大，影响性能。

  所以为了解决大文件的处理，Node.JS提供了另一种操作：文件的流式操作

- 大文件操作(流式操作)

  -  fs.createReadStream(path[, options])
  -  fs.createWriteStream(path[, options])

  ```javascript
  const path = require('path');
  const fs = require('fs');

  //读取文件的数据流和写文件的数据流
  let spath = path.join(__dirname,'../03-source','file.zip');		//原路径
  let dpath = path.join('C:\\Users\\www\\Desktop','file.zip');	//目标路径

  let readStream = fs.createReadStream(spath);
  let writeStream = fs.createWriteStream(dpath);

  // 这里的处理方式是基于事件的处理方式
  /*点一次，事件触发一次。这就是所谓的基于事件的处理方式
  $('input[type=button]').on('click',function(){
  	console.log('hello');
  });*/

  //node.js中没有click事件，它有数据读取的事件/网络请求的事件。因为node.js中没有dom操作，所以相关的事件都没。
  //但是定义了另外的一些事件
  let num = 1;
  readStream.on('data',(chunk)=>{	   //readStream绑定的事件名称叫data。事件读取时发生，读取一部分，调用一次
  	num++;				// num监听完成多少次
  	writeStream.write(chunk);	 //写入文件
  });

  //另外一个事件，什么时候写完
  readStream.on('end',()=>{
  	console.log('文件处理完成' + num);     //文件处理完成2454
  });
  //文件的处理方便，开销小

  -----------------------------------
  //另一种语法结构更方便的处理大文件
  //pipe的作用直接把输入流和输出流黏到一块        输入：磁盘到内存     输出：内存到磁盘
  readStream.pipe(writeStream);      	//pipe管道  读的数据直接给写的入口
   
  ----------------------------------
  //以上整合后的更简洁一行代码
  fs.createReadStream(spath).pipe(fs.createWriteStream(dpath));
  ```

- 目录操作

  ```javascript
  /*
      目录操作
      1、创建目录
      	fs.mkdir(path[, mode], callback)
      	fs.mkdirSync(path[, mode])
      2、读取目录
      	fs.readdir(path[, options], callback)
      	fs.readdirSync(path[, options])
      3、删除目录
      	fs.rmdir(path, callback)
      	fs.rmdirSync(path)
  */

  const path = require('path');
  const fs = require('fs');
  // 创建目录：异步
  fs.mkdir(path.join(__dirname,'abc'),(err)=>{
  	console.log(err);		//结果：null              err = null   没有错误，创建abc目录(文件夹)成功
  });

  //同步
  fs.mkdirSync(path.join(__dirname,'hello'));
  --------------------------------------
  // 读取目录：异步
  fs.readdir(__dirname,(err,files)=>{
    	//console.log(files);   结果是一个数组：包括所有的目录名和文件名   
    	//如何区分是文件还是目录？
  	files.forEach((item,index)=>{					//遍历
  		fs.stat(path.join(__dirname,item),(err,stat)=>{		//通过stat对象判断是文件还是目录
              if(stat.isFile()){
                  console.log(item,'文件');
               }else if(stat.isDirectory()){
                   console.log(item,'目录');
               }
           });
       });
  });

  //同步，效果一样，但是同步性能较低
  let files = fs.readdirSync(__dirname);
  files.forEach((item,index)=>{
       fs.stat(path.join(__dirname,item),(err,stat)=>{
           if(stat.isFile()){
               console.log(item,'文件');
           }else if(stat.isDirectory()){
               console.log(item,'目录');
           }
       });
  });
  ------------------------------
  // 删除目录:异步
  fs.rmdir(path.join(__dirname,'abc'),(err)=>{
  	console.log(err);		//结果：null         err = null   没有错误，删除abc目录(文件夹)成功
  });

  //同步
  fs.rmdirSync(path.join(__dirname,'qqq'));
  ```

- 补充：修改文件名称，可更改文件的存放路径。

  ​

  ```javascript
  fs.rename(oldPath, newPath, [callback(err)]);			//同步
  fs.renameSync(oldPath, newPath);						//异步
  ```

  实例可以到day4 express中的文件上传下载查看

### 3.文件操作案例

 文件操作案例：初始化目录结构

```javascript
//引入核心模块
const path = require('path');
const fs = require('fs');

let root = 'C:\\Users\\www\\Desktop';         //斜杠转义：两斜杠
let fileContent = `
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <div>welcome to this</div>
</body>
</html>
`;

// 初始化数据
let initData = {
    projectName : 'mydemo',
    data : [{
        name : 'img',
        type : 'dir'
    },{
        name : 'css',
        type : 'dir'
    },{
        name : 'js',
        type : 'dir'
    },{
        name : 'index.html',
        type : 'file'
    }]
}

// 创建项目根路径
fs.mkdir(path.join(root,initData.projectName),(err)=>{
    if(err) return;
    // 创建子目录和文件
    initData.data.forEach((item)=>{
        if(item.type == 'dir'){      //这是目录
            // 创建子目录
            fs.mkdirSync(path.join(root,initData.projectName,item.name));      //拼接
        }else if(item.type == 'file'){		//这是文件
            // 创建文件并写入内容
            fs.writeFileSync(path.join(root,initData.projectName,item.name),fileContent);
        }
    });
});
```



 异步I/O input/output

Node中IO最常见的操作

1. 文件操作
2. 网络操作

在浏览器中也存在异步操作：

1. 定时任务
2. 事件处理
3. Ajax回调处理

## 三、包  
> 多个模块可以形成包，不过要满足特定的规则才能形成规范的包

### 1.NPM （node.js package management）

> 全球最大的模块生态系统，里面所有的模块都是开源免费的；也是Node.js的**包管理工具**。

- [官方网站](https://www.npmjs.com/ )

  npm是个工具，也是个网站平台，分享了很多开源的项目。

### 2.npm包安装方式

对包的操作主要就包括，安装、卸载和更新操作。

- 全局安装

  全局安装的包位于Node.js环境的node_modules目录下，全局安装的包一般用于**命令行工具**

  npm install -g 包名称 (全局安装)

- 本地安装

  本地安装的包在当前目录下的node_modules里面，本地安装的包一般用于**实际的开发工作**。

  本地安装的包一般用来开发某种具体的功能，所以本地写的代码也应该做成一个规范的包，方便代码的重用。

  包的入口文件名称默认index.js

  ```javascript
  /*
      包的入口文件
  */
  console.log(hello);
  ```

  生成package.json文件，这个文件描述了包的规则。

  ```javascript
  //cmd命令
  npm init 		有提示输入，一直下一步(默认)
  //默认生成快捷
  npm init -y

  //package.json例如，详细属性意思可以看下面package.json字段分析笔记
  {
    "name": "hello",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC",
    "dependencies": {},
    "devDependencies": {}
  }
  ```

  执行包的两种方式

  ```javascript
  npm .            //然后就输出hello，实际上node . 执行"main"指向的文件

  我们也可以修改"scripts"脚本的属性来执行
  "scripts": {
      "test11": "node index.js"
    },
  //调用test11命令
  npm run test11
  ```

  注意：入口文件中require引入的包的名称和node_modules文件夹下包文件夹名以及package.json中的name指定的名称是一样的。

  **添加依赖**

  > 另外还有npm命令的两个选项 --save 和 --save-dev 。		作用：添加依赖

  在包发布后，node_modules是不会一起发布的，因为里面包含代码量比较大，传递时不太方便。包提交时只会提交源代码。它的依赖只会以这种方式添加到package.json，在运行时，再随时去安装对应的包即可。所以要添加依赖。

  >`--save`：将保存至package.json（package.json是NodeJS项目配置文件）
  >
  >`-dev`：保存至package.json的devDependencies节点，不指定-dev将保存至依赖节点
  >
  >为什么要保存至package.json？因为节点插件包相对来说非常庞大，所以不加入版本管理，将配置信息写入package.json并将其加入版本管理，其他开发者对应下载即可（命令提示符执行npm install，则会根据package.json下载所有需要的包）

  另外，好像新版本不加--save也会保存到dependencies？

  ```javascript
  开发环境（平时开发使用的环境）
  生产环境（项目部署上线之后的服务器环境）

  --save 			向生产环境添加依赖 dependencies
  --save-dev 		向开发环境添加依赖 DevDependencies 

  //例如
  npm install 包名称 --save
  生成的package.json文件多了一个属性"dependencies" ：依赖
  "dependencies": {
    	"包名称": "相应的版本号"
  },
    
  npm install 包名称 --save-dev
  "devDependencies": {
    	"包名称": "相应的版本号"
  }
  ```

为什么分开添加依赖？

生产环境的包是程序运行必须的包。而开发环境的包一般只在开发中使用，布置上线后就没用了。所以分开处理可以选择性的安装。

如何安装依赖？

```javascript
npm install --production          //安装生产环境的包
npm install 					//安装生产环境和开发环境的包
```



### 3.解决npm安装包被墙的问题

- --registry
    + npm config set registry=https//registry.npm.taobao.org 
- cnpm
    + 淘宝NPM镜像,与官方NPM的同步频率目前为10分钟一次 
    + 官网: http://npm.taobao.org/ 
    + npm install -g cnpm –registry=https//registry.npm.taobao.org 
    + 使用cnpm安装包: cnpm install 包名
- nrm
    + 作用：修改镜像源 
    + 项目地址：https://www.npmjs.com/package/nrm 
    + 安装：npm install -g nrm

### 4.npm常用命令

本地则不用加-g

- 安装包（如果没有指定版本号，那么安装最新版本）

  ```javascript
  npm install -g 包名称 (全局安装)
  npm install 包名称 (本地安装)
  ```

  安装包的时候可以指定版本

  ```javascript
  npm install -g 包名称@版本号
  ```

- 更新包

  ```javascript
  npm update -g 包名
  ```

- 卸载包

  ```javascript
  npm uninstall -g 包名
  ```

### 5.yarn基本使用

作用与npm相似，用来管理包。因为npm的性能不太好，有些特定的场景比如更新没办法正常更新。为了弥补问题，有了yarn。

- 类比npm基本使用

  安装yarn工具：

  ```javascript
  npm install -g yarn
  ```

  1.初始化包

  ```javascript
  npm init
  yarn init
  ```
   2.安装包

  ```javascript
  npm install xxx --save
  yarn add xxx
  ```
  3.移除包

  ```javascript
  npm uninstall xxx
  yarn remove xxx
  ```
  4.更新包

  ```javascript
  npm update xxx
  yarn upgrade xxx
  ```
   5.安装开发依赖的包

  ```javascript
  npm install xxx --save-dev
  yarn add xxx --dev
  ```
  6.全局安装

  ```javascript
  npm install -g xxx
  yarn global add xxx
  ```
  7.设置下载镜像的地址

  ```javascript
  npm config set registry url
  yarn config set registry url
  ```
   8.安装所有依赖

  ```javascript
  npm install
  yarn install
  ```
  9. 执行包

  ```javascript
  npm run
  yarn run
  ```

## 四、自定义包

### 1.包的规范
- package.json必须在包的顶层目录下	(强制)
- 二进制文件应该在bin目录下
- JavaScript代码应该在lib目录下
- 文档应该在doc目录下
- 单元测试应该在test目录下

### 2.package.json字段分析
- name：包的名称，必须是唯一的，由小写英文字母、数字和下划线组成，不能包含空格
- description：包的简要说明
- version：符合语义化版本识别规范的版本字符串（两个点隔开的三个数字1.0.0）
- main：入口文件，对应的必须时入口文件的名称，比如index.js。如果叫其他名，这里对应也是其他名
- keywords：关键字数组，通常用于搜索
- maintainers：维护者数组，每个元素要包含name、email（可选）、web（可选）字段
- contributors：贡献者数组，格式与maintainers相同。包的作者应该是贡献者数组的第一- 个元素
- bugs：提交bug的地址，可以是网站或者电子邮件地址
- licenses：许可证数组，每个元素要包含type（许可证名称）和url（链接到许可证文本的- 地址）字段
- repositories：仓库托管地址数组，每个元素要包含type（仓库类型，如git）、url（仓- 库的地址）和path（相对于仓库的路径，可选）字段
- dependencies：生产环境包的依赖，一个关联数组，由包的名称和版本号组成
- devDependencies：开发环境包的依赖，一个关联数组，由包的名称和版本号组成

### 3.自定义包案例

通过下载的markdown-it包实现，将md文件内容转换为对应的html语言。将md文件转为页面

原markdown内容demo.html

```javascript
# 一级标题

## 二级标题

### 三级标题

- 列表信息
	+ 二级列表
		* 三级列表
```

 入口文件index.js

```javascript
/*
    入口文件
*/

//引入相关模块
const path = require('path');
const fs = require('fs');
const md = require('markdown-it')();      //下载的模块，require('markdown-it')返回一个函数，后面的括号是调用
										//md是一个对象，调用render方法将markdown内容转换
//模板路径
let tplPath = path.join(__dirname,'tpl.html');
//md路径
let mdPath = path.join(__dirname,'demo.md');
//目标文件路径
let targetPath = path.join(__dirname,'demo.html');

// 获取markdown文件
fs.readFile(mdPath,'utf8',(err,data)=>{
    if(err) return;
    // 对markdown内容进行转换操作
    let result = md.render(data);		//拿到数据传给模板
    // 读取模板内容
    let tpl = fs.readFile(tplPath,'utf8',(err,tplData)=>{
        if(err) return;
        tplData = tplData.replace('<%content%>',result);
        // 生成的最终页面写入目标文件
        fs.writeFile(targetPath,tplData,(err)=>{
            console.log('转换完成');
        });
    })
});
```
写入的模板页面

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>模板页面</title>
    <style type="text/css">
    h1 {
        color: blue;
    }
    </style>
</head>
<body>
    //替换的模板
    <%content%>
</body>
</html>
```

转换后生成的目标文件demo.html

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>模板页面</title>
    <style type="text/css">
    h1 {
        color: blue;
    }
    </style>
</head>
<body>
    <h1>一级标题</h1>
<h2>二级标题</h2>
<h3>三级标题</h3>
<ul>
<li>列表信息
<ul>
<li>二级列表
<ul>
<li>三级列表</li>
</ul>
</li>
</ul>
</li>
</ul>

</body>
</html>
```

