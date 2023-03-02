---
title: react
date: 2023-02-28 14:21:52
tags:
---
# react 基础
# react 项目创建

```c
node -v
npm -v
    
npm i -g create-react-app // 全局下载 create-react-app 脚手架工具

D:  //进入D盘
mkdir ReactDemo  //创建ReactDemo文件夹
create-react-app demo01   //用脚手架创建React项目
cd demo01   //等创建完成后，进入项目目录
npm start   //预览项目，如果能正常打开，说明项目创建成功
```

# 第一个组件

再src 下新建一个index.js 文件

```react
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'
ReactDOM.render(<App />,document.getElementById('root'))
```

目前App组件还没有的，需要一会建立

App组件的编写

```react
import React, {Component} from 'react'

class App extends Component{
    render(){
        return (
            <div>
                Hello JSPang
            </div>
        )
    }
}
export default App;
```

# JSX简介

JSX是js和XML结合的一种格式。 React发明了JSX，可以方便HTML语法来创建虚拟DOM， 遇到<, JSX解析为HTML， 遇到{ 就当js解析

## 组件和普通JSX语法区别

组件首字母大写，JSX是小写字母开头

## JSX中使用js语法， 使用三元表达式

```react
import React from 'react'
const Component = React.Component


class App extends Component{
    render(){
        return (
            <ul className="my-list">
                <li>{false?'JSPang.com':'技术胖'}</li>
                <li>I love React</li>
            </ul>
        )
    }
}

export default App;
```

# Frament 标签

加上最外层的DIV，组件就是完全正常的，但是你的布局就偏不需要这个最外层的标签怎么办?比如我们在作`Flex`布局的时候,外层还真的不能有包裹元素。这种矛盾其实React16已经有所考虑了，为我们准备了`<Fragment>`标签。

要想使用`<Fragment>`，需要先进行引入。

```javascript
import React,{Component,Fragment } from 'react'
```

然后把最外层的`<div>`标签，换成`<Fragment>`标签，代码如下。

```javascript
import React,{Component,Fragment } from 'react'

class Xiaojiejie extends Component{
    render(){
        return  (
            <Fragment>
               <div><input /> <button> 增加服务 </button></div>
               <ul>
                   <li>头部按摩</li>
                   <li>精油推背</li>
               </ul> 
            </Fragment>
        )
    }
}
export default Xiaojiejie 
```

这时候你再去浏览器的`Elements`中查看，就回发现已经没有外层的包裹了。

# 响应式设计和数据绑定

`React`不建议你直接操作`DOM`元素,而是要通过数据进行驱动，改变界面中的效果。React会根据数据的变化，自动的帮助你完成界面的改变。所以在写React代码时，你无需关注DOM相关的操作，只需要关注数据的操作就可以了（这也是React如此受欢迎的主要原因，大大加快了我们的开发速度）。

。数据定义在`组件中的构造函数里`constructor`。

```javascript
//js的构造函数，由于其他任何函数执行
constructor(props){
    super(props) //调用父类的构造函数，固定写法
    this.state={
        inputValue:'' , // input中的值
        list:[]    //服务列表
    }
}
```

在`React`中的数据绑定和`Vue`中几乎一样，也是采用`字面量`(我自己起的名字)的形式，就是使用`{}`来标注，其实这也算是js代码的一种声明。比如现在我们要把`inputValue`值绑定到`input`框中，只要写入下面的代码就可以了。其实说白了就是在JSX中使用js代码。

```html
<input value={this.state.inputValue} /> 
```

现在需要看一下是不是可以实现绑定效果，所以把`inputValue`赋予一个'jspang'，然后预览看一下效果。在这里我们并没有进行任何的`DOM`操作，但是界面已经发生了变化，这些都时`React`帮我们作的，它还会自动感知数据的变化。

[绑定事件](https://jspang.com/detailed?id=46#toc333)

这时候你到界面的文本框中去输入值，是没有任何变化的，这是因为我们强制绑定了`inputValue`的值。如果要想改变，需要绑定**响应事件**，改变`inputValue`的值。比如绑定一个改变事件，这个事件执行`inputChange()`(当然这个方法还没有)方法。

```html
<input value={this.state.inputValue} onChange={this.inputChange} />
```

现在还没有`inputChange()`这个方法，在`render()`方法的下面建立一个`inputChange()`方法，代码如下：

```
 inputChange(e){
        console.log(e);
    }
```

这时候会发现响应事件可以使用了，但是如何获得我们输入的值那，程序中输入下面的代码。

```javascript
inputChange(e){
    console.log(e.target.value);
}
```

这时候控制台是可以打印出输入的值的，视频中会有演示。看到获得了输入的值，想当然的认为直接改变`inputValue`的值就可以了(错的).

```javascript
inputChange(e){
    console.log(e.target.value);
    this.state.inputValue=e.target.value;
}
```

写完后再进行预览，会发现程序直接报错了（加项服务还真的有点难度哦,大宝剑不好作的...........）。

其实我们范了两个错误：

1. 一个是`this`指向不对，你需要重新用`bind`设置一下指向(ES6的语法)。
2. 另一个是`React`中改变值需要使用`this.setState`方法。

第一个错误很好解决，直接再`JSX`部分，利用`bind`进行绑定就好。

```
 <input value={this.state.inputValue} onChange={this.inputChange.bind(this)} />
```

这步做完，我们还需要加入`setState`方法，改变值。代码如下:

```javascript
inputChange(e){
    // console.log(e.target.value);
    // this.state.inputValue=e.target.value;
    this.setState({
        inputValue:e.target.value
    })
}
```

现在测试一下，输入框可以改变值了，其实这节课很重要，里边设计了`React`的重要思想，建议这节课可以反复多看两遍，虽然不难，但是这是一个最基本的思想的转变。下节课可要真的增加服务项目了。

# 循环渲染列表

如何根据多个数据动态显示多个li标签

```react
constructor(props){
    super(props) //调用父类的构造函数，固定写法
    this.state={
        inputValue:'jspang' , // input中的值
        //----------主要 代码--------start
        list:['基础按摩','精油推背']   
        //----------主要 代码--------end
    }
}
```

有了数据list之后，可以再jsx部分进行循环输出。代码如下

```javascript
render() {
    return {
        <Frament>
        	<div>
        		<input type="text" name="" value={this.state.inputValue} onChange={this.inputChange.bind(this)} />
				<button type="submit">增值服务</button>
        	</div>
			<ul>
             	{
                    this.state.map((item, index) => {
                        return <li>{item}</li>
                    })
                }     
             </ul>
		
        </Frament>
    }
}
```

完成上面的步骤，数据就不再是固定的了，而是动态管理的，也为我们接下来的添加打下了基础，剩下的步骤也显得很简单了

# 解决key值错误

。这个错误的大概意思就是缺少`key值`。就是在用map循环时，需要设置一个不同的值，这个时React的要求。我们可以暂时用`index+item`的形式来实现

```react
<ul>
    {
        this.state.list.map((item,index)=>{
            return <li key={index+item}>{item}</li>
        })
    }
</ul>  
```

# 删除数据

获得了数据下标后,删除数据就变的容易起来.先声明一个局部变量,然后利用传递过来的下标,删除数组中的值.删除后用`setState`更新数据就可以了.

```javascript
//删除单项服务
deleteItem(index){
    let list = this.state.list
    list.splice(index,1)
    this.setState({
        list:list
    })

}
```

其实这里边是有一个坑的,有的小伙伴肯定会认为下面的代码也是正确的.

```javascript
//删除单项服务
deleteItem(index){
    this.state.list.splice(index,1) // 这里直接删除是有问题的
    this.setState({
        list:this.state.list
    }) 
}
```

记住React是禁止直接操作state的,虽然上面的方法也管用,但是在后期的性能优化上会有很多麻烦,所以一定不要这样操作.这也算是我`React`初期踩的比较深的一个坑,希望小伙伴们可以跳坑.

# JSX 几个坑

1. 注释的写法要再{} 中

   ```
       //第一次写注释，这个是错误的
       <div>
           <input value={this.state.inputValue} onChange={this.inputChange.bind(this)} />
           <button onClick={this.addList.bind(this)}> 增加服务 </button>
       </div>
       
       
       
       
       {/* 正确注释的写法 */}
       <div>
           <input value={this.state.inputValue} onChange={this.inputChange.bind(this)} />
           <button onClick={this.addList.bind(this)}> 增加服务 </button>
       </div>
   ```

2. class 的写法 要用className 而不是 class

   ```
    // 错误写法
    <input class="input" value={this.state.inputValue}  />
    
    
    // 正确写法 使用className
    <input className="input" value={this.state.inputValue}  />
   ```

3. html 解析问题， 如果我们想再文本input 中输入 <h1></h1> 并进行渲染， 是不会生效的

   ， 可以使用 dangerouslySetInnerHTML 属性解决

   ```
   <ul>
       {
         this.state.list.map((item, index) => {
             return <li key={index+item} onClick={this.deleteItem.bind(this, index)} dangerouslySetInnerHTML={{__html:item}}></li>
         })
       }
            </ul>
   ```

   

   ![image-20210629161829926](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210629161829926.png)

   ![image-20210629161850386](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210629161850386.png)

4. 关于label 也是个不小的坑

   ```jsx
   <div>
       <label>加入服务：</label>
       <input className="input" value={this.state.inputValue} onChange={this.inputChange.bind(this)} />
       <button onClick={this.addList.bind(this)}> 增加服务 </button>
   </div>
   ```

   我们如果想 点击加入服务时 也input 框进行聚焦的话， 按照之前html 的思路 只需要使用 id； 即：

   ```jsx
   <div>
       <label for="jspang">加入服务：</label>
       <input id="jspang" className="input" value={this.state.inputValue} onChange={this.inputChange.bind(this)} />
       <button onClick={this.addList.bind(this)}> 增加服务 </button>
   </div>
   ```

   ** 但是这个再jsx 中时会报错的， 会提示你使用htmlFor

   ```
   <div>
       <label htmlFor="jspang">加入服务：</label>
       <input id="jspang" className="input" value={this.state.inputValue} onChange={this.inputChange.bind(this)} />
       <button onClick={this.addList.bind(this)}> 增加服务 </button>
   </div>
   ```

# simple React Snippets 插件

imrc 快速引入语法

```
import React, { Component } from 'react';
```

cc  快速生成以下代码

```
class  extends Component {
    state = {  }
    render() { 
        return (  );
    }
}

export default ;
```

# 父子组件传值

## 父组件向子组件传值： 靠属性的方式进行传值

```jsx
render() { // 父组件中
	return (
		<son content = {a}> // 利用 content 属性 传递值时 a 的值 
	)
}


// 子组件中
class Son extends Component {
	render() {
		return { // 在子组件中就可以直接拿到porps 中的 content
			<div> {this.props.content} </div>
		}
	}
}
export default Son
```





## 子组件向父组件传值

因为react规定，子组件不能直接操作父组件里的数据， 所以要借助一个父组件的方法 deleteOne（也就是将父组件的deleteOne方法传递给子组件）



```jsx
// 子组件中
class Son extends Component {
	render() {
		return { // 在子组件中就可以直接拿到porps 中的 content
			<div onClick={this.handleClick.bind(this)}> {this.props.content} </div>
		}
	}
    handleClick() {
        this.props.deleteItem(1) // 这里执行deleteItem 其实就是执行父组件的 deleteOne 方法
    }
}
export default Son



render() { // 父组件中
	return (
		<son content = {a} deleteItem={this.deleteOne}> // 利用 deleteItem 属性 传递 方法给子组件 
	)
    deleteOne(val) {
        this.setState({ // val就是1
            a: val
        })
    }
}

**** react中state改变了，组件才会update。
1. 父写好state和处理该state的函数，同时将函数名通过props属性值的形式传入子，
2. 子调用父的函数可以进行传递数据，同时引起state变化
```



# react 单项数据流

对于父子间传递过来的数据， 子组件只能读取， 不能进行修改；



# PropTypes 应用

```
improt PropTypes from 'prop-types'


class XiaojiejieItem extends Component {

}

PropTypes 用来限制从父组件传递过来的数据类型；因为大型项目，如果你不校验，后期会变的异常混乱，业务逻辑也没办法保证。
XiaojiejieItem.propTypes = { 
	content: PropTypes.string,
	deleteItem: PropTypes.func,
	index: PropTypes.number.isRequired // 表示必须做
}

// 默认值 props ： 表示如果我们没有传递属性值， 可以默认给一个值

XiaojiejieItem.defaultProps = {
	avname: '松岛枫'
}
```



# ref 的使用

再编写组件方法时， 经常会遇到语义化很模糊的代码，这对于review 或者合作开发 是很不友好的。 或者当核心成员离开时， 项目甚至会面临倒闭的问题。 我们必须重视react 代码中的语义话。 ref 是个不错的工具



1. 代替原来的e.target.value

   ```jsx
   inputChange(e) {
   	this.setState({
   		inputValue: e.target.value
   	})
   }
   
   
   {/*使用ref进行， 现在jsx中进行绑定，使用es6语法*/}；
   <input type="text" name="" value={this.state.inputValue}
   	onChange={this.inputChange.bind(this)}
   	ref={(input) => {this.input = input}}
   />
   
   // 使用ref语义话绑定后的 方法代码
   inputChange() {
       this.setState({
           inputValue: this.input.value
       })
   }
   ```

# ref的坑， setState 是一个异步函数

[`ref`使用中的坑](https://jspang.com/detailed?id=46#toc367)

比如现在我们要用ref绑定取得要服务的数量，可以先用`ref`进行绑定。

```jsx
<ul ref={(ul)=>{this.ul=ul}}>
    {
        this.state.list.map((item,index)=>{
            return (
                <XiaojiejieItem 
                key={index+item}  
                content={item}
                index={index}
                deleteItem={this.deleteItem.bind(this)}
                />
            )
        })
    }
</ul>  
```

绑定后可以在`addList()`方法中，获取当前`<div>`的值.

```js
 addList(){
    this.setState({
        list:[...this.state.list,this.state.inputValue],
        inputValue:''
    })
    //关键代码--------------start
    console.log(this.ul.querySelectorAll('div').length)
    //关键代码--------------end

}
```

这时候你打开控制台，点击添加服务按钮，你会返现数量怎么少一个？（就是这个坑），其实这个坑是因为React中更多`setState`是一个异步函数所造成的。也就是这个`setState`，代码执行是有一个时间的，如果你真的想了解清楚，你需要对什么是虚拟DOM有一个了解。简单的说，就是因为是异步，还没等虚拟Dom渲染，我们的`console.log`就已经执行了。

那这个代码怎么编写才会完全正常那，其实`setState`方法提供了一个回调函数，也就是它的第二个函数。下面这样写就可以实现我们想要的方法了。

```js
addList(){
    this.setState({
        list:[...this.state.list,this.state.inputValue],
        inputValue:''
        //关键代码--------------start
    },()=>{
        console.log(this.ul.querySelectorAll('div').length)
    })
    //关键代码--------------end
}
```

现在到浏览器中查看代码，就完全正常了。这节课主要学习了`ref`的用法和`ref`中的坑。学完后练习一下吧，代码这东西，不练习你是学不会的。

# React 的生命周期

- 四个阶段

  1. 初始化阶段： initialization
  2. 挂载阶段：mounting : componentWillMount、 render、 componentDidMount
  3. 更新阶段：updating : shouldCompoentUpdate、 componentWillUpdate、 componentDidUpdate、 componentWillReceiveProps
  4. 销毁阶段: unmounting

- 什么是生命周期函数： 在某个时刻组件会自动调用执行的函数

  例如： render 函数就是一个生命周期函数， 它会在state 发生改变时自动执行

- mounting 阶段 叫做挂载阶段， 伴随这整个虚拟DOM的生成， 它里面有三个小的生命周期函数， 分别是

  1. componentWillMount: 组件即将被挂载到页面的时刻执行。

  2. render: 页面state或props发生改变时执行。

  3. componentDidMount: 组件挂载完成时执行。

     * 注意：

       componentWillMount 和 componentDIdMout 这两个生命周期函数， 只在页面中刷新时执行一次，而render 只要在 state 和 props 变化就会执行

       ![image-20210630095759823](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210630095759823.png)

- updating 阶段 组件发生改变更新的阶段：  有两个基本的部分引发更新： 1. props 属性改变， 2. state状态改变

  1. shouldComponentUpdate 在组件更新之前， 自动执行。 这个函数必须要求返回一个boolean 值

     ```
     shouldComponentUpdate(nextProps, nextState) {
     	// nextProps 表示下一个props的值
     	// nextstate 表示下一个state的值
     	// 当props 改变或者 state 改变 react 会渲染整个组件；
     	if (nextProps.title === this.props.title) {
     		return false // 如果两次title 相同没必要去进行渲染 浪费性能
     	} else {
     		return true
     	}
     }
     
     // 返回true 同意组件更新 
     // 返回false 不同一组件更新
     ```

     如果返回false 那么组件就不会执行componentWillUpdate函数

  2. componentWillUpdate 函数， 也发生在组件更新之前执行， 但在shouldComponentUpdate之后 执行。如果 shouldComponentUpdate 返回false ， 这个函数就不会执行

  3. componentDidUpdate 函数， 在组件更新之后执行，

  4. compoentWillReceiveProps

     这个函数如果是顶层组件 那么就不执行； 如果是子组件， 那么接受到父组件传递过来的参数，此时

     ` 父组件render函数执行时，这个函数也会被执行。简单说这个函数执行取决于父组件的是否更新`

- 销毁阶段

  1. componentWillUnmount 函数， 当组件从页面中删除的时候执行

# npm install  xxx  和 npm install xxx -g  、 npm i xxx --save、 npm i xxx --save-dev 的区别

1. npm i xxx 安装第三方包到项目目录下， 不会将模块依赖写入 devDependence 和 dependencies 
2. npm i xxx -g ： 安装包到磁盘中， 具体在哪个磁盘要看 npm cinfig prefix 的位置
3. npm i --save xxx:  --save 的意思是将模块安装到项目目录下并在dependencies 写入， 运行依赖
4. npm i --save-dev xxx: --save-dev 会安装在项目目录下 并在devDenpendencies 写入 开发依赖

# 安装 react-transition-group

1. 首先进行安装模块 npm install react-transition-group --savae

   这个模块有三个核心库 Transiton CssTransition TransitonGroup

2. 使用CssTransition

   - 引入 import {CssTransition} from 'react-transition-group';

   - ```
     render() { 
         return ( 
             <div>
                 <CSSTransition //这里使用csstransition 进行包裹 <div>BOSS级人物-孙悟空</div>
                     in={this.state.isShow}   //用于判断是否出现的状态
                     timeout={2000}           //动画持续时间
                     classNames="boss-text"   //className值，防止重复 这里写的classNames
                 >
                     <div>BOSS级人物-孙悟空</div>
                 </CSSTransition>
                 <div><button onClick={this.toToggole}>召唤Boss</button></div>
             </div>
             );
     }
     ```

   - css样式

     - xxx-enter: 进入（入场）前的CSS样式；
     - xxx-enter-active:进入动画直到完成时之前的CSS样式;
     - xxx-enter-done:进入完成时的CSS样式;
     - xxx-exit:退出（出场）前的CSS样式;
     - xxx-exit-active:退出动画知道完成时之前的的CSS样式。
     - xxx-exit-done:退出完成时的CSS样式

     ```
     .boss-text-enter{
         opacity: 0;
     }
     .boss-text-enter-active{
         opacity: 1;
         transition: opacity 2000ms;
     
     }
     .boss-text-enter-done{
         opacity: 1;
     }
     .boss-text-exit{
         opacity: 1;
     }
     .boss-text-exit-active{
         opacity: 0;
         transition: opacity 2000ms;
     
     }
     .boss-text-exit-done{
         opacity: 0;
     }
     ```

   - 我们给`<CSSTransition>`加上`unmountOnExit`,加上这个的意思是在元素退场时，自动把DOM也删除，这是以前用CSS动画没办法做到的。

   # transitionGroup

   <CSSTransition> 只能控制一个DOM元素的动画， 向控制多个动画  需要使用 transitionGroup 模块

   1. 引入

      ```jsx
      import {CSSTransition, TransitionGroup} from 'react-transition-group';
      ```

   2. 用<TransitionGroup> 包裹住 多个 dom 元素

      ```jsx
      <ul ref={(ul)=>{this.ul=ul}}>
          <TransitionGroup> {/*这里包裹住了多个dom内容*/}
          {
              this.state.list.map((item,index)=>{
                  return (
                      <CSSTransition 这里一定要加入<CssTranstion> 标签
                          timeout={1000}
                          classNames='boss-text'
                          unmountOnExit
                          appear={true}
                          key={index+item}  
                      >
                          <XiaojiejieItem 
                          content={item}
                          index={index}
                          deleteItem={this.deleteItem.bind(this)}
                          />
                      </CSSTransition>
                  )
              })
          }
          </TransitionGroup>
      </ul>  
      ```

   3. 这里一定要加入<CssTranstion> 标签 ， 可以为dom 设置classNames 属性和 timeout等

# react-redux
# react-router 

react-router 是一个基础react的路由库， 它可以让你向应用中快速添加视图和数据流， 同时保持页面与url间的同步。 凡是ract技术栈的项目， 都需要用到react-router；

## 用create-react-app 脚手架初始化项目

1. `cnpm install -g create-react-app` 

2. 直接使用脚手架工具创建项目

   ```react
   D:  //进入D盘
   mkdir ReactRouterDemo   //创建ReactRouterDemo文件夹
   cd ReactRouterDemo      //进入文件夹
   create-react-app demo  //用脚手架创建React项目
   cd demo   //等项目创建完成后，进入项目目录
   npm  start  //预览项
   ```

3. 安装react-router `cnpm install --save react-router-dom`

4. [编写一个最简单的路由程序](https://jspang.com/detailed?id=49#toc34)

   首先我们改写`src`文件目录下的`index.js`代码。改为下面的代码,具体的意思在视频中讲解:

   ```js
   import React from 'react';
   import ReactDOM from 'react-dom'
   import AppRouter from './AppRouter'
   
   ReactDOM.render(<AppRouter/>,document.getElementById('root'))
   ```

   现在的`AppRouter`组件是没有的，我们可以在`src`目录下建立一个`AppRouter.js`文件，然后写入下面的代码。

   ```js
   import React from "react";
   import { BrowserRouter as Router, Route, Link } from "react-router-dom";
   
   function Index() {
     return <h2>JSPang.com</h2>;
   }
   
   function List() {
     return <h2>List-Page</h2>;
   }
   
   function AppRouter() {
     return (
       <Router>
           <ul>
               <li> <Link to="/">首页</Link> </li>
               <li><Link to="/list/">列表</Link> </li>
           </ul>
           <Route path="/" exact component={Index} />
           <Route path="/list/" component={List} />
       </Router>
     );
   }
   export default AppRouter;
   ```

   这时候就可以到浏览器中看一下效果了，如果一切正常，就可以实现页面跳转。但这只是最简单的页面跳转，第一节课先学这么多。

# 使用ReactRouter

## 编写index组件

先在`/src`目录下建立一个文件夹，我这里起名叫做`Pages`（你可以起任何名字），然后建立一个组件文件`Index.js`。这里边我们就完全安装工作中的模式来写，只是没有什么业务逻辑，UI也制作的相当加简单。代码如下：

```js
import React, { Component } from 'react';

class Index extends Component {
    constructor(props) {
        super(props);
        this.state = {  }
    }
    render() { 
        return (  <h2>JSPang.com</h2> );
    }
}

export default Index;
```

## 编写List组件

编写完`Index`组件以后，继续编写`List`组件。其实这个组件和`Index`基本一样。代码如下: 再pages文件 创建 List.js 文件s

```js
import React, { Component } from 'react';

class List extends Component {
    constructor(props) {
        super(props);
        this.state = {  }
    }
    render() { 
        return (  <h2>List Page</h2> );
    }
}

export default List;
```

## 修改AppRouter.js

两个组件制作完成后，我们把它引入路由配置文件，然后进行路由的配置就可以了，代码如下：

```js
import React from "react";
import { BrowserRouter as Router, Route, Link } from "react-router-dom";
import Index from './Pages/Index'
import List from './Pages/List'

function AppRouter() {
  return (
    <Router>
        <ul>
            <li> <Link to="/">首页</Link> </li>
            <li><Link to="/list/">列表</Link> </li>
        </ul>
        <Route path="/" exact component={Index} />
        <Route path="/list/" component={List} />
    </Router>
  );
}

export default AppRouter;
```

现在看起来就和我们实际工作中差不多了，也和我们平时写的普通`html`页面很类似了。

## exact 精准匹配的意思

精准匹配就是你的路径信息要完全匹配成功。 才可以实现跳转。 匹配一部分是不行的。

```react
<Route path="/"  component={Index} />
<Route path="/list/" component={List} />

//  这种情况会 把 Index 和 List 都匹配到
```

```react
<Route path="/" exact component={Index} />
<Route path="/list/" component={List} />

// 这种情况是正常情况
```

# reactRouter 动态传值

之前已经搞定了 链接跳转的问题，

那现在想象这样一个场景，在一个很多文章的列表页面，然后点击任何一个链接，都可以准确的打开详细的文章内容，`这就需要靠传递文章ID，然后后台准确检索文章内容最后呈现`。这个过程每次传递到`详细页面的ID值`都是不一样的，所以就需要路由有动态传值的能力



## 再Route上设置允许动态传值

`<Route path="/list:/id" component={List}>`

就是path上加上`:id`。 这样就设置了允许传值规则

## Link 上传递值

设置好规则后，就可以在`Link`上设置值了，现在设置传递的ID值了，这时候不用再添加id了，直接写值就可以了。

```;javascript
 <li><Link to="/list/123">列表</Link> </li>
```

现在就可以进行传值了。为了方便你的学习，这里给出全部`AppRouter.js`代码。

```;javascript
import React from "react";
import { BrowserRouter as Router, Route, Link } from "react-router-dom";
import Index from './Pages/Index'
import List from './Pages/List'
function AppRouter() {  
    return (    
        <Router>        
            <ul>            
                <li> <Link to="/">首页</Link> </li>            
                <li><Link to="/list/123">列表</Link> </li>        
            </ul>        
            <Route path="/" exact component={Index} />        
            <Route path="/list/:id" component={List} />    
        </Router>  
    );
}
export default AppRouter;
```

## 在List组件上接受并显示传递值

组件接收传递过来的值的时候，可以在声明周期`componentDidMount`中进行，传递的值在`this.props.match`中。我们可以先打印出来,代码如下。

```js
import React, { Component } from 'react';
class List extends Component {    
    constructor(props) {        
        super(props);        
        this.state = {  }    
    }    
    render() {         
        return (  <h2>List Page</h2> );    
    }    
    //-关键代码---------start    
    componentDidMount(){        
        console.log(this.props.match)    
    }    
    //-关键代码---------end
}
export default List;
```

然后在浏览器的控制台中可以看到打印出的对象，对象包括三个部分:

- patch:自己定义的路由规则，可以清楚的看到是可以产地id参数的。
- url: 真实的访问路径，这上面可以清楚的看到传递过来的参数是什么。
- params：传递过来的参数，`key`和`value`值。

明白了match中的对象属性，就可以轻松取得传递过来的ID值了。代码如下:

```js
import React, { Component } from 'react';
class List extends Component {    
    constructor(props) {        
        super(props);        
        this.state = {  }    
    }    
    render() {         
        return (  <h2>List Page->{this.state.id}</h2> );    
    }    
    componentDidMount(){       
        // console.log(this.props.match.params.id)       
        let tempId=this.props.match.params.id        
        this.setState({id:tempId })    
    }
}
export default List;
```

这样就实现了动态传值，需要注意的是如果你不传任何东西，是没办法匹配路由成功的。下一节我们更加形象的作一个动态列表，然后进行动态传值。这节课算是一个理论基础吧。

## [模拟一个列表数组](https://jspang.com/detailed?id=49#toc315)

现在可以在`Index`组件里模拟一个列表数组，就相当于我们从后台动态获取到的内容，然后数组中包括文章的`cid`和`title`。直接在state初始化时进行设置，代码如下：

```js
 constructor(props) {    
     super(props);    
     this.state = {         
        list:[            
            {cid:123,title:'技术胖的个人博客-1'},            
            {cid:456,title:'技术胖的个人博客-2'},            
            {cid:789,title:'技术胖的个人博客-3'},        
        ]    
     }
}
```

有了`list`数组后，再修改一下UI，进行有效的遍历，`Render`代码如下。

```js
 render() {     
    return (         
        <ul>            
        {                
            this.state.list.map((item,index)=>{                    
                return (                        
                    <li key={index}> {item.title} 
                    </li>                    
                )                
            })            
        }        
        </ul>    
)}
```

列表已经可以在`Index`组件里显示出来了，接下来可以配置`<Link>`了,在配置之前，你需要先引入`Link`组件。

```js
import { Link } from "react-router-dom";
```

引入后直接使用进行跳转就可以，但是需要注意一点，要用`{}`的形式，也就是把`to`里边的内容解析成JS的形式，这样才能顺利的传值过去。

```js
render() {     
    return (         
        <ul>            
        {                
            this.state.list.map((item,index)=>{                    
                return (                        
                    <li key={index}>                            
                        <Link to={'/list/'+item.uid}> {item.title}</Link>                         
                    </li>                    
                )                
            })            
        }        
        </ul>    
)}
```

到目前为止，已经很类似我们项目中的列表向详细页进行传值了。为了方便你学习，给出`Index.js`的所有代码.

```js
import React, { Component } from 'react';
import { Link } from "react-router-dom";
class Index extends Component {    
    constructor(props) {        
        super(props);        
        this.state = {             
            list:[                
                {uid:123,title:'技术胖的个人博客-1'},                
                {uid:456,title:'技术胖的个人博客-2'},                
                {uid:789,title:'技术胖的个人博客-3'},            
            ]         
        }    
    }    
    render() {         
        return (             
            <ul>                
            {                    
                this.state.list.map((item,index)=>{                        
                    return (                            
                        <li key={index}>                               
                            <Link to={'/list/'+item.uid}> {item.title}</Link>                             
                        </li>                        
                    )                    
                })                
            }            
            </ul>        
    )    
}}
export default Index;
```

通过四小节的学习，你一定对`React Router`有了基本的了解，接下来的学习会稍微提升一点难度，所以你先要把这四小节课学好，练好.再向下进行。

# reactRouter 重定向-redirect 使用

+ 标签式重定向：利用<Redirect> 标签来进行重定向，业务逻辑不复杂时建议使用这种。
+ 编程式重定向： 利用编程的方式， 一般用于业务逻辑当中，比如登录成功跳转到会员中心

重定向和跳转有一个重要的区别：

跳转是可以用浏览器回退按钮 返回上一级的， 而重定向是不可以的

## 标签式重定向

首先建立一个`Home.js`的页面，这个页面我还是用快速生成的方式来进行编写，代码如下。

```js
import React, { Component } from 'react';
class Home extends Component {    
    constructor(props) {        
        super(props);        
        this.state = {  }    
    }    
    render() {         
        return (  <h2>我是 Home 页面</h2> );    
    }
}
export default Home;
```

这个页面非常简单，就是一个普通的有状态组件。

有了组件后可以配置一下路由规则，也就是在`AppRouter.js`里加一个`<Route>`配置，配置时记得引入`Home`组件。

```js
import Home from './Pages/Home'<Route path="/home/" component={Home} />
```

之后打开`Index.js`文件，从`Index`组件重新定向到`Home`组件，需要先引入`Redirect`。

```js
import { Link , Redirect } from "react-router-dom";
```

引入`Redirect`后，直接在`render`函数里使用就可以了。

```js
 <Redirect to="/home/" />
```

现在就可以实现页面的重定向。

## [编程式重定向](https://jspang.com/detailed?id=49#toc318)

编程式重定向就是不再利用`<Redirect/>`标签，而是直接使用`JS`的语法实现重定向。他一般用在业务逻辑比较发杂的场合或者需要多次判断的场合。我们这里就不模拟复杂场景了，还是利用上边的例子实现一下，让大家看到结果就好。

比如直接在构造函数`constructor`中加入下面的重定向代码。

```react
 this.props.history.push("/home/");  
```

就可以顺利实现跳转，这样看起来和上面的过程是一样的。这两种方式的重定向你可以根据真实需求使用，这样能让你的程序更加的灵活。课后你可以试着模拟用户的登录过程试着用一下这样的跳转。



# 嵌套路由

后台管理系统， 大部分是用嵌套路由，来实现页面总体划分。

## 创建项目

`create-react-app demo2`

`npm isntall --save react-router-dom`

## 初始化基本目录

再src下建立page文件夹。 然后再page下创建video 和 workplace。 然后再src下建立Approuter.js 该文件是路由配置文件

# react-router
