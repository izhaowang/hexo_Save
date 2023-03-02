---
title: websever
tags:
  - 网站服务器
  - node
categories:
  - web前端
date: 2023-03-03 00:10:48
---
# 模版字符串
解决 搭建网站服务器，实现增删改查 这节中 因为大量字符串拼接过于繁琐，而且容易出错；

模板引擎是第三方模块；   让开发者已更加友好的方式拼接字符串； 更加容易维护

# art-template 模板引擎
1. 使用 npm install art-template 命令进行下载
2. 使用const template = require('art-template'); 引入模板引擎
3. 告诉模板引擎 要拼接的数据和模板在哪  const html = template('模板路径'， 数据)
```javascript
    const template = require('art');
    // 这里的后缀名.art 是固定的写法，这是模板引擎规定的； 但是里面还是写的html语法
    const html = template('./views/index.art', {
        data: {
            name: '张三'，
            age: 20
        }     
    })

    // views/index.art 模板代码
    <div>
        <span>
            {{data.name}} // data就是 template方法的第二个参数
        </span>
        <span>
            {{data.age}}
        </span>
    </div>
```
# 模板语法
art-template 支持两种模板语法： 标准语法和原始语法。
+ 标准语法 可读性强
+ 原始语法 逻辑性强
## 输出
```javascript
{% raw %}
标准语法 {{ XXX }}
原始语法 <%= 数据 %>
{% endraw %}
```
```javascript
{% raw %}

    // 标准语法
    <h2>{{value}}</h2>
    <h2>{{a? b: c}}</h2>
    <h2>{{a + b }}</h2>

    // 原始语法
    <h2><%= value %></h2>
    <h2><%= a ? b: c %></h2>
    <h2><%= a + b %></h2>
    {% endraw %}

```
# 原文数据
如果数据是HTML标签， 默认模板引擎不会解析标签， 回将其转义后输出。

{% raw %}
+ 标准语法： {{@ 数据 }}
+ 原始语法： <%- 数据 %>
{% endraw %}
```javascript
{% raw %}

    // 标准语法
        <span>
            {{data.name}} // data就是 template方法的第二个参数
        </span>
        <span>
            {{data.age}}
        </span>
        <span>{{@ data.content }}</span>

     //原始语法
        <span>
            <%= data.name %> // data就是 template方法的第二个参数
        </span>
        <span>
            <%= data.age %>
        </span>
        <span>
            <%= 1+1 %>
        </span>
        <span> <%- data.content %></span>
{% endraw %}

```

# 条件判断
在模板中可以根据条件条件来决定显示哪块HTML代码。
```javascript
{% raw %}

    // 标准语法
    {{if 条件}} ... {{/if}}
    {{if v1}}...{{elese if v2}} ... {{/if}}


    <span>
            {{if data.age > 20}}
                我已经老了
            {{else if data.age <= 20}}
                我还很年轻
            {{ else}}
                年龄不符合要求
            {{/if}}
     </span>




    // 原始语法
    <% if (value) { %> ...... <% } %>
    <% if (v1) { %> ...... <% }  else if (v2) { %>  ...... <% } %>



        <% if(data.age > 20) { %>
            我是vn
        <% } %>

        <% if(data.age <= 20) {%>
            我是女枪
        <% }  else if (data.age > 20) { %> 
            我是女警
        <% } else  {%>
            年龄不符合要求 
        <% } %>
    {% endraw %}

```

# 循环语法
{% raw %}
+ 标准语法 {{each 数据}} {{/each}}
+ 原始语法 <% for() { %> <% } %>
{% endraw %}
```javascript
{% raw %}

    // 标准语法
    <ul>
        {{each users}}
            <li>
                // 这里的$value 相当于个每个循环想。相当于forEach中的item
                {{$value.name}}
                {{$value.age}}
                {{$value.sex}}
            </li>
        {{/each}}
    </ul>
{% endraw %}

```
```javascript
{% raw %}

    // 原始语法
    <ul>
        <% for (var i = 0; i < users.length; i++) { %>
                <li>
                    <%= users[i].name %>
                    <%= users[i].age %>
                    <%= users[i].sex %>
                </li>
        <% } %>
    </ul>
{% endraw %}
```

# 子模版
使用子模版可以将网站的公共区块（头部， 底部）抽离到单独的文件中。 抽离出的文件叫做子模版
{% raw %}
+ 标准语法 {{include '子模板路径'}}
+ 原始语法 <% include('子模版路径')%>
{% endraw %}
```javascript
{% raw %}
    // 标准语法
    {{ include './header.art'}}


   // 原始语法
   <% include('./header.art') %>
    {% endraw %}

```
# 模板的继承
模板继承可以将网站的HTML骨架抽离到单独的文件中， 其他页面模板可以继承 骨架文件; 
HTML骨架 html 标签 body header标签； 这些不能用子模版语法生效； 所以需要继承
```javascript
    // 抽离出模板骨架
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        // 用block 去创建一个插槽
        {{block 'header'}} {{/block}}
    </head>
    <body>
        {{block 'content'}} {{/block}}
    </body>
    </html>

    
     // 在其他文件中 继承，并填充这个骨架
     {{ extend './layout.atr' }}
     {{ block 'head'}} <link rel="stylesheet" href="custom.css"> {{/block}}
     {{ block 'content' }} <div>我是填充内容</div> {{ /block }}
```


# 模板配置
1. 想模板中导入变量 template.defaults.imports.变量名 = 变量
```javascript
    
const template = require('art-template'); 
const path = require('path');

// 在模板中导入 cc变量名是 字符串'heel'
template.defaults.imports.cc = 'heel';
const html = template(path.join(__dirname,'views', 'index.art'), {
    users: [{
        name: 'zhangsan',
        age: 21,
        sex: 'nan'
    }, {
        name: 'lisi',
        age:22,
        sex: 'nan'
    }, {
        name: 'mali',
        age: 24,
        sex: 'nv'
    }]
})
console.log(html)



//  在index.art 中使用 cc变量

<h1>
    {{cc}}
</h1>
```
2. 全局配置模板的根目录， 和设置axios 的 defaultBaseURL 差不多
```javascript
// 设置模板的根目录
template.defaults.root = path.join(__dirname, 'views')

// 这里template 的第一个参数直接写要使用模板的文件名即可
const html = template('index.art', 数据)
```

3. 设置模板默认后缀 template.defaults.extname = ".art";
```javascript
// 设置模板默认后缀 
template.defaults.extname = ".art";

// 设置模板的根目录
template.defaults.root = path.join(__dirname, 'views')

// 这里由于已经设置了根目录和默认后缀， 那么只需要写index即可不需要 写成 index.art
const html = template('index', 数据)
```
*** 查看 4ce8adf 提交记录 ***
// 搭建网站服务器, 实现增删改查 实现客户端和服务器的通信,
// 连接数据库, 创建用户集合, 向集合中插入文档,
// 当用户访问/list时, 将所有用户信息查询出来
// 将用户信息和表格HTML进行拼接并将拼接结果响应回客户端
// 当用户访问/add时, 呈现表单页面, 并实现添加用户信息功能

    // 1. 增加页面路由 呈现页面
        // 点击修改时 根据点击的id 将id传递到当前页面
        // 从数据库中查询当前用户信息, 将用户信息展示到页面
    // 2. 实现用户修改功能
        // 1. 指定表单的体骄傲地址以及请求方式
        // 2. 接受客户端传递过来的修改信息 找到用户， 将用户信息改为最新的信息
// 当用户访问/modify时, 呈现修改页面, 并实现修改用户信息功能
// 当用户访问/delete时, 实现用户删除功能

// ****
// 用模板引擎解决字符串拼接的问题
// ****
```javascript
const http = require('http');
const url = require('url');
const queryString = require('querystring');
require('./model/index'); // 引入数据库连接
const User = require('./model/user'); // 引入用户构造函数

// 创建服务器
const app = http.createServer();

// 为服务器对象添加请求事件
app.on('request', async (req, res) => {
    // 请求方式
    const method = req.method;
    // console.log(req)
    const {pathname, query} = url.parse(req.url, true);

    if (method === 'GET') {
        if (pathname === '/list') {
            // 查找数据库
            let users = await User.find()
            // 如是访问地址时list, 那么呈现列表页面
            let list = `
            <!DOCTYPE html>
                <html lang="en">
                <head>
                    <meta charset="UTF-8">
                    <meta http-equiv="X-UA-Compatible" content="IE=edge">
                    <meta name="viewport" content="width=device-width, initial-scale=1.0">
                    <title>Document</title>
                </head>
                <body>
                    
            `;
            users.forEach(item => {
                list += `
                    <h1>
                        我是${item.name}, 我今年${item.age}了,
                        我的email是${item.email};
                        我的爱好
                `;
                item.hobbies.forEach(item => {
                    list += `${item},`
                })
                list += `
                    <a href="" >删除</a>
                    <a href="/modify?id=${item._id}" >修改</a>
                
                </h1><hr/>`
            })
            list += `
            </body>
            </html>
            `
            res.end(list)
        } else if (pathname === "/add") {
            let list = `
            <!DOCTYPE html>
            <html lang="en">
            <head>
                <meta charset="UTF-8">
                <meta http-equiv="X-UA-Compatible" content="IE=edge">
                <meta name="viewport" content="width=device-width, initial-scale=1.0">
                <title>Document</title>
            </head>
            <body>
                <h1>
                    我是html
                </h1>
                <button>
                    添加用户
                </button>
                <form method="post" action="/add">
                    <label for="">名字</label>
                    <input type="text" name="name">
                    <label for="">年龄</label>
                    <input type="number" name="age">
                    <label for="">邮箱</label>
                    <input type="email" name="email">
                    <label for="">爱好</label>
                    <input type="checkbox" name="hobbies" id="" value="足球">足球
                    <input type="checkbox" name="hobbies" id="" value="睡觉">睡觉
                    <input type="checkbox" name="hobbies" id="" value="吃饭">吃饭
                    <button type="submit">提交</button>
                </from>
            </body>
            </html>
            `;
            res.end(list)
        } else if (pathname === '/modify') {
            // ****
            // query 时url.parse(req.url, true) 第二个参数传true 返回一个对象可以得到id
            // ****
            console.log(query.id)

            // 在数据库中拿到数据
            let user = await User.findOne({_id: query.id});
            let hobbies = ["足球", "吃饭", "睡觉"];
            console.log(user)
            let modify = `
            <!DOCTYPE html>
            <html lang="en">
            <head>
                <meta charset="UTF-8">
                <meta http-equiv="X-UA-Compatible" content="IE=edge">
                <meta name="viewport" content="width=device-width, initial-scale=1.0">
                <title>Document</title>
            </head>
            <body>
                <h1>
                    我是html
                </h1>
                <button>
                    修改用户
                </button>
                <form method="post" action="/modify?id=${user._id}">
                    <label for="">名字</label>
                    <input type="text" name="name" value="${user.name}">
                    <label for="">年龄</label>
                    <input type="number" name="age" value="${user.age}">
                    <label for="">邮箱</label>
                    <input type="email" name="email" value="${user.email}">
                    <label for="">爱好</label>
                   
                   
            `;
            // 用所有爱好去循环 user对象；
            // 如果包含了 那么就添加 checked 属性
            hobbies.forEach(item => {
                let isHobby = user.hobbies.includes(item);
                if (isHobby) {
                    modify += `<input type="checkbox" name="hobbies" id="" value="${item}" checked>${item}`
                } else {
                    modify += `<input type="checkbox" name="hobbies" id="" value="${item}" >${item}`
                }
            })
            modify += `
            <button type="submit">修改用户</button>
            </from>
            </body>
            </html>
            `
            res.end(modify)
        }
        
        
    } else if (method === 'POST') {
        if(pathname === '/add') {
            console.log(123);
            let formData = '';
            // 接受post参数
            req.on('data', (params) => {
                formData += params
            });

            // post参数接受完毕
            req.on('end', async () => {
                console.log(formData)
                console.log(queryString.parse(formData));
                await User.create(queryString.parse(formData));
                // **** 
                // 添加成功重定向到 /list 地址
                // 301 表示重定向
                // ****
                res.writeHead(301, {
                    location: '/list'
                })
            })
        } else if (pathname === '/modify') {
            let formData = '';
            // 接受post参数
            req.on('data', (params) => {
                formData += params
            });

            // post参数接受完毕
            req.on('end', async () => {
                console.log(formData)
                // console.log(queryString.parse(formData));
                // 用queryString 可以拿到post请求的参数
                let user = queryString.parse(formData);
                
                // updateOne 第一个参数是查询条件， 第二个参数是呀修改的数据
                await User.updateOne({_id: query.id}, user);
                // **** 
                // 添加成功重定向到 /list 地址
                // 301 表示重定向
                // ****
                res.writeHead(301, {
                    location: '/list'
                })
                res.end()
            })
        }
    }
});
app.listen(3000);
```
