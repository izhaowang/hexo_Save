---
title: js
date: 2022-03-28 14:21:27
categories: 
- 前端js
tags:
- es5
- js
---


在url地址栏输入 地址访问 经历 ：
    1. 浏览器把输入的网址，解析为ip；
	1.1 首先查找浏览器缓存，如果有缓存，那么直接返回ip，否则进行下一步
	1.2 查看系统缓存， 浏览器进行系统调用（window 的gethostname ），查找host文件， 直接返回ip； 没有下一步
	1.3 查找路由器缓存， 借助网络，查找isp服务商缓存的DNS服务器，找到返回ip，没有下一步
	（递归查询， 迭代查询）
    2. 浏览器与目标建立tcp连接： 
	2.1 主机浏览器拿到目标服务器的ip地址后， 与服务器建立tcp连接
	2.2 tcp3次握手建立连接：  
			第一次：浏览器所在的主机（本机）向目标服务器发出请求报文（SYN标志为1）
			第二次：目标服务器接受报文后，同意建立连接，向客户端发出确认报文（SYN, ACK 标志均为1）
			第三次：客户端确认收到报文后，再次向服务器发出报文，确认已收到确认报文（客户端与服务器TCP连接确认完成，开始通信）
   3. 浏览器通过http协议发送请求
	浏览器发送一个HTTP-GET 方法报文请求。 请求中包含URL， KeepAlive 长连接，还有 User-Agent 
	用户浏览器操作系统信息，编码等。 其中Accep-Encoding （压缩gzip）和 Cookies（用户首次访问，会提示
	服务器建立用户缓存信息， 不是首次访问利用Cookies对应的键值对，找到相应的缓存，缓存中存放着用户名，密码和一些用户设置项）项，
    4. 某些服务会做永久重定向响应：
	大型网站 一般不会直接返回请求页面，（状态码不是200， 而是301,302 以3开头的重定向码，浏览器获取了重定向响应
	后，在响应报文中获取到location项找到重定向地址，重新第一步访问即可）
		重定向作用： 主要是2点 : 1.为了负载均衡（减少服务器压力）；  2.重定向可以将多个域名的访问，集中多一个站点
			如：baidu.com 和 www.baidu.com 搜索引擎认为是两个网站，永久重定向会将两个地址关联起来
			，搜索引擎任务是同一个网站，提高排名
    5. 浏览器跟踪重定向地址： 浏览器拿到重定向地址后，重新发送一个http请求
    6. 服务器处理请求： 服务器收到获取请求，处理并返回一个响应报文。
    7. 根据报文的Content-type  响应文件呈现形式
    8. 释放tcp连接:
	8.1 浏览器所在的主机向服务器发出连接释放报文，然后停止发送数据；
	8.2 服务器接收到报文后确认报文，然后将服务器上未传送完的数据发送完；
	8.3 服务器数据传输完毕后，向客户端发送连接释放报文；
	8.4 客户端接受到报文后，发出确认，然后释放tcp连接
    9. 浏览器根据对应的渲染机制 渲染html
1. 浏览器渲染原理
	首先下载静态资源
	解析html时，发现其中有其他外部资源链接 如css、js、图片等， 会立即开启其他线程下载。
		当外部资源下载js时， html解析会停下来。 等js下载 执行结束后才继续 解析HTML
	解析html=》构建DOM树
	解析CSS => 构建CSSOM树
	利用DOM 和 CSSOM树 构建 Render树
	进行布局 layout
	进行绘制	painting 到屏幕上
	
	当某个部分发生改变，影响到布局， 进行重排 reflow 
	当某个元素背景、边框颜色等发生改变， 进行重回 repaint
	
# script 标签的defer属性和async属性
```html
	<script src=""> </script>
	<script src="script.js"></script>

没有 defer 或 async，浏览器会立即加载并执行指定的脚本，“立即”指的是在渲染该 script 标签之下的文档元素之前，也就是说不等待后续载入的文档元素，读到就加载并执行。

<script async src="script.js"></script>

有 async，加载和渲染后续文档元素的过程将和 script.js 的加载与执行并行进行（异步）。一旦加载完成后会执行js；
有可能在 DOMContentLoaded之前执行，也有可能在DOMContentLoaded之后执行。但是一定在load之前执行；

<script defer src="myscript.js"></script>

有 defer，加载后续文档元素的过程将和 script.js 的加载并行进行（异步），但是 script.js 的执行要在所有元素解析完成之后，DOMContentLoaded 事件触发之前完成。
```
## defer和async的主要不同就是defer会保证脚本的顺序，async不保证顺序
# DOMContentLoaded ， onload
1、当 onload 事件触发时，页面上所有的DOM，样式表，脚本，图片，flash都已经加载完成了。

2、当 DOMContentLoaded 事件触发时，仅当DOM加载完成，不包括样式表，图片，flash。

window.onload ：页面全部加载完成包括图片
DOMContentLoad：dom渲染完成即可，此时图片视频可能还没有加载完
2. 变量
	let 在同一个作用域中，不能重复声明同一个变量。 for 循环 判断条件和 代码块中 是两个作用域
	let 声明的变量不能 变量提升到 顶层。   =》 导致存在暂时性 死区。

	const 同let 一样。 const 声明的变量 内存地址不能改变
3. 结构赋值
	let {foo: {bar}} = {baz: 'baz'}; // 会报错
	原因很简单，因为foo这时等于undefined，再取子属性就会报错，请看下面的代码。 
	
	1. 数组的 结构赋值
	2. 对象的结构赋值
	3. let arr = [1, 2, 3] ；
	    let {0：a, [arr.length-1]: b, 1:c} = arr // 此时将数组转化为 伪数组对象
	4. 字符串的结构赋值 =》 转化为 伪数组对象 const [a,b] = 'hi'  let {length: len} = 'hello'
	5. 数值和布尔值的结构赋值 数值和布尔值 都有 封装的toString 属性， 因此
		let {toString：s} = 123；
		let {toString：s} = true；
	6. let {proxy： x} = undefined  报错 let {prop： x} = null  
		null 和 undefined 没有包装对象。 对他们进行结构都会报错
	7. 函数参数的解构赋值
4. 字符串：
 	1. "hellow".charAt(0)， 返回0处的 值  =》 h
	2. 'hellow'.charCodeAt(0), 返回0处值 的 unicode 编码 是一个数字
		String.fromCharCode(数字) =》 将 unicode 编码数字 转化为对应的 字符串
	3. 字符串是已UTF-16 格式储存， 每个字符固定是 2个字节。 对于 4个字节储存的 字符， js认为是2个字符
	   ES3 提供了 codePointAt方法，能够正确处理4个字节储存的字符。 返回一个字符的 码点 数字
	  "级".codePointAt(0) // 32423
	 Strgin.fromCodePoint(32423) => '级'
5. 函数 
	length 属性 含义： 该函数预期传入的参数个数， 某个参数指定默认值后， 预期参数个数就不包括这个参数
			rest 参数也不计入length属性； 因为rest 意义就是 不知道会传入几个参数，统一代替的
	(function (a = 0, b, c) {}).length // 0
	(function (a, b = 1, c) {}).length // 1  如果设置了默认值的参数不是尾参数，那么length属性也不再计入后面的参数了。	
 
	默认参数，函数声明初始化时，参数会形成一个单独的作用域。  有且只有设置默认值时，才会出现 这个作用域

arguments.callee 指向  是arguments 的一个属性指向正在运行的 函数
（function（） {}）.caller 是函数的一个 一个属性 指向正在运行的 函数 在window 下 调用 直接是null
6.箭头函数	 
	如果箭头函数直接返回一个对象， 就必须在对象外面加上括号，否则会报错： let getTime = time => ({time: Date.now()});


 Vue 通过建立一个虚拟DOM 来追踪自己如果要改变真实DOM
return createElement（'h1', this.title）； =》》 createElement 函数返回的就是一个{Vnode} 虚拟 节点 


Vue的生命周期
1. beforeCreate 实例创建之前除非，不能访问data computed watch 的数据
2.created 实例创建后发生， 可以访问data中的数据， 当前不能与dom 进行交互
3. beforeMount 发生在挂载之前， 此时虚拟dom已经创建完成
4.mounted 在挂载完成后发生， 真实的dom挂载完成，数据完成双向绑定；可以访问dom节点，
5. beforeUpdate 发生在更新之前，响应式数据更新，但是虚拟dom还没有渲染，
6.updated 发生在更新完成之后，当前阶段组件dom已经更新完成
7.beforeDestory 发生在实例销毁之前，在当前阶段实例可以被使用， 我们可以清除计时器等
8.destoryed 发生在实例销毁之后， 这个时候组件被拆解，数据被卸载，监听被移除

1. 实例创建之前，不能访问data 、 实例创建之后；可以访问到data数据； 不能与 dom 进行交互
 挂载之前， 此时虚拟dom已经创建完成；  挂载之后完成； 真实的dom完成挂咋； 数据完成双向绑定；可以访问dom节点
发生再更新之前， 响应式数据； 但是虚拟dom 没有渲染之前被触发；  发生再更新完成之后， 当前阶段组件dom已经更新完成
发生再实例销毁之前， 此时实例可以被调用  发生再实例销毁之后， 这个时候组件被拆解 数据被卸载， 监听被移除



Vue.extend() 创建vue 的一个子类
nextTick 异步队列，主要是监听DOM 更新完成； 
主要当前浏览器环节使用 promise mutationObserver setImmediate 或者 setTimout方法

Vue2.x 监听数组的变化： vue 会将数组的原型方法进行重写； 每次调用时，会监听到数组的更改，然后通知视图进行更新；
 如果是有引用类型，那么就对数组进行递归进行监听，

1.浮动 影响自身 和同级后面元素的位置
2.浮动会造成父级高度的塌陷；影响正常文档流布局； 所以需要清除浮动
	1.清除浮动的方法: 给父级 添加overflow hidden属性； 给浮动元素同级后面 添加一个div元素，设置clear:both
	; 给父级设置伪类：after {display: block; content: ''; visibility:hidden;clear:both; height:0} 三种方法
3. 

key值： vue为了尽可能降低dom操作，尽可能的复用已有的dom 而非重新渲染；key的作用是给节点一个唯一标识，
以便能够再操作dom时找到可以复用的节点；


keep-alive 的用法 缓存动态组件； vue2.0版本后， 内置组件已经封装了两个属性， include 表示缓存的组件； exclude表示不需要缓存
	可以是名字或者 


2.4 $attrs 和 $listeners 属性
inheritAttrs 默认是 true 继承所有父组件属性,除了props 的特定绑定;作为普通的html属性应用在 子组件的根元素;
如果inheritAttrs是false，但是class 属性会继承；

$listeners--属性，它是一个对象，里面包含了作用在这个组件上的所有监听器，你就可以配合 v-on="$listeners" 
将所有的事件监听器指向这个组件的某个特定的子元素。


inheritAttrs 继承所有的属性 除了props； 


模块化的前世今生
模块化就是将一个复杂的系统分解成多个独立的模块； 之前都是通过一系列的script 标签来维护 模块之前的关系；
但是一旦项目复杂。 这个方式 就会使我们的代码混乱不堪； 
由于早期官方 并没有提供统一的解决方案，所以早期 关于模块化方案层出不穷。 
IIFE ： 模块化的一大作用就是用来隔离作用域，避免变量冲突； 最早为了避免与全局变量污染， 智能使用闭包来实现模块化。 
（function (window) {
	window.query = {} ;
}）(window)
// 虽然IIFE 有效解决了命名冲突的问题， 但是对于依赖管理， 还是束手无策。 因为浏览器时自上而下的执行脚本；为了维持脚本
间的依赖关系；就必须手动维护好script 标签 相对顺序
AMD： 一种模块化规范， 主要提供了异步加载功能；  需要使用RequireJS去实现模块化；  所有的依赖像必须提前声明好；
在导入模块的时候，也会先加载对应的依赖模块， 然后再执行后续代码，AMD可以并发加载所有的依赖模块；
define（'./index.js', function(code) {
	// code 时index.js 返回的内容	
return {
	}
}）
CMD：通由阿里玉伯提出；通AMD类型， CMD需要SeaJS 库来实现模块化；同AMD 一样； CMD也是为了解决 依赖管理问题；
define（function （require） {
	var path = require.resolve('./cmdDefine');
	alert(path)
）
  CMD 加载完某个依赖模块后并不执行，只是下载而已， 再所有依赖模块加载完成后进入主逻辑， 如果 require语句才执行
对应的模块， 用require.asynce（） 可以实现赖加载
CommonJS， 随着模块化深入； 需要一种标准的模块化方案； 此时commonJS 应运而生； node.js应用就是采用这个规范；
内置module对象 定义模块； require 函数来加载模块文件
var add = funciton（a, b） {}
module.exports = { 
	add: add
}

// 加载模块
var utils = require（'./utils'）
console.log(utils.add(1,2)) // 3
这种模块化方案特点就是：同步阻塞式加载，无法实现按需异步加载。如果时浏览器环境使用CommonJS 模块 需要Browserify
进行解析；

UMD： 上面CommonJs 合AMD 等模块化方案都是针对特定的平台； 如果想要实现 跨平台的模块化， 可以兼容 AMD 合 commonJs
等模块化语法

ES6 modules
export
import

CommonJS 合 ES6 模块的两大差异
1. CommonJS 输出的是一个值的拷贝； Es6模块输出的是一个值的引用
2. CommonJS 模块是运行时加载， es6模块时编译时输出接口

第二个差异是因为 CommonJS 加载的是一个对象（module.exports 属性），
该对象只有在脚本运行完才会生成。 

而es6模块不是对象； 它的对外接口只是一种静态定义’

第一个差异CommonJS 输出的是一个值的拷贝

// lib.js
var counter = 3;
function incCounter() {
   counter++
}
module.exports = {
    counter: counter,
    incCounter: incCounter
}


// main,js 里面加载这个模块
var mod = require('./lib');
console.log(mod.counter); // 3
mod.incCounter();
console.log(mod.counter); // 3 这里说明 lib.js 模块加载以后， 它的内部变化影响不到输出的 mod.counter 了；
除非写成一个函数，才能得到内部变动后的值。

// lib.js
var counter = 3；
function incCounter（） {
	counter++
}
module.exports = { // 此时输出的其实是一个 取值器函数
	get counter() {
		return counter 
	},
	incCounter: incCounter
}
<div></div>

es6 模块的运行机制与CommonJS 不一样。 JS引擎对脚本静态分析的时候，遇到模块加载命令 import，
就会生成一个只读的引用。 等到脚本真正执行时，再根据这个只读的引用， 到被加载的哪个模块里面去取值；
（es6模块时动态引用，并且不会缓存值， 模块里面的变量绑定其所在的模块）

// lib.js
export let counter = 3；
export function incCounter() {
	coun3ter++;
}


// mian.js
import {counter, incCounter} from './lib';
console.log(counter); // 3
incCounter();
console.log(counter); // 4
{}

