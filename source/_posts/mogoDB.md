---
title: mogoDB
tags:
  - 数据库
  - node
categories:
  - web前端
date: 2023-03-03 00:11:11
---
# 数据库： 
存储数据的仓库， 将数据进行分门别类的存储， 它本身是一个软件， 但是可以通过api 去操作它
数据库软件 mysql， mongDB、 oracle
mongoDB api 使用js语法；  mysql =》 php语言
node.js 可以操作 mysql

# 安装mongoDB 和 可视化软件 mongoDBcompass；

# 数据库相关概念
| 术语 | 解释 |
|---|---|
|dataBase|数据仓库，mongoDB可以创建多个数据仓库|
|collection|集合， 表示一组数据的集合；类似于js中的数组|
|document|文档， 表示具体的一条数据，类似于js中的对象|
|field|字段， 文档中的属性名称，类似于js中对象的属性|

![数据库和集合](https://images.gitee.com/uploads/images/2021/1111/113223_17be9e73_6525038.png "屏幕截图.png")


![整个列表就是文档， 每一条数据就是字段](https://images.gitee.com/uploads/images/2021/1111/113355_5ca7bf7a_6525038.png "屏幕截图.png")

# node.js 操作数据库； 
操作数据需要 借助第三方包 ： Mongoose
可以借助这个包进行数据库链接 和 其他操作

1. 首先再磁盘G中的node文件夹中建立 dataBase 文件夹
2. 然后用powershell 进入改文件夹下
![输入图片说明](https://images.gitee.com/uploads/images/2021/1111/113919_638870ab_6525038.png "屏幕截图.png")
3. 执行 npm install mongoose
4. 安装第三方包 mongoose 完成
5. 启动mongoDB服务， 就是打开数据库
   net start mongodb 启动
   net stop mongodb
   ![输入图片说明](https://images.gitee.com/uploads/images/2021/1111/114304_bbc2664e_6525038.png "屏幕截图.png")
6. 用mongoose 链接数据库
    创建01.js文件；
   01.js中
   ```;javascript
    // 引入 mongoose
    const mongoose = require("mongoose");
    // mongoose.connect("mongodb/数据库地址/数据库名称")
    mongoose.connect('mongodb://localhost/playground')
    .then(
        () => { console.log('链接成功')}
    )
    .catch(err => console.log("数据库连接失败"))
   ```
 *** 再mongodb中不需要显示的创建数据库， 你可直接使用数据库， 当这个数据不存在时，它会自动帮你创建一个数据库 ***

# 数据库中 增、 删、 改、 查

1. 创建集合。 并为集合设置集合规则
   然后才能插入文档
   1.1 首先你要对创建的集合设定规则，
   1.2 然后才去创建集合 mongoose.Schema 构造函数创建
```;javascript
   //  设定集合规则
    const courseSchema = new mongoose.Schema({
        name: String,
        author: String,
        isPublished: Boolean
    })
    

   // 创建集合并应用规则 第一个参数表示集合名称 Courses再集合里面会自动加s， 第二个参数表示为这个集合应用哪个规则
   const Course = mongoose.model('Course', courseSchema); // Course
```
2. 插入数据（文档），
    2.1 创建集合实例 上面的Course  就是一个构造函数
    2.2 调用实例下的 save 方法将数据保存到数据库中
```;javascript
    // 创建集合实例-- 创建文档
const course = new Course({
    name: 'node.js实战',
    author: 'cc',
    isPublished: true
})

// 将文档插入到数据库
course.save()
```
![输入图片说明](https://images.gitee.com/uploads/images/2021/1111/151722_69fefab0_6525038.png "屏幕截图.png")

3. 插入数据（文档）另一种方式
```;javascript
    Course.create({
        name: 'javascript',
        author: 'cc',
        isPublished: true
    }, (err, result) => {console.log(err); console.log(result)});

    // 数据库操作都是异步的， 所以你也可以使用 promise的方式去进行结果的处理
    Course.create({
        name: 'javascript',
        author: 'cc',
        isPublished: true
    })
        .then(doc => console.log(doc))
        .catch(e => console.log(e))
```

4. 向数据库中导入数据
[参考连接](https://blog.csdn.net/weixin_44517301/article/details/114823073)
```;javascript
    // 将现成的数据插入到数据库中 通过命令行工具 mongoimport
    mongoimport -d 数据库名称 -c 集合名称 --file 要导入的数据文件

    // 1. 首先你需要将mongoimport 添加到系统环境变量中去 然后才能再命令行中使用 mongoimport
    
    // 2. 建立一个json文件， 命名为user.json
    
    // 3. 确定已经将环境变量path 添加了 mongoimport

    // 4. 再命令行中使用命令 mongoimport -d playground -c users --file user.json 
```

5. 在数据库的集合中查询文档 
## find 方法 *** 注意： find 方法返回的永远是一个数组， 哪怕只有一条数据， 也是一个数组； 如果返回的数据不存在那么 返回空数组 ***
```;javascript
const mongoose = require("mongoose"); // 引入 mongoose

mongoose.connect('mongodb://localhost/playground') // 连接数据库
    .then(() => console.log('链接chengg'))
    .catch(err => console.log(err));

// 设定集合规则
const courseSchema = new mongoose.Schema({
    name: String,
    author: String,
    isPublished: Boolean
});

// 创建集合构造函数
const Course = mongoose.model('Course', courseSchema);

// 利用集合的构造函数下的 find方法 find() 里面没有参数会查询到所有文档数据
Course.find().then(result => console.log(result))

// 查询某个指定id 的文档数据

Course.find({_id: '618cc3216e019fb68cb4959c'}).then(result => console.log(result));
*** 注意： find 方法返回的永远是一个数组， 哪怕只有一条数据， 也是一个数组； 如果返回的数据不存在那么 返回空数组 ***

```

## 用findOne 查找， 该方法返回一条 数据； 默认返回集合中第一条文档

```;javascript

Course.findOne().then(res => console.log(res)) // 默认返回第一条文档

// 条件查找 查找 name 是javascript 的数据
Course.findOne({name: 'javascript'}).then(res => console.log(res));
```

## 条件查找 如： 年龄大于 多少 年龄小于多少

    User.find({age: {$gt:0, $lt:20}})  gt=== great than  lt === less than
    find 接受一个对象， age 属性值也是一个对象 {$gt:0, $lt:20}
```;javascript
 User.find({age: {$gt:0, $lt:20}}).then(res => console.log(res));

```

## 查询爱好包含足球的, 使用{$in: ['敲代码'， '打豆豆']}；

```;javascript
    User.find({bobbies: {$in: ['敲代码', '打豆豆']}}).then(res => console.log(res));
```

## 查询出固定的字段， 比如数据只包含 name；find方法后面链式调用select 方法
```;javascript
    User.find().select("name").then(res => console.log(res));
```
如图只有名字 没有age 和 hobbies 字段
![输入图片说明](https://images.gitee.com/uploads/images/2021/1112/134044_f57d6895_6525038.png "屏幕截图.png")

```;javascript
    // 如果你不需要_id 那么你只需要在_id 前使用-
    User.find().select("-name -_id age").then(res => console.log(res));
```

## 对查询出来的数据进行排序， 年龄从小到大排列 或者从大到小降序（-age）排列  
```;javascript
    // sort 从小到大升序 排列
    User.find().sort("age").then(res => console.log(res));


    // sort 从大到小降序 排列
    User.find().sort("-age").then(res => console.log(res));
```

## skip limit 讲解 分页功能可以用到
```;javascript
    // skip(2) 查询出来的数据 跳过前两个文档

    // limit(2) 不管结果有多少条数据 我只显示两条数据
    User.find().skip(2).limit(2).then(res => console.log(res));

```

# 删除文档

```;javascript
   // findOneAndDelete() 删除单个 方法里面的参数表示的是 条件 ， 如果匹配到多条文档 也只会删除第一条数据
    // findOneAndDelete 方法返回 删除的那个数据
   User.findOneAndDelete({age: 5}).then(res => console.log(res))


  // 删除多条数据  如果条件写 空对象{}； 那么他会删除整个集合；
    // deleteMany 方法返回一个对象 { deletedCount: 2 } deletedCount 代表删除了几条数据；
   User.deleteMany({}).then(res => console.log(res)) 
```

# 跟新文档

```;javascript
   // updateOne({查询条件}， {要修改的值}) 接受两个参数
   User.updateOne({age: 5}, ).then(res => console.log(res))
    // 返回值 {
      acknowledged: true,
      modifiedCount: 1,
      upsertedId: null,
      upsertedCount: 0,
      matchedCount: 1
    }


    // updataMany 一次更新多个文档；
    updataMany({查询条件}， {要修改的值}) 接受两个参数

    User.updateMany({}, {age: 40}).then(res => console.log(res));
    // 将所有的数据的 age 变为40
    // 返回值 {
      acknowledged: true,
      modifiedCount: 2,
      upsertedId: null,
      upsertedCount: 0,
      matchedCount: 3
    }
```

# mongoose 验证
在创建集合规则时， 可以设置当前字段的验证规则， 验证失败就插入失败

 1. required: true 表示该字段必传
 2. maxlength 字符串最大长度
 3. minlenght 字符串最小长度
 4. trim 插入时是否去除字符串前后空格
 5. min 数值最小2
 6. max 数值
 7. default 默认值
 8. enum: ["javascript", "c", "php"] // 表示当前字段可以填入 的值
 9. validate: { // 自定义的函数
        validator: v => { v && v.length > 4},
        message: '传入的值不符合验证规则' // 自定义错误信息
    }
```
const userSchema = new  mongoose.Schema({
    name: {
        type: String,
        required: ['true', '请传入文章标题'] // 第二个参数表示 报错信息
        maxlength: 2, // 最大长度
        minlength: [0, 'name 长度不能小于0']
        tirm: true //去除前后空格
    },
    age: {
        type: Number,
        min:10,
        max: 90
    },
    hobbies: [String],
    date: {
        type: Date // 日期
        default: Date.now // 默认当前时间
    }
});
// 创建一个集合构造函数
const User = mongoose.model('user', userSchema);
User.create({name: 'wanggoudaner'}).then(res => console.log(res))
```

# 集合关联
通常情况下， 不同的集合之间是有关系的； 例如文章信息和用户信息存储在不同的集合中； 但文章是某个用户发表的。 要查询文章所有的信息也包括了了作者的年龄，名字， 爱好等（用户信息）； 此时就需要用到集合关联。
![输入图片说明](https://images.gitee.com/uploads/images/2021/1115/101619_b4348fc8_6525038.png "屏幕截图.png")
在文章集合中 的author字段 中 和用户集合的 _id 进行关联
***用popolate 进行关联
```;javascript
    // 用户集合
    const User = mongoose.model('User', new mongoose.Schema({
        name: {type: String}
    ))

    // 文章集合
    const Post = mongoose.model('Post', new mongoose.Schema({
        title: {type: String},
        // 使用ID将文章集合和作者集合进行关联
        author: {type: mongoose.Schema.Types.ObjectId, ref: 'user'}
    }));
    
    // 插入字段 这里author 的id 是 user里面的 _id 值
    Post.create({title: 'kuangren', author: '618e04e5e4328b7a4c2ccad3'}).then(result => console.log(result)).catch(err => console.log(err));
    

    // 联合查询 author 字段关联的集合
    Post.find()
        .populate('author')
        .then((err, result) => console.log(result));
```
