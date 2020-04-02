# 一、Git

## 什么是Git?
- Git是一款源代码管理工具(**版本控制工具**)
    - 我们写的代码需要使用Git进行管理。
- 源代码有必要管理起吗？
    - 1.0
    - 2.0 .....
    - 版本管理工具：svn,vss,vcs.... git
    - 有必要，因为人工的去处理不同的版本，做相应备份会很麻烦。
    - Git是linux之父当年为了维护linux---linus之前也是手动维护合并把文件发给Linus
    - linus自己写了一个版本管理的工具(Git)

##Git安装

https://git-scm.com/download/win

下载后，任意目录鼠标右键可以看到多了两项Git GUI Here和Git Bash Here。分别是Git的图形化操作界面和敲命令的操作界面。在Git Bash Here中输入命令完成我们想要的功能。

## 一、初始化Git仓储/(仓库)

在项目的目录右键打开Git Bash Here。命令行窗口有以下代码。前面是用户，后面是命令行窗口对应的路径

```git
CH@LAPTOP-B1C17RAH MINGW64 /d/Tools/Projects/Node/Express/mybook--restful
$ 

```

新建一个文件夹专门让git帮我们管理代码。

输入`git init` ，回车会输出信息：`Initialized empty Git repository in D:/Tools/Projects/Node/Express/mybook--restful/.git/`。初始化一个空的git仓库在这个目录，这个.git是隐藏目录，(需要开启''显示隐藏目录''才可以看见)。git会将代码都备份到.git里面去。

- 在项目目录右键打开Git Bash Here
- 命令: `git init`
- 这个仓库会存放，git对我们项目代码进行备份的文件


比如当做完一个功能，开发第二功能希望将第一个功能备份，就不再是复制粘贴了。而是在项目当前的路径打开命令窗口。(注意路径必须匹配当前项目的，因为命令只会执行命令行后面对应的目录)

## 二、配置个人信息（自报家门）

合作开发时，别人也有可能往.git目录里放代码，别人放和我们放敲的命令 是一样的，为了区分是谁往.git里放代码。我们可以配置个人信息。方便日后看到代码是谁做的。

- 就是在git中设置当前使用的用户是谁
- 每一次备份都会把当前备份者的信息存储起来
- 命令: 
    - 配置用户名:`git config --global user.name "ch"`
    - 配置邮箱:  `git config --global user.email "xm@sina.com"`
    - config表示配置。 `--`开头表示一些参数，参数紧接着会有一个值，表示参数值。回车后继续弹出初始的用户名+路径的命令表示成功。git提供了user对象。


## 三、把代码存储到.git仓储中

把大象放到冰箱要几步？

1. 打开冰箱门
2. 放大象
3. 关上冰箱



-   1.把代码(不包含.git的**<u>工作区</u>**)放到仓储的门口(**<u>暂存区</u>**)
    + `git add ./要添加的文件名` 所指定的文件放到大门口

      ​	比如`git add ./reademe.md`

    + `git add ./`将当前目录**所有**修改过的重新放在仓库大门
-   2.把仓储门口的代码放到里面的房间中去（门口所有的文件全部放进仓库）
    + `git commit -m "这是对这次添加的东西的说明"`

      ​		( -m 表示消息message 说明。如果不加会进入另一种VIM编辑器模式，这里也可以写代码。按一下i键，下面变成insert就可以写东西，写的还是说明。遇到这里，按esc，输入英文版分号加一个q退出(quit)`:q`。如果红警告退出不了，后面加一个!强制退出`:q!` )

      ​

**可以一次性把我们A修改的代码放到房间里**       (不放在门口，直接从**工作区**放到**版本库**)

`git commit -a -m "一些说明"`            	-all 表示是把所有修改的文件提交到版本库

## 四、查看当前的状态

- 可以用来查看当前代码有没有被放到仓储中去
- 命令: `git status`
  - 出现modified :  文件名 	说明这个文件放到了暂存区
  - 红色说明未提交
  - 如果我第三个版本保存进仓储了。然后我在工作区修改第四个版本，这时查看`git status`会出现红色的`modified :  文件名 `，说明修改了但是未提交到版本库。如果删掉回原来第三个版本。再查看状态，就会出现'nothing is commit,working directory is clean'的提示。
    - 也就是说git会比较工作区与最终保存到仓库房间（版本区）的代码。如果一样就会认为工作区是干净的。

## 五、git中的忽略文件
-   .gitignore,在这个文件中可以设置要被忽略的文件或者目录。
-   被忽略的文件不会被提交仓储里去.
-   在.gitignore中可以书写要被忽略的文件的路径，以/开头，
              一行写一个路径，这些路径所对应的文件都会被忽略，
              不会被提交到仓储中
    + 写法
        * ` /.idea  ` 会忽略.idea文件
        * ` /js`      会忽略js目录里的所有文件
        * ` /js/*.js` 会忽略js目录下所有js文件

`touch .gitignore`生成.gitignore文件

## 六、查看日志

查看提交的记录，提交了多少次

- `git log` 查看历史提交的日志

  最下面的第一次

  ```javascript
  //commit后每次随机生成的值唯一标识提交
  commit 6b1a0ca486cf352c574b93ff2f12036cc2a46d5e (HEAD -> master)
  Author: “ch 819731316@qq.com
  Date:   Wed Feb 12 17:16:36 2020 +0800
  第一个功能

  commit cb00c03529d857c277d8df4118b9feb019f670b9 (HEAD -> master)
  Author: “ch 819731316@qq.com
  Date:   Wed Feb 12 14:58:25 2020 +0800
  测试添加
  ```

- `git log --oneline` 可以看到简洁版的日志

  ```javascript
  6b1a0ca (HEAD -> master) 第一个功能
  cb00c03 “测试添加
  ```

## 七、回退到指定的版本

通过git log查看到了提交的日志。可以通过以下命令回退到指定版本。

-   `git reset --hard Head~0`
    + 表示回退到上一次代码提交时的状态   
      + --hard表示将代码拿来并覆盖工作区已有的代码
      + Head正常情况下指向最新的版本，~0回退一次，~1回退两次

-   `git reset --hard Head~1`
    + 表示回退到上上次代码提交时的状态
      + 此时`git log --oneline`只会看到当前head之前的版本，但是只是没显示并没有消失

-   `git reset --hard [版本号]`
    + 可以通过版本号精确的回退到某一次提交时的状态

      比如`git reset --hard cb00c03`

-   `git reflog`

    -   如果想回到head之后的，可以通过此命令看所有的然后切换。
    -   可以看到每一次切换版本的记录:可以看到所有提交的版本号

## 八、分支

当开发一个功能，量很大一时半会写不完，开发到一半。希望做个备份，可是功能没完成，提交到主分支其他人拿到运行不了。

- 默认是有一个主分支master

### 1.创建分支
- `git branch 分支名字`
    + 例如`git branch dev`创建了一个dev分支
      + 查看分支，`git branch`回车，就可以看到dev分支和master分支。master是绿色而且前面有个*，代表当前是master分支
    + 在刚创建时dev分支里的东西和master分支里的东西是一样的

删除分支：`git branch -d 分支名字`  -d就是delete

### 2.切换分支

-   `git checkout dev`
    + 切换到指定的分支,这里的切换到名为dev的分支

      然后现在提交就提交到了dev的分支

      `git branch` 可以查看当前有哪些分支


别人只能拿到之前仓库的(master)，而创建的分支(dev)只有我自己可以访问。当我们在分支写了新的功能，要让其他人也能拿到。就将两个合并.

### 3.合并分支

-   `git merge dev`
    + 合并分支内容,把当前分支与指定的分支(dev),进行 合并
    + 当前分支指的是`git branch`命令输出的前面有*号的分支

-   合并时如果有冲突，需要手动去处理，处理后还需要再提交一次。

    -   比如我在master和dev的分支都做了改动。合并就会冲突。打开后是这样。

        ```javascript
        功能一
        功能二
        功能三
        <<<<<<< Head
        在master分支提交的
        ======
        在dev分支的
        >>>>>>> dev
        ```

        手动处理后，还需要再提交一次

    ​

注意：不要点进.git和修改里面的文件，是看不到代码的，git已经将其转为二进制形式的文件。不过存储在本地仓库，仅仅是针对个人的。团队的共同开发，代码共享。  将代码上传到服务器，其他人使用时从服务器下载即可。而且下载也是通过命令下载很方便。这个服务器就是GitHub。

## 八、GitHub 

- https://github.com
- 不是git,只是一个网站
- 只不过这个网站提供了允许别**通过git上传代码**的功能

### 提交代码到github(当作git服务器来用)

github创建仓库。右上角加号"+"，然后 New repository。输入文件名、描述。公开的仓库是免费的，而私有的就要收费。另外一般来说，readme.md自己就写了上传，下面的初始化readme文件不用勾上。 

创建后得到页面，页面上方点击HTTPS会在右侧生成链接。将链接复制就是我们上传的地址

### 1.上传到github

- `git push [地址] 分支名`。会把当前分支的内容**上传**到远程的分支上

  - github服务器上也有.git，里面有分支。

    第一次使用会要求输入用户名和密码，是github的用户名和密码。然后刷新页面就可以看到上传的。

  - 示例: `git push https://github.com/huoqishi/test112.git master `

    将本体的master分支上传到github服务器上的master分支。


### 2.从github下载

最简单的可以在链接中，点击页面的绿色按钮Clone or download，然后直接点Download Zip。

或者点击Clone or download后有个链接，复制链接。 

- git pull [地址] master`
  - 从远程的master中拿到本地。而且能获取到每次提交的代码的版本，通过`git log --oneline`可查看
  - 会把远程分支的数据得到     (注意：本地要初始一个仓储!)
  - 示例: `git pull https://github.com/huoqishi/test112.git master`


- `git clone [地址]`和pull不一样，会新建一个文件名，是仓库名。开发中一般都是用pull，clone一般会在第一次使用。
  - 会得到远程仓储相同的数据,如果多次执行会覆盖本地内容。而pull会做合并的处理。

# 二、流行框架

在提交时，通过地址将代码push上去，但是需要输入用户名和密码。也就是其他人要上传必要要把密码给他。不安全。

在点击Clone or download弹出的小窗口右上角有一个Use HTPPS，点击可以切换到Use SSSH。两种不同的上传方式。 

## 一、ssh方式上传代码

不需要输入用户名和密码就可以身份验证。git有个命令可以生成两个文件。

- ssh概念:公钥、私钥，两者之间是有关联的。

- 生成公钥,和私钥

  1. 任何一个目录打开git bash here。输入`ssh-keygen -t rsa -C 邮箱 `回车，会要求指定存储的位置，然后全部直接回车。C盘找到自己的用户，点开会有.ssh文件夹，点开有文件。id_rsa和id_rsa.pub，前者是私钥，后者是公钥。

  2. 打开id_rsa.pub，复制不修改。回到github，点开页面最右上角加号"+"右边的下拉，点击''设置''，跳转后，左侧有SSH and GPG keys项。点击后，右上角有 New SSH key，点击后将内容粘贴到下方的key文本域，然后给它起个名。生成后就可以直接提交了。

  3. github上新建仓储后的页面，将HTTPS换成SSH，然后复制右边的地址。复制后回到git，然后push

     `git push 地址 master`。然后就上传上去了。

     - 示例`ssh-keygen -t rsa -C "xiaoming@sina.com"`

## 二、在push和pull操作时，先pull再push

- 先pull , 再push

  - 因为服务器版本和本地可能不一样，先pull拉下来在本地解决了，再把新的push上去

- 简写地址:

  - 简写方式，每次不用写长地址。地址像写变量一样。remote add表明添加远程地址。

    `git remote add 变量名 地址` ，这一步后，只要是在当前仓库(项目目录)下，push时地址都可以写变量名

- 当我们在push时，加上-u参数，那么在下一次push时，我们只需要写上`git push`就能上传我们的代码。

  ( 加上-u之后，git会把当前分支与远程的指定的分支进行关联。)

  第一次：`git push 变量名 -u master`

  第二次：`git push`

# 三、服务器上

vim 加文件名 打开文件（不能使用鼠标）

按i进入编辑模式 可以打字

按esc进入命令模式

然后 输入  :q

:wq是保存

:q是退出

:q!强制退出 不保存



# 四、gulp

[官网](http://www.gulpjs.com)
[中文网](http://www.gulpjs.com.cn)

- 前端自动化构建工具
- js: function(){//},代码压缩,混淆 : var name = 123,var x = 123
- css, 
- 合并: 1个js 1kb ,有10个js.

### 5个核心方法

- gulp.task('任务名',function(){}) // 创建任务。
- gulp.src('./*.css') 指定想要处理的文件
- gulp.dest() // 指定最终处理后的文件的存放路径
- gulp.watch() // 自动的监视文件的变化，然后执行相应任务。
- gulp.run('任务名')，直接执行相应的任务。

### 安装gulp

- 通过npm安装:`npm install gulp-cli -g`

### gulp使用

- 1.在当前项目中也要安装gulp: `npm install gulp --save`
- 2.还需要在当前项目中新建一个文件: gulpfile.js

```javascript
    var gulp =  require('gulp');

    // 创建任务
    // 第一个参数: 任务名
    // 第二个参数: 回调函数,当我们执行任务时就会执行这个函数
    gulp.task('test', function(){
      console.log(123)
})
```

- 3.执行任务: `gulp 任务名`
  - 示例: `gulp test`

### 对js进行压缩

- `npm install gulp-uglify --save`

### 对js进行合并操作

- `npm install gulp-concat --save`

```javascript
    gulp.task('script', function(){
  // 1.要匹配到要处理的文件
  // 指定指定的文件:参数是匹配的规则
  // 参数也可以是数组，数组中的元素就是匹配的规则
  gulp.src(['./app.js','./sign.js'])
  // concat 的参数是合并之后的文件名字
  .pipe(concat('index.js'))
  .pipe(uglify())
  // dest方法参数，指定输出文件的路径
  .pipe(gulp.dest('./dist'))
})
```

### 对css进行压缩操作

- `npm install gulp-cssnano --save`

```javascript
   // 新建一个任务，对css进行处理
gulp.task('style', function(){
  // 对项目中的2个css文件进行合并，压缩操作
  // 1.匹配到要处理的文件
  gulp.src(['./*.css'])
  // 2.合并文件
  .pipe(concat('index.css'))
  // 3.压缩操作
  .pipe(cssnano())
  // 4.输出到指定目录
  .pipe(gulp.dest('./dist'))
  })
```

### 对html进行压缩

- `npm install gulp-htmlmin --save`
- https://github.com/kangax/html-minifier

```javascript
    // 新建一个任务，对html进行压缩
gulp.task('html', function(){
 // 1.匹配到要处理的文件
 gulp.src(['./index.html'])
 // 2.压缩操作
 .pipe(htmlmin({collapseWhitespace:true}))
 // 3.指定输出目录
 .pipe(gulp.dest('./dist'))
})
```

### gulp.watch

- 监视文件的变化，然后执行相应的任务
- gulp.run, 直接执行指定的任务

```javascript
// gulp.watch 监视文件变化，执行相应任务
gulp.task('mywatch', function(){
	// 执行指定的任务
  	gulp.run('script')
  	// 1.监视js文件的变化，然后执行script任务
  	// 第一个参数：要监视的文件的规则
  	// 第二个参数：是要执行的任务
  	gulp.watch(['./app.js','sign.js'],['script'])
})
```

