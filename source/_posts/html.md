---
title: html
tags:
  - html
  - web前端
categories:
  - web前端
date: 2023-03-06 21:38:31
---
# html属性
## 全局属性
1. accesskey 可以给元素添加快捷键,当你使用alt+n后会自动聚焦
```html
  <input accesskey="n">
```
2. dir 属性值后rtl 和 ltr 表示从右到左和从⬅️到右显示文本
3. draggable dropzone 
4. hidden 
5. lang 表示元素的使用语言
```html
  <p lang="en">hello world</p>
   <p lang="ch">你好-</p>

```
6. spellcheck 表面浏览器是否检查拼写， 该属性只有用在用户可以编辑的内容上才有效果
7. tabindex 表面是否可以用tab切换各元素的聚集， tabindex=‘1’ 可以用数字表示顺序
8. title 鼠标移动到元素上会显示额外的信息
9. id， class， style 
10. contenteditable h5新增属性，可以让用户修改页面的内容，用来实现富文本编辑
# H5新增标签及其语义化理解
article、 section、 aside、 header、 nav、 main、 footer。。
audio， video， canvas标签
新增input type属性扩展 data， eamial， url， search， range， month，color， number
1. 利于搜索引擎优化， seo（Search Engine Optimization） 搜索引擎可以更准确地识别页面内容，从而提高搜索结果的质量
2. 更好的阅读性和可维护性，让html代码更加容易阅读和理解，也方便开发人员进行维护
3. 访问性，方便视障用户更好的访问页面内容
4. 兼容性，方便多端设备（屏幕阅读器， 盲人阅读器， 移动设备）和浏览器访问

# canvas 和 svg区别
1. canvas 是h5新增的标签 绘制出来的是一个标量图，可以用作大型游戏的画布， 其绘制图形通常是用js来实现的。并且可以引入jpg或者png，一旦绘制完成，浏览器不会进行关注，如果某个位置发生改变那么整个场景都要重新绘制。
2. svg 比较久远，绘制的是一个矢量图，所以经常用于地图， 基于xml，意味svg每个dom元素都能获取到，并且添加事件处理器。绘制完成后，svg的对象属性发生变化，浏览器会自动重现图形。

# Dom事件模型
DOM全称是Document Object Model，即文档对象模型。DOM是W3C的标准，定义了访问 HTML 和 XML 文档的标准
1. dom事件
当用户对dom对象进行各种操作时，做出的响应，可称之为事件。比如鼠标点击，键盘按下，拖拽等操作发生时，用js去处理（响应）这类操作的结果。
2. 事件流 dom结构是一个树形结构，当事件发生时，事件会在节点之间进行传播，所有经过的节点都是接受到这个事件
3. 事件模型 ie的冒泡模型（bubble）和 dom的标准事件处理模型 冒泡，捕获
冒泡型事件处理模型在事件发生时，首先在最精确的元素上触发，然后向上传播，直到根节点。反映到DOM树上就是事件从叶子节点传播到根节点。


标准的事件处理模型分为三个阶段：

  1. 父元素中所有的捕捉型事件（如果有）自上而下地执行
  2. 目标元素的冒泡型事件（如果有）
  3. 父元素中所有的冒泡型事件（如果有）自下而上地执行

4. 注册事件
+ 传统方式 
这种方式是无论在IE还是Firefox等其他浏览器上都可以成功运行的通用方式。这便是它最大的优势了，而且在Event处理函数内部的this变量无一例外的都只想被绑定的DOM元素，这使得Js程序员可以大大利用this关键字做很多事情。

　至于它的缺点也很明显，就是传统方式只支持Bubbling，而不支持Capturing，并且一次只能绑定一个事件处理器在DOM元素上，无法实现多Handler绑定。

```javascript
  window.onload=function(){…}
  obj.onmouseover=function(e){…}
  .onclick=function(){…}

```

+ w3c
addEventListener带有三个参数，第一个参数是事件类型，就是我们熟知的那些事件名字去掉前面的’on’，第二个参数是处理函数，可以直接给函数字面量或者函数名，第三个参数是boolean值，表示事件是否支持Capturing。

W3C的事件模型优点是Bubbling和Capturing都支持，并且可以在一个DOM元素上绑定多个事件处理器，各自并不会冲突。并且在处理函数内部，this关键字仍然可以使用只想被绑定的DOM元素。另外function参数列表的第一个位置(不管是否显示调用)，都永远是event对象的引用。

至于它的缺点，很不幸的就只有在市场份额最大的IE浏览器下不可使用这一点。
```javascript
window.addEventListener(‘load’,function(){…},false);
document.body.addEventListener(‘keypress’,function{…},false);
obj.addEventListener(‘mouseover’,MV,true);
function MV(){…}

```

+ ie event module
它跟W3C的区别是没有第三个参数，而且第一个表示事件类型的参数也必须把’on’给加上。这种方式的优点就是能绑定多个事件处理函数在同一个DOM元素上。

　　至于它的缺点，为什么如今在实际开发中很少见呢？首先IE浏览器本身只支持Bubbling不支持Capturing；而且在事件处理的function内部this关键字也无法使用，因为this永远都只想window object这个全局对象。要想得到event对象必须通过window.event方式，最后一点，在别的浏览器中，它显然是无法工作的。


```javascript
window.attachEvent(‘onload’,function(){…});
document.body.attachEvent(‘onkeypress’,myKeyHandler);
```
# 阻止冒泡和默认事件
1. 阻止冒泡
```javascript
  // ie 
  e.cancelbubble = true
  // w3c
  e.stopPropagation()
```
2. 阻止默认事件
```javascript
  // a链接的跳转， 表单提交， 右键呼出菜单选项都是默认事件
  if(e.preventDefault){
      e.preventDefault(); //w3c
  }else{
      event.returnValue = false; // ie9以下
  }

```
# 事件委托
1. 为什么要用事件委托
当页面中很多表格或列表需要添加事件时，如果逐个添加那就太麻烦了，但是使用事件委托就能极大的减轻我们的工作量，同时也能提高页面的性能。

2. 事件委托实现原理
事件委托是利用事件的冒泡原理来实现的，大致可以分为三个步骤：
+ 确定要添加事件元素的父级元素；
+ 给父元素定义事件，监听子元素的冒泡事件；
+ 使用 event.target 来定位触发事件冒泡的子元素。
```html
  <ul id="list">
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
    </ul>
```
```javascript
  // 传统的方法
  window.onload = function() {
    var the_ul = document.getElementById('list');
    var the_li = the_ul.getElementsByTagName('li');
    for( var i=0; i < the_li.length; i++ ){
        the_li[i].onclick = function(){
            console.log(this.innerHTML)
        }
    }
  }

  // 事件委托的方法
  window.onload = function(){
      var the_ul = document.getElementById('list');
      the_ul.onclick = function(e){
        // e.target 就是实际点击的那个li
          console.log(e.target.innerHTML)
      }
  }


```
3. 总结事件
1) 减小内存消耗
使用事件委托可以大量节省内存，减少事件的定义，通过上面的示例可以看出，要为 ul 标签下的所有 li 标签添加点击事件，如果分别为每个 li 标签绑定事件，不仅写起来比较繁琐，而且对内存的消耗也非常大。而使用事件委托的方式将点击事件绑定到 ul 标签上，就可以实现监听所有 li 标签，简洁、高效。
2) 动态绑定事件
在网页中，有时我们需要动态增加或移除页面中的元素，比如上面示例中动态的在 ul 标签中添加 li 标签，如果不使用事件委托，则需要手动为新增的元素绑定事件，同时为删除的元素解绑事件。而使用事件委托就没有这么麻烦了，无论是增加还是减少 ul 标签中的 li 标签，即不需要再为新增的元素绑定事件，也不需要为删除的元素解绑事件。

3. 要使用事件委托，需要保证事件能够发生冒泡，适合使用事件委托的事件有 click、mousedown、mouseup、keydown、keyup、keypress 等。需要注意的是，虽然 mouseover 和 mouseout 事件也会发生事件冒泡，但处理起来非常麻烦，所以不推荐在 mouseover 和 mouseout 事件中使用事件委托。对于不会发生事件冒泡的事件（例如 load、unload、abort、focus、blur 等），则无法使用事件委托。

# 拖拽

```html
被拖拽的元素需要添加 draggable 属性

拖拽的元素事件
1. dragestart 按下鼠标并进行移动时出发
2. drag 在拖动的过程中持续触发
3. dragend 在移动后，松开鼠标的一瞬间触发，或者按下esc


  <div class='a' draggable>拖拽的元素</div>






  <div class='b'>接受拖拽的元素/目标元素</div>
  目标元素，如果元素合法即可称为目标元素
  接受拖拽的元素素需要在 dragover 和 dragenter阻止默认事件
  如果某个元素同时设置了dragover和drop的监听，那么必须阻止dragover的默认行为，否则drop将不会被触发

目标元素的事件
1. dragenter 进入目标元素时触发
2. dragover 进入目标元素后并进行移动触发
3. drop 当被拖动的元素在目标区域被放下时触发此事件
4. dragleave 拖拽元素离开目标区域时触发

```
```javascript
    let app = document.getElementsByClassName('a')[0]; // 拖拽元素
    let target = document.getElementsByClassName('b')[0]; // 目标元素
    app.addEventListener('dragstart', function(e){
        console.log("dragstart")
        console.log(e)
        // 添加datatransfer设置标志位biji 值时 id
        e.dataTransfer.setData('biji', this.id)
        this.style.backgroundColor = 'yellow'
    })
    app.addEventListener('drag', function(e){
        console.log('drag')
    })
    app.addEventListener('dragend', function (e) {
        console.log('dragend')
    })

    // 目标元素，接受拖拽的元素
    target.addEventListener('dragenter', function(e){
        console.log('dragenter')
        e.stopPropagation()
        e.preventDefault()

    })
    target.addEventListener('dragover', function(e){
        console.log('dragover')
        e?.stopPropagation() 
        e?.preventDefault() // 要想drop生效比较在dragover事件中阻止默认事件

    })
    target.addEventListener('drop', function(e){
        let _uid = e.dataTransfer.getData('biji');
        // 将拖拽元素添加为目标元素的子元素
        this.appendChild(document.getElementById(_uid));
    })
    target.addEventListener('dragleave', function(e){
        console.log('dragleave')
    })
```

## 元素js完成拖拽
[图文小解clientX, scrollX及offsetX的区别
](https://juejin.cn/post/6844904094285430798)
```javascript
  var dragDiv = document.querySelector('.a') // 拖拽元素
	// 绑定鼠标按下对象
	dragDiv.onmousedown = function(e) {
	
    // 获取鼠标在div上按下的位置
    var e = e || window.event;  //接收事件对象
	// 获取鼠标在当前事件源的位置
    var x1 = e.offsetX // 鼠标按下时相对带有定位盒子的 x
    var y1 = e.offsetY // 鼠标按下时相对带有定位盒子的 y
    
    // 绑定移动事件
    document.onmousemove = function(e) {
    
        // 获取鼠标在浏览器中的位置 - 每个事件都有自己独特的事件对象
        var e = e || window.event;
        var x2 = e.clientX // client 鼠标相对当前可是区域的 x
        var y2 = e.clientY // client 鼠标相对当前可是区域的 y
        
        // 计算left和top
        var l = x2 - x1 // 计算出盒子距离可视区域的left
        var t = y2 - y1 // 计算出盒子距离可视区域的top
        
        // 设置不能超出左上角和右上角
        if(l < 0) {
            l = 0
        }
        if(t < 0) {
            t = 0
        }
          // 设置left和top的最大值 不能超过事件源本身
          // clientHeight 可视区域的高度， 
          // offsetHeight 盒子的高度
          // top 值如果超出了这个高度 就按照这个高度计算
        if(t > document.documentElement.clientHeight - box.offsetHeight) {
            t = document.documentElement.clientHeight - box.offsetHeight
        }
        if(l > document.documentElement.clientWidth - box.offsetWidth) {
            l = document.documentElement.clientWidth - box.offsetWidth
        }

        // 设置div的left和top
        box.style.left = l + 'px'
        box.style.top = t + 'px'
    }
}
// 拖拽行为一定要记得解绑mousemove事件
window.onmouseup = function() {
    document.onmousemove = null
}

```