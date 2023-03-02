---
title: node
date: 2022-03-20 14:23:33
tags:
- node.js
categories:
- web服务端
---
#  三种主流的 node  程序

1. web应用程序： 提供单页面应用的简单程序， REST微服务以及全栈的web应用

2. 命令行工具： npm、 Gulp、 webpack

3. 后台程序： 后台服务、比如PM2  

   以及桌面程序：Electorn 框架写的软件

## module.exports 和 exports

CommonJS规范规定，每个模块内部，module变量代表当前模块。这个变量是一个对象，它的exports属性（即module.exports）是对外的接口。加载某个模块，其实是加载该模块的module.exports属性。

1. module.exports 级别高于 exports

2. 如果exports和module.exports 指向不同的内存地址时，应该以module.exports 的结果为准； 

   因为正常情况（下图）exports是modules.exports 都是本来都是指向相同内存地址； 这个内存地址指向同一个对象

   ```js
   const name = 'lili'
   exports.name = name;
   module.exports.name = name
   ```

   

   ![image-20210527220512887](C:\Users\Mloong\AppData\Roaming\Typora\typora-user-images\image-20210527220512887.png)

   如果你改变了 exports 的引用将他设置为另一个对象 或者将 module.exports 设置为另一个对象， 应该以module.exports的对象为准

   ```javascript
   const name = 'lili'
   exports = {
     name : name // 这准写法以module.exports 为准 
   };
   module.exports = {
     cname: name // 这里导出的是对象，里面只有cname
   
   }
   ```

   

   ![image-20210527221209462](C:\Users\Mloong\AppData\Roaming\Typora\typora-user-images\image-20210527221209462.png)

## 模块查找原则

模块既可以是一个文件，也可以是包含一个或多个文件的目录， 

如果模块是一个目录， node通常会在这个目录下找一个index.js 的文件作为文件的入口（***这个默认值也可重写， 在package.json 中修改main 自动***）

node 以同步的方法去找模块， 定位到模块并加载文件中的内容， node 查找文件的顺序是  

1. require(‘./find.js’) 和 require('./find')区别 ***这里是都有./ **

   require(‘./find.js’); 去当前路径找 find.js 文件

   require('./find') ：

   + 先去找这个路径的 find.js 文件
   + 如果没有找到find.js文件， 那么就找find 的文件夹
   + 如果找到了find的文件夹 ， 再找index.js；如果找到就执行，如果没有找到， 就去 find 文件夹下找package.json 文件中找main 选择的入口文件
   + 如果找到知道的入口文件不存在或者没有入口文件就会报错， 模块没有找到

2. requires('find') 区别  *** 注意这里是没有路径，也没有后缀名，只写了模块名字  ， 此时node.js 会当做这个 是系统模块**  

   1. （先找系统模块： require('fs')  require('http')， 如果没有
   2. 然后是当前目录， node_modules 文件夹中查找
   3. 首先查看是否有改名字的JS 文件
   4. 再看是否有改名字的文件夹， 改文件夹下里面是否有index.js， 如果没有继续
   5. 查找 package.json 文件 的 main 选项， 查看入口文件， 如果没有
   6. 查找父级文件（重复2-5）

   7. 看模块在当前目录下的node_modules 中吗  在返回 ；不在继续
   8. 尝试进入父目录 重复 4-6； 如果父目录不存在
   9. 模块在环境变量NODE_PATH 指定的目录下吗 在返回 ； 不在抛出异常

## ***require方法是同步方法 所以一般写在文件顶部***

## node会把模块作为对象缓存起来。 如果两个文件引入了相同的模块，第一个require会把模块返回的数据存在内存中， 这样第二个require就不在去访问和计算源文件了。

## 异步编程技术

  前端中异步， 鼠标点击的后回调 就是异步

  服务端异步， 事件发生时的响应， node 世界中流行两种响应： 回调和事件监听。

1. 回调 通常用来定义一次性事件响应的逻辑。 比如数据库查询的回调
2. 事件监听器 其本质也是一个回调， 不同的是 它跟一个实体事件相关联，比如 鼠标点击就是一个事件， 需要事件监听器 去监听点击事件，

***事件发射器***： 一个可以继承、能够添加事件发射及处理能力的类（EventEmitter；)

1. 用事件发射器处理重复性事件： HTTP 服务器， TCP服务器 和流，都做成了事件发射器

   ```
      const net = require('net');
      const server = net.createServer(socket => {
         socket.on('data', data => { // 这里使用了*** on ***进行重复性事件回调
         socket.write(data)
         })
      }).listen(8888) 
   ```

2. 响应一次性事件

```
   const net = require('net');
      const server = net.createServer(socket => {
         socket.once('data', data => { // 这里使用了***once*** 进行重复性事件回调
         socket.write(data)
         })
      }).listen(8888) 
```

## 事件发射器  一个可以继承、能够添加事件发射及处理能力的类（EventEmitter；)

1. 语法 

   + *** 首先引入events模块**
   + *** 通过new 调用 events模块中的EventEmitter静态方法去创建发射器实例对象**

   1. 实操

   ```
      //event.js 文件
   var EventEmitter = require('events').EventEmitter; 
   var event = new EventEmitter(); // 创建事件发射器的实例对象
   event.on('some_event', function() { // 通过 on 去订阅一个事件的逻辑
      console.log('some_event 事件触发'); 
   }); 
   setTimeout(function() {  // 通过emit 去触发这个事件
      event.emit('some_event'); 
   }, 1000); 
   ```



# fs 文件的系统模块

1. 文件的读取 readFile

   ```
   const fs = require('fs');
   fs.readFile(文件路径，[文件读取的编码]， callback)
   ```

2. 文件的写入

   ```
   const fs = require('fs');
   fs.writeFile(文件的路径， '数据'， callback)
   ```



# path 模块

1. 路径拼接api ： 在不同的操作系统中 系统路径的分隔符是不一样的

   windows 上是 /  或者 \

   linux 只有 / （linux系统通常被用作网站的服务器）

   ```js
   const path = require('path');
   let finialPath = path.join('路径1', '路径2', '路径3')
   // finialPath: '路径1\路径2\路径3'
   ```

2. 相对路径绝对路径

   相对路径  './a.js';

   绝对路径 __dirname 来获取 当前文件的绝对路径 （绝对路径使用较多）

   ```
   const path = require('path');let finialPath = path(__dirname, 'a.js');// finialPath： '当前文件的绝对路径/a.js'
   ```

# http 模块 去HTTP协议查看

```
const http = require('http');http.createServer((req, res) => {	// req	res.end('hello world')})
```

# url 模块 处理url地址 去报文get请求参数查看

```
const url = require('url');url.parse() // 解析请求地址
```



# nodemon 模块

nodemon 是一个名利行工具，用以辅助项目开发。

在nodejs中，每次修改文件都要在命令行中重新执行该文件，非常繁琐。

nodemon用以在修改文件后直接可以看到修改后的效果，不需要

使用步骤 

1. 全局瞎子 npm install nodemon -g  （在管理员权限运行）
2. 在命令行中 用nodemon命令代替node 执行

# nrm 

npm 默认下载地址再国外， 国内下载速度慢

nrm 是可以切换下载地址的第三方工具

1. npm install nrm -g 
2. nrm ls （查看下载地址）
3. nrm use taobao 即可

# gulp

## gulp 基于node 平台开发的前端构建工具

将机械化操作编写成任务， 我们想要执行机械化操作去 执行命令行 命令任务就可以自动执行， 不需要程序员手动操作

gulp 能做什么

​	项目上线 html  js css 文件压缩

​	语法转化 es6 、 less

​    公共文件抽离

​    修改文件是 浏览器启动自动刷新

### gulp 使用

1. 使用 npm install gulp  下载库文件

2. 在项目根目录下建立 gulpfile.js 文件

3. 重构项目的文件夹结构 src目录下放置源代码文件 dist 目录下放置构建后的文件

4. gulpfile.js 编写任务

   ***Gulp中的方法**

   + gulp.src('文件路径') 获取任务要处理的文件

   + gulp.dest('输出文件路径') 输出文件

   + gulp.task('任务名称'， 回调函数) 建立gulp任务

   + gulp.watch() 监控文件的变化

     ```
     // 引入gulpconst gulp = require('gulp');const path = require('path');// 使用gulp.task()建立任务, 任务名称是fitstgulp.task('first', () => {	// 要处理的文件	gulp.src(path.join(__dirname, '/src/css/base.css'))	// 将处理后的文件输出到dist目录	.pipe(gulp.dest('./dist/css'))})
     ```

5. 命令行工具中执行任务

   *** 怎样去执行gulp的任务？  用node 命令是执行整个文件，我们要下载gulp-cli 去执行gulp的 first 任务**

6. 下载命令行工具 npm i gulp-cli -g

7. gulp 空格 然后是任务名称  如 gulp first

## gulp插件去实现文件的合并压缩功能 

由于gulp只有基础的5个方法，如果要实现一些辅助的操作，需要用到gulp插件

+ gulp-htmlmin: html  文件压缩
+ gulp-csso: 压缩css
+ gulp-babel: js语法转化
+ gulp-less: less 语法转化
+ gulp-uglify： 压缩混淆js代码
+ gulp-file-include: 公共文件内容抽取
+ browsersync 浏览实时同步

***使用方法**

1. 下载插件 如 npm install gulp-htmlmin

2. 引用

3. 调用

   ### 压缩html文件   gulp-htmlmin

   ```
   // 引入gulpconst gulp = require('gulp');const path = require('path');const htmlmin = require('gulp-htmlmin');// 使用gulp.task()建立任务, 任务名称是htmlmingulp.task('htmlmin', async () => {	gulp.src('./src/*.html') // 获取src下面的所有htm使用通配符*    // 压缩html文件中的代码    .pipe(htmlmin({collapseWhitespace: true}))    .pipe(gulp.dest('dist'))})
   ```

   ### 抽取公共文件包含 gulp-file-include

   ```
   const fileinclude  = require('gulp-file-include');gulp.task('htmlmin', async () => {	gulp.src('./src/*.html') // 获取src下面的所有htm使用通配符*    // 抽取公共代码    .pipe(fileinclude())    // 压缩html文件中的代码    .pipe(htmlmin({collapseWhitespace: true}))    .pipe(gulp.dest('dist'))})
   ```

   

   在common 文件中建立common文件夹， 然后抽取了公共代码

   ![image-20210528012917550](C:\Users\Mloong\AppData\Roaming\Typora\typora-user-images\image-20210528012917550.png)

   最后在文件中通过@@include('公共代码文件路径') 把公共文件引回来；

   ![image-20210528013137440](C:\Users\Mloong\AppData\Roaming\Typora\typora-user-images\image-20210528013137440.png)

   ![image-20210528013152023](C:\Users\Mloong\AppData\Roaming\Typora\typora-user-images\image-20210528013152023.png)

   最后执行 htmlmin  任务 命令行中输入 gulp htmlmin

   ### css代码less转cass语法转化 css代码压缩 

   ```
   //  还是要先进行下载 npm install gulp-less // npm install gulp-cssoconst less = require('gulp-less');const csso = require('gulp-csso')// css 任务： // 1. less 语法转化// 2. css代码压缩gulp.task('cssmin', async () => {    // 获取src/css下面的所有.less 和 所有的css文件 注意这里是数组的方式包含了2种文件    gulp.src(['./src/css/*.less', './src/css/*.css'])    .pipe(less()) // 先转化成css文件    .pipe(csso()) // 然后用gulp-csso 插件进行压缩css代码    .pipe(gulp.dest('dist/css'))})
   ```

   

   ## es6 代码转化  js 代码压缩

   1. est代码转化首先要先用到 gulp-babel

      ```
      # Babel 7$ npm install --save-dev gulp-babel @babel/core @babel/preset-env# Babel 6$ npm install --save-dev gulp-babel@7 babel-core babel-preset-env
      ```

   2. js 代码压缩用到 gulp-uglify   npm install gulp-uglify

   3. 实现代码

      ```
      const babel = require('gulp-babel');const uglify = require('gulp-uglify');gulp.task('jsmin', async () => {    gulp.src('./src/js/*.js')    .pipe(babel({        // 它可以判断当前代码的运行环境，将代码转化为当前运行环境所支持的代码        presets: ['@babel/preset-env']    }))    .pipe(uglify()) // 压缩js代码    .pipe(gulp.dest('dist/js'))})
      ```

   ##  将项目资源的文件夹复制到 打包后的文件夹中

   ```node.js
   // 复制静态资源 如image 文件夹gulp.task('copy', () => {    // 复制image文件夹下的东西    gulp.src('./src/images/*')    .pipe(gulp.dest('dist/images'))    // 复制lib文件夹下的东西    gulp.src('./src/lib/*')    .pipe(gulp.dest('dist/lib'))})
   ```

##  我们创建一个任务 去执行上面所有的任务  *** 第二个参数是个数组**

```
// 建立构建任务 第二个参数是一个数组表示 要执行任务的集合gulp.task('deafult', ['htmlmin', 'cssmin', 'jsmin', 'copy'])
```

*** 踩坑: 直接执行报错** 

****

![image-20210529232332631](C:\Users\Mloong\AppData\Roaming\Typora\typora-user-images\image-20210529232332631.png)

 这个gulp -v 查看版本

![image-20210529232144682](C:\Users\Mloong\AppData\Roaming\Typora\typora-user-images\image-20210529232144682.png)

如果是4.0 版本的 

第二个参数：即要执行的所有任务名称必须写成 gulp.series() 的参数

 ```
// 建立构建任务 第二个参数是一个数组表示 要执行任务的集合gulp.task('default', gulp.series('htmlmin', 'cssmin', 'jsmin', 'copy'))
 ```



# package.json

项目描述文件，记录的当前项目信息， 例如项目名称。 版本。作者， github地址。 当前依赖了哪些第三方模块等。 可使用 npm init -y命令生产

```
{  "name": "description", // 名称  "version": "1.0.0", // 版本  "description": "", // 版本  "main": "index.js", // 入口文件  "scripts": { // 命令的别名，用别名代替较长的命令    "test": "echo \"Error: no test specified\" && exit 1"  },  "keywords": [], // 关键词  "author": "", // 作者  "license": "ISC",  // isc 开业协议  "dependencies": { // 项目依赖，（本地或者线上都需要的依赖）    "formidable": "^1.2.2",    "mime": "^2.5.2"  }，  "devDependencies": { // 开发依赖（本地依赖） npm i --save-dev 进行下载  	"gulp": "^4.0.2"  }}
```

**npm install --production 安装生产依赖（dependencies）**

**下面的依赖** 

## scripts 命令的别名

```
"scripts": { // 命令    "test": "echo \"Error: no test specified\" && exit 1",    "buld": "nodemon app.js" // npm run build 相当于=> nodemon app.js   },
```



# package-lock.json

记录了模块与模块之前的依赖关系， 以及模块的版本， 以及包的下载地址；

# HTTP 协议 

超文本传输协议， 规定了 如何从网站传输超文本 到浏览页， 是客户端（用户）和服务器端（网站）请求和应对的标准



## 报文

在HTTP 和响应过程中传递数据块 叫作报文， 包括要传送的数据和一些附加信息， 并且要遵守规定的格式。

 报文分为请求报文和响应报文 

请求报文：

| 请求方式（method）  | post， get    |
| ------------------- | ------------- |
| 请求地址（url）     | www.baidu.con |
| 请求报文（headers） |               |

![image-20210530161502955](C:\Users\Mloong\AppData\Roaming\Typora\typora-user-images\image-20210530161502955.png)



响应报文：

| 内容类型                         | text/html                                                    |
| -------------------------------- | :----------------------------------------------------------- |
| 内容长度                         | 20                                                           |
| HTTP状态码                       | 200 请求成功  404 请求资源没有找到 500 服务端错误  400 客户端有语法错误 |
| res.writeHead(状态码， 响应报文) | 设置响应报文                                                 |
| 内容类型（content-type）         | text/plain、text/html、 text/css、application/javascripte、 image/jpeg、 application/json |
| content-type                     | text/html;charset=utf8 (表明以utf8编码响应字符)              |

```
const http = require('http'); // 获取服务器模块const app = http.createServer(); // app就是网络服务器对象// 当客服端有请求的时候app.on('request' , (req, res) => {        // 设置响应内容和字符串编码    // 响应 res    res.writeHead(状态码, 内容类型)；    res.writeHead(400, {    	'content-type': 'text/html;charset=utf8'    })            // 请求方式    if(req.method === 'POST') {  // req.method 请求方式        res.end('我提交post')    } else if (req.method === 'GET') {        res.end('end');    }            // req.url 是请求地址    if (req.url === '/index' || req.url === '/') {        res.end('welcome homepage')    } else if(req.url === '/list') {        res.end('welocome list')    } else {        res.end('not found')    }                // req.headers 获取请求报文    { host: 'localhost:3000',  connection: 'keep-alive',  'cache-control': 'max-age=0',  'upgrade-insecure-requests': '1',  'user-agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.80 Safari/537.36',  accept:  'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3',  'accept-encoding': 'gzip, deflate, br',  'accept-language': 'zh-CN,zh;q=0.9' }                                // 服务器发送    // res.end('<h1>http</h1>');})app.listen(3000);
```

## 请求参数

客户端向服务器端发送请求时， 有时需要向服务端携带一些数据（如用户的信息等）， 这都是通过请求参数的形式传递到服务器端；

![image-20210530163631713](C:\Users\Mloong\AppData\Roaming\Typora\typora-user-images\image-20210530163631713.png)

### GET请求参数

请求参数被放置在浏览器地址中： 如 http://localhost:3000/?name=zhangsan&age=20； 传递了 name： zhangsan   和 age： 20

```
req.url = http://localhost:3000/index?name=zhangsan&age=20 const url = require('url');url.parse(req.url)返回的是一个对象{ // url 系统模块用来处理url地址，这里处理的是请求地址 返回对象  protocol: null,  slashes: null,  auth: null,  host: null,  port: null,  hostname: null,  hash: null,  search: '?name=zhangsan&age=20',  query: 'name=zhangsan&age=20',  pathname: '/index',  path: '/index?name=zhangsan&age=20',  href: '/index?name=zhangsan&age=20'  } 
```

**我们只想拿到 name： zhangsan   和 age： 20 这种参数以键值对的形式 用 url.parse(req.url, true), 第二个参数传true 就可以了**

```
const url = require('url');url.parse(req.url, true)第二个参数传入true，返回的是一个对象注意 这个对象的 query值  已经是以键值对{  protocol: null,  slashes: null,  auth: null,  host: null,  port: null,  hostname: null,  hash: null,  search: '?name=zhangsan&age=20',  query: [Object: null prototype] { name: 'zhangsan', age: '20' },  pathname: '/index',  path: '/index?name=zhangsan&age=20',  href: '/index?name=zhangsan&age=20'}
```

### POST 请求参数； 

由于post参数不是在url中， 我们不能用req.url 获取；

*** POST 参数是通过2个事件的方式接受的data事件和end 事件**

data 事件： 当请求参数传递的时候触发data事件，（有可能是100M的数据，这里每次都是触发data事件）

end事件：当参数传递完成时候触发end事件

***post参数处理使用 queryString的parse方法，不同get处理，不能使用url 模块， **

```
const http = require('http'); // 获取服务器模块const app = http.createServer(); // app就是网络服务器对象const queryString = require("querystring"); // 处理post请求参数模块// 当客服端有请求的时候app.on('request' , (req, res) => {    let postParams = '';    req.on('data', params => {        postParams +=  params    })    req.on('end', () => {    	// 使用queryString.parse() 方法处理参数        let data = queryString.parse(postParams);        console.log(data)        // data是一个对象 {}    })    res.end('ok')})app.listen(3000);
```



# 路由

在网站服务中 我们访问 http://localhost:3000/index 是首页 

访问http://localhost:3000/login 是登录页 这个是通过路由做到的

路由 是指客户端请求地址与服务器程序代码的对应关系。 简单的说，就是根据不同请求 去响应不同的内容；

路由当中是一堆判断代码， 根据不同的请求路径 去判断 应该是什么响应内容

```
const http = require('http'); // 获取服务器模块const url = require('url');const app = http.createServer(); // app就是网络服务器对象// 当客服端有请求的时候app.on('request' , (req, res) => {    res.writeHead(200, {        'content-type': 'text/html;charset=utf8'    })      let {pathname} = url.parse(req.url, true)     if (pathname === '/index' || pathname === '/') {        res.end('登录页')    } else if(pathname === '/list') {        res.end('列表页')    } else {        res.end('没有找到对应内容')    }})app.listen(3000);
```



# 静态资源的处理

## 静态资源

客户端向服务器发送一些请求， 服务器不需要做任何的处理， 可以直接响应的文件。 如 js、 css 、 html 、 image 文件

https://www.itcast.cn/images/logo.png

原理：

1. 拿到用户的请求路径 default.html； 
2. 去读取磁盘上的物理路径上的文件，然后把这个文件返回给客户端

```
const http = require('http');const app = http.createServer();const url = require('url');const path = require('path');const fs = require('fs');app.on('request', (req, res) => {   // 1.    let pathname = url.parse(req.url).pathname; // 这里直接获取请求的 不包括参数    let realpath = path.join(__dirname + pathname); // 这里用path将 当前的路径 和 请求路径进行拼接    // 2. 接下来直接调用fs， 进行读取路径下的文件内容； 然后返回    fs.readFile(realpath, (err, result) => {        if (err) {            res.writeHead(200, {                'content-type': 'text/html;charset=utf8'            })            res.end('文件读取失败');            return;        }        res.end(result) // 返回读取的结果    })})
```

### mime 第三方插件； 可以区分出当前请求的文件类型； 如当前请求的是 html 文件还是 css文件 ， 然后设置正确的 响应content-type 从而展示正确的文件

```
const mime = require('mime');1. 拿到请求的路径let realpath = path.join(__dirname + pathname); // 这里用path将 当前的路径 和 请求路径进行拼接let type = mime.getType(realpath); // 这里拿到本次正确的文件类型2. 去该路径下去读响应的文件fs.readFile(realpath, (err, result) => {        if (err) {            res.writeHead(200, {                'content-type': 'text/html;charset=utf8'            })            res.end('文件读取失败');            return;        }        res.writeHead(200, {            'content-type': type // 设置响应内容        })        res.end(result) // 返回读取的结果    })
```



## 动态资源： 

相同的请求地址，返回不同的响应，这种资源就是动态资源

http://yun.itheima.com/map?id=2

http://yun.itheima.com/map?id=1



# promise 异步 编程解决方案， 解决回调地狱问题

```
let promise = new Promise((resolve, reject) => {	setTimeout(() => {		resolve({name: 'zhangsan'})	}, 1000)})promise.then(res => {	console.log(res) // {name: 'zhangsan'}})// 读取文件案例const fs = require('fs');let promise = new Promise((resolve, reject) => {	fs.readFile('./a.js', (err, res) => {		if(err) {			reject(err)		} else {			resolve(res)		}	})})promise.then((res) => {	console.log(res) // 文件内容}).catch((err) => {	console.log(err);})async function fn() {	throw '发生了错误'}fn().then() {}.catch( e => {	console.log(e)})
```

# async await

普通函数定义前加async 普通函数就变成了异步函数

异步函数都是返回的promise 对象， return 的值就是 resolve 的值

```
async function fn() {	return 1}fn().then(res) {// fn 执行结果就是一个promise 对象	console.log(res) // res 就是 fn函数中 return 的值}.catch( e => {	console.log(e)})
```

await 后面要跟着一个 promise 对象 await 可以暂定异步函数向下执行， 后续的代码等 await 有了结果才执行

```
async function fn() {
	return 1
}
async function fun() {
	let f1 = await fn() // fn是异步函数，它执行就是返回一个promise对象
	let f2 = await new Promise((res, rej) => {
		res(1)
	}) // 这里 new Promise 创建的也是一个promise 对象
	let f = f1 + f2
	console.log(f)
}
fun()
```

