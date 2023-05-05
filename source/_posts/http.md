---
title: http
tags:
  - http
  - web前端
  - 面试
categories:
  - web前端
  - http
date: 2023-03-07 21:25:43
---
[知识点](https://juejin.cn/post/6994629873985650696)
# http状态吗
[link]()
1xx （临时响应）表示临时响应并需要请求者继续执行操作的状态代码。
2xx  表示成功
3xx 表示重定向
4xx 请求错误
5xx 请求没问题，服务器发生了错误
# get 和 post区别
[知乎](https://www.zhihu.com/question/28586791)
GET产生一个TCP数据包；POST产生两个TCP数据包。
w3c
GET在浏览器回退时是无害的，而POST会再次提交请求。
GET产生的URL地址可以被Bookmark，而POST不可以。
GET请求会被浏览器主动cache，而POST不会，除非手动设置。
GET请求只能进行url编码，而POST支持多种编码方式。
GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
GET请求在URL中传送的参数是有长度限制的，而POST么有。
对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
GET参数通过URL传递，POST放在Request body中。

他们底层都是tcp/ip， 你要愿意get耶可以增request-body， post也可以在url上带上参数。
对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；
而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。

# http 缓存方案 [zhihu](https://www.jianshu.com/p/c78b5de7a889)
一个网站，访问次数太多就会对服务器造成太多的压力， 为了提升性能所以要做出优化
http根据是否要向服务器发送请求，将缓存分为强缓存 和 对比缓存（协商缓存）

# 强缓存， 直接从缓存数据库中获取资源，无需向服务器发起强求
客户端向缓存仓库请求数据， 如果命中缓存， 切缓存没有失效那么直接取出数据
http中用来判断是否命中强缓存的字段为Expires和Cache-Control，Cache-Control优先级高于Expires。

expires 是一个绝对时间，表示在这个时间之前的请求都是可以直接从缓存中读取，该字段时http1.0时代的产物，目前http1.1时代该字段可以忽略了
cache-control 是响应头中的信息
cache-control：  public 和 max-age=7200，这是一个相对时间（单位：秒），这里代表资源的缓存在这个请求之后的2小时内都有效
强缓存的s tatus-code是200 （这里是注释）
强缓存时，这个注释会有两种情况：
from memory cache： 缓存资源在内存中，浏览器（或页面标签）关闭后内存中的缓存就会被释放，重新打开页面取不到该缓存。
from disk cache： 缓存资源在硬盘中，浏览器（或页面标签）关闭后硬盘中的缓存不会消失，下次进入页面还能从硬盘中获取。

# 对比缓存 客户端向服务器发送请求，带上缓存对比标志，经过服务端对比客户端提供的标志，发现缓存可用，返回304状态码，然后直接向缓存仓库获取资源。
如何对比标志来确认是否使用缓存？这里主要涉及到两种标志：

Last-Modified / If-Modified-Since
Etag / If-None-Match
1. Last-Modified / If-Modified-Since
当浏览器第一次访问一个资源的时候，服务器会在response header中返回一个Last-Modified，代表这个资源最后的修改时间，当浏览器再次访问这个资源的时候，会在request header中带上 If-Modified-Since，值为上次请求时服务器返回的 Last-Modified 的值，然后服务器根据资源上次修改的时间确认资源在这段期间内是否更改过，如果没有，则返回304，如果有，则返回200并返回最新的资源。

2. Etag / If-None-Match 
Etag / If-None-Match 与 Last-Modified / If-Modified-Since 的机制类似，不同的是，Etag是通过一个校验码来对比资源是否更改过的，而不是通过资源的修改时间。当一个资源修改时，其校验码也会更改。当浏览器请求资源时，服务器会返回一个Etag字段，然后浏览器下一次请求时，会带上 If-None-Match ，值为上次服务器返回的Etag的值，服务器经过校验码的对比后决定返回200或304。
3. Etag 的优先级高于 Last-Modified

# http 和 https 区别
在互联网初期，网页只是用来做一个简单的咨讯，浏览页，大家只要输入url就可以访问，因此也不存在安全性，几乎任何信息是以明文的方式在服务器上进行传输。 随着网络发展，类似于亚马逊等购物网站的出现，用户登陆，账户密码，网络支付等功能越来越常用化，网络安全性就显得尤为重要。
1. http ：超文本传输协议（Hypertext Transfer Protocol，http）是一个简单的请求-响应协议，它通常运行在TCP之上。它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。请求和响应消息的头以ASCII形式给出；而消息内容则具有一个类似MIME的格式。这个简单模型是早期Web成功的有功之臣，因为它使开发和部署非常地直截了当。
2. https： https （Hyper Text Transfer Protocol over SecureSocket Layer），是以安全为目标的 http 通道，在http的基础上通过传输加密和身份认证保证了传输过程的安全性。https 在http 的基础下加入SSL，https 的安全基础是 SSL，因此加密的详细内容就需要 SSL。 https 存在不同于 http 的默认端口及一个加密/身份验证层（在 http与 TCP 之间）。这个系统提供了身份验证与加密通讯方法。它被广泛用于万维网上安全敏感的通讯，例如交易支付等方面
1. HTTP是明文传输，不安全的，HTTPS是加密传输，安全的多
2. HTTP标准端口是80，HTTPS标准端口是443
3. HTTP不用认证证书免费，HTTPS需要认证证书要钱
4. 连接方式不同，HTTP三次握手，HTTPS中TLS1.2版本7次，TLS1.3版本6次
5. HTTP在OSI网络模型中是在应用层，而HTTPS的TLS是在传输层
6. HTTP是无状态的，HTTPS是有状态的

# http1.0 / http1.1 / http2
## http1.0
每一个请求建立一个TCP连接，请求完成后立马断开连接。
这将会导致2个问题：连接无法复用。
阻塞： 连接无法复用会导致每次请求都经历三次握手和慢启动。三次握手在高延迟的场景下影响较明显，慢启动则对文件类大请求影响较大。会导致带宽无法被充分利用，以及后续健康请求被阻塞。客户端同时发起的请求数目是固定的，如果太多就会排队阻塞。

## http1.1
+ 引入更多的请求方法类型PUT、PATCH、DELETE、OPTIONS、TRACE、CONNECT
+ 引入长连接，就是TCP连接默认不关闭，可以被多个请求复用，通过请求头connection:keep-alive设置
+ 引入管道连接机制，可以在同一TCP连接里，同时发送多个请求
+ 强化了缓存管理和控制Cache-Control、ETag/If-None-Match
+ 支持分块响应，断点续传，利于大文件传输，能过请求头中的Range实现
+ 使用了虚拟网络，在一台物理服务器上可以存在多个虚拟主机，并且共享一个IP地址、、
缺点： 主要是连接缓慢，服务器只能按顺序响应，如果某个请求花了很长时间，就会出现请求队头阻塞
虽然出了很多优化技巧：为了增加并发请求，做域名拆分、资源合并、精灵图、资源预取...等等
最终为了推进从协议上进行优化，Google跳出来，推出SPDY协议

# http2.0
+ 使用新的二进制协议，不再是纯文本，避免文本歧义，缩小了请求体积
+ 多路复用，同域名下所有通信都是在单链接(双向数据流)完成，提高连接的复用率，在拥塞控制方面有更好的能力提升
+ 使用HPACK算法将头部压缩，用哈夫曼编码建立索表，传送索引大大节约了带宽
+ 允许服务端主动推送数据给客户端
+ 增加了安全性，使用HTTP 2.0，要求必须至少TLS 1.2
+ 使用虚拟的流传输消息，解决了应用层的队头阻塞问题
# http1.1 和  http2.0
HTTP1 和 HTTP2

HTTP2是一个二进制协议，HTTP1是超文本协议，传输的内容都不是一样的
HTTP2报头压缩，可以使用HPACK进行头部压缩，HTTP1则不论什么请求都会发送
HTTP2服务端推送(Server push)，允许服务器预先将网页所需要的资源push到浏览器的内存当中
HTTP2遵循多路复用，代替同一域名下的内容，只建立一次连接，HTTP1.x不是，对域名有6~8个连接限制
HTTP2引入二进制数据帧和流的概念，其中帧对数据进行顺序标识，这样浏览器收到数据之后，就可以按照序列对数据进行合并，而不会出现合并后数据错乱的情况，同样是因为有了序列，服务器就可以并行的传输数据，这就是流所做的事情。HTTP2对同一域名下所有请求都是基于流的，也就是说同一域名下不管访问多少文件，只建立一次连接

# 同源策略， 
协议，域名，端口不一致就会触发（跨域）同源策略，是一种浏览器的安全机制

# 跨域解决方法
## cors cross-origin-resource-sharing 跨域资源共，这种方法跟同源的ajax访问没有区别，
1. 在请求中增加头部自动 origin： xxxx  包含请求页面的源信息（协议、域名和端口） 服务端根据这个自动判断是否可以响应
2. 如果服务器认为这个请求可以接受，就在 Access-Control-Allow-Origin 头部中回发相同的源信息（如果是公共资源，可以回发 * ）。例如：Access-Control-Allow-Origin：http://www.laixiangran.cn
3. 没有这个头部或者有这个头部但源信息不匹配，浏览器就会驳回请求。正常情况下，浏览器会处理请求。注意，请求和响应都不包含 cookie 信息。
4. 如果需要包含 cookie 信息，ajax 请求需要设置 xhr 的属性 withCredentials 为 true，服务器需要设置响应头部 Access-Control-Allow-Credentials: true。

有点支持所有http请求

## jsonp 动态插入script 标签，通过script 标签的src 进行跨域
```javascript
  // 1. 定义一个 回调函数 handleResponse 用来接收返回的数据
function handleResponse(data) {
    console.log(data);
};

// 2. 动态创建一个 script 标签，并且告诉后端回调函数名叫 handleResponse
var body = document.getElementsByTagName('body')[0];
var script = document.gerElement('script');
script.src = 'http://www.laixiangran.cn/json?callback=handleResponse';
body.appendChild(script);

// 3. 通过 script.src 请求 `http://www.laixiangran.cn/json?callback=handleResponse`，
// 4. 后端能够识别这样的 URL 格式并处理该请求，然后返回 handleResponse({"name": "laixiangran"}) 给浏览器
// 5. 浏览器在接收到 handleResponse({"name": "laixiangran"}) 之后立即执行 ，也就是执行 handleResponse 方法，获得后端返回的数据，这样就完成一次跨域请求了。
```
优点， 简单方便， 没有兼容性的问题
缺点 只支持get请求

## postMessage html5新引入的特性，用它可以向window对象发送消息
```javascript
  
  <iframe src="http://laixiangran.cn/b.html" id="myIframe" onload="test()" style="display: none;">
  <script>
    // 1. iframe载入 "http://laixiangran.cn/b.html 页面后会执行该函数
    function test() {
        // 2. 获取 http://laixiangran.cn/b.html 页面的 window 对象，
        // 然后通过 postMessage 向 http://laixiangran.cn/b.html 页面发送消息
        var iframe = document.getElementById('myIframe');
        var win = iframe.contentWindow;
        win.postMessage('我是来自 http://www.laixiangran.cn/a.html 页面的消息', '*');
    }
</script>
```


```javascript
  <script type="text/javascript">
    // 注册 message 事件用来接收消息
    window.onmessage = function(e) {
        e = e || event; // 获取事件对象
        console.log(e.data); // 通过 data 属性得到发送来的消息
    }
</script>
```

# cookie / session / localstorage / sessionstorage/ indexdb详解
[juejin](https://juejin.cn/post/7137247442038161444)
## indexdb
```javascript
  var db;
var request = window.indexedDB.open(dbName, version);
request.onsuccess = function(event) {
    db = request.result;
}

request.onerror = function(event) {
    alert('error');
}

```
```javascript
  // 在onupgradeneeded事件内操作
request.onupgradeneeded = function(event) {
    db = event.target.result;
    var objectStore = db.createObjectStore('item', {
        keyPath: 'id',
        autoIncrement: true
    });
    
    /**
     * 创建索引通过id搜索：
     * objectStore.createIndex(indexName, keyPath, objectParameters)
     *    indexName: 创建的索引名，可为空
     *    keyPath: 索引使用的关键路径，可为空
     *    objectParameters: 可选参数，unique标识该字段是否唯一，不能重复 
     */
    objectStore.createIndex('id', 'id', { unique: true });
    
    // 添加信息到数据库
    objectStore.add({ name: 'nnn', age: 'aa'});
}

// 在onupgradeneeded事件外操作
// 新建事务
var transaction = db.transaction(['item'], 'readwrite');

// 创建存储对象
var objectStore = transaction.objectStore('item');

// 添加数据
objectStore.add({ name:'nnn', age:'aa' });
```

```javascript
  // 在onupgradeneeded事件内操作
request.onupgradeneeded = function(event) {
    db = event.target.result;
    var objectStore = db.createObjectStore('item', {
        keyPath: 'id',
        autoIncrement: true
    });
    
    /**
     * 创建索引通过id搜索：
     * objectStore.createIndex(indexName, keyPath, objectParameters)
     *    indexName: 创建的索引名，可为空
     *    keyPath: 索引使用的关键路径，可为空
     *    objectParameters: 可选参数，unique标识该字段是否唯一，不能重复 
     */
    objectStore.createIndex('id', 'id', { unique: true });
    
    // 添加信息到数据库
    objectStore.add({ name: 'nnn', age: 'aa'});
}

// 在onupgradeneeded事件外操作
// 新建事务
var transaction = db.transaction(['item'], 'readwrite');

// 创建存储对象
var objectStore = transaction.objectStore('item');

// 添加数据
objectStore.add({ name:'nnn', age:'aa' });
```


