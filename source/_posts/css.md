---
title: css
date: 2019-05-28 14:21:21
tags:
- css
- 面试
- less
categories:
- web前端
---

# 如何画一个三角形 [地址](https://zhuanlan.zhihu.com/p/482361933)

## border-width 增加， div 宽度高度变为0，其余三条边 border-color：tarnsparent
```css
// 首先一个正常的div，这是一个很正常的div
div {
    width: 50px;
    height: 50px;
    border: 1px solid block;
    border-top-color: red;
    border-left-color: blue;
    border-right-color: yellow;
}

// 我们将border-width 变大， 同时将width，height 变为0,同时将其与三条边的边框颜色 设置为
//transparent
div {
    width: 0;
    height: 0;
    border-width: 20px;
    border-top-color: transparent;
    border-left-color: transparent;
    border-right-color: transparent;
}
```

## 画一个长方形使用liear-grident 线性渐变的方法绘制出来
```css
.div {
  width: 160px;
   height: 200px;
   outline: 2px solid skyblue;
   background-repeat: no-repeat;
background-image: linear-gradient(32deg, orangered 50%, rgba(255, 255, 255, 0) 50%), linear-gradient(148deg, orangered 50%, rgba(255, 255, 255, 0) 50%);
  background-size: 100% 50%;
  background-position: top left, bottom left;
}
```
## clip-path  这个属性就是用来绘制多边形或者圆和椭圆的
这个div的是一个长方形
这个值是怎么来的呢？使用 clip-path 可以为沿路径放置的每个点定义坐标。在这种情况下，就定义了三个点：top-left (0 0)、bottom-left (0% 100%)、right-center (100% 50%)
```css
.div{

    margin: 100px;
    width: 160px;
    height: 200px;
    background-color: skyblue;
    clip-path: polygon(0 0, 0% 100%, 100% 50%);
}
```
## 首先将一个正方形tranfrom： rotate(45deg) 2d的旋转45度,转成菱形， 然后用个另一个div 定位盖住

# px/em/rem/vw的区别 都是相对
## 相对长度
1. px 是 pixel像素的缩写，是一个相对单位，基于屏幕的分辨率
    - px值必须是整数，
    - in 表示英寸
    - cm 表示厘米
    - mm 表示毫米
2. em 是相对单位， 如果自身定义了font-size那么就按照自身为参考 ，否则就往父级查找，知道找到html，
    浏览器的默认字体是16px， 所以 0.5em = 16px * 0.5 = 8px;
3. rem 是root+ em的意思，它的参考物是html的font-size值

4. vw是view width的缩写 1vw = 浏览器视图区域的1/100 ； 视图区域不包括工具栏和按钮的

## 绝对长度
1. cm 厘米
2. in 英寸
3. mm 毫米
4. pt:point（磅），1pt = 大约1/72英寸
绝对长度单位，多用于字体尺寸，1px = 0.75pt。
5. pc： pica  1pc = 12pt 磅 也是绝对长度

# calc 一般是百分比减去固定px长度。

# BFC的理解 [cankao1](https://wybing.blog.csdn.net/article/details/112447876?spm=1001.2101.3001.6661.1&utm_medium=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-112447876-blog-125100383.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-112447876-blog-125100383.pc_relevant_default&utm_relevant_index=1)
[cankao2](http://blog.tiaozaoj.com/index.php/archives/221/)
其中FC是formatting context的首字母缩写，直译过来是格式化上下文，它是页面中的一块独立的渲染区域，有一套渲染规则，决定了其子元素如何布局，以及和其他元素之间的关系和作用。

## 触发BFC条件
根元素（html就是根元素，整个html就是一个独立的BFC）
float属性不为none（如：left | right）
overflow的值不为visible（如：hidden | auto | scroll）
display属性值为inline-block | flex | inline-flex | table-cell | table-caption
position为absolute或fixed

## 应用BFC
解决margin塌陷问题
也就是解决，在一个标准流body（body元素就是一个BFC）中相邻盒子之间垂直margin重叠的问题。
方法：触发其中一个盒子的BFC，成为一个独立的容器，根据BFC规则，这个盒子的布局不受外部元素影响。
解决高度塌陷问题
当一个标准流中的盒子中所有的子元素都进行了浮动，并且没有给盒子设置高度时，那么这个盒子的整个高度就会塌陷，浮动的子元素高度不计算在父元素内，父元素高度就为0。
方法：触发这个盒子生成BFC，根据规则计算BFC的高度时，浮动元素也参与计算。

# 伪类和伪元素
伪类：伪类是用来定义元素特殊状态的，他可以用来设置鼠标悬停样式、元素获取焦点样式、设置链接样式等。如常见的 hover、active、link 等都是伪类。

伪元素：伪元素也称为伪对象，它不存在于 DOM 文档中、是一个虚拟的元素。它可以用来代表某个元素的子元素，但是这个子元素并不存在于文档树中。比如::before表示选择元素内容的之前内容，也就是""；

伪类和伪元素的根本区别在于：是否创造了新的元素。


# 核模型 ie核模型和标准核模型的区别
## ie核模型ie5.5之前的是ie核模型 
 **width = content + padding + borderWdith**
也就是说其宽度width 是 content + padding + border，
此时div所占实际大小就是width
box-sizing： border-box 

## 标准核模型
**width = content**  宽度就是content内容
box-sizing： content-box 
此时一个div所占大小是width（也就是content区域）+ padding + border

box-sizing: border-box | content-box | inherit(继承父级的box-sizing属性)

# grid / table / flex
# grid
[grid布局](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)
```css
// 容器属性
.pr {
    display: grid;
    grid-template-column: 10px, 20px, 30px, // 定义了3列 每列宽度分别是 10px 20px 30px；
    grid-template-column: repeat(2, 50%) //定义2列， 每列宽度占50%
    grid-template-column: repeat(auto-fill， 100px)；表示每列宽度100px，但是每列尽可能填充多个单元格
    grid-template-column: 1fr， 2fr， 3fr
    // fr是fraction缩写表示片段 表示三列，第二列宽度是第一列的一倍，第三列是第一列的3倍

    grid-template-column：10px, maxmin(100px, 50px); 表示2列， 第二列的宽度在100px和50px范围内
    grid-template-column：10px， auto// auto表示第二列宽度由浏览器决定

    grid-column-gap： 10px； // gap表示间距空白处， 表示每列之间的间距是10px
    grid-row-gap：20px // 每行行间距是20px
    grid-gap： 20px， 10px； <grid-row-gap> 和 <grid-column-gap> 的缩写


    // 还可以使用方括号，指定每一根网格线的名字，方便以后的引用。
    grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
    grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];

    // 先划分出9个单元格，然后将其定名为a到i的九个区域，分别对应这九个单元格。
    grid-template-areas: 'a b c'
                       'd e f'
                       'g h i';


    // 多个单元格合并成一个区域的写法如下
    grid-template-areas: 'a a a'
                     'b b b'
                     'c c c';

    // 划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是"先行后列"，
    // 即先填满第一行，再开始放入第二行，即下图数字的顺序
    // 1 2 3
    // 4 5 6

    //但是这个顺序可以改变 默认是 row 先行后列 ； 
    // 1 3 5
    // 2 4 5
    grid-auto-flow: column; // 先列后行


    justify-items: start | end | end | stretch; //  单元格内容水平对齐方式
    align-items: start | end | center | stretch; // 垂直对齐
    place-items: align-items | justify-content; //  复合属性

    // 整个整个表格在容器中的布局
    justify-content: start | end | center | stretch | space-around | space-between | space-evenly; 
    // space-around 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍。
    // space-between 项目与项目的间隔相等，项目与容器边框之间没有间隔。
    // space-evenly 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔。

    align-content: start | end | center | stretch | space-around | space-between | space-evenly; 
    place-content: align-content ｜ <justify-content // 复合属性



}

//  项目属性
{
    .item-1 {
        justify-self:  start | end | center | stretch ;
        align-self: start | end | center | stretch; 
        place-self: <align-self> <justify-self>; // 合体属性
        grid-area: a  // 属性指定项目放在哪一个区域
    }
}
```
## table
[table]()

## flex
[flex语法篇](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
[实例篇](http://www.ruanyifeng.com/blog/2015/07/flex-examples.html)
将一个元素设置为 display： flex 或者 inline-flex 这个元素称为flex容器；子元素称为flex项目
### 容器属性
```css
div {
    display: flex;
    flex-direction: row | row-reverse | column | column-reverse;
    flex-wrap:  wrap | nowrap | wrap-reverse; /** wrap-reverse 表示 换行，第一行在下方。*/
    flex-flow: <flex-direction> || <flex-wrap>; 
    justify-content: flex-start | flex-end | center | space-between | space-around;
    /**baseline 表示项目的第一行文字的基线对齐 */
    align-items: flex-start | flex-end | center | baseline | stretch;

    /**align-content属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。
     */
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```
### flex项目属性
```css
    .item {
        order: -1;/* 属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。 */
        flex-grow: 1; /**
        grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，不放大。
        grow属性为1，则它们将等分剩余空间（如果有的话）
        grwo为2， 其他项目都为1，则前者占据的剩余空间将比其他项多一倍。
        */
        flex-shrink: 1; /**
        项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
        属性都为1，当空间不足时，都将等比例缩小
        shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。
        */
        flex-basis: <length> | auto; 
        /**
        basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）
        计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小（width或者height的值）。
        */


        flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ];
        /**
            两个快捷值：auto (1 1 auto) 会放大，也会缩小
                    和 none (0 0 auto)。有剩余空间也不会放大，也不会缩小
                    flex : 1 等同于 flex: auto
        */


        /**
            align-self的权重比align-items高用来设置单个项目的对齐方式
            默认是auto，继承了align-items，没有父元素等同于stretch

        */
        align-self: auto | flex-start | flex-end | center | baseline | stretch;

    }
```
# css3的新增属性

# 如何将一个盒子水平垂直居中
```css
// 1. 用display ： table-cell； text-align: center; vertical-align: middle
.box{
  width: 400px;
  height: 400px;
  display: table-cell;
  text-align: center;
  vertical-align: middle;
  border: 1px solid #ccc;
}

/*2. 已知父级盒子的height， 子元素设置 margin： 父级盒子高度的一半 减去 子元素高度的一半 */
    .fa{
      height: 800px;
      background-color: black;
      border-width: 1px;
    }
    /* 子容器样式 */
    .son{
      width: 300px;
      height: 300px;
      background-color: white;
      /* 水平垂直居中 */
      /* 父级盒子高度的一半 减去 子元素高度的一半 */
      margin: 250px auto;
    }

/**
    3. 父元素相对定位，子元素绝对定位，并且子元素 top，left，bottom，right设置为0，margin：auto
*/
    /* 父容器 */
    .fa{
      height: 700px;
      position: relative;
      background-color: black;
    }
    /* 子容器 */
    .son{
      width: 300px;
      height: 300px;
      background-color: white;
      position:absolute;
      /* 水平垂直居中 */
      left: 0;
      right: 0;
      top: 0;
      bottom: 0;
      margin: auto;
    }


/**
    4. 父元素相对定位，子元素绝对定位，并且子元素 top： 50%，，left：50%，
    margin-top: 子元素高度一半的负值
    margin-left: 子容器宽度一半的负值
*/
    .pa{
      height: 700px;
      background-color: black;
       /* 父容器开启相对定位*/
      position: relative;
    }
    /* 子容器 */
    .son{
      width: 300px;
      height: 300px;
      background-color: white;
      /* 子容器开启绝对定位*/
      position:absolute;
      /* 水平垂直居中 */
      top: 50%;
      margin-top: -150px; /** 子元素高度一半的负值 */
      left: 50%;
      margin-left: -150px;
    }

/**
    原理同上先通过绝对定位将div定位到上，左各一半的位置，然后通过translate，向上或者向左移动自身一半的位置。

*/  
    .fa{
        width: 100%;
        height:100%;
        position: relative;
        border: 5px solid #000000;
        background:#ddd;
    }
    .children{
        position: absolute;
        width:500px;
        height:300px;
        top: 50%;
        left: 50%; 
        transform: translate(-50%, -50%);
        background:green;
    }

/**
    flex布局
*/
    /* 父容器 */
    .fa{
      height: 700px;
      background-color: black;
      /* 父容器开启flex布局 */
      display: flex;
      /* 水平垂直居中 */
      justify-content: center;
      align-items: center;
    }
```
# less.js
是属于css的扩展语言，css作为文本的样式语言有以下缺点
1. 缺少编程特性，没有变量，函数，逻辑控制性语句，因此代码较为冗余
2. 模块化程度低， 代码碎片化高，且较为零散，不容易维护
3. 注释只支持/* */
less作为css的扩展语言，由js进行编译处理，一定程度上弥补了css的不足，使css的写法更加简洁，高效。
1. 加入了一些编程特性，比如：变量、Mixin、函数、命名空间、流程控制等，提高函数复用、减少重复代码
2. 支持嵌套（Nesting）写法，代码更加模块化，方便维护
3. 除了支持 /**/ 注释外，还支持 // 注释
浏览器环境
<link rel="stylesheet/less" type="text/css" href="styles.less" />

node环境
npm install -g less 
lessc style.less style.css 

## less的基础语法
```less
    // 注释
    /* css一般使用的语法 */ 
    // less使用的注释

    // 引入
    @import "lib"; //lib.less
    @import "common.css";

    // 变量 @变量名：变量值
    @height:100px;
    @color:red;
    a{
        height:@height;
        width:100px;
        color:@color;
    }

    // 选择器
    @my-selector: my-div;
    .@{my-selector} {
        color: red;
    }

    // URL
    @images: "../images";
    div {
        background: url("@{images}/test.png");
    }

    // 属性名
    @property: color;
        .widget {
        @{property}: #0ee;
        background-@{property}: #999;
    }

    // 内部变量
    .module {
        --lh: 1.2rem; //变量
        line-height: var(--lh); //用var(--变量名)
        max-height: calc(var(--lh) * 3);
        overflow: hidden;
    } 

    // 操作符 变量可以进行加减乘除
    @height: 100px;
    @width: @height + 50px;
    #operations-test {
        width: @width * 2;
        height: @height / 3;
    }

    // 可以进行嵌套
    @leftWidth: 300px;
    .container {
        color: black;
        .left { // 表示.container .left
            width: @leftWidth;
            span {
            color: red;
            }
        }
        .right {
            width: calc(100% - @leftWidth)
        }
        &:hover{ // 伪类的使用&
            border: 1px solid red;
        }
        &-danger{ //表示container-danger元素
            width: 100px
        }
    }
```
## less 混入可以让一组或一个类名/id名 混入到另一组或一个中
```less
// 单个样式规则混入
.classtest {
  border: 1px solid #ccc;
}
#idtest {
  margin-top: 10px;
}
#a span {
  .classtest(); // 使用classtest的样式
  #idtest(); // 使用了#idtest的样式
  color: blue;
}
.tips {
  .classtest(); 使用了classtest的样式
  #idtest();
}

```
```less
// 一组 多个样式混入
.grouptest() {
  a { 
    color: red;
  }
  .btn { 
    width: 100px;
  }
}
.some {
  .grouptest();
}
.some2 {
  .grouptest.btn();
}

// 输出的css
.some a {
  color: red;
}
.some .btn {
  width: 100px;
}
.some2 {
  width: 100px;
}

```
## 键值对的使用
```less
@sizes: {
  a: 320px;
  b: 768px;
  c: 1024px;
}

.test {
  width: @sizes[a];
  height: @sizes[b]
}
```
## 转义 可以用～任意字符串用作属性名或者属性值
```less
@min768: ~"(min-width: 768px)"; //定义变量
// @min768: (min-width: 768px); // less 3.5+ 可以直接这样使用
@media @min768 {
  a {
    color: blue;
  }
}

```
## 函数
ess 内置了一些系统函数，比如逻辑控制函数 if，列表函数 each，
数学函数 round，颜色操作函数 lighten、darken 等
### 判断类型函数
isnumber(值)、isstring(值)、iscolor(值)，他们的返回值是 Boolean 类型：true or false
```less
isnumber(blue);     // false
isnumber(1234);     // true
isnumber(56px);     // true
isnumber(7.8%);     // true

isstring(blue);     // false
isstring("string"); // true

iscolor(#ff0);     // true
iscolor(blue);     // true
iscolor("string"); // false
```
### 逻辑函数
if((表达式),表达式成立的值，表达式不成立的值)
boolean（表达式）
```less
    @some: foo;
    @result: boolean(isstring(@some));

    div {
    margin: if((2 > 1), 0, 3px);  // 2 > 1 成立 margin 是 0
    color:  if((iscolor(@some)), @some, black);
    height: if(@result, 100px, 200px)
    }

    .test {
    color: if(not (true), foo, bar);
    size: if((true) and (2 > 1), foo, bar);
    width: if((false) or (isstring("boo!")), foo, bar);
    }


    // 输出css 
    div {
        margin: 0;
        color: black;
        height: 200px;
    }
    .test {
        color: bar;
        size: foo;
        width: foo;
    }
````
### 列表函数
使用逗号或空格分隔的数据就是 list，先来看列表相关的基础函数

1. length(列表)，返回列表长度
2. extract(列表, index)，用 index 取列表里的值，注意：index 从 1 开始
3. range(可选的 start, end, 可选的 step)，创建一个 list 列表
```less
    @colors: red, rgb(30, 30, 31), yellow; // 列表
    @list: 768px 1024px 1366px 1920px; // 列表
    .btn {
        width: length(@colors); //取长度 3
        height: length(@list); // 4
    }
    span {
    // extract 从 1 开始
        color: extract(@colors, 3); // yellow
        width: extract(@colors, 1); // red
        padding: range(4); // 创建一个列表
        margin: range(10px, 30px, 10); //创建了一个列表
    }

    //输出
    .btn {
        width: 3;
        height: 4;
    }
    span {
        color: yellow;
        width: red;
        padding: 1 2 3 4;
        margin: 10px 20px 30px;
    }

```

each 
对列表、键值对的遍历，并将里面的每个值绑定到对应规则集(ruleset)里。 each(列表或者Map键值对, 
一个匿名的 ruleset 或 mixins)

```less
    @selectors: blue, green, yellow;
    each(@selectors, {
        .btn-@{value} {  // @{value} 表示便利的每个列表值
            color: @value;
        }
    })
    // 输出
    .btn-blue{
      color: blue;
    }
    .btn-green {
        color: green;
    }
    .btn-yellow {
        color: yellow;
    }




    @set: {
        one: blue;
        two: green;
        three: yellow;
    }
    .set {
        each(@set, {
            @{key}-@{index}: @value;
        })
    }

    // 输出
    .set {
        one-1: blue;
        two-2: green;
        three-3: yellow;
    }

```
### 数学函数
```less
    @k: ceil(2.4) floor(2.6) round(2.4) round(2.6) percentage(0.5) sqrt(25px) abs(-12%) min(1, 2);
    .btn {
        each(@k, {
            a-@{index}: @value;
        })
    }
    // ceil 获得不小于给定值的最小整数
    // floor 向下取整
    // round 四舍五入
    // percentage 百分比
    // sqrt开方
    // abs绝对值
    // min最小值

    // 输出
    .btn {
        a-1: 3;
        a-2: 2;
        a-3: 2;
        a-4: 3;
        a-5: 50%;
        a-6: 5px;
        a-7: 12%;
        a-8: 1;
    }
```
### 颜色函数 
1. lighten(color, 0-100百分比) 将颜色亮度调高，变亮
2. darken(color, 0-100百分比) 将颜色亮度调低，变暗
```less
     @color: #80e619;
    @colors: @color lighten(@color, 30) darken(@color, 30);
    .btn {
        display: inline-block;
        width: 100px;
        height: 100px;
    }
    each(@colors,  {
        .btn-@{index} {
            background: @value;
        }
    })
```
## 作用域
嵌套的样式可以重写变量的值。优先级较高
```less
    @var: red;
    #page {
        @var: white; // 这里重写了变量var 因此下面用到@var值都为white
        #header {
            color: @var; // white
        }
    }
```