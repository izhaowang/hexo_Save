---
title: js
date: 2022-03-28 14:21:27
categories: 
- 前端js
tags:
- es5
- js
---


# 静态属性，私有属性，公有属性
```javascript
	// 静态属性，方法 不用实力化就可以直接访问
	function Fn () {
	
	}
	Fn.staticProperty =  '静态属性' 
	FN.func = funciton() {} // 静态方法
	// 不会被实例继承和prototype平级只能通过Fn.staticName访问
	// 静态属性es6写法
	class Fn () {
		static prop = '静态属性'
	}


	// 私有属性，方法 只能在类的内部可以访问到，也不会继承到实例化对象中去
	function Fn(prop) {
		// 
		var prop = prop

		function func(){ //私有方法
			alert(prop); //只能在类的内部可以访问到
		}
		func()
	}

	// es6
	class Fn {
		#prop = 'c'  // 在私有属性前面加#
		constructor() {
			this.name = name // 公有属性
		}

		// es6 不提供私有方法可以通过变通来实现
		baz(name){ // 公有方法
			console.log(name);
			this._bar(name)
		}
		_bar(name) { // 私有方法
			console.log(#prop);
		}
	}	


	// 公有属性 可以被实例化继承
	function Fn(prop) {
		this.prop = prop // 公有属性
	}
	// 公有方法
	Fn.prototype.func = function() {
		console.log(this.prop)
	}

	class Fn{
		constructor(p) {
			console.log(prop)
			this.p = p // 公有属性
		}
		func = function() { //公有方法
			console.log(this.p);
		}
	}

```
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


# promise 是异步编程的一种解决方案，传统的异步编程作为回调函数作为处理异步结果的用法，很容易造成回调嵌套代码冗余。
promise、可以看作是一个容器，里面保存着未来结束的异步结果处理事件，它提供统一的api进行处理异步结果
## 特点有2
1. promise有三种状态pendding表示进行中， fulfilled表示已完成， reject表示已失败。而异步结果只能是有pendding到fulfilled 或者 pendding到reject
2. 如果状态一旦改变，状态就凝固了并且状态不会改变，并且结果就确定了resolve，任何时候都可以得到这个结果。这个和事件完全不同。 事件的特点是事件只要一触发，就会立即执行，如果你一旦错过了，那么就很难再去监听。
## promise的缺点
1. promise无法取消，如果建立了promise就会立即执行，中途是无法取消的
2. 如果回调中不设置，promise内部发生了错误，不会反应到外部
3. 处于pendding 不知道是刚刚开始还是即将要结束pendding状态了

### 1、在 定义 new Promise 里面属于同步代码，遇到 reslove 和 reject 不会停止代码执行； new 里面的代码全部执行完

```javascript
const P1 = new Promise（（res，rej）=>{
    console.log(111);
    res(10); // 状态只能改变 一次。
    console.log(222)
    res(20)
}）
P1.then(res1=>{
    consoel.log(res1)  // 10
})

// 打印： 111 、 222、 10
注意： Promise 函数是同步代码， then系列属于微任务。 setTimeout 输入宏任务
```
### 如果定义 new promise中先return 
```javascript
	// 这里先return的和状态没有任何关系那么此时p一直是pendding 状态。
	// 因此p.then 回调永远不会执行
	let p = new Promise((resolve, reject) => {
		return 1
		resolve(0)
	})
	p.then(res => {
		console.log(res);
	})


	// 这里return了的事一个resolve函数
	let p = new Promise((resolve, reject) => {
		return reslove(10)
		console.log(0) // 这句后面不执行
	})
	p.then(res => {
		console.log(res); // 打印10
	})
```

### 2、 如果resolve 是一个Promise， 那么后续的then将以被resolve 的那个promise 的状态为准

```javascript
let p = new Promise((resolve,reject) =>{
    setTimeout(()=>{reject('fail')}, 3000)  // 3s的时候 p才被reject 
})
let P1 = new Promise(resolve, reject) =>{
  	setTimeout(() => { resolve(p) }， 1000)   // 1s的时候 resolve了一个p （promise对象）
})



// 这里的 p1.then（res） res 是一个promise对象，同步代码此时then系列不会执行，等到3秒后， then进去， 会执行catch 里面的 err
P1.then(res => console.log(res)).catch(err => console.log(err)); 

// 定义函数中 reject 之后所有的then都不会执行，除非遇到catch， catch之后的then 取决catch中的代码

// 打印： err信息
```



### 3、new Promise的函数中的  return 和 resolve/reject

3.1  如果 new Promise 中 一直是pendding状态， 不进行状态改变， return 什么东西都不会执行 then 里面的代码

```javascript
//  eg
console.log('1');
let p = new Promise((res, rej) =>{
    console.log('2');
    return new Promise(res1 => {  // 这里只是进行了 return 并没有 resolve 任何东西
        console.log('3');
        res1()
    })
}).then(() => {console.log('promise-then')}); //
console.log('4')

// 打印： 1 2 3 4



//  eg
console.log('1');
let p = new Promise((res, rej) =>{
    console.log('2');
    res(new Promise(res1 => { // 这里resolve 了一个 promise
        console.log('3');
        res1()
    }))
}).then((res) => {
console.log(res) // 这里res 打印undefined，是res1（）中没有内容
console.log('promise-then')}); // 这里会执行 因为和上面不同的是 p 的状态是 resolve
console.log('4')

// 打印： 1 2 3 4 promise-then
```

3.2  如果 new Promise 里面进行了状态改变也执行了 return

```javascript
console.log('1');
let p = new Promise((res, rej) => {
    console.log('2');
    resolve(123);
    return 'abc';
}).then(res => { console.log('promise-then', res) }) // 这里的 res是 resolve的值123
console.log('3');

// 打印： 1 2 3 promise-then 123



console.log('1');
let p = new Promise((res, rej) => {
    console.log('2');
    resolve(123);
    return new Promise((res1， rej1) => {
        console.log('4');
        res1(456)
    });
}).then(res => { console.log('promise-then', res) 
}) // 这里的 res是 resolve的值 123 ； 而不是 return Promise中的 resolve 的 456；
// *** 这里除非 resolve 的是一个 promise 后面的 then 参数才会是 456，参考第二条
console.log('3');

// 打印： 1 2 4 3 promise-then， 123


```

定义 Promise的函数， 不进行状态改变， 就后续的then 不会执行； 不管定义函数中 return 的是什么， 哪怕return的 是一个promise 函数也不会执行

如果定义Promise 函数，有 进行状态改变， 那么 then后面的 参数，跟有没有 return 或者 return的东西没有关系

定义Promise 函数， 如果resolve 了一个 promise 那么 下一个then 的状态和 resolve 的 这个promise 状态有关系（看3.1 的第二个例子）

$$**总结： 定义函数中 return 不值钱， 只要状态不改变 then 里面的代码不会执行； then里面的参数永远是 定义函数中 resolve/ reject 中的值**$$ 



```

```

### 4、 then里面不return、 return一个值、 return 一个 promise的区别

```javascript
// 1. then 里面不进行return 下一个then 接受到的参数是 undefined
new Promise(res => {
    res(10);
}).then(res => {
    console.log(res); // 10
}).then(res => {
    console.log(res); // undefined
})
// 打印： 10 undefined

// 2.  then 里面return 一个值， 那么下一个then 接受到的参数就是 这个值
new Promise(res => {
    res(10);
}).then(res => {
    console.log(res); // 10
    return 'abc‘
}).then(res => {
    console.log(res); // 'abc
})
// 打印： 10 abc

// 3. then 里面return 一个promise， 下一个then 就根据 上一个 Promise的状态 来分情况看是否需要执行

new Promise(res => {
    res(10);
}).then(res => {
    console.log(res); // 10
    return new Promise(res1 => {
       console.log('hello')
       res1(0)
    })
}).then(res => {
    console.log(res); // 0
})

// 打印：  10 hello  0



// 3. then 里面return 一个promise， 下一个then 就根据 上一个 Promise的状态 来分情况看是否需要执行

new Promise(res => {
    res(10);
}).then(res => {
    console.log(res); // 10
    return new Promise(res1 => {
       console.log('hello')
       return 10
    })
}).then(res => {
    console.log(res); // 由于上一个then 返回了一个新的promise 但是状态一直是pendding状态， 因此 这个then 里面的代码是不会执行的。
})

// 打印：  10 hello 
```



1. 首先我们要知道， then方法执行就会返回一个 promise对象， 所以才可以支持链式调用
2. 那么就好理解了， 如果 你不return 一个 promise 对象 那么 我认为 then 返回的 promise 就是 一个resolve状态，（ 也就是永远执行下一个then）；  区别是 resolve 的值 是一个 undefined 还是 一个 具体值；
3. 如果你 return了一个promise对象， 那么下一个then 根据 return 的这个promise 的状态来具体看是否会执行。 

### 5、 往一个Promise实例挂载多个 并列的 then 和 一串 后辈的then 区别

```javascript
let p8 = new Promise((resolve, reject) => {
        console.log("init promise");
        setTimeout(() => {
            resolve("promise resolved");
        }, 1000);
    });
    let p8_1 = p8.then(res => console.log("p8_1: ", res)); // 同一个promise 兄弟
    let p8_3 = p8.then(res => {  // 同一个promise 兄弟
        console.log("p8_3: ", res);
        return "p8_3 finish";
    });
    let p8_2 = p8.then(res => console.log("p8_2: ", res));  // 同一个promise 兄弟
    let p8_3_1 = p8_3.then(res => { // 儿子 then
        console.log("p8_3_1: ", res);
    });
    let p8_4 = p8.then(res => console.log("p8_4: ", res));  // 同一个promise 兄弟
    // output:
    // init promise
    // p8_1:  promise resolved
    // p8_3:  promise resolved
    // p8_2:  promise resolved
    // p8_4:  promise resolved
    // p8_3_1:  p8_3 finish






let p8_6 = new Promise((resolve, reject) => {
        console.log("init promise");
        setTimeout(() => {
            resolve({a: 1, b: 2, c: 3});
        }, 2000);
    });
    let p8_7 = p8_6.then(obj => {
        console.log("p8_7:");
        console.log(obj);
        obj.d = 4;  // 这里修改了 传入的参数（改参数是一个 引用类型的数据）
    });
    let p8_8 = p8_6.then(obj => {
        console.log("p8_8:");
        console.log(obj); // 这里也就自然而然多了一个 d属性；
    });
    // output:
    // init promise
    // p8_7:
    // {a: 1, b: 2, c: 3}
    // p8_8:
    // {a: 1, b: 2, c: 3, d: 4}
```

- 并列的兄弟then是按照绑定的先后关系执行的
- 所有的"儿子then"都执行完了之后，才轮到"孙子then"执行，不论"孙子then"是啥时候绑定绑定的
- 并列的兄弟then会共享同一个res(的浅拷贝)，如果这个res是个Object，并且靠前执行的then修改了这个Object的内容，那么靠后执行的then也会受到修改的影响

### 六、关于定义函数里面 throw Error、 语法错误、 reject 

定义Promise 函数里面， 不论是 reject 还是 throw 还是 语法错误， 都会产生一个错误信息后， 后面的catch会接住， 如果没有catch 就不会产生任何作用， 

定义函数里面的error信息只在promise里面流转，不会印象promise外面的流程执行；

关于错误： throw Error 和 语法错误 的后续代码不会继续执行。 但是reject 之后的代码会继续执行

catch之前的then环节都被忽略，不执行。 catch之后的then环节正常进行。
```javascript
(function () {
    new Promise((res, rej)=> {
        rej('111') // 这里 reject了一个
    }).then(res => {
        return 10
    }).then(res => {
        console.log(res); // 这里不会执行 会进入到错误的回调中去
    }, err => {
       //  不管是前面的 reject状态还是 throw new Error，错误都会穿透到这下面的then中
        console.log(err) // 这里打印字符串'111'， 
    }).catch(err => {
        console.log(err); // 由于上面then中已经有捕获错误了，这里并不会执行
    })
    // 打印：‘111’
})();

```

### 七、在then里面 throw Error 和 throw Promise 区别

- then里面的throw也会被catch抓住，如果throw的是一个promise实例，那么catch抓住的err就是这个promise实例

```javascript
 let p11_1 = new Promise((resolve, reject) => {
        console.log("init promise");
        resolve();
    }).then(res => {
        console.log("1st then");
        throw new Error("error 1");
    }).catch(err => {
        console.log(err);
    });
    // output:
    // init promise
    // 1st then
    // Error: error 1 xxxx   打印err信息



    let p11_2 = new Promise((resolve, reject) => {
        console.log("init promise");
        resolve();
    }).then(() => {
        console.log("1st then");
        throw new Promise((resolve1, reject1) => {
            resolve1("resolve in promise");
        });
    }).then(res => {
        console.log("2nd then: ", res); // 这一步不会执行
    }).catch(err => {
        console.log("catch fn");
        console.log(err);
    });
    // output:
    // init promise
    // 1st then
    // catch fn
    // Promise {<resolved>: "resolve in promise"}   // 这里是一个Promise实例
```





##### 12. 一个catch面对多个then抛出的错误，会如何处理

```javascript
    let p12_1 = new Promise((resolve, reject) => {
        reject("reject in promise fn");
    }).then(() => {
        throw new Error("1st then error");
    }).then(() => {
        throw new Error("2nd then error");
    }).catch(err => {
        console.log("catch fn: ", err);
    });
    // output:
    // catch fn:  reject in promise fn

    let p12_2 = new Promise((resolve, reject) => {
        resolve();
    }).then(() => {
        throw new Error("1st then error");
    }).then(() => {
        throw new Error("2nd then error");
    }).catch(err => {
        console.log("catch fn: ", err);
    });
    // output:
    // catch fn:  Error: 1st then error xxxx
```

- 对于catch前面的多个then环节产生的error，catch只会接受到第一个产生的error(或者说，第一个error产生时后续then就都被忽略了，没有产生error的机会了)





##### 13. 错误一旦被catch处理过，就不会再漏到后面的环节

```javascript
    let p13 = new Promise((resolve, reject) => {
        reject("something wrong");
    }).catch(err => {
        console.log("1st catch: ", err);
    }).then(res => {
        console.log("1st then: ", res);
    }).catch(err => {
        console.log("2st catch: ", err);  // catch 被处理过， 这里不会再处理
    });
    // output:
    // 1st catch:  something wrong
    // 1st then:  undefined
```

- **error不会穿越catch：** catch处理error的同时也是在消费error。catch里面没有异常的话，后面的then会正常执行。



### 八、Promise.resolve 

1 Promise.resolve 传入的是一个 promise 实例。 则会原封不动的 吐出来。。

2 如果吃进去的是个 thenableObj， 那么吐出一个新的 Promise，把thenableObj.then 当做实例定义函数执行

3如果吃进来的不属于上来两种， 那么就吐出一个新的resolve的实例， 后面then的入参就是 吃进来的参数。 （没有入参就相当于undefined）

```javascript
// 1 Promise.resolve 传入的是一个 promise 实例。 则会原封不动的 吐出来。。 
let p = new Promise((res, rej) => { res()});
let p1 = Promise.resolve(p)； // Promise.resolve 传入的是一个 promise 实例。 则会原封不动的 吐出来。。 
console.log(p === p1) // true

// 2 如果吃进去的是个 thenableObj， 那么吐出一个新的 Promise，把thenableObj.then 当做实例定义函数执行
let thenableObj = {
        then: () => {
            console.log("thenableObj.then");
            return 123;
        }
    }
    let p14_3 = Promise.resolve(thenableObj);
    p14_3.then(res => {  // 这里相当于 定义函数中 return了 123， 在定义函数中return 任何东西都是不起作用的； 因此这块代码永远不会去执行
        console.log("p14_3 then");
    });
    // output:
    // thenableObj.then
    // 无论怎么等，都没有等到"p14_3 then"，控制台打p14_3显示的是"Promise {<pending>}"，也就是Promise.resolve
    // 把thenableObj.then当做Promise实例的定义函数运行，因为没有resolve函数，所以一直是pending状态


// 3. 如果吃进来的不属于上来两种， 那么就吐出一个新的resolve的实例， 后面then的入参就是 吃进来的参数。 （没有入参就相当于undefined）
Promise.resolve（res= 1 => res）.then(res => {console.log(res)});  // 吃进去一个函数
// 打印  (res = 1) => res

```



### 九、 Promise.reject 不论任何情况， 将吃进来的东西变成promise实例（状态为rejected） 再吐出去

- Promise.reject吃什么都返回一个rejected状态的新实例，并且err就是吃进去的东西。

  ```javascript
     let p15_1 = new Promise((resolve, reject) => { reject() });
      let p15_2 = Promise.reject(p15_1); // 吃进去一个 promise 实例
      p15_2.catch(err => { // 注意 err 就是吃进去的东西
          console.log("p15_2.catch");
          console.log(err === p15_1);
          console.log("p15_2.catch end");
      });
      // output:
      // p15_2.catch
      // true
      // p15_2.catch end
  
      let thenableObj = {
          then: () => {
              return 123;
          }
      }
      let p15_3 = Promise.reject(thenableObj);
      p15_3.catch(err => {
          console.log("p15_3 catch");
          console.log(err === thenableObj);
          console.log("p15_3 catch end");
      });
      // output:
      // p15_3 catch
      // true
      // p15_3 catch end
  
      let thenableObjWithResolve = {
          then: (resolve, reject) => {
              reject(123);
          }
      }
      let p15_4 = Promise.reject(thenableObjWithResolve);
      p15_4.catch(err => {
          console.log("p15_4 catch");
          console.log(err === thenableObjWithResolve);
          console.log("p15_4 catch end");
      });
      // output:
      // p15_4 catch
      // true
      // p15_4 catch end
  ```

### 队列执行顺序

```javascript
 // 一道轮回： 宏任务 => 清空微任务 => 渲染UI

    (function () {
    // new Promise((res, rej)=> {
    //     rej('111') // 这里 reject了一个
    // }).then(res => {
    //     return 10
    // }).then(res => {
    //     console.log(res); // 这里不会执行 会进入到错误的回调中去
    // }, err => {
    //    //  不管是前面的 reject状态还是 throw new Error，错误都会穿透到这下面的then中
    //     console.log(err) // 这里打印字符串'111'， 
    // }).catch(err => {
    //     console.log(err); // 由于上面then中已经有捕获错误了，这里并不会执行
    // })
    // // 打印：‘111’
     // 一道轮回： 宏任务 => 清空微任务 => 渲染UI

     Promise.resolve().then(() => { // 已经有resolve状态的同步任务1
        console.log('process 1');
    })

    setTimeout(()=>{  // 同1
        console.log('setTimeout 1')
        Promise.resolve().then(() => { // 同1-微3
            console.log('process 2') 
            setTimeout(()=>{ 
                console.log('setTimeout 4')
            })
        })
    })

    setTimeout(()=>{ // 同2
        console.log('setTimeout 2')
        Promise.resolve().then(() => { // 同2-微4
            console.log('process 4')
            setTimeout(()=>{
                console.log('setTimeout 5')
            })
        })
    })

    setTimeout(()=>{ // 同3
        console.log('setTimeout 3')
    })

    Promise.resolve().then(() => { // 已经resolve状态的同步2
        console.log('process 3')
    })

    // 结果：
    // process 1
    // process 3
    // setTimeout 1
    // process 2
    // setTimeout 2
    // process 4
    // setTimeout 3
    // setTimeout 4
    // setTimeout 5
})();

    
```



### 手动实现一个Mypromise 构造器

原理 promise 里面  是一个构造函数， 里面有 then， catch， finally， all， race 等 属性， 并且有三种状态，pendding， fulfilled， rejected； 每次只能由pendding 变为其他状态，这就很好的解决了某些插件 控制反转等缺陷

```javascript
const isFunction = (value) => typeof value === 'function'
const PENDING = 'pending'
const RESOLVED = 'fulFilled'
const REJECTED = 'rejected'
const resolvePromise = (promise2, x, resolve, reject) => {
    // x和promise2不能是同一个人，如果是同一个人就报错
    // 加一个开关，防止多次调用失败和成功，跟pending状态值一样的逻辑一样,走了失败就不能走成功了，走了成功一定不能在走失败
    if (promise2 === x) {
        return reject(
            new TypeError('Chaining cycle detected for promise #<promise>')
        )
    }
    // 判断如果x是否是一个对象，判断函数是否是对象的方法有：typeof instanceof constructor toString
    if ((typeof x === 'object' && x != null) || typeof x === 'function') {
        let called
        try { // 预防取.then的时候错误
            let then = x.then // Object.definePropertype
            if (typeof then === 'function') {
                // 用then.call()为了避免在使用一次x.then报错
                then.call(x, y => {
                    // resolve(y)// 采用promise的成功结果，并且向下传递
                    if (called) {
                        return
                    }
                    called = true
                    // y有可能是一个promise，那么我们就要继续使用回调函数,直到解析出来的值是一个普通值
                    resolvePromise(promise2, y, resolve, reject)
                }, r => {
                    if (called) {
                        return
                    }
                    called = true
                    reject(r)// 采用promise的失败结果，并且向下传递
                })
            } else {
                if (called) {
                    return
                }
                called = true
                resolve(x)// x不是一个函数，是一个对象
            }
        } catch (err) {
            if (called) {
                return
            }
            called = true
            reject(err)
        }
    } else {
        // x是一个普通值
        resolve(x)
    }
}m
class MyPromise {
    constructor（opt） {
        this.status = 'pendding' // 状态值
        this.sucValue = void 0
        this.rejValue = void 0
        this.resolveQuenue = [] // 成功的任务队列，存放成功任务 
        this.rejectQuenue = [] // 失败的任务队列，存放失败任务
        let reslove = (value) => {
            if (this.status === 'pendding') {
                this.status = 'fulfilled'
                this.sucValue = value
                // 执行发布
                this.resolveQuenue.forEach（ fn => fn() ）
            }
        }
        let reject = (value) => {
            if (this.status === 'pendding') {
                this.status = 'rejecetd'
                this.rejValue = value
                this.rejectQuenue.forEach( fn => fn() )
            }
        }
        try {
        	opt(resolve， rejcet)；// 执行函数
        } catch(e) { // catch到错误 用reject 接收
            reject（e）
        }
    }
    }，
    then（resolveFn， rejectedFn）{
        // 成功状态执行成功的value
        if（this.status === 'fulfilled'）{
            resolveFn(this.sucValue)
        } 
        // 失败状态执行失败的value
        if (this.status === 'rejected') {
            rejectedFn(this.rejValue)
        }
        // 利用订阅发布设计模式 解决异步代码一直时pendding 问题
        // pendding 状态
        if (this.status === 'pendding') {
            this.resolveQuenue.push( () => {
                resolveFn(this.sucValue) // pendding状态订阅：往成功任务队列中推入一个函数
            })
            this.rejectQuenue.push( () => {
                rejectFn(this.rejValue) //  pendding状态订阅：往失败任务队列中推入一个函数
            })
        }
        
    }，
}

new MyPromise ((res, rej) => {
    setTimeOut( () => {
        res('success')
    }, 1000)
}).then(res => { // 此时这里并不会去执行
    console.log(res)
})
```
# 关于async/await

关于async 的作用

```javascript
time = function（）{
    console.log('1-1');
}
time()； // undefined
var asyFn = asycn function() {
    setTimeout( () => console.log('1-2'), 1000)
}
asyFn() // 返回promise 对象 
*** async 作用：返回的promise对象 和 Promise.resolve() 接受参数返回的值是一样

async/await 用同步的方式写异步的代码
let asy1 = async () => {
    let value = await Promise.resolve(10);
    
    console.log(value) //  这里是value是10，而不是undefined
    // 也就是说 await 等待一个promise 返回值， 或者其它的值；
    // 代码会等 await 后面 “表达式“有结果后，才继续执行 下面的代码。 （同步的方式， 去写异步的代码）
    
}
asy1.then(res => {
    console.log(res) // undefined
})


总结： async 函数返回一个promise 对象可以进行then执行， async函数里面遇到 await后不会执行，等待其有结果后才继续下面的代码 
async的函数提高了代码阅读性，精简了繁琐的promise链式调用
function p(druation) {
    return new Promise((res, rej) => {
        setTimeout(() => {
            res(druation + 200)
        }, druation)
    })
}

// 用promise来实现 计时器的 延时调用
function doP() {
    const time = 3000
    p(time)
        .then(res => {
        console.log(res); // 3200
        return p(res)
    }).then(res => {
        console.log(res) // 3400
        return p(res)
    }).then(res => {
        console.log(res) // 3600
    })
}


// 用async/await 来执行
async function asyDoP() {
    const time = 3000;
    let time1 = await p(time)
    let time2 = await p(time1)
    await p(time2)
}
asyDoP();  // 简单易懂
```

## 关于生成器函数

生成器执行返回一个 可迭代对象（生成器对象）

```javascript
function* y（） {
	yield 1；
    yield 2；
}
let iterator = y(); // iterator 可迭代对象
// 该对象 拥有以下特质
1.  for of 循环 用于获取 yield 后面的值
2.  扩展运算符获取 yield
3.  调用next 方法，才是真正开始执行生成器函数，每次遇到yield 或者 return 就会返回一个对象 
{ value： 1, done: false }

// return 
return 的值不会被 for of 和 扩展运算符遍历到。 之恶能用 next（） 获取到return 的值
return 之后的 yield 不会被执行


// yield* 可以为委托给其他生成器或者可迭代对象。

function* g1() {
    yield 2;
    yield 3;
    yield 4;
}

function* g2() {
    yield 1;
    yield* g1();
    yield 5;
}

var ite1 = g2() // 生成器执行返回一个 可迭代对象
for (let i  of ite1) {
    console.log(i);
}




// next 的传参
如果给 next 方法传参， 那么这个参数讲作为上一次 yield 语句的返回值，

function* foo(x) {
    var y = 2 * (yeild x + 1);
    var z = yield y / 3;
    return x + y + z;
}

var a = foo(5) // 对象
a.next() // 6
a.next() // 这次 没有传参； 相当于 上一次 yield x +1 是 undefine / 3 时NaN
a.next() // NaN


var b = foo(5);
b.next() // 6;
b.next(12) // 这次进行了传参， 因此12作为 yield x + 1 是 12  ；  y = 24   24 / 3 = 8
b.next(13); // 5 + 24 + 13  = 42
```



浏览器地址输入 网址到呈现 内容的流程：1.输入url；

1. 获取url的ip 地址： 获取浏览器缓存 / 获取系统缓存 host 文件/  路由缓存 isp服务商缓存 DNS 服务器 返回ip / 递归查询、 迭代查询
2. 浏览器与目标服务器建立tcp 连接
   1. 第一次客户端向目标服务器发送 连接请求报文SYN1
   2. 目标服务器接受报文后， 同意建立连接，向客户端发出确认报文SYN1， ACK1
   3. 客户端接受到报文后， 再次向服务器发出报文， 确认已收到报文  连接建立成功
3. 浏览器通过http发送请求
   5.服务器处理请求， 并返回一个响应报文
4. 释放tcp连接 
   1浏览器所在的主机向服务器发出连接释放报文，返回停止发送数据
   服务器接收到报文后确认报文，然后将服务器上为传送完的数据发送完
   服务器数据传输完毕后，想客户端发送连接释放报文   

CommonJs 模块化  node.js 中

加载原理：第一次加载模块会加载整个模块， 再次用到去缓存中读取

exports 和 module.exports   输出的是一个值得拷贝；

1. 何为一个值得的拷贝： 在栈内存中重新开辟一块新的空间， 进行复制； 复制之后两个值互不影响

****es6的模块化输出的是值得引用： 在堆内存中 开辟一个内存地址，这个内存地址指向引用过来的哪个值，当引用发生改变， 会影响另一个值

1. CommonJs 模块是运行是加载， es6模块是编译是输出接口。

   CommonJs 这种设计加载理念符合后端的开发； 后端开发只需要做到一次加载， 因此启动服务器 进行加载即可。

   如果采用后端的这种加载方式， 那么很容易出现网页假死，因此 es6 在进行编译的时候， 就进行按需加载， 在引用之后，可直接调用接口。

2. 我们都知道 JS 是属于解释性语言：

   	简而言之， Js 如要在某个环境（解释器下）运行时转化WE

   而C++ Java 属于编译性语言， 而编译语言 在运行前， 首先对所有代码进行编译，生成中间文件， 然后在执行程序

3. **** js的编译过程

   首先 V8引擎对代码进行词法分析， 分割

   然后解析生成一个 AST（抽象语法书）

   变量声明提示： 注这里声明变量并不是你所 认为的 js引擎真的会把变量提升到执行期上下文的最顶端；

   而是， 遇到声明语句就要为变量分配内存， 分配内存时只是将变量默认为undefined

   最后 js引擎生成 cpu可以执行的机器代码

   代码执行完成）

   CommonJS 是运行的时候加载模块， 而es6模块化是 编译的时候加载;******

   **** CommonJs 输出的是一个值得拷贝， 而Es6 输出的是一个值得引用

require

```javascript

```



Es6的模块运行机制与CommonJs 不一样， JS引擎对脚本进行静态分析的时候（词法分析的时候）， 遇到加载模块命令

import， 就会生成一个只读的引用。 等到脚本真正执行时， 在根据这个只读的引用。 到被加载的哪个模块里面取值；

Es6 模块加载是动态的， 并不会想CommonJs一样去缓存值；