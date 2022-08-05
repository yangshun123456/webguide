> 《千锋HTML5好程序员第三阶段React》学习材料——吴大勋

# 一、create-react-app	 

create-react-app. -----   cra

全局安装create-react-app --- 但并不是一定

```sh
$ npm install -g create-react-app
```

创建一个项目

```sh
$ create-react-app your-app 注意命名方式，不要出现项目名称为驼峰式

Creating a new React app in /dir/your-app.

Installing packages. This might take a couple of minutes. 安装过程较慢，
Installing react, react-dom, and react-scripts... 
```

如果不想全局安装，可以直接使用npx

```sh
$ npx create-react-app react-baisc	也可以实现相同的效果
```



这需要等待一段时间，这个过程实际上会安装三个东西

- react:  react的顶级库
- react-dom: 因为react有很多的运行环境，比如app端的react-native, 我们要在web上运行就使用react-dom
- react-scripts: 包含运行和打包react应用程序的所有脚本及配置

出现下面的界面，表示创建项目成功:

```sh
Success! Created react-baisc at /Users/wudaxun/Desktop/workspace/gpcourse/gp26/code/week4/day01/react-baisc
Inside that directory, you can run several commands:

  yarn start
    Starts the development server.

  yarn build
    Bundles the app into static files for production.

  yarn test
    Starts the test runner.

  yarn eject
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd react-baisc
  yarn start

Happy hacking!
```

根据上面的提示，通过`cd react-baisc`命令进入目录并运行`yarn start`即可运行项目。

生成项目的目录结构如下：

```sh
├── README.md							使用方法的文档
├── node_modules					所有的依赖安装的目录
├── package-lock.json			锁定安装时的包的版本号,保证团队的依赖能保证一致。
├── package.json					
├── public								静态公共目录
└── src										开发用的源代码目录
└── yarn.lock							锁住项目的依赖的版本号
```



常见问题：

- npm安装失败
  - 切换为npm镜像为淘宝镜像
  - 使用yarn，如果本来使用yarn还要失败，还得把yarn的源切换到国内
  - 如果还没有办法解决，请删除node_modules及package-lock.json（或者yarn.lock）然后重新执行`npm install命令`
  - 再不能解决就删除node_modules及package-lock.json的同时清除npm缓存`npm cache clean --force`之后再执行`npm install`命令



# 二、关于React

https://zh-hans.reactjs.org/

## 1、React的起源和发展

React 起源于 Facebook 的内部项目，因为该公司对市场上所有 JavaScript MVC 框架，都不满意，就决定自己写一套，用来架设Instagram 的网站。做出来以后，发现这套东西很好用，就在2013年5月开源了。

react在发展过程中，一直跟随原生js的脚步

2015年推出了使用react来编写移动端的app ---- react-native

## 2、React与传统MVC的关系

轻量级的视图层**库**！*A JavaScript library for building user interfaces*

MVC model view controller

MVP model view presenter

View 非常薄，不部署任何业务逻辑，称为”被动视图”（Passive View），即没有任何主动性，而 Presenter非常厚，所有逻辑都部署在那里。

MVVM

React不是一个完整的MVC框架，最多可以认为是MVC中的V（View），甚至React并不非常认可MVC开发模式；React 构建页面 UI 的库。可以简单地理解为，React 将界面分成了各个独立的小块，每一个块就是组件，这些组件之间可以组合、嵌套，就成了我们的页面。

## 3、React高性能的体现：虚拟DOM

**React高性能的原理：**

在Web开发中我们总需要将变化的数据实时反应到UI上，这时就需要对DOM进行操作。而复杂或频繁的DOM操作通常是性能瓶颈产生的原因（如何进行高性能的复杂DOM操作通常是衡量一个前端开发人员技能的重要指标）。

React为此引入了虚拟DOM（Virtual DOM）的机制：在浏览器端用Javascript实现了一套DOM API。基于React进行开发时所有的DOM构造都是通过虚拟DOM进行，每当数据变化时，React都会重新构建整个DOM树，然后React将当前整个DOM树和上一次的DOM树进行对比，得到DOM结构的区别，然后仅仅将需要变化的部分进行实际的浏览器DOM更新。而且React能够批处理虚拟DOM的刷新，在一个事件循环（Event Loop）内的两次数据变化会被合并，例如你连续的先将节点内容从A-B, B-A，React会认为A变成B，然后又从B变成A  UI不发生任何变化，而如果通过手动控制，这种逻辑通常是极其复杂的。

尽管每一次都需要构造完整的虚拟DOM树，但是因为**虚拟DOM是内存数据**，性能是极高的，而对实际DOM进行操作的仅仅是**Diff算法**，因而能达到提高性能的目的。这样，在保证性能的同时，开发者将不再需要关注某个数据的变化如何更新到一个或多个具体的DOM元素，而只需要关心在任意一个数据状态下，整个界面是如何Render的。

**React Fiber:**

在react 16之后发布的一种react 核心算法，**React Fiber是对核心算法的一次重新实现**(官网说法)。之前用的是diff算法。

在之前React中，更新过程是同步的，这可能会导致性能问题。

当React决定要加载或者更新组件树时，会做很多事，比如调用各个组件的生命周期函数，计算和比对Virtual DOM，最后更新DOM树，这整个过程是同步进行的，也就是说只要一个加载或者更新过程开始，中途不会中断。因为JavaScript单线程的特点，如果组件树很大的时候，每个同步任务耗时太长，就会出现卡顿。

React Fiber的方法其实很简单——分片。把一个耗时长的任务分成很多小片，每一个小片的运行时间很短，虽然总时间依然很长，但是在每个小片执行完之后，都给其他任务一个执行的机会，这样唯一的线程就不会被独占，其他任务依然有运行的机会。

> js单线程
>
> js如何实现多线程   html5新增了 woker 的api（chrome浏览器中不支持woker的嵌套，纯js中使用并不多见，但是在微信小程序中针对渲染的容器重置，可以使用）
>
> https://www.runoob.com/html/html5-webworkers.html

## 4、React的特点和优势

**(1) 虚拟DOM**

我们以前操作dom的方式是通过document.getElementById()的方式，这样的过程实际上是先去读取html的dom结构，将结构转换成变量，再进行操作。而reactjs定义了一套变量形式的dom模型，一切操作和换算直接在变量中，这样减少了操作真实dom，性能真实相当的高，和主流MVC框架有本质的区别，并不和dom打交道

**(2) 组件系统**

react最核心的思想是将页面中任何一个区域或者元素都可以看做一个组件 component

那么什么是组件呢？

组件指的就是同时包含了html、css、js、image元素的聚合体

使用react开发的核心就是将页面拆分成若干个组件，并且react一个组件中同时耦合了css、js、image，这种模式整个颠覆了过去的传统的方式

**(3) 单向数据流**

其实reactjs的核心内容就是数据绑定，所谓数据绑定指的是只要将一些服务端的数据和前端页面绑定好，开发者只关注实现业务就行了

**(4) JSX  语法**

在vue中，我们使用render函数来构建组件的dom结构性能较高，因为省去了查找和编译模板的过程，但是在render中利用createElement创建结构的时候代码可读性较低，较为复杂，此时可以利用jsx语法来在render中创建dom，解决这个问题，但是前提是需要使用工具来编译jsx



# 三、编写第一个react应用程序

> react目前默认匹配的版本为 17.0.2
>
> 需要注意react 17版本的新特性 https://www.jianshu.com/p/59e5a4422131

react开发需要引入多个依赖文件：react.js、react-dom.js，分别又有开发版本和生产版本，create-react-app里已经帮我们把这些东西都安装好了。

把通过CRA创建的工程目录下的src目录清空，然后在里面重新创建一个index.js. 写入以下代码:

```jsx
// vue项目中 src/main.js 作为项目的入口文件
// react项目中 src/index.js 作为项目的入口文件

// 从 react 的包当中引入了 React。
// 只要你要写 React.js 组件就必须引入React, 因为react 里有一种语法叫JSX，
// 稍后会讲到JSX，要写JSX，就必须引入React
import React from 'react'
// ReactDOM 可以帮助我们把 React 组件渲染到页面上去，没有其它的作用了。
// 它是从 react-dom 中引入的，而不是从 react 引入
// 老以前react-dom是包含到react模块中的
import ReactDOM from 'react-dom'

// ReactDOM里有一个render方法，功能就是把组件渲染并且构造 DOM 树，然后插入到页面上某个特定的元素上
ReactDOM.render(
  // 这里就比较奇怪了，它并不是一个字符串，
  // 看起来像是纯 HTML 代码写在 JavaScript 代码里面。
  // 语法错误吗？这并不是合法的 JavaScript 代码,
  //  “在 JavaScript 写的标签的”语法叫 JSX - JavaScript XML。
  <h1>hello world</h1>,
  // 渲染到哪里
  document.getElementById('root')
)

```



# 四、元素与组件

https://zh-hans.reactjs.org/docs/rendering-elements.html

如果代码多了之后，不可能一直在render方法里写，所以就需要把里面的代码提出来，定义一个变量，像这样：

```jsx
import React from 'react'
import ReactDOM from 'react-dom'
// 这里感觉又不习惯了？这是在用JSX定义一下react元素
const app = <h1>欢迎进入React的世界</h1>
ReactDOM.render(
  app,
  document.getElementById('root')
)
```



## 1、函数式组件

由于元素没有办法传递参数，所以我们就需要把之前定义的变量改为一个方法，让这个方法去return一个元素:

```js
import React from 'react'
import ReactDOM from 'react-dom'

// 函数式组件的原理： 函数返回jsx代码，调用函数
// 这里感觉又不习惯了？这是在用JSX定义一下react元素
// 函数式组件
const app = function () {
  // ... 其他业务逻辑
  return <h1>hello react</h1>
}

ReactDOM.render(
  app(),
  document.getElementById('root')
)
```



```jsx
import React from 'react'
import ReactDOM from 'react-dom'

// 函数式组件的原理： 函数返回jsx代码，调用函数
// 这里感觉又不习惯了？这是在用JSX定义一下react元素
// 函数式组件
const app = function (props) {
  // ... 其他业务逻辑
  // 使用props来作为参数的接收的形参
  // 渲染时使用 {} 形式，vue中使用的是 {{}}
  // 属性的值是变量，boolean类型，number类型，对象，数组，null，undefined
  // vue 绑定属性 -- 本质  --- 指令 
  // react 使用 {}
  // react开发其实就是 js开发，遇到使用 react的特性时添加{}
  return <h1>hello react - { props.name }</h1>
}

ReactDOM.render(
  app({
    name: '吴大勋'
  }),
  document.getElementById('root')
)
```

这里我们定义的方法实际上也是react定义组件的第一种方式-定义函数式组件，这也是无状态组件(**现学习阶段**)。但是这种写法不符合react的jsx的风格，更好的方式是使用以下方式进行改造

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

// React组件的首字母必须大写
// 因为 标签字母小写，会被默认为是 固有的 html 标签
// 如果固有的html标签未找到，不能正确的渲染页面
const App = function (props) {
  return <h1>hello world - { props.name }</h1>
}

// 可以在调用React组件时传递属性 -- 父组件给子组件传值
// 子组件通过 默认的形参 props 来获取数据
// vue 在子组件的定义的地方，添加了props选项
ReactDOM.render(
  <App name="吴大勋" />,
  document.getElementById('root')
)
```

这样一个完整的函数式组件就定义好了。但要**注意！注意！注意！**组件名必须**大写**，否则报错。

> html元素周期表：https://www.xuanfengge.com/funny/html5/element/

## 2、class组件

ES6的加入让JavaScript直接支持使用class来定义一个类，react的第二种创建组件的方式就是使用的类的继承，`ES6 class`是目前官方推荐的使用方式，它使用了ES6标准语法来构建，看以下代码：

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

// 得到父组件的数据，通过 this.props 获取
class App extends React.Component {
  render () {
    return <h1>hello React - { this.props.name }</h1>
  }
}

ReactDOM.render(
  <App name="吴大勋"/>,
  document.getElementById('root')
)
```

运行结果和之前完全一样，因为JS里没有真正的class，这个class只是一个语法糖, 但二者的运行机制底层运行机制不一样。

- 函数式组件是直接调用, 在前面的代码里已经有看到

- ```es6 class```组件其实就是一个构造器,每次使用组件都相当于在实例化组件，像这样：

  ```jsx
  import React from 'react'
  import ReactDOM from 'react-dom'
  
  // js中 es5 的继承 和 es6的继承机制的不同之处
  // ES5 的继承，实质是先创造子类的实例对象this，
  // 然后再将父类的方法添加到this上面（Parent.apply(this)）。
  // ES6 的继承机制完全不同，实质是先将父类实例对象的属性和方法，
  // 加到this上面（所以必须先调用super方法），然后再用子类的构造函数修改this。
  class App extends React.Component {
    render () { // 继承父类必须实现的一个钩子函数
      return <h1>hello React</h1>
    }
  }
  
  const app = new App()
  
  ReactDOM.render(
    app.render(),
    document.getElementById('root')
  )
  ```



## 3、更老的一种方法

在16以前的版本还支持这样创建组件, 但现在的项目基本上不用

```jsx
React.createClass({
  render () {
    return (
      <div>{this.props.xxx}</div>
  	)
  }
})
```



## 4、组件的组合、嵌套

将一个组件渲染到某一个节点里的时候，会将这个节点里原有内容覆盖

组件嵌套的方式就是将子组件写入到父组件的模板中去，且react没有Vue中的内容分发机制（slot），所以我们在一个组件的模板中只能看到父子关系

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

const Header = () => {
  return <header>欢迎来到react的世界</header>
}

function Content () {
  return <section>React 核心库只关注于视图层</section>
}

const Footer = function () {
  return <footer>函数式组件被成为无状态组件</footer>
}

class App extends React.Component {
  render () {
    // jsx代码结构复杂，建议使用()包裹即可
    return (
      <div>
        <Header></Header>
        <Content></Content>
        <Footer></Footer>
      </div>
    )
  }
}

ReactDOM.render(
  <App />,
  document.querySelector('#root')
)
```



# 五、JSX 原理

要明白JSX的原理，需要先明白如何用 JavaScript 对象来表现一个 DOM 元素的结构?

看下面的DOM结构

```html
<div class='app' id='appRoot'>
  <h1 class='title'>欢迎进入React的世界</h1>
  <p>
    React.js 是一个帮助你构建页面 UI 的库
  </p>
</div>
```

上面这个 HTML 所有的信息我们都可以用 JavaScript 对象来表示：

```js
{
  tag: 'div',
  attrs: { className: 'app', id: 'appRoot'},
  children: [
    {
      tag: 'h1',
      attrs: { className: 'title' },
      children: ['欢迎进入React的世界']
    },
    {
      tag: 'p',
      attrs: null,
      children: ['React.js 是一个构建页面 UI 的库']
    }
  ]
}
```

但是用 JavaScript 写起来太长了，结构看起来又不清晰，用 HTML 的方式写起来就方便很多了。

于是 React.js 就把 JavaScript 的语法扩展了一下，让 JavaScript 语言能够支持这种直接在 JavaScript 代码里面编写类似 HTML 标签结构的语法，这样写起来就方便很多了。编译的过程会把类似 HTML 的 JSX 结构转换成 JavaScript 的对象结构。

下面代码:

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

const App = () => {
  return (
    <div className="app" id="appRoot">
      <h1 className="title">欢迎来到react的世界</h1>
      <p>React 核心库只关注于视图层</p>
    </div>
  )
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

编译之后将得到这样的代码:

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

// const App = () => {
//   return (
//     <div className="app" id="appRoot">
//       <h1 className="title">欢迎来到react的世界</h1>
//       <p>React 核心库只关注于视图层</p>
//     </div>
//   )
// }

// jsx 原理，也就是为什么使用React组件时必须得引入React的原因
const App = () => {
  return (
    React.createElement(
      'div', { className: 'app', id: 'appRoot' },
      React.createElement('h1', { className: 'title' }, '欢迎来到react的世界'),
      React.createElement('p', {}, 'React 核心库只关注于视图层')
    )
  )
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

`React.createElement` 会构建一个 JavaScript 对象来描述你 HTML 结构的信息，包括标签名、属性、还有子元素等, 语法为

```jsx
React.createElement(
  type,
  [props],
  [...children]
)
```

所谓的 JSX 其实就是 JavaScript 对象，所以使用 React 和 JSX 的时候一定要经过编译的过程:

> JSX —使用react构造组件，bable进行编译—> JavaScript对象 — `ReactDOM.render()`—>DOM元素 —>插入页面

```vue
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <my-com></my-com>
  </div>
</body>

<script src="lib/vue.js"></script>
<script>
  Vue.component('MyCom', {
    // template: `<h1 class="test">hello world</h1>`
    render (createElement) {
      return createElement('h1', { class: 'test' }, 'hello vue jsx')
    }
  })
  new Vue({
    el: '#app'
  })
</script>
</html>
```





# 六、组件中DOM样式

- 行内样式

想给虚拟dom添加行内样式，需要使用表达式传入样式对象的方式来实现：

```jsx
// 注意这里的两个括号，第一个表示我们在要JSX里插入JS了，第二个是对象的括号
 <p style={{color:'red', fontSize:'14px'}}>Hello world</p>
```

```js
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

// 属性的值包含变量 boolean类型，number类型，对象，数组，null，undefined时
// 使用 {} 包裹，且属性不要加""
// 样式处理方式1: 行内样式  ----- 如何在js中写样式（驼峰式）
class App extends Component{
  render () {
    return <div style={ {
      width: '200px', // string
      height: 200, // number
      backgroundColor: '#f66', // 驼峰式
      fontSize: 50
    } }>行内样式</div>
  }
} 

ReactDOM.render(
  <App />,
  document.getElementById('root')
)

```



行内样式需要写入一个样式对象，而这个样式对象的位置可以放在很多地方，例如`render`函数里、组件原型上、外链js文件中

- 使用`class`

React推荐我们使用行内样式，因为React觉得每一个组件都是一个独立的整体

其实我们大多数情况下还是大量的在为元素添加类名，但是需要注意的是，`class`需要写成`className`（因为毕竟是在写类js代码，会收到js规则的现在，而`class`是关键字）

```jsx
<p className="hello" style = {this.style}>Hello world</p>
```

```js
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

import './11style.css'

// 样式处理方式1: 行内样式  ----- 如何在js中写样式（驼峰式）
// 样式处理方式2: class样式  ---- 外部的css文件支持 --- class 写为 className
class App extends Component{
  render () {
    return <div className="active">行内样式</div>
  }
} 

ReactDOM.render(
  <App />,
  document.getElementById('root')
)

```

```css
.active {
  width: 300px;
  height: 300px;
  background-color: #f66;
}
```



- 不同的条件添加不同的样式

有时候需要根据不同的条件添加不同的样式，比如：完成状态，完成是绿色，未完成是红色。那么这种情况下，我们推荐使用[classnames](<https://www.npmjs.com/package/classnames>)这个包：

```
cnpm i classnames -S
```

```js
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

import classnames from 'classnames'

// 样式处理方式1: 行内样式  ----- 如何在js中写样式（驼峰式）
// 样式处理方式2: class样式  ---- 外部的css文件支持 --- class 写为 className
// 样式处理方式3: 不同的条件添加不同的样式 - classnames
// class App extends Component{
//   state = { // 初始化状态
//     flag: true
//   }
//   // <button className="btn btn-success">成功按钮</button>
//   // <button className="btn btn-danger">失败按钮</button>
//   render () {
//     return <button className = {
//       this.state.flag ? 'btn btn-success' : 'btn btn-danger'
//     }>成功按钮</button>
//   }
// } 
class App extends Component{
  state = { // 初始化状态
    flag: false
  }
 
  render () {
    // const str = classnames('btn', {
    //   'btn-success': this.state.flag,
    //   'btn-danger': !this.state.flag
    // })
    const str = classnames({
      'btn': true,
      'btn-success': this.state.flag,
      'btn-danger': !this.state.flag
    })
    return <button className = { str }>成功按钮</button>
  }
} 

ReactDOM.render(
  <App />,
  document.getElementById('root')
)

```



- css-in-js

`styled-components`是针对React写的一套css-in-js框架，简单来讲就是在js中写css。[npm链接](<https://www.npmjs.com/package/styled-components>)

https://styled-components.com/

```
cnpm i styled-components -S
```

```js
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

import styled from 'styled-components'

// 样式处理方式1: 行内样式  ----- 如何在js中写样式（驼峰式）
// 样式处理方式2: class样式  ---- 外部的css文件支持 --- class 写为 className
// 样式处理方式3: 不同的条件添加不同的样式 - classnames
// 样式处理方式4: css-in-js styled-components


// 首字母必须大写，不便于排错，容易于自定义组件混淆
const Button = styled.button`
  width: 400px;
  height: 30px;
  border-radius: 15px;
`

// const Button = () => {
//   return (<button>按钮</button>)
// }
class App extends Component{
 
  render () {
    
    return <Button>css-in-js</Button>
  }
} 

ReactDOM.render(
  <App />,
  document.getElementById('root')
)

```

* css的局部作用域 --- 类似于vue组件中的scoped

https://www.html.cn/create-react-app/

https://www.html.cn/create-react-app/docs/adding-a-css-modules-stylesheet/

```js
import React, { Component } from 'react'
import ReactDOM from 'react-dom'
import App from './App.js'
ReactDOM.render(
  <App />,
  document.getElementById('root')
)

```

```js
import React, { Component } from 'react'
import styles from  './App.module.css'
import './13index.css'
export default class App extends Component {
  render() {
    return (
      <div className={ styles.test }>
        app
        <div className ="test">测试</div>
      </div>
    )
  }
}

```

```css
// 13index.css
.test {
  color: #f66
}
```

```css
// App.module.css
.test {
  color: #0f0
}
```

> css文件必须是。*.module.css 方式。引入时必须使用。import styles from 结构，调用 使用styles.**方式

# 七、组件的数据挂载方式

## 1、属性(props)

`props`是正常是外部传入的，组件内部也可以通过一些方式来初始化的设置，属性不能被组件自己更改，但是你可以通过父组件主动重新渲染的方式来传入新的 `props`

属性是描述性质、特点的，组件自己不能随意更改。

之前的组件代码里面有`props`的简单使用，总的来说，在使用一个组件的时候，可以把参数放在标签的属性当中，所有的属性都会作为组件 `props` 对象的键值。通过箭头函数创建的组件，需要通过函数的参数来接收`props`:

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

const Header = (props) => {
  return <header>欢迎来到{ props.title }的世界!</header>
}

function Content (props) {
  return <section>{ props.content } 核心库只关注于视图层</section>
}

// const Footer = function () {
//   return <footer>函数式组件被成为无状态组件</footer>
// }
class Footer extends React.Component {
  render () {
    return <footer>{ this.props.title }函数式组件被称为无状态组件</footer>
  }
}

const App = () => {
  return (
    <div>
      <Header title="react" ></Header>
      <Content content="react.js"></Content>
      <Footer title="react"></Footer>
    </div>
  )
}

ReactDOM.render(
  <App />,
  document.querySelector('#root')
)
```

### (1) 设置组件的默认props

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

const Header = (props) => {
  return <header>欢迎来到{ props.title }的世界!</header>
}
// 设置函数式组件的默认值
Header.defaultProps = {
  title: 'react'
}

class Content extends React.Component {
  static defaultProps = { // 设置类组件的默认值
    content: 'facebook react.js'
  }
  render () {
    return <section>{ this.props.content } 核心库只关注于视图层</section>
  }
}

// 设置类组件的默认值
// Content.defaultProps = {
//   content: 'react.js'
// }

const App = () => {
  return (
    <div>
      <Header ></Header>
      <Content ></Content>
    </div>
  )
}

ReactDOM.render(
  <App />,
  document.querySelector('#root')
)
```

> 不管是类组件还是函数式组件，都可以通过定义好组件之后，通过 组件.defaultProps = { key: value }定义属性的默认值
>
> 如果组件是类组件，还可以通过 static defaultProps = { key: value } 的静态属性设置属性的默认值 ---- 推荐

### (2) 使用prop-types检查props

React其实是为了构建大型应用程序而生, 在一个大型应用中，根本不知道别人使用你写的组件的时候会传入什么样的参数，有可能会造成应用程序运行不了，但是不报错。为了解决这个问题，React提供了一种机制，让写组件的人可以给组件的```props```设定参数检查，需要安装和使用[prop-types](<https://www.npmjs.com/package/prop-types>):

```
$ cnpm i prop-types -S
```

```js
import React from 'react'
import ReactDOM from 'react-dom'
// react 15.5版本以后抽离出来的
import PropTypes from 'prop-types'

const Header = (props) => {
  return <header>欢迎来到{ props.title }的世界!</header>
}
Header.defaultProps = {
  title: 'react'
}
Header.propTypes = {
  // title: PropTypes.string // 只是校验了基本的数据类型
  // title: PropTypes.string.isRequired // 校验数据类型并声明该属性是必须传递的
  // 多个类型中的其中的一个并且是必传项的设定
  // title: PropTypes.oneOfType([ PropTypes.string, PropTypes.number ]).isRequired
  title: function (props, propName, componentName) { // 自定义验证器
    console.log(props) // { title: 'react' }
    console.log(propName) // title
    console.log(componentName) // Header
    if (props[propName].length < 5) {
      return new Error('传递的字符必须至少5位')
    }
  }
}

class Content extends React.Component {
  static defaultProps = {
    content: 'facebook react.js'
  }
  render () {
    return <section>{ this.props.content } 核心库只关注于视图层</section>
  }
}
Content.propTypes = {
  content: PropTypes.string
}


const App = () => {
  return (
    <div>
      <Header ></Header>
      <Content ></Content>
    </div>
  )
}

ReactDOM.render(
  <App />,
  document.querySelector('#root')
)
```



```js
import PropTypes from 'prop-types';

MyComponent.propTypes = {
  // 你可以将属性声明为 JS 原生类型，默认情况下
  // 这些属性都是可选的。
  optionalArray: PropTypes.array,
  optionalBool: PropTypes.bool,
  optionalFunc: PropTypes.func,
  optionalNumber: PropTypes.number,
  optionalObject: PropTypes.object,
  optionalString: PropTypes.string,
  optionalSymbol: PropTypes.symbol,

  // 任何可被渲染的元素（包括数字、字符串、元素或数组）
  // (或 Fragment) 也包含这些类型。
  optionalNode: PropTypes.node,

  // 一个 React 元素。
  optionalElement: PropTypes.element,

  // 一个 React 元素类型（即，MyComponent）。
  optionalElementType: PropTypes.elementType,

  // 你也可以声明 prop 为类的实例，这里使用
  // JS 的 instanceof 操作符。
  optionalMessage: PropTypes.instanceOf(Message),

  // 你可以让你的 prop 只能是特定的值，指定它为
  // 枚举类型。
  optionalEnum: PropTypes.oneOf(['News', 'Photos']),

  // 一个对象可以是几种类型中的任意一个类型
  optionalUnion: PropTypes.oneOfType([
    PropTypes.string,
    PropTypes.number,
    PropTypes.instanceOf(Message)
  ]),

  // 可以指定一个数组由某一类型的元素组成
  optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

  // 可以指定一个对象由某一类型的值组成
  optionalObjectOf: PropTypes.objectOf(PropTypes.number),

  // 可以指定一个对象由特定的类型值组成
  optionalObjectWithShape: PropTypes.shape({
    color: PropTypes.string,
    fontSize: PropTypes.number
  }),

  // An object with warnings on extra properties
  optionalObjectWithStrictShape: PropTypes.exact({
    name: PropTypes.string,
    quantity: PropTypes.number
  }),

  // 你可以在任何 PropTypes 属性后面加上 `isRequired` ，确保
  // 这个 prop 没有被提供时，会打印警告信息。
  requiredFunc: PropTypes.func.isRequired,

  // 任意类型的必需数据
  requiredAny: PropTypes.any.isRequired,

  // 你可以指定一个自定义验证器。它在验证失败时应返回一个 Error 对象。
  // 请不要使用 `console.warn` 或抛出异常，因为这在 `oneOfType` 中不会起作用。
  customProp: function(props, propName, componentName) {
    if (!/matchme/.test(props[propName])) {
      return new Error(
        'Invalid prop `' + propName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  },

  // 你也可以提供一个自定义的 `arrayOf` 或 `objectOf` 验证器。
  // 它应该在验证失败时返回一个 Error 对象。
  // 验证器将验证数组或对象中的每个值。验证器的前两个参数
  // 第一个是数组或对象本身
  // 第二个是他们当前的键。
  customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
    if (!/matchme/.test(propValue[key])) {
      return new Error(
        'Invalid prop `' + propFullName + '` supplied to' +
        ' `' + componentName + '`. Validation failed.'
      );
    }
  })
};

```

> 如果既要验证数据类型也要自定义验证器 显得无能为力
>
> 如果非要做，验证数据类型写到自定义验证器内 

### (3) props.children

我们知道使用组件的时候，可以嵌套。要在自定义组件的使用嵌套结构，就需要使用 `props.children` 。在实际的工作当中，我们几乎每天都需要用这种方式来编写组件。

等同于 vue中的 slot 插槽

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

const Header = (props) => {
  return <header>
    { props.children }
  </header>
}


class Content extends React.Component {
  render () {
    return <section>
      { this.props.children }
    </section>
  }
}

const App = () => {
  return (
    <div>
      <Header>欢迎来到<i>react</i>的世界!</Header>
      <Content><i>react.js</i>核心库只关注于视图层</Content>
    </div>
  )
}

ReactDOM.render(
  <App />,
  document.querySelector('#root')
)
```

```js
import React from 'react'
import ReactDOM from 'react-dom'

const Header = (props) => {
  console.log(props)
  return <header>
    { props.children[1] } 说 { props.children[0] }
  </header>
}


class Content extends React.Component {
  render () {
    return <section>
      { this.props.children }
    </section>
  }
}

const App = () => {
  return (
    <div>
      <Header>
        <h1>欢迎来到<i>react</i>的世界!</h1>
        <div>吴大勋</div>
      </Header>
      <Content><i>react.js</i>核心库只关注于视图层</Content>
    </div>
  )
}

ReactDOM.render(
  <App />,
  document.querySelector('#root')
)
```

> 实现类似vue的具名插槽，需要通过 props.children 的下标去访问



## 2、状态(state)

状态就是组件描述某种显示情况的数据，由组件自己设置和更改，也就是说由组件自己维护，使用状态的目的就是为了在不同的状态下使组件的显示不同(自己管理)

### (1) 定义state

第一种方式(推荐)

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  // es7中类的属性的方式添加状态
  // 自己搭建webpack开发react的脚手架时：现在不需要额外的配置webpack对于es7的属性的支持了，默认就支持（大概去年20年11月还要配置）
  state = {
    msg: 'hello react'
  }
  render () {
    return (
      <div>
        <h1>react的状态</h1>
        { this.state.msg }
      </div>
    )
  }
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

另一种方式

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  // 子类必须在constructor方法中调用super方法，否则新建实例时会报错。
  // 这是因为子类自己的this对象，必须先通过父类的构造函数完成塑造，
  // 得到与父类同样的实例属性和方法，然后再对其进行加工，
  // 加上子类自己的实例属性和方法。
  // 如果不调用super方法，子类就得不到this对象。
  constructor (props) { // es6定义react组件状态的方式
    super(props)
    this.state = {
      msg: 'hello world'
    }
  }
  render () {
    return (
      <div>
        <h1>react的状态</h1>
        { this.state.msg }
      </div>
    )
  }
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

`this.props`和`this.state`是纯js对象,在vue中，data属性是利用`Object.defineProperty`处理过的，更改data的数据的时候会触发数据的`getter`和`setter`，但是React中没有做这样的处理，如果直接更改的话，react是无法得知的，所以，需要使用特殊的更改状态的方法`setState`。

### (2) setState

`isLiked` 存放在实例的 `state` 对象当中，组件的 `render` 函数内，会根据组件的 `state` 的中的`isLiked`不同显示“取消”或“收藏”内容。下面给 `button` 加上了点击的事件监听。

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  // es7中类的属性的方式添加状态
  // 现在不需要额外的配置webpack对于es7的属性的支持了，默认就支持（大概去年20年11月还要配置）
  state = {
    name: 'React',
    isLiked: false
  }
  render () {
    return (
      <div>
        <h1>欢迎来到 { this.state.name } 的世界</h1>
        <button onClick = { () => {
          // ? 为什么要使用箭头函数
          // ？如何修改状态，能否像vue一样，直接修改值
          // this.state.isLiked = !this.state.isLiked  错误的
          this.setState({
            isLiked: !this.state.isLiked
          }, () => {
            console.log('updated')
          })
        } }>
          {
            this.state.isLiked ? '❤️取消' : '🖤收藏'
          }
        </button>
      </div>
    )
  }
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```

`setState`有两个参数

第一个参数可以是对象，也可以是方法return一个对象，我们把这个参数叫做`updater`

- 参数是对象

  ```jsx
  this.setState({
    isLiked: !this.state.isLiked
  })
  ```

- 参数是方法

  ```jsx
  import React from 'react'
  import ReactDOM from 'react-dom'
  
  class App extends React.Component {
    // es7中类的属性的方式添加状态
    // 现在不需要额外的配置webpack对于es7的属性的支持了，默认就支持（大概去年20年11月还要配置）
    state = {
      name: 'React',
      isLiked: false
    }
    render () {
      return (
        <div>
          <h1>欢迎来到 { this.state.name } 的世界</h1>
          <button onClick = { () => {
            // ? 为什么要使用箭头函数
            // ？如何修改状态，能否像vue一样，直接修改值
            // this.state.isLiked = !this.state.isLiked  错误的
            this.setState((prevState, props) => {
              return {
                isLiked: !prevState.isLiked
              }
            }, () => {
              console.log('更新了')
            })
          } }>
            {
              this.state.isLiked ? '❤️取消' : '🖤收藏'
            }
          </button>
        </div>
      )
    }
  }
  
  ReactDOM.render(
    <App />,
    document.getElementById('root')
  )
  ```
  
  注意的是这个方法接收两个参数，第一个是上一次的state, 第二个是props

`setState`是异步的，所以想要获取到最新的state，没有办法获取，就有了第二个参数，这是一个可选的回调函数

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

class App extends React.Component {
  // es7中类的属性的方式添加状态
  // 现在不需要额外的配置webpack对于es7的属性的支持了，默认就支持（大概去年20年11月还要配置）
  state = {
    name: 'React',
    isLiked: false
  }
  render () {
    return (
      <div>
        <h1>欢迎来到 { this.state.name } 的世界</h1>
        <button onClick = { () => {
          // ? 为什么要使用箭头函数
          // ？如何修改状态，能否像vue一样，直接修改值
          // this.state.isLiked = !this.state.isLiked  错误的
          this.setState((prevState, props) => {
            return {
              isLiked: !prevState.isLiked
            }
          }, () => {
            console.log('更新了')
            console.log('回调里的',this.state.isLiked)
          })
          console.log('回调外的',this.state.isLiked)
        } }>
          {
            this.state.isLiked ? '❤️取消' : '🖤收藏'
          }
        </button>
      </div>
    )
  }
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
)
```



## 3、属性vs状态

相似点：都是纯js对象，都会触发render更新，都具有确定性（状态/属性相同，结果相同）

不同点： 

1. 属性能从父组件获取，状态不能
2. 属性可以由父组件修改，状态不能
3. 属性能在内部设置默认值，状态也可以
4. 属性不在组件内部修改，状态要改
5. 属性能设置子组件初始值，状态不可以
6. 属性可以修改子组件的值，状态不可以

`state` 的主要作用是用于组件保存、控制、修改自己的可变状态。`state` 在组件内部初始化，可以被组件自身修改，而外部不能访问也不能修改。你可以认为 `state` 是一个局部的、只能被组件自身控制的数据源。`state` 中状态可以通过 `this.setState`方法进行更新，`setState` 会导致组件的重新渲染。

`props` 的主要作用是让使用该组件的父组件可以传入参数来配置该组件。它是外部传进来的配置参数，组件内部无法控制也无法修改。除非外部组件主动传入新的 `props`，否则组件的 `props` 永远保持不变。

如果搞不清 `state` 和 `props` 的使用场景，记住一个简单的规则：**尽量少地用 `state`，多用 `props`**。

没有 `state` 的组件叫无状态组件（stateless component），设置了 state 的叫做有状态组件（stateful component）。因为状态会带来管理的复杂性，我们尽量多地写无状态组件，尽量少地写有状态的组件。这样会降低代码维护的难度，也会在一定程度上增强组件的可复用性。



## 4、状态提升

如果有多个组件共享一个数据，把这个数据放到共同的父级组件中来管理



## 5、受控组件与非受控组件 

React组件的数据渲染是否被调用者传递的`props`完全控制，控制则为受控组件，否则非受控组件。



## 6、渲染数据

- 条件渲染

  React 中的条件渲染和 JavaScript 中的一样，使用 JavaScript 运算符 [`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) 或者[条件运算符](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)去创建元素来表现当前的状态，然后让 React 根据它们来更新 UI。

  条件渲染中，不要在jsx代码中使用 if else，jsx代码中只能使用条件运算符

  但是可以在if语句中返回不同的jsx代码

  ```jsx
  import React, { Component } from 'react';
  
  export default class App extends Component {
    state = {
      flag: true
    }
    render() {
      return (
        <div>
          <button onClick={ () => {
            this.setState((prevState, props) => {
              return {
                flag: !prevState.flag
              }
            })
          } }>切换</button>
          {
            this.state.flag ? '😍喜欢' : '😠讨厌'
          }
        </div>
      );
    }
  }
  
  ```

  ```jsx
  import React, { Component } from 'react';
  
  export default class App extends Component {
    state = {
      flag: true
    }
    render () {
      if(this.state.flag) {
        return (
          <div>
            <button onClick={ () => {
              this.setState((prevState, props) => {
                return {
                  flag: !prevState.flag
                }
              })
            } }>切换</button>
            '😍喜欢'
          </div>
        );
      } else {
        return (
          <div>
            <button onClick={ () => {
              this.setState((prevState, props) => {
                return {
                  flag: !prevState.flag
                }
              })
            } }>切换</button>
            '😠讨厌'
          </div>
        )
      }
    }
  }
  
  ```

  

  ```jsx
  import React, { Component } from 'react';
  
  export default class App extends Component {
    state = {
      flag: true
    }
    render () {
      let str = '😍喜欢'
      if (this.state.flag) {
        str = '😍喜欢'
      } else {
        str =  '😠讨厌'
      }
      return (
        <div>
          <button onClick={ () => {
            this.setState((prevState, props) => {
              return {
                flag: !prevState.flag
              }
            })
          } }>切换</button>
          { str }
        </div>
      );
    }
  }
  
  ```

- 列表渲染

  不要在jsx中使用 for 系列循环（for/for in / for of /foreach）

  但是可以在for 系列循环内部构建react元素，将react元素压入一个数组，最好显示数组即可

  ```jsx
  import React, { Component } from 'react';
  
  export default class App extends Component {
  
    render () {
      const arr = [ 1, 2, 3, 4]
      const list = [
        <h1 key="1">标题1</h1>,
        <h1 key="2">标题2</h1>,
        <h1 key="3">标题3</h1>,
        <h1 key="4">标题4</h1>
      ]
      return (
        <div>
          <div>{ arr }</div>
          <div>{ list }</div>
        </div>
      );
    }
  }
  
  ```

  ```jsx
  import React, { Component } from 'react';
  
  export default class App extends Component {
    state = {
      proList: []
    }
  
    // vue mounted
    // react componentDidMount 
    componentDidMount () {
      fetch('http://121.89.205.189/api/pro/list?limitNum=20')
        .then(res => res.json())
        .then(res => {
          console.log(res)
          this.setState({
            proList: res.data
          })
        })
    }
  
    render () {
      const { proList } = this.state
      return (
        <div>
          {
            proList && proList.map(item => {
              return (
                <div key={item.proid}>{ item.proname }</div>
              )
            })
          }
        </div>
      );
    }
  }
  
  ```

  ```jsx
  import React, { Component } from 'react';
  
  export default class App extends Component {
    state = {
      proList: []
    }
  
    // vue mounted
    // react componentDidMount 
    componentDidMount () {
      fetch('http://121.89.205.189/api/pro/list?limitNum=20')
        .then(res => res.json())
        .then(res => {
          console.log(res)
          this.setState({
            proList: res.data
          })
        })
    }
  
    render () {
      const { proList } = this.state
  
      if (proList) {
        const arr = []
        proList.forEach(item => {
          arr.push(<div key = { item .proid }>{ item.proname }</div>)
        })
  
        return (
          <div>
            <h1>数组特性</h1>
            { arr }
          </div>
        )
      } else {
        return null
      }
      
    }
  }
  
  ```

React的高效依赖于所谓的 Virtual-DOM，尽量不碰 DOM。对于列表元素来说会有一个问题：元素可能会在一个列表中改变位置。要实现这个操作，只需要交换一下 DOM 位置就行了，但是React并不知道其实我们只是改变了元素的位置，所以它会重新渲染后面两个元素（再执行 Virtual-DOM ），这样会大大增加 DOM 操作。但如果给每个元素加上唯一的标识，React 就可以知道这两个元素只是交换了位置，这个标识就是```key```，这个 `key` 必须是每个元素唯一的标识

- dangerouslySetInnerHTML - 类似于vue中的v-html

对于富文本创建的内容，后台拿到的数据是这样的：

```js
content = "<p>React.js是一个构建UI的库</p>"
```

处于安全的原因，React当中所有表达式的内容会被转义，如果直接输入，标签会被当成文本。这时候就需要使用`dangerouslySetHTML`属性，它允许我们动态设置`innerHTML`

```jsx
import React, { Component } from 'react';

export default class App extends Component {
  state = {
    msg: '<h1>dangerouslySetInnerHTML</h1>'
  }
  render() {
    return (
      <div> 
        { this.state.msg }
        <div dangerouslySetInnerHTML={ { __html: this.state.msg } }></div>
      </div>
    );
  }
}

```

* react中空标签 - vue中 template

  https://zh-hans.reactjs.org/docs/fragments.html#keyed-fragments

  ```jsx
  import React, { Component, Fragment } from 'react';
  
  // <></>
  // <React.Fragment></React.Fragment>
  // export default class App extends Component {
  //   render() {
  //     return (
  //       <>
  //         <div> textInComponent </div>
  //         <div> textInComponent </div>
  //       </>
  //     );
  //   }
  // }
  
  
  // React.Fragment 可以添加 key值，添加的key 具有唯一性
  // <></>不可以添加任何的属性
  export default class App extends Component {
    render() {
      return (
        <Fragment>
          <div> textInComponent </div>
          <div> textInComponent </div>
        </Fragment>
      );
    }
  }
  
  ```

  

# 八、事件处理

## 1、绑定事件

采用on+事件名的方式来绑定一个事件，注意，这里和原生的事件是有区别的，原生的事件全是小写`onclick`, React里的事件是驼峰`onClick`，**React的事件并不是原生事件，而是合成事件**。

合成事件：https://zh-hans.reactjs.org/docs/events.html

## 2、事件handler的写法

**事件本身处的位置是一个属性，如果属性的值是一个函数，使用{}包裹，一定要保证该函数内的this指向**

- 直接在render里写行内的箭头函数
- 在组件内使用箭头函数定义的一个方法
- 直接在组件内定义一个非箭头函数的方法，然后在render里直接使用`onClick={this.handleClick.bind(this)}`
- 直接在组件内定义一个非箭头函数的方法，然后在constructor里bind(this)

```jsx
import React, { Component } from 'react';

export default class App extends Component {
  constructor (props) {
    super(props)
    this.addFn = this.add.bind(this)
  }
  state = {
    count: 1
  }

  add10Handler = () => {
    this.setState({ count: this.state.count + 10 })
  } 
  
  add100Handler () {
    this.setState({ count: this.state.count + 100 })
  } 

  add () {
    this.setState({ count: this.state.count + 1000 })
  }
  render() {
    return (
      <>
        { this.state.count }
        {/* 1.直接在render里写行内的箭头函数 */}
        <button onClick = { () => {
          this.setState({ count: this.state.count + 1 })
        } }>点击事件加1</button>
        {/* 在组件内使用箭头函数定义的一个方法 */}
        <button onClick = { this.add10Handler }>点击事件加10</button>
        {/* 直接在组件内定义一个非箭头函数的方法，然后在render里直接使用`onClick={this.handleClick.bind(this)} */}
        <button onClick = { this.add100Handler.bind(this) }>点击事件加100</button>
        {/* 直接在组件内定义一个非箭头函数的方法，然后在constructor里bind(this) */}
        <button onClick = { this.addFn }>点击事件加1000</button>

      </>
    );
  }
}

```



## 3、Event 对象

和普通浏览器一样，事件handler会被自动传入一个 `event` 对象，这个对象和普通的浏览器 `event` 对象所包含的方法和属性都基本一致。不同的是 React中的 `event` 对象并不是浏览器提供的，而是它自己内部所构建的。它同样具有`event.stopPropagation`、`event.preventDefault` 这种常用的方法

```jsx
import React, { Component } from 'react';

export default class App extends Component {
  constructor (props) {
    super(props)
    this.addFn = this.add.bind(this)
  }
  state = {
    count: 1
  }

  add10Handler = (e) => {
    console.log(e)
    this.setState({ count: this.state.count + 10 })
  } 
  
  add100Handler (e) {
    console.log(e)
    this.setState({ count: this.state.count + 100 })
  } 

  add (e) {
    console.log(e)
    this.setState({ count: this.state.count + 1000 })
  }
  render() {
    return (
      <>
        { this.state.count }
        {/* 1.直接在render里写行内的箭头函数 */}
        <button onClick = { (e) => {
          console.log(e)
          this.setState({ count: this.state.count + 1 })
        } }>点击事件加1</button>
        {/* 在组件内使用箭头函数定义的一个方法 */}
        <button onClick = { this.add10Handler }>点击事件加10</button>
        {/* 直接在组件内定义一个非箭头函数的方法，然后在render里直接使用`onClick={this.handleClick.bind(this)} */}
        <button onClick = { this.add100Handler.bind(this) }>点击事件加100</button>
        {/* 直接在组件内定义一个非箭头函数的方法，然后在constructor里bind(this) */}
        <button onClick = { this.addFn }>点击事件加1000</button>

      </>
    );
  }
}

```



## 4、事件的参数传递

```
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

```jsx
import React, { Component } from 'react';

export default class App extends Component {
  constructor (props) {
    super(props)
    this.addFn = this.add.bind(this)
  }
  state = {
    count: 1
  }

  add10Handler = (e) => {
    console.log(e)
    this.setState({ count: this.state.count + 10 })
  } 
  
  add100Handler (e) {
    console.log(e)
    this.setState({ count: this.state.count + 100 })
  } 

  add (e) {
    console.log(e)
    this.setState({ count: this.state.count + 1000 })
  }

  // addParams (params, e) {
  //   console.log(this)
  //   this.setState({ count: this.state.count + params })
  // }
  addParams = (params, e) => {
    console.log(this)
    this.setState({ count: this.state.count + params })
  }

  addParamsFn (params, event) {
    console.log(event)
    this.setState({ count: this.state.count + params })
  }
  render() {
    return (
      <>
        { this.state.count }
        {/* 1.直接在render里写行内的箭头函数 */}
        <button onClick = { (e) => {
          console.log(e)
          this.setState({ count: this.state.count + 1 })
        } }>点击事件加1</button>
        {/* 在组件内使用箭头函数定义的一个方法 */}
        <button onClick = { this.add10Handler }>点击事件加10</button>
        {/* 直接在组件内定义一个非箭头函数的方法，然后在render里直接使用`onClick={this.handleClick.bind(this)} */}
        <button onClick = { this.add100Handler.bind(this) }>点击事件加100</button>
        {/* 直接在组件内定义一个非箭头函数的方法，然后在constructor里bind(this) */}
        <button onClick = { this.addFn }>点击事件加1000</button>
        <button onClick = { (e) => {
          this.addParams(50, e)
        }}>点击事件加 参数50</button>
        <button onClick = { this.addParamsFn.bind(this, 500)}>点击事件加 参数500</button>

      </>
    );
  }
}

```



## 5、处理用户输入

```jsx
import React, { Component } from 'react';

export default class App extends Component {
  state = {
    firstName: '1',
    lastName: '2'
  }

  changeFirstName = (event) => {
    this.setState({
      firstName: event.target.value
    })
  }
  changeLastName = (event) => {
    this.setState({
      lastName: event.target.value
    })
  }
  render() {
    return (
      <div>
        <input type="text" value = { this.state.firstName }  onChange = { this.changeFirstName }/> +   
        <input type="text" value = { this.state.lastName } onChange = { this.changeLastName } /> =
        { this.state.firstName + this.state.lastName}   
      </div>
    );
  }
}

```



# 九、表单

在 React 里，HTML 表单元素的工作方式和其他的 DOM 元素有些不同，这是因为表单元素通常会保持一些内部的 state。例如这个纯 HTML 表单只接受一个名称：

```html
<form>
  <label>
    名字:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="提交" />
</form>
```

此表单具有默认的 HTML 表单行为，即在用户提交表单后浏览到新页面。如果你在 React 中执行相同的代码，它依然有效。但大多数情况下，使用 JavaScript 函数可以很方便的处理表单的提交， 同时还可以访问用户填写的表单数据。实现这种效果的标准方式是使用“受控组件”。



## 1、受控组件

在 HTML 中，表单元素（如<input>、 <textarea> 和 <select>）通常自己维护 state，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用`setState()`来更新。

我们可以把两者结合起来，使 React 的 state 成为“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。

例如，我们可以将上一个表单进行改造，保证还是受控组件：

```javascript
import React, { Component } from 'react';

// 受控组件 --- value 配合 onChange 且value的值为state中的值
export default class App extends Component {
  state = {
    firstName: '1',
    lastName: '2'
  }

  handler = (event) => {
    console.log(event.target.name)
    this.setState({
      [event.target.name]: event.target.value
    })
  }
  render() {
    return (
      <div>
        <input name="firstName" type="text" value = { this.state.firstName }  onChange = { this.handler }/> +   
        <input name="lastName" type="text" value = { this.state.lastName } onChange = { this.handler } /> =
        { this.state.firstName + this.state.lastName}   
      </div>
    );
  }
}

```

由于在表单元素上设置了 `value` 属性，因此显示的值将始终为 `this.state.value`，这使得 React 的 state 成为唯一数据源。由于 `handler` 在每次按键时都会执行并更新 React 的 state，因此显示的值将随着用户输入而更新。

对于受控组件来说，输入的值始终由 React 的 state 驱动。你也可以将 value 传递给其他 UI 元素，或者通过其他事件处理函数重置，但这意味着你需要编写更多的代码。



## 2、textarea 标签
在 HTML 中, `<textarea>` 元素通过其子元素定义其文本:

```
<textarea>
  你好， 这是在 text area 里的文本
</textarea>
```

而在 React 中，`<textarea>` 使用 `value` 属性代替。这样，可以使得使用 `<textarea>` 的表单和使用单行 input 的表单非常类似：

```javascript
import React, { Component } from 'react';

// 受控组件 --- value 配合 onChange 且value的值为state中的值
export default class App extends Component {
  state = {
    firstName: '1',
    lastName: '2',
    note: ''
  }

  handler = (event) => {
    console.log(event.target.name)
    this.setState({
      [event.target.name]: event.target.value
    })
  }
  render() {
    return (
      <div>
        <input name="firstName" type="text" value = { this.state.firstName }  onChange = { this.handler }/> +   
        <input name="lastName" type="text" value = { this.state.lastName } onChange = { this.handler } /> =
        { this.state.firstName + this.state.lastName}   
        <br />
        <textarea name="note" value = { this.state.node } onChange = { this.handler }></textarea>
        { this.state.note }
      </div>
    );
  }
}

```

请注意，`this.state.value` 初始化于构造函数中，因此文本区域默认有初值。



## 3、select 标签

在 HTML 中，`<select>` 创建下拉列表标签。例如，如下 HTML 创建了水果相关的下拉列表：

```
<select>
  <option value="grapefruit">葡萄柚</option>
  <option value="lime">酸橙</option>
  <option selected value="coconut">椰子</option>
  <option value="mango">芒果</option>
</select>
```

请注意，由于 `selected` 属性的缘故，椰子选项默认被选中。React 并不会使用 `selected` 属性，而是在根 `select` 标签上使用 `value` 属性。这在受控组件中更便捷，因为您只需要在根标签中更新它。例如：

```javascript
import React, { Component } from 'react';

// 受控组件 --- value 配合 onChange 且value的值为state中的值
export default class App extends Component {
  state = {
    firstName: '1',
    lastName: '2',
    note: '',
    hobby: '篮球'
  }

  handler = (event) => {
    console.log(event.target.name)
    this.setState({
      [event.target.name]: event.target.value
    })
  }
  render() {
    return (
      <div>
        <input name="firstName" type="text" value = { this.state.firstName }  onChange = { this.handler }/> +   
        <input name="lastName" type="text" value = { this.state.lastName } onChange = { this.handler } /> =
        { this.state.firstName + this.state.lastName}   
        <br />
        <textarea name="note" value = { this.state.note } onChange = { this.handler }></textarea>
        { this.state.note }
        <br />
        <select name="hobby" value = { this.state.hobby } onChange = { this.handler }>
          <option value="篮球">篮球</option>
          <option value="排球">排球</option>
          <option value="网球">网球</option>
          <option value="足球">足球</option>
          <option value="羽毛球">羽毛球</option>
        </select>{this.state.hobby}
      </div>
    );
  }
}

```

总的来说，这使得, `<input type="text">`, `<textarea>` 和 `<select>`之类的标签都非常相似—它们都接受一个 `value` 属性，你可以使用它来实现受控组件。

> 注意
>
> 你可以将数组传递到 `value` 属性中，以支持在 `select` 标签中选择多个选项：
>
> ```
> <select multiple={true} value={['B', 'C']}>
> ```

```javascript
class MulFlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: "coconut",
      arr: [],
      options: [
        { value: "grapefruit", label: "葡萄柚" },
        { value: "lime", label: "酸橙" },
        { value: "coconut", label: "椰子" },
        { value: "mango", label: "芒果" }
      ]
    };

    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e){
    let idx = this.state.arr.findIndex(item=>{
      return item === e.target.value
    })
    if (idx >= 0) {
      this.state.arr.splice(idx,1);
    } else {
      this.state.arr.push(e.target.value);
    }
    let arr = this.state.arr;
    this.setState({arr});
  }

  render() {
    return (
      <div>
        <select multiple={true} value={this.state.arr} onChange={this.handleChange}>
          {this.state.options.map((item,index) => {
            return <option value={item.value} key={index}>{item.label}</option>;
          })}
        </select>
      </div>
    );
  }
}

export default Test4;
```

## 4.radio 以及 checkbox

```jsx
import React, { Component } from 'react';

// 受控组件 --- value 配合 onChange 且value的值为state中的值
export default class App extends Component {
  state = {
    firstName: '1',
    lastName: '2',
    note: '',
    hobby: '篮球',
    sex: '男',
    course: []
  }

  handler = (event) => {
    console.log(event.target.name)
    this.setState({
      [event.target.name]: event.target.value
    })
  }

  changeSex = (event) => {
    console.log(event.target.checked)
    this.setState({
      [event.target.name]: event.target.value
    })
  }

  changeCourse = (event) => {
    // console.log(event.target.checked, event.target.value)
    const arr = this.state.course
    const index = arr.findIndex(item => item === event.target.value) 
    if (index === -1 && event.target.checked) {
      arr.push( event.target.value )
    } 
    if (!event.target.checked && index !== -1 ) {
      arr.splice(index, 1)
    } 
    console.log(arr)
    this.setState({
      course: arr
    })
  }
  render() {
    return (
      <div>
        <input name="firstName" type="text" value = { this.state.firstName }  onChange = { this.handler }/> +   
        <input name="lastName" type="text" value = { this.state.lastName } onChange = { this.handler } /> =
        { this.state.firstName + this.state.lastName}   
        <br />
        <textarea name="note" value = { this.state.note } onChange = { this.handler }></textarea>
        { this.state.note }
        <br />
        <select name="hobby" value = { this.state.hobby } onChange = { this.handler }>
          <option value="篮球">篮球</option>
          <option value="排球">排球</option>
          <option value="网球">网球</option>
          <option value="足球">足球</option>
          <option value="羽毛球">羽毛球</option>
        </select>{this.state.hobby}

        <br />
        <input name="sex" type="radio" onChange = { this.changeSex } value="男"/>男
        <input name="sex" type="radio" onChange = { this.changeSex } value="女"/>女
        ---- { this.state.sex }

        <br />

        <input name="course" onChange = { this.changeCourse } type="checkbox" value="vue"/>vue
        <input name="course" onChange = { this.changeCourse } type="checkbox" value="react"/>react
        <input name="course" onChange = { this.changeCourse } type="checkbox" value="小程序"/>小程序
        <input name="course" onChange = { this.changeCourse } type="checkbox" value="jQuery"/>jQuery
         -- {
           this.state.course.map(item => {
             return (
              <div key = { item }>{ item }</div>
             )
           })
         }
      </div>
    );
  }
}

```





## 4、处理多个输入

当需要处理多个 `input` 元素时，我们可以给每个元素添加 `name` 属性，并让处理函数根据 `event.target.name` 的值选择要执行的操作。

```javascript
import React, { Component } from 'react';

// 受控组件 --- value 配合 onChange 且value的值为state中的值
export default class App extends Component {
  state = {
    firstName: '1',
    lastName: '2'
  }

  handler = (event) => {
    console.log(event.target.name)
    this.setState({
      [event.target.name]: event.target.value
    })
  }
  render() {
    return (
      <div>
        <input name="firstName" type="text" value = { this.state.firstName }  onChange = { this.handler }/> +   
        <input name="lastName" type="text" value = { this.state.lastName } onChange = { this.handler } /> =
        { this.state.firstName + this.state.lastName}   
      </div>
    );
  }
}


```



## 5、文件 input 标签

在 HTML 中，`<input type="file">` 允许用户从存储设备中选择一个或多个文件，将其上传到服务器，或通过使用 JavaScript 的 File API 进行控制。

```html
<input type="file" />
```
因为它的 value 只读，所以它是 React 中的一个非受控组件。将与其他非受控组件在后续文档中一起讨论。



## 6、受控输入空值

在受控组件上指定 value 的 prop 会阻止用户更改输入。如果你指定了 value，但输入仍可编辑，则可能是你意外地将value 设置为 undefined 或 null。

下面的代码演示了这一点。（输入最初被锁定，但在短时间延迟后变为可编辑。）

```javascript
ReactDOM.render(<input value="hi" />, mountNode);

setTimeout(function() {
  ReactDOM.render(<input value={null} />, mountNode);
}, 1000);
```



## 7、非受控组件

在大多数情况下，我们推荐使用 [受控组件](https://zh-hans.reactjs.org/docs/forms.html#controlled-components) 来处理表单数据。在一个受控组件中，表单数据是由 React 组件来管理的。另一种替代方案是使用非受控组件，这时表单数据将交由 DOM 节点来处理。

要编写一个非受控组件，而不是为每个状态更新都编写数据处理函数，你可以 [使用 ref](https://zh-hans.reactjs.org/docs/refs-and-the-dom.html) 来从 DOM 节点中获取表单数据。

例如，下面的代码使用非受控组件接受一个表单的值：

```jsx
import React, { Component } from 'react';

// 受控组件 --- value 配合 onChange 且value的值为state中的值
export default class App extends Component {
  // ref的字符串类型被废弃，在严格模式下是报出警告
  render() {
    return (
      <div>
        <input ref="firstName" type="text" onChange = { this.firstHandler }/>   
        <input ref="lastName" type="text" onChange = { this.LastHandler } /> 
        <button onClick = { () => {
          console.log('firstName', this.refs.firstName.value)
          console.log('lastName', this.refs.lastName.value)
        }}>获取数据</button>
      </div>
    );
  }
}

```



```javascript
import React, { Component } from 'react';

// 受控组件 --- value 配合 onChange 且value的值为state中的值
export default class App extends Component {
  firstNameRef = React.createRef() 
  lastNameRef = React.createRef() 
  // ref的字符串类型被废弃，在严格模式下是报出警告
  render() {
    return (
      <div>
        <input ref={ this.firstNameRef } type="text" onChange = { this.firstHandler }/>   
        <input ref={ this.lastNameRef } type="text" onChange = { this.LastHandler } /> 
        <button onClick = { () => {
          console.log('FirstName', this.firstNameRef.current.value)
          console.log('lastName', this.lastNameRef.current.value)
        }}>获取数据</button>
      </div>
    );
  }
}

```

因为非受控组件将真实数据储存在 DOM 节点中，所以在使用非受控组件时，有时候反而更容易同时集成 React 和非 React 代码。如果你不介意代码美观性，并且希望快速编写代码，使用非受控组件往往可以减少你的代码量。否则，你应该使用受控组件。

### (1) 默认值

在 React 渲染生命周期时，表单元素上的 `value` 将会覆盖 DOM 节点中的值，在非受控组件中，你经常希望 React 能赋予组件一个初始值，但是不去控制后续的更新。 在这种情况下, 你可以指定一个 `defaultValue` 属性，而不是 `value`。

```javascript
render() {
  return (
    <form onSubmit={this.handleSubmit}>
      <label>
        Name:
        <input
          defaultValue="Bob"
          type="text"
          ref={this.input} />
      </label>
      <input type="submit" value="Submit" />
    </form>
  );
}
```
同样，`<input type="checkbox">` 和 `<input type="radio">` 支持 `defaultChecked`，`<select>` 和 `<textarea>` 支持 `defaultValue`。



### (2) 文件输入

在 HTML 中，`<input type="file">` 可以让用户选择一个或多个文件上传到服务器，或者通过使用 [File API](https://developer.mozilla.org/en-US/docs/Web/API/File/Using_files_from_web_applications) 进行操作。

在 React 中，`<input type="file">` 始终是一个非受控组件，因为它的值只能由用户设置，而不能通过代码控制。

您应该使用 File API 与文件进行交互。下面的例子显示了如何创建一个 [DOM 节点的 ref](https://zh-hans.reactjs.org/docs/refs-and-the-dom.html) 从而在提交表单时获取文件的信息。

```javascript
import React, { Component } from 'react';

// 受控组件 --- value 配合 onChange 且value的值为state中的值
export default class App extends Component {
  fileRef = React.createRef() 
  imgRef = React.createRef()
  // ref的字符串类型被废弃，在严格模式下是报出警告

  handler = () => {
    console.log(1111)
    const files = this.fileRef.current.files
    const reader = new FileReader()
    reader.readAsDataURL(files[0])
    var that = this
    reader.onload = function () {
      console.log(this.result)
      that.imgRef.current.src = this.result
    }
  }
  render() {
    return (
      <div>
        <input ref = { this.fileRef } type="file" onChange= {this.handler}/>   
        <img ref={ this.imgRef } src="" alt=""/>
        <button onClick = { this.handler }>获取数据</button>
      </div>
    );
  }
}

```



# 十、TodoList -作业



# 十一、组件的生命周期

React中组件也有生命周期，也就是说也有很多钩子函数供我们使用, 组件的生命周期，我们会分为四个阶段，初始化、运行中、销毁、错误处理(16.3之后)

https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/

## 1、初始化

在组件初始化阶段会执行 
1. constructor 

2. static getDerivedStateFromProps()

3. componentWillMount() / UNSAFE_componentWillMount() 

4. render() 

5. componentDidMount()

   

## 2、更新阶段

`props`或`state`的改变可能会引起组件的更新，组件重新渲染的过程中会调用以下方法： 

1. componentWillReceiveProps() / UNSAFE_componentWillReceiveProps() 
2. static getDerivedStateFromProps()
3. shouldComponentUpdate() 
4. componentWillUpdate() / UNSAFE_componentWillUpdate() 
5. render() 
6. getSnapshotBeforeUpdate() 
7. componentDidUpdate()



## 3、卸载阶段

1. componentWillUnmount()



## 4、错误处理

1. componentDidCatch()



## 5、各生命周期详解

**(1) constructor(props)**

React组件的构造函数在挂载之前被调用。在实现`React.Component`构造函数时，需要先在添加其他内容前，调用`super(props)`，用来将父组件传来的`props`绑定到这个类中，使用`this.props`将会得到。

官方建议不要在`constructor`引入任何具有副作用和订阅功能的代码，这些应当使用`componentDidMount()`。

`constructor`中应当做些初始化的动作，如：初始化`state`，将事件处理函数绑定到类实例上，但也不要使用`setState()`。如果没有必要初始化state或绑定方法，则不需要构造`constructor`，或者把这个组件换成纯函数写法。

当然也可以利用`props`初始化`state`，在之后修改`state`不会对`props`造成任何修改，但仍然建议大家提升状态到父组件中，或使用`redux`统一进行状态管理。

```jsx
constructor(props) {
  super(props);
  this.state = {
    isLiked: props.isLiked
  };
}
```

**(2) static getDerivedStateFromProps(nextProps, prevState)**

React 的 16.3 版本中对生命周期进行了较大的调整，这是为了开发者能正确地使用生命周期，避免误解其概念而造成反模式。

本节将重点介绍 getDerivedStateFromProps 这个生命周期。要注意的是，React 16.3 的版本中 getDerivedStateFromProps 的触发范围是和 16.4^ 是不同的，主要区别是在 `setState` 和 `forceUpdate` 时会不会触发，具体可以看这个[生命全周期图](http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/) 。

可能的使用场景有两个：

- 无条件的根据 prop 来更新内部 state，也就是只要有传入 prop 值， 就更新 state
- 只有 prop 值和 state 值不同时才更新 state 值。

我们接下来看几个例子。

假设我们有个一个表格组件，它会根据传入的列表数据来更新视图。

```jsx
class Table extends React.Component {
    state = {
        list: []
    }
    static getDerivedStateFromProps (props, state) {
        return {
            list: props.list
        }
    }
    render () {
        .... // 展示 list
    }
}
```

上面的例子就是第一种使用场景，但是无条件从 prop 中更新 state，我们完全没必要使用这个生命周期，直接对 prop 值进行操作就好了，无需用 state 值类保存。



再看一个例子，这个例子是一个颜色选择器，这个组件能选择相应的颜色并显示，同时它能根据传入 prop 值显示颜色。

```jsx
Class ColorPicker extends React.Component {
    state = {
        color: '#000000'
    }
    static getDerivedStateFromProps (props, state) {
        if (props.color !== state.color) {
            return {
                color: props.color
            }
        }
        return null
    }
    ... // 选择颜色方法
    render () {
        .... // 显示颜色和选择颜色操作
    }
}
```

现在我们可以这个颜色选择器来选择颜色，同时我们能传入一个颜色值并显示。但是这个组件有一个 bug，如果我们传入一个颜色值后，再使用组件内部的选择颜色方法，我们会发现颜色不会变化，一直是传入的颜色值。

这是使用这个生命周期的一个常见 bug。为什么会发生这个 bug 呢？在开头有说到，在 React 16.4^ 的版本中 `setState` 和 `forceUpdate` 也会触发这个生命周期，所以内部 state 变化后，又会走 getDerivedStateFromProps 方法，并把 state 值更新为传入的 prop。

接下里我们来修复这个bug。

```jsx
Class ColorPicker extends React.Component {
    state = {
        color: '#000000',
        prevPropColor: ''
    }
    static getDerivedStateFromProps (props, state) {
        if (props.color !== state.prevPropColor) {
            return {
                color: props.color
                prevPropColor: props.color
            }
        }
        return null
    }
    ... // 选择颜色方法
    render () {
        .... // 显示颜色和选择颜色操作
    }
}
```

通过保存一个之前 prop 值，我们就可以在只有 prop 变化时才去修改 state。这样就解决上述的问题。

这里小结下 getDerivedStateFromProps 方法使用的注意点：

- 在使用此生命周期时，要注意把传入的 prop 值和之前传入的 prop 进行比较。
- 因为这个生命周期是静态方法，同时要保持它是纯函数，不要产生副作用。

我们应该谨慎地使用 getDerivedStateFromProps 这个生命周期。使用时要注意下面几点：

- 因为这个生命周期是静态方法，同时要保持它是纯函数，不要产生副作用。
- 在使用此生命周期时，要注意把传入的 prop 值和之前传入的 prop 进行比较（这个 prop 值最好有唯一性，或者使用一个唯一性的 prop 值来专门比较）。
- 不使用 getDerivedStateFromProps，可以改成组件保持完全不可控模式，通过初始值和 key 值来实现 prop 改变 state 的情景。

**(3) componentWillMount() / UNSAFE_componentWillMount()**

`componentWillMount()`将在React未来版本(官方说法 17.0)中被弃用。`UNSAFE_componentWillMount()`在组件挂载前被调用，在这个方法中调用`setState()`不会起作用，是由于他在`render()`前被调用。

为了避免副作用和其他的订阅，官方都建议使用`componentDidMount()`代替。这个方法是用于在服务器渲染上的唯一方法。这个方法因为是在渲染之前被调用，也是惟一一个可以直接同步修改state的地方。

**(4) render()**

render()方法是必需的。当他被调用时，他将计算`this.props`和`this.state`，并返回以下一种类型： 
- React元素。通过jsx创建，既可以是dom元素，也可以是用户自定义的组件。 
- 字符串或数字。他们将会以文本节点形式渲染到dom中。 
- Portals。react 16版本中提出的新的解决方案，可以使组件脱离父组件层级直接挂载在DOM树的任何位置。 
- null，什么也不渲染 
- 布尔值。也是什么都不渲染。

当返回`null`,`false`,`ReactDOM.findDOMNode(this)`将会返回null，什么都不会渲染。

`render()`方法必须是一个纯函数，他不应该改变`state`，也不能直接和浏览器进行交互，应该将事件放在其他生命周期函数中。 
如果`shouldComponentUpdate()`返回`false`，`render()`不会被调用。

**(5) componentDidMount**

`componentDidMount`在组件被装配后立即调用。初始化使得DOM节点应该进行到这里。

**通常在这里进行ajax请求**

如果要初始化第三方的dom库，也在这里进行初始化。只有到这里才能获取到真实的dom.

**(6) componentWillReceiveProps()/UNSAFE_componentWillReceiveProps(nextProps)**

官方建议使用`getDerivedStateFromProps`函数代替`componentWillReceiveProps`。当组件挂载后，接收到新的`props`后会被调用。如果需要更新`state`来响应`props`的更改，则可以进行`this.props`和`nextProps`的比较，并在此方法中使用`this.setState()`。

如果父组件会让这个组件重新渲染，即使`props`没有改变，也会调用这个方法。

React不会在组件初始化props时调用这个方法。调用`this.setState`也不会触发。

**(7) shouldComponentUpdate(nextProps, nextState)**

调用`shouldComponentUpdate`使React知道，组件的输出是否受`state`和`props`的影响。默认每个状态的更改都会重新渲染，大多数情况下应该保持这个默认行为。

在渲染新的`props`或`state`前，`shouldComponentUpdate`会被调用。默认为`true`。这个方法不会在初始化时被调用，也不会在`forceUpdate()`时被调用。返回`false`不会阻止子组件在`state`更改时重新渲染。

如果`shouldComponentUpdate()`返回`false`，`componentWillUpdate`,`render`和`componentDidUpdate`不会被调用。

> 官方并不建议在`shouldComponentUpdate()`中进行深度查询或使用`JSON.stringify()`，他效率非常低，并且损伤性能。

**(8) UNSAFE_componentWillUpdate(nextProps, nextState)**

在渲染新的`state`或`props`时，`UNSAFE_componentWillUpdate`会被调用，将此作为在更新发生之前进行准备的机会。这个方法不会在初始化时被调用。

*不能在这里使用this.setState()*，也不能做会触发视图更新的操作。如果需要更新`state`或`props`，调用`getDerivedStateFromProps`。

**(9) getSnapshotBeforeUpdate()**

在react `render()`后的输出被渲染到DOM之前被调用。它使您的组件能够在它们被潜在更改之前捕获当前值（如滚动位置）。这个生命周期返回的任何值都将作为参数传递给componentDidUpdate（）。

**(10) componentDidUpdate(prevProps, prevState, snapshot)**

在更新发生后立即调用`componentDidUpdate()`。此方法不用于初始渲染。当组件更新时，将此作为一个机会来操作DOM。只要您将当前的props与以前的props进行比较（例如，如果props没有改变，则可能不需要网络请求），这也是做网络请求的好地方。

如果组件实现`getSnapshotBeforeUpdate()`生命周期，则它返回的值将作为第三个“快照”参数传递给`componentDidUpdate()`。否则，这个参数是`undefined`。

**(11) componentWillUnmount()**

在组件被卸载并销毁之前立即被调用。在此方法中执行任何必要的清理，例如使定时器无效，取消网络请求或清理在`componentDidMount`中创建的任何监听。

**(12) componentDidCatch(error, info)**

错误边界是React组件，可以在其子组件树中的任何位置捕获JavaScript错误，记录这些错误并显示回退UI，而不是崩溃的组件树。错误边界在渲染期间，生命周期方法以及整个树下的构造函数中捕获错误。

如果类组件定义了此生命周期方法，则它将成错误边界。在它中调用`setState()`可以让你在下面的树中捕获未处理的JavaScript错误，并显示一个后备UI。只能使用错误边界从意外异常中恢复; 不要试图将它们用于控制流程。

错误边界只会捕获树中下面组件中的错误。错误边界本身不能捕获错误。



## 6、PureComponent

`PureComponnet`里如果接收到的新属性或者是更改后的状态和原属性、原状态相同的话，就不会去重新render了
在里面也可以使用`shouldComponentUpdate`，而且。是否重新渲染以`shouldComponentUpdate`的返回值为最终的决定因素

`React.PureComponent` 与 [`React.Component`](https://zh-hans.reactjs.org/docs/react-api.html#reactcomponent) 很相似。两者的区别在于 [`React.Component`](https://zh-hans.reactjs.org/docs/react-api.html#reactcomponent) 并未实现 [`shouldComponentUpdate()`](https://zh-hans.reactjs.org/docs/react-component.html#shouldcomponentupdate)，而 `React.PureComponent` 中以浅层对比 prop 和 state 的方式来实现了该函数。

```jsx
import React, { Component, PureComponent } from 'react';
// class Child extends Component {
  
//   render() {
//     console.log('1') // 点击按钮持续打印
//     return (
//       <div> textInComponent </div>
//     );
//   }
// }
// class Child extends Component {
//   shouldComponentUpdate () {
//     return false
//   }
//   render() {
//     console.log('1') // 只打印1此
//     return (
//       <div> textInComponent </div>
//     );
//   }
// }
// class Child extends PureComponent {
//   render() {
//     console.log('1') // 只打印1次
//     return (
//       <div> textInComponent </div>
//     );
//   }
// }
class Child extends PureComponent {
  shouldComponentUpdate () {
    return true
  }
  render() {
    console.log('1') // 持续打印报警告
    return (
      <div> textInComponent </div>
    );
  }
}

export default class App extends Component {
  state = {
    count: 100
  }
  render() {
    return (
      <div> 
        <button onClick = { () => {
          this.setState({ count: this.state.count + 1 })
        }}>add</button>{ this.state.count }
        <Child />
      </div>
    );
  }
}

```



##  7、ref

React提供的这个`ref`属性，表示为对组件真正实例的引用，其实就是`ReactDOM.render()`返回的组件实例,`ref`可以挂载到组件上也可以是dom元素上。

- 挂到组件(`class`声明的组件)上的ref表示对组件实例的引用。**不能**在函数式组件上使用 ref 属性，因为它们没有实例：
- 挂载到dom元素上时表示具体的dom元素节点。

在React 最新的版本中，要使用`ref`, 需要使用`React.createRef`方法先生成一个`ref`。

```jsx
import React, { Component, useState } from 'react';

// vue ref 可以用到组件也可以用到标签
// react中同样成立，但是注意函数式组件是没有实例的（如果子组件是函数式组件，不要轻易使用ref）
// 如果非要使用ref，也不是不可以，可以通过  React.forwardRef() 包裹函数式组件，该函数式组件的第一个参数为props，第二个参数为ref，在函数式组件的顶级标签上添加ref --- 转发refs
const Header = React.forwardRef((props, ref) => {
  const [count, setCount] = useState(10000)
  const fn = () => {
    setCount(count + 500)
  }
  return (
    <header ref = {ref}>header - { count }</header>
  )
})
class Content extends Component {
  state = {
    msg: 'hello msg'
  }
  render () {
    return (
      <div className="content">content</div>
    )
  }
} 
// 非严格模式，ref可以设置为字符串
// 严格模式下，需要使用 createRef / useRef
export default class App extends Component {
  btnRef = React.createRef()
  headerRef = React.createRef() 
  contentRef = React.createRef()
  render() {
    return (
      <>
        <button ref = { this.btnRef }>按钮</button>
        <Header ref = { this.headerRef }/>
        <Content ref = { this.contentRef }/>

        <button onClick = {() => {
          // console.log(this.btnRef.current)
          console.log(this.headerRef.current)
          console.log(this.contentRef.current.state.msg)
          this.btnRef.current.style.color = 'red'
          this.headerRef.current.style.color = 'green'

        }}>获取子组件以及dom节点</button>
      </>
    );
  }
}

```



# 十二、组件通信

**父组件与子组件通信**

```js
// 父组件调用子组件的地方，添加自定义的属性
// 属性的值就是需要传递给子组件的值
// 如果属性的值是变量，boolean类型，number类型，对象，数组，函数，null，undefined
// 需要使用 {} 包裹

// 如果需要设置子组件的属性的默认值
//  不管是类组件还是函数式组件，可以在组件定义之后通过 Com.defaultProps 设置属性的默认值
//  但是如果子组件是类组件，还可以通过组件的静态的属性 static defaultProps 设置属性的默认值
// 如果需要验证子组件的属性的数据类型，需要借助于prop-types模块
//  通过 Com.propTypes = { key: PropTypes.数据类型 }
// 如果需要给该子组件的属性设定 多个数据类型时，
//  通过 Com.propTypes = { key: PropTypes.oneOfType([ PropTypes.类型1， PropTypes.类型2 ]) }
// 如果子组件的属性是必须传递的，可以添加 isRequired,如果设置了默认值，可以不传递该属性
// 如果需要给该属性编写校验函数，可以通过 函数实现，实现时 返回值必须为null 或者是 new Error其中的一种
// ？如果急需要校验你的数据类型，还需要自定义验证函数时，如何处理？
```



**子组件与父组件通信**

```jsx
import React, { Component } from 'react';
// 子传父其实也是父传子
// 在父组件调用子组件的地方，添加一个自定义的属性 fn，
// 该属性的值是一个函数，该函数由父组件设定，该函数的默认值就是子组件需要传递给父组件的值

// 在子组件的某一个事件内部，通过this.props.自定义属性(参数)完成值的传递
class Child extends Component {
  state = {
    msg: 'child'
  }
  render() {
    return (
      <div>
        <button onClick = { () => {
          this.props.fn('100000')
        }}>传递数据给父组件</button>  
      </div>
    );
  }
}

export default class App extends Component {
  getData = (val) => {
    console.log(val)
  }
  render() {
    return (
      <div>
        <Child fn = { this.getData }/>
      </div>
    );
  }
}

```



**跨组件通信**

在react没有类似vue中的事件总线来解决这个问题，我们只能借助它们共同的父级组件来实现，将非父子关系装换成多维度的父子关系。react提供了`context` api来实现跨组件通信, React 16.3之后的`context`api较之前的好用。

实例

```jsx
import React, { Component } from 'react'
class Third extends Component {
  render() {
    return (
      <div>
        { this.props.theme }
      </div>
    )
  }
}
class Second extends Component {
  render() {
    return (
      <div>
        <Third theme = { this.props.theme }/>
      </div>
    )
  }
}
export default class App extends Component {
  state = {
    theme: 'dark'
  }
  render() {
    return (
      <div>
        <Second theme = { this.state.theme }/>
      </div>
    )
  }
}

```

```jsx
import React, { Component } from 'react'
const ThemeContext = React.createContext()

// 创建上下文对象（ThemeContext）
// 祖先组件通过 上下文对象的 Provider 配合 value属性传值
// 如果后代组件是类组件，可以使用类的静态属性的特性（2中写法）赋值为上下文对象
//  后代组件就可以通过 this.context 获取到祖先组件的数据
// 如果后代组件是函数式组件

class Third extends Component {
  static contextType = ThemeContext
  render() {
    return (
      <div>
        theme: { this.context }
      </div>
    )
  }
}
// Third.contextType = ThemeContext

class Second extends Component {
  render() {
    return (
      <div>
        <Third/>
      </div>
    )
  }
}
export default class App extends Component {
  state = {
    theme: 'dark'
  }
  render() {
    return (
      <ThemeContext.Provider value = { this.state.theme }>
        <div>
          <Second theme = { this.state.theme }/>
        </div>
      </ThemeContext.Provider>
    )
  }
}

```

```jsx
import React, { Component } from 'react'
const ThemeContext = React.createContext()

// 创建上下文对象（ThemeContext）
// 祖先组件通过 上下文对象的 Provider 配合 value属性传值
// 如果后代组件是类组件，可以使用类的静态属性的特性（2中写法）赋值为上下文对象
//  后代组件就可以通过 this.context 获取到祖先组件的数据
// 如果后代组件是函数式组件,可以通过上下对象提供的Consumer传值，
//   内部写为箭头函数，返回jsx代码，箭头函数的参数为祖先组件传递过来的值
// ？类组件能不能使用Consumer传值 ---- 必然可以

// ？如果项目中有多个 上下文传值呢？
const Third = () => {
  return (
    <>
      theme为： 
      <ThemeContext.Consumer>
        {
          (val) => {
            return (
              <span>{ val}</span>
            )
          }
        }
      </ThemeContext.Consumer>
    </>
  )
}


class Second extends Component {
  render() {
    return (
      <div>
        <Third/>
      </div>
    )
  }
}
export default class App extends Component {
  state = {
    theme: 'dark'
  }
  render() {
    return (
      <ThemeContext.Provider value = { this.state.theme }>
        <div>
          <Second theme = { this.state.theme }/>
        </div>
      </ThemeContext.Provider>
    )
  }
}

```

```jsx
import React, { Component } from 'react'
const ThemeContext = React.createContext()
const ColorContext = React.createContext()

// 创建上下文对象（ThemeContext）
// 祖先组件通过 上下文对象的 Provider 配合 value属性传值
// 如果后代组件是类组件，可以使用类的静态属性的特性（2中写法）赋值为上下文对象
//  后代组件就可以通过 this.context 获取到祖先组件的数据
// 如果后代组件是函数式组件,可以通过上下对象提供的Consumer传值，
//   内部写为箭头函数，返回jsx代码，箭头函数的参数为祖先组件传递过来的值
// ？类组件能不能使用Consumer传值 ---- 必然可以

// ？如果项目中有多个 上下文传值呢？
//  可以使用 上下文的Consumer传值，但是要注意其 嵌套问题

// 查看开发者工具

// class Third extends Component {
//   // 后来者居上
//   static contextType = ColorContext
//   static contextType = ThemeContext
//   render() {
//     return (
//       <div>
//         theme: { this.context }
//       </div>
//     )
//   }
// }
const Third = () => {
  return (
    <>
       {/* theme为： 
     <ThemeContext.Consumer>
        {
          (val) => {
            return (
              <span>{ val}</span>
            )
          }
        }
      </ThemeContext.Consumer>
      颜色为：
      <ColorContext.Consumer>
        {
          (val) => {
            return (
              <span>{ val}</span>
            )
          }
        }
      </ColorContext.Consumer> */}

      <ThemeContext.Consumer>
        {
          (themeVal) => {
            return (
              <div>
                <ColorContext.Consumer>
                  {
                    (colorVal) => {
                      return (
                        <div>
                          theme: { themeVal }
                          <hr />
                          color: { colorVal }
                        </div>
                      )
                    }
                  }
                </ColorContext.Consumer>
              </div>
            )
          }
        }
      </ThemeContext.Consumer>
    </>
  )
}


class Second extends Component {
  render() {
    return (
      <div>
        <Third/>
      </div>
    )
  }
}
export default class App extends Component {
  state = {
    theme: 'dark',
    color: '#f66'
  }
  render() {
    return (
      <ThemeContext.Provider value = { this.state.theme }>
        <ColorContext.Provider value = { this.state.color }>
          <div>
            <button onClick = {
              () => {
                this.setState({
                  theme: this.state.theme === 'dark' ? 'light': 'dark'
                })
              }
            }>切换主题</button>
            <input type="color" value={ this.state.color } onChange = { (e) => {
              this.setState({
                color: e.target.value
              })
            }}/>
            <Second theme = { this.state.theme }/>
          </div>
        </ColorContext.Provider>
      </ThemeContext.Provider>
    )
  }
}

```

```jsx
import React, { Component } from 'react'
const ThemeContext = React.createContext()
const ColorContext = React.createContext()
ThemeContext.displayName = 'ThemeContext'
ColorContext.displayName = 'ColorContext'

// 创建上下文对象（ThemeContext）
// 祖先组件通过 上下文对象的 Provider 配合 value属性传值
// 如果后代组件是类组件，可以使用类的静态属性的特性（2中写法）赋值为上下文对象
//  后代组件就可以通过 this.context 获取到祖先组件的数据
// 如果后代组件是函数式组件,可以通过上下对象提供的Consumer传值，
//   内部写为箭头函数，返回jsx代码，箭头函数的参数为祖先组件传递过来的值
// ？类组件能不能使用Consumer传值 ---- 必然可以

// ？如果项目中有多个 上下文传值呢？
//  可以使用 上下文的Consumer传值，但是要注意其 嵌套问题

// 查看开发者工具，如果存在多个Context使用，我们从开发者工具中不能够直接区分
// 使用上下文对象 的 displayName 属性可以做到

// class Third extends Component {
//   // 后来者居上
//   static contextType = ColorContext
//   static contextType = ThemeContext
//   render() {
//     return (
//       <div>
//         theme: { this.context }
//       </div>
//     )
//   }
// }
const Third = () => {
  return (
    <>
       {/* theme为： 
     <ThemeContext.Consumer>
        {
          (val) => {
            return (
              <span>{ val}</span>
            )
          }
        }
      </ThemeContext.Consumer>
      颜色为：
      <ColorContext.Consumer>
        {
          (val) => {
            return (
              <span>{ val}</span>
            )
          }
        }
      </ColorContext.Consumer> */}

      <ThemeContext.Consumer>
        {
          (themeVal) => {
            return (
              <div>
                <ColorContext.Consumer>
                  {
                    (colorVal) => {
                      return (
                        <div>
                          theme: { themeVal }
                          <hr />
                          color: { colorVal }
                        </div>
                      )
                    }
                  }
                </ColorContext.Consumer>
              </div>
            )
          }
        }
      </ThemeContext.Consumer>
    </>
  )
}


class Second extends Component {
  render() {
    return (
      <div>
        <Third/>
      </div>
    )
  }
}
export default class App extends Component {
  state = {
    theme: 'dark',
    color: '#f66'
  }
  render() {
    return (
      <ThemeContext.Provider value = { this.state.theme }>
        <ColorContext.Provider value = { this.state.color }>
          <div>
            <button onClick = {
              () => {
                this.setState({
                  theme: this.state.theme === 'dark' ? 'light': 'dark'
                })
              }
            }>切换主题</button>
            <input type="color" value={ this.state.color } onChange = { (e) => {
              this.setState({
                color: e.target.value
              })
            }}/>
            <Second theme = { this.state.theme }/>
          </div>
        </ColorContext.Provider>
      </ThemeContext.Provider>
    )
  }
}

```



> 复杂的非父子组件通信在react中很难处理，多组件间的数据共享也不好处理，在实际的工作中我们会使用flux、redux、mobx来实现



# 十三、HOC(高阶组件)

Higher-Order Components就是一个函数，传给它一个组件，它返回一个新的组件。

```jsx
const NewComponent = higherOrderComponent(YourComponent)
```

比如，我们想要我们的组件通过自动注入一个版权信息。

```jsx
import React, { Component } from 'react';
class ZhangSan extends Component {
  state = {
    userName: ''
  }
  changeUserName = (e) => {
    this.setState({
      userName: e.target.value
    })
  }
  render() {
    
    return (
      <div> 
        <input type="text" value={ this.state.userName } onChange = { this.changeUserName }/>
        我是张三 { this.state.userName }
      </div>
    );
  }
}
class LiSi extends Component {
  state = {
    userName: ''
  }
  changeUserName = (e) => {
    this.setState({
      userName: e.target.value
    })
  }
  render() {
    
    return (
      <div> 
        <input type="text" value={ this.state.userName } onChange = { this.changeUserName }/>
        我是李四 { this.state.userName }
      </div>
    );
  }
}
export default class App extends Component {
  render() {
    return (
      <div> 
        <ZhangSan />
        <LiSi />  
      </div>
    );
  }
}

```

```jsx
import React, { Component } from 'react';
// HOC本质上是一个函数，参数为旧组件，返回一个新的组件

// 一般在组件暴露的使用调用高阶组件包裹原来的组件
const withUserName = (OldCom) => {
  return class extends Component {
    state = {
      userName: ''
    }
    changeUserName = (e) => {
      this.setState({
        userName: e.target.value
      })
    }
    render () {
      return (
        <div> 
          <input type="text" value={ this.state.userName } onChange = { this.changeUserName }/>
          <OldCom userName={ this.state.userName }/> 
        </div>
      )
    }
  }
}

class ZhangSan extends Component {
  
  render() {
    
    return (
      <div> 
        我是张三 { this.props.userName }
      </div>
    );
  }
}
ZhangSan = withUserName(ZhangSan)
// export default ZhangSan


// class LiSi extends Component {
//   render() {
    
//     return (
//       <div> 
//         我是李四 { this.props.userName }
//       </div>
//     );
//   }
// }
const LiSi = (props) => {
    return (
      <div> 
        我是李四 { props.userName }
      </div>
    );
}
const LiSiCom = withUserName(LiSi)
export default class App extends Component {
  render() {
    return (
      <div> 
        <ZhangSan />
        <LiSiCom />  
      </div>
    );
  }
}

```

> 调用高阶组件可以使用函数调用的方式 --- 普通
>
> 还可以使用 装饰器语法 ----- 单独讲解
>
> 在类组件中，可以直接使用装饰器，函数式组件只能使用函数调用的方式



# 十四、Portal

Portals 提供了一个最好的在父组件包含的DOM结构层级外的DOM节点渲染组件的方法。

```javascript
ReactDOM.createPortal(child,container);
```

第一个参数child是可渲染的react子项，比如元素，字符串或者片段等。第二个参数container是一个DOM元素。

## 1、用法

普通的组件，子组件的元素将挂载到父组件的DOM节点中。

有时需要将元素渲染到DOM中的不同位置上去，这是就用到的portal的方法。

```javascript
cnpm i styled-components -S
```

```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import styled from 'styled-components'

const ModalDiv = styled.div`
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.4);
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
  z-index: 999;
`
class Modal extends React.Component {
  render() {
    return ReactDOM.createPortal(
      <ModalDiv>
        <button onClick = { () => {
          this.props.onClose()
        }}>关闭</button>
      </ModalDiv>,
      document.getElementsByTagName('body')[0]
    );
  }
}

export default class App extends React.Component {
  state = { show: false }

  handleClick() {
    // 当子元素里的按钮被点击时，
    // 这个将会被触发更新父元素的 state，
    // 即使这个按钮在 DOM 中不是直接关联的后代
    this.setState(state => ({
      show: true
    }));
  }
  closeHandler = () => {
    this.setState(state => ({
      show: false
    }));
  } 

  render() {
    return (
      <div >
        <button onClick={this.handleClick.bind(this)}>打开模态框</button>
        <div style={{ height: 3000}}>
          {/* ul>li*100{$$} */}
        </div>
        {
          this.state.show ? <Modal onClose = { this.closeHandler }></Modal> : null
        }
      </div>
    );
  }
}

```

一个典型的用法就是当父组件的dom元素有 `overflow:hidden`或者`z-inde`样式，而你又需要显示的子元素超出父元素的盒子。举例来说，如对话框，悬浮框，和小提示。

## 2、在protal中的事件冒泡

虽然通过portal渲染的元素在父组件的盒子之外，但是渲染的dom节点仍在React的元素树上，在那个dom元素上的点击事件仍然能在dom树中监听到。

# 十五、状态管理

##  1、传统MVC框架的缺陷

**什么是MVC？**

![](images/mvc-base.png)

`MVC`的全名是`Model View Controller`，是模型(model)－视图(view)－控制器(controller)的缩写，是一种软件设计典范。

`V`即View视图是指用户看到并与之交互的界面。

`M`即Model模型是管理数据 ，很多务逻辑都在模型中完成。在MVC的三个部件中，模型拥有最多的处理任务。

`C`即Controller控制器是指控制器接受用户的输入并调用模型和视图去完成用户的需求，控制器本身不输出任何东西和做任何处理。它只是接收请求并决定调用哪个模型构件去处理请求，然后再确定用哪个视图来显示返回的数据。

**MVC只是看起来很美**

MVC框架的数据流很理想，请求先到Controller, 由Controller调用Model中的数据交给View进行渲染，但是在实际的项目中，又是允许Model和View直接通信的。然后就出现了这样的结果：

![image-20190420012010718](./images/defect-of-mvc.png)

## 2、Flux

在2013年，Facebook让`React`亮相的同时推出了Flux框架，`React`的初衷实际上是用来替代`jQuery`的，`Flux`实际上就可以用来替代`Backbone.js`，`Ember.js`等一系列`MVC`架构的前端JS框架。

其实`Flux`在`React`里的应用就类似于`Vue`中的`Vuex`的作用，但是在`Vue`中，`Vue`是完整的`mvvm`框架，而`Vuex`只是一个全局的插件。

`React`只是一个MVC中的V(视图层)，只管页面中的渲染，一旦有数据管理的时候，`React`本身的能力就不足以支撑复杂组件结构的项目，在传统的`MVC`中，就需要用到Model和Controller。Facebook对于当时世面上的`MVC`框架并不满意，于是就有了`Flux`, 但`Flux`并不是一个`MVC`框架，他是一种新的思想。

![image-20190420012450223](./images/flux.png)

- View： 视图层
- ActionCreator（动作创造者）：视图层发出的消息（比如mouseClick）
- Dispatcher（派发器）：用来接收Actions、执行回调函数
- Store（数据层）：用来存放应用的状态，一旦发生变动，就提醒Views要更新页面

Flux的流程：

1. 组件获取到store中保存的数据挂载在自己的状态上
2. 用户产生了操作，调用actions的方法
3. actions接收到了用户的操作，进行一系列的逻辑代码、异步操作
4. 然后actions会创建出对应的action，action带有标识性的属性
5. actions调用dispatcher的dispatch方法将action传递给dispatcher
6. dispatcher接收到action并根据标识信息判断之后，调用store的更改数据的方法
7. store的方法被调用后，更改状态，并触发自己的某一个事件
8. store更改状态后事件被触发，该事件的处理程序会通知view去获取最新的数据



## 3、Redux

React 只是 DOM 的一个抽象层，并不是 Web 应用的完整解决方案。有两个方面，它没涉及。

- 代码结构 
- 组件之间的通信

2013年 Facebook 提出了 Flux 架构的思想，引发了很多的实现。2015年，Redux 出现，将 Flux 与[函数式编程](<https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/>)结合一起，很短时间内就成为了最热门的前端架构。

如果你不知道是否需要 Redux，那就是不需要它

只有遇到 React 实在解决不了的问题，你才需要 Redux

简单说，如果你的UI层非常简单，没有很多互动，Redux 就是不必要的，用了反而增加复杂性。

- 用户的使用方式非常简单
- 用户之间没有协作
- 不需要与服务器大量交互，也没有使用 WebSocket
- 视图层（View）只从单一来源获取数据

**需要使用Redux的项目:**

- 用户的使用方式复杂
- 不同身份的用户有不同的使用方式（比如普通用户和管理员）
- 多个用户之间可以协作
- 与服务器大量交互，或者使用了WebSocket
- View要从多个来源获取数据

**从组件层面考虑，什么样子的需要Redux：**

- 某个组件的状态，需要共享
- 某个状态需要在任何地方都可以拿到
- 一个组件需要改变全局状态
- 一个组件需要改变另一个组件的状态

**Redux的设计思想：**

1. Web 应用是一个状态机，视图与状态是一一对应的。
2. 所有的状态，保存在一个对象里面（唯一数据源）。

> 注意：flux、redux都不是必须和react搭配使用的，因为flux和redux是完整的架构，在学习react的时候，只是将react的组件作为redux中的视图层去使用了。

**Redux的使用的三大原则：**

- Single Source of Truth(唯一的数据源)
- State is read-only(状态是只读的)
- Changes are made with pure function(数据的改变必须通过纯函数完成)



### (1) 自己实现Redux

这个部分，不使用react，直接使用原生的html/js来写一个简易的的redux

**基本的状态管理及数据渲染：**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>自定义redux</title>
</head>
<body>
  <button onclick="dispatch({ type: 'reduce', payload: 10 })">-</button>
  <div id="count"></div>
  <button onclick="dispatch({ type: 'add', payload: 10 })">+</button>
</body>
<script>
  // 设置初始化状态
  const countState = {
    count: 10
  }
  // 定义一个方法用于渲染计数器的dom
  const renderCount = (state) => {
    const countDom = document.querySelector('#count')
    countDom.innerHTML = state.count
  }
  // 渲染默认数据
  renderCount(countState)

  // 分发
  function dispatch (action) {
    changeState(action) // 修改状态
    renderCount(countState) // 渲染数据
  }

  // 定义的纯函数，纯函数
  function changeState (action) {
    const { type, payload } = action
    switch (type) {
      case 'add': 
        countState.count += payload
        break
      case 'reduce':
        countState.count -= payload
        break
      default:
        countState
    }
  }

</script>
</html>
```

**创建createStore方法**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>自定义redux</title>
</head>
<body>
  <button onclick="dispatch({ type: 'reduce', payload: 15 })">-</button>
  <div id="count"></div>
  <button onclick="dispatch({ type: 'add', payload: 15 })">+</button>
</body>
<script>
  // 定义一个方法，用于集中管理state和dispatch
  const createStore = (state, changeState) => {
    // getState 获取状态
    const getState = () => {}

    // 定义一个监听器，用于管理一些方法
    const listeners = []
    const subscribe = (listener) => listeners.push(listener)
  
    // 定义一个dispatch方法，让每次有action传入的时候返回render执行之后的结果
    const dispatch = (action) => {
      // 调用changeState来处理数据
      changeState(state, action)
      // 让监听器里的所以方法运行
      listeners.forEach(listener => listener())
    }

    return {
      getState,
      dispatch,
      subscribe
    }
  }
  // 设置初始化状态
  const countState = {
    count: 10
  }
  // 定义一个方法用于渲染计数器的dom
  const renderCount = (state) => {
    const countDom = document.querySelector('#count')
    countDom.innerHTML = state.count
  }
  // 渲染默认数据
  renderCount(countState)

  // 分发
  function dispatch (action) {
    changeState(action) // 修改状态
    renderCount(countState) // 渲染数据
  }

  // 定义的纯函数，纯函数
  function changeState (action) {
    const { type, payload } = action
    switch (type) {
      case 'add': 
        countState.count += payload
        break
      case 'reduce':
        countState.count -= payload
        break
      default:
        countState
    }
  }

  // 创建状态管理
  const store = createStore(countState, changeState)

  // 订阅数据变化
  store.subscribe(renderCount)
</script>
</html>
```



**(最终版)**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>自定义redux</title>
</head>
<body>
  <button onclick="reduce()">-</button>
  <div id="count"></div>
  <button onclick="add()">+</button>
</body>
<script>
  const countID = document.getElementById('count')
  countID.innerHTML = 10
  const countState = {
    count: 10
  }
  const reducer = (state, action) => {
    // 如果state的值是 null ,设置默认值
    if (!state) state = countState
    switch (action.type) {
      case 'REDUCE':
        // 为了返回的是全新的一个对象
        return { ...state, count: state.count - action.payload }
      case 'ADD':
        // 为了返回的是全新的一个对象
        return Object.assign({}, state, { count: state.count + action.payload })
      default:
        return state
    }
  }
  // 定义创建状态管理器方法
  function createStore (reducer) {
    let state = countState
    const getState = () => {
      return state
    }
    // 设置js中的监听器
    let listeners = []

    const dispatch = (action) => {
      state = reducer(state, action)
      // 状态已更新， 让监听器里的所有方法运行
      listeners.forEach(listener => listener())
    }

    const subscribe = (render) => {
      listeners.push(render)
    }

    return {
      getState,
      dispatch,
      subscribe
    }
  }

  // 创建store
  const store = createStore(reducer)

  function render () {
    countID.innerHTML = store.getState().count
  }
  render()

  store.subscribe(render)

  // console.log(store.getState())
  // 定义加减函数
  function reduce () {
    store.dispatch({
      type: 'REDUCE',
      payload: 10
    })
    // 手动更新
    // console.log(store.getState())
    // countID.innerHTML = store.getState().count
  }
  function add () {
    store.dispatch({
      type: 'ADD',
      payload: 10
    })
    // console.log(store.getState())
    // countID.innerHTML = store.getState().count
  }
  /**
   * cnpm i redux -S
   * // 定义状态管理器
   * import { createStore } from 'redux'
   * const reducer = (state= {
   *  count: 10
   * }, action) => {
   *  switch(action.type) {
   *    case 'REDUCE':
   *      return { ...state, count: state.count - action.payload }
   *    case 'ADD':
   *      return { ...state, count: state.count + action.payload }
   *    default: 
   *      return state
   *  }
   * } 
   * 
   * const store = createStore(reducer)
   * 
   * // 订阅更新
   * function render() {
   *  ReactDOM.render(<App />, document.getElementById('root'))
   * }
   * render()
   * store.subscribe(render)
   * 
   * // 使用状态管理器
   * store.getState() { count: 10 }
   * store.dispatch({
   *  type: 'ADD'
   *  payload: 10
   * })
   * */
</script>
</html>
```



### (2) 使用Redux框架

**Redux的流程：**

![image-20190420013410981](./images/redux.png)

- store通过reducer创建了初始状态

- view通过store.getState()获取到了store中保存的state挂载在了自己的状态上

- 用户产生了操作，调用了actions 的方法

- actions的方法被调用，创建了带有标示性信息的action

- actions将action通过调用store.dispatch方法发送到了reducer中

- reducer接收到action并根据标识信息判断之后返回了新的state

- store的state被reducer更改为新state的时候，store.subscribe方法里的回调函数会执行，此时就可以通知view去重新获取state

**Reducer必须是一个纯函数：**

Reducer 函数最重要的特征是，它是一个纯函数。也就是说，只要是同样的输入，必定得到同样的输出。Reducer不是只有Redux里才有，之前学的数组方法`reduce`, 它的第一个参数就是一个reducer

纯函数是函数式编程的概念，必须遵守以下一些约束。

- 不得改写参数

- 不能调用系统 I/O 的API

- 不能调用Date.now()或者Math.random()等不纯的方法，因为每次会得到不一样的结果

由于 Reducer 是纯函数，就可以保证同样的State，必定得到同样的 View。但也正因为这一点，Reducer 函数里面不能改变 State，必须返回一个全新的对象，请参考下面的写法。

```js
// State 是一个对象
function reducer(state = defaultState, action) {
  return Object.assign({}, state, { thingToChange });
  // 或者
  return { ...state, ...newState };
}

// State 是一个数组
function reducer(state = defaultState, action) {
  return [...state, newItem];
}
```

最好把 State 对象设成只读。要得到新的 State，唯一办法就是生成一个新对象。这样的好处是，任何时候，与某个 View 对应的 State 总是一个不变(immutable)的对象。

我们可以通过在createStore中传入第二个参数来设置默认的state，但是这种形式只适合于只有一个reducer的时候。

```
cnpm i redux -S
```

```js
// src/store/index.js 创建状态管理器
import { createStore } from 'redux'

// 唯一修改状态的方式就是通过纯函数
// 必须返回全新的状态
const reducer = (state = { // 初始值，只有第一次访问时才是 10
  count: 10
}, action) => {
  switch (action.type) {
    case 'REDUCE':
      return { ...state, count: state.count - action.payload }
    case 'ADD':
      return Object.assign({}, state, { count: state.count + action.payload })
    default:
      return state
  }
}

// 创建状态管理器
const store = createStore(reducer)

export default store
```

```js
// src/index.js 入口文件处订阅数据的变化
import React from 'react'
import ReactDOM from 'react-dom'
import store from './store'
import App from './App.jsx'

function render () {
  ReactDOM.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>,
    document.getElementById('root')
  )
}
render()

// 订阅
store.subscribe(render)
console.log(store)
```

```js
// src/App.jsx 组件中使用
import React, { Component } from 'react';
import store from './store'
export default class App extends Component {
  render() {
    return (
      <div>
        <button onClick = { () => {
          store.dispatch({
            type: 'REDUCE',
            payload: 5
          })
        }}>-</button>
        <span>{ store.getState().count }</span>
        <button onClick = { () => {
          store.dispatch({
            type: 'ADD',
            payload: 5
          })
        }}>+</button>
      </div>
    );
  }
}

```



**划分reducer**:

因为一个应用中只能有一个大的state，这样的话reducer中的代码将会特别特别的多，那么就可以使用combineReducers方法将已经分开的reducer合并到一起

> 注意：
>
> 1. 分离reducer的时候，每一个reducer维护的状态都应该不同
> 2. 通过store.getState获取到的数据也是会按照reducers去划分的
> 3. 划分多个reducer的时候，默认状态只能创建在reducer中，因为划分reducer的目的，就是为了让每一个reducer都去独立管理一部分状态

*最开始一般基于计数器的例子讲解redux的基本使用即可*。

关于action/reducer/store的更多概念，请查看[官网](<https://www.redux.org.cn/>)

````js
// src/store/modules/app.js
// 唯一修改状态的方式就是通过纯函数
// 必须返回全新的状态
const reducer = (state = { // 初始值，只有第一次访问时才是 10
  count: 10
}, action) => {
  switch (action.type) {
    case 'REDUCE':
      return { ...state, count: state.count - action.payload }
    case 'ADD':
      return Object.assign({}, state, { count: state.count + action.payload })
    default:
      return state
  }
}
export default reducer
````

```js
// src/store/modules/pro.js
const reducer = (state = {
  bannerList: [],
  proList: []
}, action) => {
  switch (action.type) {
    case 'CHANGE_BANNER_LIST':
      return { ...state, bannerList: action.payload }
    case 'CHANGE_PRO_LIST':
      return Object.assign({}, state, { proList: action.payload })
    default:
      return state
  }
}
export default reducer
```

```js
// src/store/index.js
import { createStore, combineReducers } from 'redux'
import app from './modules/app'
import pro from './modules/pro'
const reducer = combineReducers({ // 划分reducer，便于多人合作
  app,
  pro
})

// 创建状态管理器
const store = createStore(reducer)

export default store
```

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom'
import store from './store'
import App from './App.jsx'

function render () {
  ReactDOM.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>,
    document.getElementById('root')
  )
}
render()

// 订阅
store.subscribe(render)
console.log(store)
```

```js
// src/App.jsx 使用状态时注意我们模块
import React, { Component } from 'react';
import store from './store'
export default class App extends Component {
  render() {
    return (
      <div>
        <button onClick = { () => {
          store.dispatch({
            type: 'REDUCE',
            payload: 5
          })
        }}>-</button>
        {/* <span>{ store.getState().count }</span> */}
        <span>{ store.getState().app.count }</span>
        <button onClick = { () => {
          store.dispatch({
            type: 'ADD',
            payload: 5
          })
        }}>+</button>
      </div>
    );
  }
}

```





### (3) 容器组件（Smart/Container Components）和展示组件（Dumb/Presentational Components）

|                | 展示组件                   | 容器组件                           |
| -------------: | :------------------------- | :--------------------------------- |
|           作用 | 描述如何展现（骨架、样式） | 描述如何运行（数据获取、状态更新） |
| 直接使用 Redux | 否                         | 是                                 |
|       数据来源 | props                      | 监听 Redux state                   |
|       数据修改 | 从 props 调用回调函数      | 向 Redux 派发 actions              |
|       调用方式 | 手动                       | 通常由 React Redux 生成            |

### (4) 使用react-redux

可以先结合`context`来手动连接react和redux。

react-redux提供两个核心的api：

- Provider: 提供store
- connect: 用于连接容器组件和展示组件

  - Provider

     根据单一store原则 ，一般只会出现在整个应用程序的最顶层。

  - connect

     语法格式为

     `connect(mapStateToProps?, mapDispatchToProps?, mergeProps?, options?)(component)`

     一般来说只会用到前面两个，它的作用是：

     - 把`store.getState()`的状态转化为展示组件的`props`
     - 把`actionCreators`转化为展示组件`props`上的方法

>特别强调：
>
>官网上的第二个参数为mapDispatchToProps, 实际上就是actionCreators

只要上层中有`Provider`组件并且提供了`store`, 那么，子孙级别的任何组件，要想使用`store`里的状态，都可以通过`connect`方法进行连接。如果只是想连接`actionCreators`，可以第一个参数传递为`null`



```
cnpm i react-redux -S
```

```js
// src/store/modules/app.js
// 唯一修改状态的方式就是通过纯函数
// 必须返回全新的状态
const reducer = (state = { // 初始值，只有第一次访问时才是 10
  count: 10
}, action) => {
  switch (action.type) {
    case 'REDUCE':
      return { ...state, count: state.count - action.payload }
    case 'ADD':
      return Object.assign({}, state, { count: state.count + action.payload })
    default:
      return state
  }
}
export default reducer
```

```js
// src/store/modules/pro.js
const reducer = (state = {
  bannerList: [],
  proList: []
}, action) => {
  switch (action.type) {
    case 'CHANGE_BANNER_LIST':
      return { ...state, bannerList: action.payload }
    case 'CHANGE_PRO_LIST':
      return Object.assign({}, state, { proList: action.payload })
    default:
      return state
  }
}
export default reducer
```

```js
// src/store/index.js
import { createStore, combineReducers } from 'redux'

import app from './modules/app'
import pro from './modules/pro'

const reducer = combineReducers({
  app, pro
})

const store = createStore(reducer)

export default store
```

```js
// src/index.js 入口文件处改变使用方式
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from 'react-redux' // +++++++++++
import store from './store'
import App from './App.jsx'


ReactDOM.render(
  <React.StrictMode>
    <Provider store = { store }>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
)
```

```jsx
// src/App.jsx
import React, { Component } from 'react';
// connect 一般会把它称之为 高阶组件,原则上 connect()称之为高阶组件
import { connect } from 'react-redux'
// 展示组件 --- UI组件
class App extends Component {
  render() {
    console.log(this.props)
    return (
      <div> 
        <button onClick = { () => {
          this.props.reduceFn()
        }}>-</button>  
        { this.props.count }
        <button onClick = { () => {
          this.props.addFn()
        }}>+</button>  
      </div>
    );
  }
}

// connect()(App)
// connect(() => {}, () => {})

// mapStateToProps 负责给组件提供状态使用的，默认参数为state
const mapStateToProps = (state) => {
  return {
    count: state.app.count
  }
}

// mapDispatchToProps 负责组件的业务逻辑，默认参数为disaptch,可以触发修改状态
const mapDispatchToProps = (dispatch) => {
  return {
    reduceFn () {
      dispatch({
        type: 'REDUCE',
        payload: 5
      })
    },
    addFn () {
      dispatch({
        type: 'ADD',
        payload: 5
      })
    }
  }
}

// 暴露出去的其实就是 容器组件
export default connect(mapStateToProps, mapDispatchToProps)(App)
```

> ？实现pro.js中的数据的展示

```jsx
// src/App.jsx
import React, { Component } from 'react';
// connect 一般会把它称之为 高阶组件,原则上 connect()称之为高阶组件
import { connect } from 'react-redux'
// 展示组件 --- UI组件
class App extends Component {
  componentDidMount () {
    this.props.getBannerList()
    this.props.getProList()
  }
  render() {
    console.log(this.props)
    const { count, proList } = this.props
    return (
      <div> 
        <button onClick = { () => {
          this.props.reduceFn()
        }}>-</button>  
        { count }
        <button onClick = { () => {
          this.props.addFn()
        }}>+</button>  
        <hr />
        {
          proList && proList.map(item => {
            return (
              <h4 key = { item.proid }>{
                item.proname
              }</h4>
            )
          })
        }
      </div>
    );
  }
}
// connect()(App)
// connect(() => {}, () => {})
// mapStateToProps 负责给组件提供状态使用的，默认参数为state
const mapStateToProps = (state) => {
  return {
    count: state.app.count,
    proList: state.pro.proList,
    bannerList: state.pro.bannerList
  }
}
// mapDispatchToProps 负责组件的业务逻辑，默认参数为disaptch,可以触发修改状态
const mapDispatchToProps = (dispatch) => {
  return {
    reduceFn () {
      dispatch({
        type: 'REDUCE',
        payload: 5
      })
    },
    addFn () {
      dispatch({
        type: 'ADD',
        payload: 5
      })
    },
    getBannerList () {
      fetch('http://121.89.205.189/api/banner/list')
        .then(res => res.json())
        .then(res => {
          dispatch({
            type: 'CHANGE_BANNER_LIST',
            payload: res.data
          })
        })
    },
    getProList () {
      fetch('http://121.89.205.189/api/pro/list')
        .then(res => res.json())
        .then(res => {
          dispatch({
            type: 'CHANGE_PRO_LIST',
            payload: res.data
          })
        })
    }
  }
}
// 暴露出去的其实就是 容器组件
export default connect(mapStateToProps, mapDispatchToProps)(App)
```

> vuex中 可以把异步操作提取到vuex中的actions中，react中异步操作方式会有很多，最常见的就是 redux-thunk

### (5)**Redux异步**

通常情况下，action只是一个对象，不能包含异步操作，这导致了很多创建action的逻辑只能写在组件中，代码量较多也不便于复用，同时对该部分代码测试的时候也比较困难，组件的业务逻辑也不清晰，使用中间件了之后，可以通过actionCreator异步编写action，这样代码就会拆分到actionCreator中，可维护性大大提高，可以方便于测试、复用，同时actionCreator还集成了异步操作中不同的action派发机制，减少编码过程中的代码量

常见的异步库：

- Redux-thunk ✅✅✅✅
- Redux-saga ✅✅
- Redux-effects
- Redux-side-effects
- Redux-loop
- Redux-observable
- …

基于Promise的异步库：

- Redux-promise ✅
- Redux-promises
- Redux-simple-promise
- Redux-promise-middleware
- …

### (6)redux-thunk

```
cnpm i redux-thunk -S
```

```js
// src/store/modules/app.js
// 唯一修改状态的方式就是通过纯函数
// 必须返回全新的状态
const reducer = (state = { // 初始值，只有第一次访问时才是 10
  count: 10
}, action) => {
  switch (action.type) {
    case 'REDUCE':
      return { ...state, count: state.count - action.payload }
    case 'ADD':
      return Object.assign({}, state, { count: state.count + action.payload })
    default:
      return state
  }
}
export default reducer
```

```js
// src/store/modules/pro.js
const reducer = (state = {
  bannerList: [],
  proList: []
}, action) => {
  switch (action.type) {
    case 'CHANGE_BANNER_LIST':
      return { ...state, bannerList: action.payload }
    case 'CHANGE_PRO_LIST':
      return Object.assign({}, state, { proList: action.payload })
    default:
      return state
  }
}
export default reducer
```

```js
// src/store/index.js
import { createStore, combineReducers, applyMiddleware } from 'redux'

import thunk from 'redux-thunk' // 异步操作

import app from './modules/app'
import pro from './modules/pro'

const reducer = combineReducers({
  app, pro
})

const store = createStore(reducer, applyMiddleware(thunk)) // 配置异步操作

export default store
```

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from 'react-redux'
import store from './store'
import App from './App.jsx'


ReactDOM.render(
  <React.StrictMode>
    <Provider store = { store }>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
)
```

Redux Thunk [middleware](https://redux.js.org/tutorials/fundamentals/part-4-store#middleware) allows you to write action creators that return a function instead of an action.

```js
// src/store/actionCreator/pro.js
// 专门用来触发异步操作
// thunk模块执行时，actionCreator 函数有默认的参数为 dispatch
// 该dispatch可以用来触发 reducer 
// 有时候在触发 异步时，需要传递参数，这个时候，可以在函数内部返回一个actionCreator 函数
const actions = {
  getBannerListAction (dispatch) {
    fetch('http://121.89.205.189/api/banner/list')
      .then(res => res.json())
      .then(res => {
        dispatch({
          type: 'CHANGE_BANNER_LIST',
          payload: res.data
        })
      })
  },
  getProListAction (count) { // 有参数，返函数，函数默认为dispatch
    count = count || 1
    return function (dispatch) {
      fetch('http://121.89.205.189/api/pro/list?count=' + count)
        .then(res => res.json())
        .then(res => {
          dispatch({
            type: 'CHANGE_PRO_LIST',
            payload: res.data
          })
        })
    }
  }
} 

export default actions
```

```jsx
// src/App.jsx
import React, { Component } from 'react';
// connect 一般会把它称之为 高阶组件,原则上 connect()称之为高阶组件
import { connect } from 'react-redux'
import actions from './store/actionCreator/pro'
// 展示组件 --- UI组件
class App extends Component {
  componentDidMount () {
    this.props.getBannerList()
    this.props.getProList()
  }
  render() {
    console.log(this.props)
    const { count, proList } = this.props
    return (
      <div> 
        <button onClick = { () => {
          this.props.reduceFn()
        }}>-</button>  
        { count }
        <button onClick = { () => {
          this.props.addFn()
        }}>+</button>  
        <hr />
        {
          proList && proList.map(item => {
            return (
              <h4 key = { item.proid }>{
                item.proname
              }</h4>
            )
          })
        }
      </div>
    );
  }
}
// connect()(App)
// connect(() => {}, () => {})
// mapStateToProps 负责给组件提供状态使用的，默认参数为state
const mapStateToProps = (state) => {
  return {
    count: state.app.count,
    proList: state.pro.proList,
    bannerList: state.pro.bannerList
  }
}
// mapDispatchToProps 负责组件的业务逻辑，默认参数为disaptch,可以触发修改状态
const mapDispatchToProps = (dispatch) => {
  return {
    reduceFn () {
      dispatch({
        type: 'REDUCE',
        payload: 5
      })
    },
    addFn () {
      dispatch({
        type: 'ADD',
        payload: 5
      })
    },
    getBannerList () {
      // 不要加括号
      // dispatch 触发 thunk 异步操作
      dispatch(actions.getBannerListAction)
    },
    getProList () {
      // fetch('http://121.89.205.189/api/pro/list')
      //   .then(res => res.json())
      //   .then(res => {
      //     dispatch({
      //       type: 'CHANGE_PRO_LIST',
      //       payload: res.data
      //     })
      //   })
      // 因为要参数，需要加（）传递
      dispatch(actions.getProListAction(2))
    }
  }
}
// 暴露出去的其实就是 容器组件
export default connect(mapStateToProps, mapDispatchToProps)(App)
```

### (7)redux-saga

```
cnpm i redux-saga -S
```

```js
// src/api/home.js
export function getBannerList () {
  return fetch('http://121.89.205.189/api/banner/list')
            .then(res => res.json())
}

export function getProList () {
  return fetch('http://121.89.205.189/api/pro/list')
            .then(res => res.json())
}
```

```js
// src/store/modules/home.js
const reducer = (state = {
  bannerList: [],
  proList: []
}, action) => {
  switch (action.type) {
    case 'CHANGE_BANNER_LIST':
      return { ...state, bannerList: action.payload }
    case 'CHANGE_PRO_LIST':
      return Object.assign({}, state, { proList: action.payload })
    default:
      return state
  }
}
export default reducer
```

```js
// src/store/modules/app.js
// 唯一修改状态的方式就是通过纯函数
// 必须返回全新的状态
const reducer = (state = { // 初始值，只有第一次访问时才是 10
  count: 10
}, action) => {
  switch (action.type) {
    case 'REDUCE':
      return { ...state, count: state.count - action.payload }
    case 'ADD':
      return Object.assign({}, state, { count: state.count + action.payload })
    default:
      return state
  }
}
export default reducer
```

```js
// src/store/mySaga.js
// 处理异步操作
// call 可以用来触发请求
// put 类似于以前的 dispatch
// takeLatest 执行一个 执行哪一个任务  takeEvery(执行多个)
import { call, put, takeLatest } from 'redux-saga/effects'

import { getBannerList, getProList } from './../api/home'

// redux-saga ---- generator函数结合一起使用
function * getBannerListAction () {
  const res = yield call(getBannerList)
  yield put({
    type: 'CHANGE_BANNER_LIST',
    payload: res.data
  })
}

function * getProListAction () {
  const res = yield call(getProList)
  yield put({
    type: 'CHANGE_PRO_LIST',
    payload: res.data
  })
}

function * mySaga () {
  // 本模块的相关异步请求声明
  yield takeLatest('REQUEST_BANNER_LIST', getBannerListAction)
  yield takeLatest('REQUEST_PRO_LIST', getProListAction)
}

export default mySaga
```

```js
// src/store/index.js
import { createStore, combineReducers, applyMiddleware } from 'redux'

import createSagaMiddleware from 'redux-saga'

import mySaga from './mySaga' // 异步操作说明

import home from './modules/home'
import app from './modules/app'

const reducer = combineReducers({
  home, 
  app
})

const sagaMiddleware = createSagaMiddleware() // 生成saga的中间件

const store = createStore(reducer, applyMiddleware(sagaMiddleware))

// saga 的run 函数 必须 写 在调用 中间件之后
sagaMiddleware.run(mySaga)

export default store
```

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom'

import { Provider } from 'react-redux'
import store from './store'

import App from './App.jsx'

ReactDOM.render(
  <React.StrictMode>
    <Provider store = { store }>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
)
```

```jsx
// src/App.jsx
import React, { Component } from 'react'
import { connect } from 'react-redux'
class App extends Component {
  componentDidMount () {
    this.props.getBannerList()
    this.props.getProList()
  }
  render() {
    const { proList } = this.props
    return (
      <div>
        {
          proList && proList.map(item => {
            return (
              <h5 key = { item.proid }>
                { item.proname }
              </h5>
            )
          })
        }
      </div>
    )
  }
}
export default connect(
  (state) => {
    return {
      bannerList: state.home.bannerList,
      proList: state.home.proList
    }
  },
  (dispatch) => {
    return {
      getBannerList () {
        // 触发的saga中自定义中  mySaga
        dispatch({ type: 'REQUEST_BANNER_LIST' })
      },
      getProList () {
        dispatch({ type: 'REQUEST_PRO_LIST' })
      }
    }
  }
)(App)
```

> 如何在请求时传递参数

```js
// src/api/home.js
export function getBannerList () {
  return fetch('http://121.89.205.189/api/banner/list')
            .then(res => res.json())
}

export function getProList (payload) { // ++++++++
  return fetch('http://121.89.205.189/api/pro/list?count=' + payload)
            .then(res => res.json())
}
```

```js
// src/store/mySaga.js
// 处理异步操作
// call 可以用来触发请求
// put 类似于以前的 dispatch
// takeLatest 执行一个 执行哪一个任务  takeEvery(执行多个)
import { call, put, takeLatest } from 'redux-saga/effects'

import { getBannerList, getProList } from './../api/home'

// redux-saga ---- generator函数结合一起使用
function * getBannerListAction () {
  const res = yield call(getBannerList)
  yield put({
    type: 'CHANGE_BANNER_LIST',
    payload: res.data
  })
}

function * getProListAction (action) { // ++++++++
  const res = yield call(getProList, action.payload) // ++++++++
  yield put({
    type: 'CHANGE_PRO_LIST',
    payload: res.data
  })
}

function * mySaga () {
  // 本模块的相关异步请求声明
  yield takeLatest('REQUEST_BANNER_LIST', getBannerListAction)
  yield takeLatest('REQUEST_PRO_LIST', getProListAction)
}

export default mySaga
```

```jsx
// src/App.jsx
import React, { Component } from 'react'
import { connect } from 'react-redux'
class App extends Component {
  componentDidMount () {
    this.props.getBannerList()
    this.props.getProList()
  }
  render() {
    const { proList } = this.props
    return (
      <div>
        {
          proList && proList.map(item => {
            return (
              <h5 key = { item.proid }>
                { item.proname }
              </h5>
            )
          })
        }
      </div>
    )
  }
}
export default connect(
  // (state) => {
  //   return {
  //     bannerList: state.home.bannerList,
  //     proList: state.home.proList
  //   }
  // },
  ({ home: { bannerList, proList }}) => ({ bannerList, proList }),
  (dispatch) => {
    return {
      getBannerList () {
        // 触发的saga中自定义中  mySaga
        dispatch({ type: 'REQUEST_BANNER_LIST' })
      },
      getProList () {
        dispatch({ type: 'REQUEST_PRO_LIST', payload: 2 }) // +++++++++++++++++++
      }
    }
  }
)(App)
```

深入了解redux-saga:https://redux-saga-in-chinese.js.org/



- 



# 十六、Immutable.js



## 1、JavaScript数据修改的问题

看一段大家熟悉的代码

```js
// immutable/01 test.js
const state = {
  str: '千锋教育',
  obj: {
    y: 1
  },
  arr: [1, 2, 3]
}
const newState = state

// 由于js的对象和数组都是引用类型。所以newState的state实际上是指向于同一块内存地址的, 
// 所以结果是newState和state是相等的
console.log(newState === state) // true
```

由于js的对象和数组都是引用类型。所以newState的state实际上是指向于同一块内存地址的, 所以结果是newState和state是相等的。

尝试修改一下数据

```js
// immutable/02test.js
const state = {
  str: '千锋教育',
  obj: {
    y: 1
  },
  arr: [1, 2, 3]
}
const newState = state

newState.str = '千锋HTML5学院'
// newState的修改也会引起state的修改
console.log(state.str, newState.str) // 锋HTML5学院 千锋HTML5学院 
```

可以看到，newState的修改也会引起state的修改。要解决这个问题，js中提供了另一种修改数据的方式，要修改一个数据之前先制作一份数据的拷贝，像这样

```js
// immutable/03test.js
const state = {
  str: '千锋教育',
  obj: {
    y: 1
  },
  arr: [1, 2, 3]
}
// 修改一个数据之前先制作一份数据的拷贝
// 这些都是浅拷贝，就是只拷贝第一层数据，更深层的数据还是同一个引用
const newState = Object.assign({}, state)

newState.str = '千锋HTML5学院'

console.log(state.str, newState.str) // 千锋教育 千锋HTML5学院
```

我们可以使用很多方式在js中复制数据，比如`…`,  `Object.assign`, `Object.freeze`, `slice`, `concat`, `map`, `filter`,  `reduce`等方式进行复制，但这些都是浅拷贝，就是只拷贝第一层数据，更深层的数据还是同一个引用，比如：

```js
// immutable/04 test.js
const state = {
  str: '千锋教育',
  obj: {
    y: 1
  },
  arr: [1, 2, 3]
}
// 修改一个数据之前先制作一份数据的拷贝
// 这些都是浅拷贝，就是只拷贝第一层数据，更深层的数据还是同一个引用
const newState = Object.assign({}, state)

newState.obj = {
  y: 2
}
newState.arr.push(4)

console.log(state, newState) // 千锋教育 千锋HTML5学院
// { str: '千锋教育', obj: { y: 1 }, arr: [ 1, 2, 3, 4 ] } 
// { str: '千锋教育', obj: { y: 2 }, arr: [ 1, 2, 3, 4 ] }
```

可以看到，当在更改newState更深层次的数据的时候，还是会影响到state的值。如果要深层复制，就得一层一层的做递归拷贝，这是一个复杂的问题。虽然有些第三方的库已经帮我们做好了，比如`lodash`的`cloneDeep`方法。深拷贝是非常消耗性能的。

```js
import { cloneDeep } from 'lodash'

const state = {
  str: '千锋教育',
  obj: {
    y: 1
  },
  arr: [1, 2, 3]
}
const newState = cloneDeep(state)

newState.obj.y = 2
newState.arr.push(4)

console.log(state, newState)
```



## 2、什么是不可变数据

不可变数据 (Immutable Data )就是一旦创建，就不能再被更改的数据。对 Immutable 对象的任何修改或添加删除操作都会返回一个新的 Immutable 对象。Immutable 实现的原理是**持久化数据结构**（ Persistent Data Structure），也就是使用旧数据创建新数据时，要保证旧数据同时可用且不变。同时为了避免 deepCopy 把所有节点都复制一遍带来的性能损耗，Immutable 使用了 结构共享（Structural Sharing），即如果对象树中一个节点发生变化，只修改这个节点和受它影响的父节点，其它节点则进行共享。

![react](./images/structure-sharing.png)

## 3、immutable.js的优缺点

**优点：**

- 降低mutable带来的复杂度
- 节省内存
- 历史追溯性（时间旅行）：时间旅行指的是，每时每刻的值都被保留了，想回退到哪一步只要简单的将数据取出就行，想一下如果现在页面有个撤销的操作，撤销前的数据被保留了，只需要取出就行，这个特性在redux或者flux中特别有用
- 拥抱函数式编程：immutable本来就是函数式编程的概念，纯函数式编程的特点就是，只要输入一致，输出必然一致，相比于面向对象，这样开发组件和调试更方便。推荐一本函数式编程的在线免费书《[JS 函数式编程指南](<https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/>)》。

**缺点：**

- 需要重新学习api
- 资源包大小增加（源码5000行左右）
- 容易与原生对象混淆：由于api与原生不同，混用的话容易出错。



## 4、使用Immutable.js

```
cnpm i immutable -S
```



**01-get-started**

```javascript
const { Map } = require('immutable')

const map1 = Map({
  a: 1,
  b: 2,
  c: 3
})

const map2 = map1.set('b', 50)

console.log(map1.get('b') + ' vs. ' + map2.get('b')) // 2 vs. 50
```



**02-case-for-immutability-1.js**

```javascript
const { Map } = require('immutable')

const map1 = Map({
  a: 1,
  b: 2,
  c: 3
})

const map2 = Map({
  a: 1,
  b: 2,
  c: 3
})

console.log(map1.equals(map2)) // true   比较的是对象下的值的异同
console.log(map1 == map2) // false
console.log(map1 === map2) // false
```



**02-case-for-immutability-2.js**

```javascript

const { Map } = require('immutable')

const map1 = Map({
  a: 1,
  b: 2,
  c: 3
})

const map2 = map1.set('b', 2)

console.log(map1.equals(map2)) // true   比较的是对象下的值的异同
console.log(map1 == map2) // true
console.log(map1 === map2) // true
```



**02-case-for-immutability-3.js**

```javascript

const { Map } = require('immutable')
const map = Map({ a: 1, b: 2, c: 3})
const mapCopy = map

console.log(mapCopy.equals(map))

// true
```



**03-JavaScript-first-API-0.js**

```javascript
const { List } = require('immutable')

const list1 = List([1, 2])
const list2 = list1.push(3, 4, 5)
const list3 = list2.unshift(0)
const list4 = list1.concat(list2, list3)

console.log(list1.size === 2)
console.log(list2.size === 5)
console.log(list3.size === 6)
console.log(list4.size === 13)

// true
// true
// true
// true
```



**03-JavaScript-first-API-1.js**

```javascript
const { Map } = require('immutable')
const alpha = Map({ a: 1, b: 2, c: 3, d: 4 })
const upperCase = alpha.map((v, k) => k.toUpperCase()).join()

console.log(upperCase)
// A,B,C,D
```



**03-JavaScript-first-API-2.js**

```javascript
const { Map, List } = require('immutable');

const map1 = Map({ a: 1, b: 2, c: 3, d: 4 });
const map2 = Map({ c: 10, a: 20, t: 30 });

const obj = { d: 100, o: 200, g: 300 };
const map3 = map1.merge(map2, obj)

console.log(map3)

// Map { "a": 20, "b": 2, "c": 10, "d": 100, "t": 30, "o": 200, "g": 300 }

const list1 = List([ 1, 2, 3 ]);
const list2 = List([ 4, 5, 6 ]);

const array = [ 7, 8, 9 ];
const list3 = list1.concat(list2, array);

console.log(list3)

// List [ 1, 2, 3, 4, 5, 6, 7, 8, 9 ]
```



**03-JavaScript-first-API-3.js**

```javascript
const { Seq } = require('immutable')

const myObject = {a: 1, b: 2, c: 3}
const seq = Seq(myObject).map(v => v * v)

const seqToObject = seq.toObject()
console.log(seq, seqToObject)

// Seq { "a": 1, "b": 4, "c": 9 } { a: 1, b: 4, c: 9 }
```



**03-JavaScript-first-API-4.js**

```javascript
const { fromJS } = require('immutable')

const obj = { 1: 'one' }
console.log(Object.keys(obj)) *// [ '1' ]*
console.log(obj['1'], obj[1]) *// one one*

const map = fromJS(obj)
console.log(map.get('1'), map.get(1)) *// one undefined*
```



**03-JavaScript-first-API-5.js**

```javascript
const { Map, List } = require('immutable')

const deep = Map({ a: 1, b: 2, c: List([ 3, 4, 5 ]) })
console.log(deep.toObject())
console.log(deep.toArray())
console.log(deep.toJS())
console.log(JSON.stringify(deep))

// { a: 1, b: 2, c: List [ 3, 4, 5 ] }
// [ 1, 2, List [ 3, 4, 5 ] ]
// { a: 1, b: 2, c: [ 3, 4, 5 ] }
// {"a":1,"b":2,"c":[3,4,5]}
```



**03-JavaScript-first-API-6.js**

```javascript
const { Map, List } = require('immutable')

const mapped = Map({ a: 1, b: 2, c: 3 })
console.log(mapped.map(x => x * x))
console.log(mapped.map(function (x) {
  return x * x
}))

// Map { "a": 1, "b": 4, "c": 9 }
// Map { "a": 1, "b": 4, "c": 9 }

const aList = List([ 1, 2, 3 ])
const anArray = [ 0, ...aList, 4, 5 ]
console.log(anArray)

// [ 0, 1, 2, 3, 4, 5 ]
```



**04-nested-structures.js**

```javascript
const { fromJS } = require('immutable')

const nested = fromJS({ a: { b: { c: [ 3, 4, 5 ] } } })
console.log(nested)
// Map { "a": Map { "b": Map { "c": List [ 3, 4, 5 ] } } }

const nested2 = nested.mergeDeep({ a: { b: { d: 6 } } })
console.log(nested2)
// Map { "a": Map { "b": Map { "c": List [ 3, 4, 5 ], "d": 6 } } }*

console.log(nested2.getIn([ 'a', 'b', 'd' ]))
// 6

const nested3 = nested2.updateIn([ 'a', 'b', 'd' ], value => value + 1)
console.log(nested3)
// Map { "a": Map { "b": Map { "c": "List [ 3, 4, 5 ]1", "d": 7 } } }

// setIn 和 updateIn 都可以修改深层次的Immutable对象，setIn 直接传值，updateIn 传入回调函数
const nested4 = nested3.updateIn([ 'a', 'b', 'c' ], list => list.push(6))
console.log(nested4)
// Map { "a": Map { "b": Map { "c": List [ 3, 4, 5, 6 ], "d": 7 } } }

const nested5 = nested4.setIn(['a', 'b', 'd'], 90)

console.log(nested5)
console.log(nested)
console.log(nested2)
console.log(nested3)
console.log(nested4)

// Map { "a": Map { "b": Map { "c": List [ 3, 4, 5 ] } } }
// Map { "a": Map { "b": Map { "c": List [ 3, 4, 5 ], "d": 6 } } }
// Map { "a": Map { "b": Map { "c": List [ 3, 4, 5 ], "d": 7 } } }
// Map { "a": Map { "b": Map { "c": List [ 3, 4, 5, 6 ], "d": 7 } } }
```



**05-Equality-treats-Collections-as-Values-0.js**

```javascript
const { Map, is } = require('immutable')

const obj1 = { a: 1, b: 2, c: 3 };
const obj2 = { a: 1, b: 2, c: 3 };
console.log(obj1 !== obj2)
// true

const map1 = Map({ a: 1, b: 2, c: 3 });
const map2 = Map({ a: 1, b: 2, c: 3 });

console.log(map1 !== map2)
console.log(map1.equals(map2))
console.log(is(map1, map2))

// true
// true
// true
```



**05-Equality-treats-Collections-as-Values-1.js**

```javascript
const { Map, Set } = require('immutable')

const map1 = Map({ a: 1, b: 2, c: 3 });
const map2 = Map({ a: 1, b: 2, c: 3 });
const set = Set().add(map1)

console.log(set.has(map2))

// true
```



**05-Equality-treats-Collections-as-Values-2.js**

```javascript
const { Map } = require('immutable');

const originalMap = Map({ a: 1, b: 2, c: 3 });
const updatedMap = originalMap.set('b', 2);
console.log(updatedMap === originalMap)

// true
```



**05-Equality-treats-Collections-as-Values-3.js**

```javascript
const { Map, is } = require('immutable');

const originalMap = Map({ a: 1, b: 2, c: 3 });
const updatedMap = originalMap.set('b', 1000);
console.log(updatedMap !== originalMap)
// true

const anotherUpdatedMap = originalMap.set('b', 1000);
console.log(anotherUpdatedMap !== updatedMap)
console.log(anotherUpdatedMap.equals(updatedMap))
console.log(is(anotherUpdatedMap, updatedMap))
// true
// true
// true
```



**06-Batching-Mutations.js**

```javascript
const { List } = require('immutable');

const list1 = List([ 1, 2, 3 ]);
const list2 = list1.withMutations(function (list) {
  list.push(4).push(5).push(6);
});

console.log(list1.size === 3);
console.log(list2.size === 6);
// true
// true

let map2 = map1.withMutations((map) => {
   // 逻辑
   map.setIn(['c', 'd'], 9)
   map.set('a', 1)
})

let map3 = map1.updateIn(['c', 'd'], (v) => {
   return 9
})

console.log(map1 === map3)
```



**07-Lazy-Seq-0.js**

```javascript
const { Seq } = require('immutable');

const oddSquares = Seq([ 1, 2, 3, 4, 5, 6, 7, 8 ])
  .filter(x => {
		console.log('filter x:' + x)
		return x % 2 !== 0
  })
  .map(x => {
		console.log('map x:' + x)
		return x * x
  });

console.log(oddSquares.get(1))

// filter x:1
// filter x:2
// filter x:3
// map x:3

// 9
```



**07-Lazy-Seq-1.js**

```javascript
const { Seq, Map } = require('immutable');

const map = Map({ a: 1, b: 2, c: 3 });
const lazySeq = Seq(map);
const newMap = lazySeq
  .flip()
  .map(key => key.toUpperCase())
  .flip();

console.log(newMap)

// Seq { A: 1, B: 1, C: 1 }
```



**07-Lazy-Seq-2.js**

```javascript
const { Range } = require('immutable');

const aRange = Range(1, Infinity)
  .skip(1000)
  .map(n => -n)
  .filter(n => n % 2 === 0)
  .take(2)
  .reduce((r, n) => r * n, 1);
  
console.log(aRange)

// 1006008
```





## 5、在redux中使用immutable.js

[redux官网](<https://redux.js.org/recipes/using-immutablejs-with-redux>)推荐使用[redux-immutable](<https://www.npmjs.com/package/redux-immutable>)进行redux和immutable的集成。几个注意点：

`redux`中，利用`combineReducers`来合并多个`reducer`, `redux`自带的`combineReducers`只支持原生js形式的，所以需要使用`redux-immutable`提供的`combineReducers`来代替

```
cnpm i redux-immutable -S
```

### 1.redux + react-redux + redux-saga +immutable

```js
// src/store/modules/home.js
import { Map } from 'immutable' // +++++++
const reducer = (state = Map({// +++++++
  bannerList: [],
  proList: []
}), action) => {
  switch (action.type) {
    case 'CHANGE_BANNER_LIST':
      return state.set('bannerList', action.payload)// +++++++
    case 'CHANGE_PRO_LIST':
      return state.set('proList', action.payload)// +++++++
    default:
      return state
  }
}
export default reducer
```

```js
// src/store/modules/app.js
import { Map } from 'immutable'
// 唯一修改状态的方式就是通过纯函数
// 必须返回全新的状态
const reducer = (state = Map({ // 初始值，只有第一次访问时才是 10
  count: 10
}), action) => {
  switch (action.type) {
    case 'REDUCE':
      return state.set('count', state.get('count') - action.payload)
    case 'ADD':
      return state.set('count', state.get('count') + action.payload)
    default:
      return state
  }
}
export default reducer
```

```js
// src/store/index.js
import { createStore, applyMiddleware } from 'redux'

import { combineReducers } from 'redux-immutable' // +++++++++++++++++++++++

import createSagaMiddleware from 'redux-saga'

import mySaga from './mySaga' // 异步操作说明

import home from './modules/home'
import app from './modules/app'

const reducer = combineReducers({
  home, 
  app
})

const sagaMiddleware = createSagaMiddleware() // 生成saga的中间件

const store = createStore(reducer, applyMiddleware(sagaMiddleware))

// saga 的run 函数 必须 写 在调用 中间件之后
sagaMiddleware.run(mySaga)

export default store
```

```js
// src/App.jsx
import React, { Component } from 'react'
import { connect } from 'react-redux'
class App extends Component {
  componentDidMount () {
    this.props.getBannerList()
    this.props.getProList()
  }
  render() {
    const { proList, count } = this.props
    return (
      <div>
        { count }
        {
          proList && proList.map(item => {
            return (
              <h5 key = { item.proid }>
                { item.proname }
              </h5>
            )
          })
        }
      </div>
    )
  }
}
export default connect(
  (state) => {
    return {
      bannerList: state.getIn(['home', 'bannerList']), // +++++++++++++++++
      proList: state.getIn(['home', 'proList']),// +++++++++++++++++
      count: state.getIn(['app', 'count'])// +++++++++++++++++
    }
  },
  (dispatch) => {
    return {
      getBannerList () {
        // 触发的saga中自定义中  mySaga
        dispatch({ type: 'REQUEST_BANNER_LIST' })
      },
      getProList () {
        dispatch({ type: 'REQUEST_PRO_LIST', payload: 5 })
      }
    }
  }
)(App)
```

### 2.redux + react-redux + redux-thunk + immutable

```js
// src/store/modules/pro.js
import { Map } from 'immutable' // +++++++
const reducer = (state = Map({// +++++++
  bannerList: [],
  proList: []
}), action) => {
  switch (action.type) {
    case 'CHANGE_BANNER_LIST':
      return state.set('bannerList', action.payload)// +++++++
    case 'CHANGE_PRO_LIST':
      return state.set('proList', action.payload)// +++++++
    default:
      return state
  }
}
export default reducer
```

```js
// src/store/modules/app.js
import { Map } from 'immutable'
// 唯一修改状态的方式就是通过纯函数
// 必须返回全新的状态
const reducer = (state = Map({ // 初始值，只有第一次访问时才是 10
  count: 10
}), action) => {
  switch (action.type) {
    case 'REDUCE':
      return state.set('count', state.get('count') - action.payload)
    case 'ADD':
      return state.set('count', state.get('count') + action.payload)
    default:
      return state
  }
}
export default reducer
```

```js
// src/store/index.js
import { createStore, applyMiddleware } from 'redux'
import { combineReducers } from 'redux-immutable'
import thunk from 'redux-thunk'

import app from './modules/app'
import pro from './modules/pro'

const reducer = combineReducers({
  app, pro
})

const store = createStore(reducer, applyMiddleware(thunk))

export default store
```

```js
// src/App.jsx
import React, { Component } from 'react';
// connect 一般会把它称之为 高阶组件,原则上 connect()称之为高阶组件
import { connect } from 'react-redux'
import actions from './store/actionCreator/pro'
// 展示组件 --- UI组件
class App extends Component {
  componentDidMount () {
    this.props.getBannerList()
    this.props.getProList()
  }
  render() {
    console.log(this.props)
    const { count, proList } = this.props
    return (
      <div> 
        <button onClick = { () => {
          this.props.reduceFn()
        }}>-</button>  
        { count }
        <button onClick = { () => {
          this.props.addFn()
        }}>+</button>  
        <hr />
        {
          proList && proList.map(item => {
            return (
              <h4 key = { item.proid }>{
                item.proname
              }</h4>
            )
          })
        }
      </div>
    );
  }
}
// connect()(App)
// connect(() => {}, () => {})
// mapStateToProps 负责给组件提供状态使用的，默认参数为state
const mapStateToProps = (state) => {
  return {
    // count: state.app.count,
    // proList: state.pro.proList,
    // bannerList: state.pro.bannerList
    count: state.getIn(['app', 'count']),
    bannerList: state.getIn(['pro', 'bannerList']),
    proList: state.getIn(['pro', 'proList'])
  }
}
// mapDispatchToProps 负责组件的业务逻辑，默认参数为disaptch,可以触发修改状态
const mapDispatchToProps = (dispatch) => {
  return {
    reduceFn () {
      dispatch({
        type: 'REDUCE',
        payload: 5
      })
    },
    addFn () {
      dispatch({
        type: 'ADD',
        payload: 5
      })
    },
    getBannerList () {
      // 不要加括号
      // dispatch 触发 thunk 异步操作
      dispatch(actions.getBannerListAction)
    },
    getProList () {
      // fetch('http://121.89.205.189/api/pro/list')
      //   .then(res => res.json())
      //   .then(res => {
      //     dispatch({
      //       type: 'CHANGE_PRO_LIST',
      //       payload: res.data
      //     })
      //   })
      // 因为要参数，需要加（）传递
      dispatch(actions.getProListAction(2))
    }
  }
}
// 暴露出去的其实就是 容器组件
export default connect(mapStateToProps, mapDispatchToProps)(App)
```



# 十七、React Router

React Router现在的版本是6, 于2019年3月21日搞笑的发布，[搞笑的官网链接](<https://reacttraining.com/blog/react-router-v5/>)， 本来**是要发布4.4的**版本的，结果成了5。从4开始，使用方式相对于之前版本的思想有所不同。之前版本的思想是传统的思想：**路由应该统一在一处渲染**， Router 4之后是这样的思想：**一切皆组件**

React Router包含了四个包:

| 包名                                                         | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`react-router`](https://github.com/ReactTraining/react-router/blob/master/packages/react-router) | React Router核心api                                          |
| [`react-router-dom`](https://github.com/ReactTraining/react-router/blob/master/packages/react-router-dom) | React Router的DOM绑定，在浏览器中运行不需要额外安装`react-router` |
| [`react-router-native`](https://github.com/ReactTraining/react-router/blob/master/packages/react-router-native) | [React Native](https://facebook.github.io/react-native/) 中使用，而实际的应用中，其实不会使用这个。 |
| [`react-router-config`](https://github.com/ReactTraining/react-router/blob/master/packages/react-router-config) | 静态路由的配置                                               |

主要使用`react-router-dom`

## 1.v6版本 -- 熟悉

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom'
// import { HashRouter } from 'react-router-dom'
import { HashRouter as Router } from 'react-router-dom'
import App from './App.jsx'
import './style.css'

ReactDOM.render(
  <React.StrictMode>
    <Router>
      <App />
    </Router>
  </React.StrictMode>,
  document.getElementById('root')
)
```

```js
// src/App.jsx
import React, { Component } from 'react';
import { Routes, Route, NavLink, Link, useParams, useNavigate } from 'react-router-dom'
const Home = (props) => {
  const navigate = useNavigate() // 编程式导航
  // v5  const history = useHistory()
  return (
    <div>
      首页
      <Link to="/detail/1"><button >详情</button></Link>
      <button onClick = {() => {
        console.log(props)
        navigate('/detail/2', { replace: true })
        // v5 history.push('/detail/2')
        // v5 history.replace('/detail/2')
      }}>详情2</button>
    </div>
  )
}
// v5通过withRouter 使组件的props拥有history
// histtory.push()  replace()
// const Kind = withRouter(() => <div>分类</div>)
const Kind = () => <div>分类</div>
const Cart = () => <div>购物车</div>
const User = () => <div>我的</div>
const Detail = (props) => {
  const params = useParams()
  console.log(params)
return <div>详情 - { params.proid }</div>
}
const NotFound = () => <div>404</div>
export default class App extends Component {
  render() {
    return (
      <>
      {/* v5 Switch */}
        <Routes>
          {/* v5 */}
          {/* <Route path="/home" component= { Home } /> */}

          <Route path="/home" element= { <Home />  } />
          <Route path="/kind" element= { <Kind /> } />
          <Route path="/cart" element= { <Cart /> } />
          <Route path="/user" element= { <User /> } />
          <Route path="/detail/:proid" element= { <Detail /> } />
          <Route path="*" element= { <NotFound /> } />
        </Routes>

        <ul>
          <li><NavLink to="/home">首页</NavLink></li>
          <li><NavLink to="/kind">分类</NavLink></li>
          <li><NavLink to="/cart">购物车</NavLink></li>
          <li><NavLink to="/user">我的</NavLink></li>
        </ul>
      </>
    );
  }
}

```

## 5. v5版本

### 1.创建项目

```
npx create-react-app router-app
cd router-app
cnpm i react-router-dom@5 redux react-redux redux-thunk redux-saga immutable redux-immutable prop-types antd-mobile@next -S
```

配置装饰器语法（但并不是必须）

```
cnpm i @babel/plugin-proposal-decorators customize-cra react-app-rewired -D
```

根目录下创建config-overrides.js

```js
// 参考配置连接：https://www.npmjs.com/package/customize-cra
const {override,addDecoratorsLegacy}=require("customize-cra");

module.exports=override(
    addDecoratorsLegacy(),//配置装饰器模式
);
```

修改package.json运行命令

```js
"scripts": {
  "start": "react-app-rewired start", // update
  "build": "react-app-rewired build", // update
  "test": "react-app-rewired test",  // update
  "eject": "react-scripts eject"
},
```

```
yarn start

```



## 、使用方式

正常情况下，直接按照[官网](<https://reacttraining.com/react-router/web/guides/quick-start>)的demo就理解 路由的使用方式，有几个点需要特别的强调：

- Route组件的exact属性

`exact`属性标识是否为严格匹配， 为`true`是表示严格匹配，为`false`时为正常匹配。

- Route组件的render属性而不是component属性

怎么在渲染组件的时候，对组件传递属性呢？使用`component`的方式是不能直接在组件上添加属性的。所以，React Router的`Route`组件提供了另一种渲染组件的方式 `render`, 这个常用于页面组件级别的权限管理。

- 路由的参数传递与获取

- Switch组件

总是渲染第一个匹配到的组件

- 处理404与默认页

- withRoute高阶组件的使用
- 管理一个项目路由的方法
- [code spliting](<https://reacttraining.com/react-router/web/guides/code-splitting>)
- HashRouter和BrowserRouter的区别，前端路由和后端路由的区别。



## 2、React Router基本原理

React Router甚至大部分的前端路由都是依赖于[`history.js`](<https://github.com/browserstate/history.js>)的，它是一个独立的第三方js库。可以用来兼容在不同浏览器、不同环境下对历史记录的管理，拥有统一的API。

- 老浏览器的history: 通过`hash`来存储在不同状态下的`history`信息，对应`createHashHistory`，通过检测`location.hash`的值的变化，使用`location.replace`方法来实现url跳转。通过注册监听`window`对象上的`hashChange`事件来监听路由的变化，实现历史记录的回退。
- 高版本浏览器: 利用HTML5里面的history，对应`createBrowserHistory`, 使用包括`pushState`， `replaceState`方法来进行跳转。通过注册监听`window`对象上的`popstate`事件来监听路由的变化，实现历史记录的回退。
- node环境下: 在内存中进行历史记录的存储，对应`createMemoryHistory`。直接在内存里`push`和`pop`状态。

# 十八、Lazy 和 Suspense

## 1、React.lazy 定义

`React.lazy` 函数能让你像渲染常规组件一样处理动态引入（的组件）。

什么意思呢？其实就是懒加载。其原理就是利用`es6 import()`函数。这个`import`不是`import命令`。同样是引入模块，`import命令`是同步引入模块，而`import()`函数动态引入。

当 Webpack 解析到该语法时，它会自动地开始进行代码分割(Code Splitting)，分割成一个文件，当使用到这个文件的时候会这段代码才会被异步加载。

### (1) 为什么代码要分割

当你的程序越来越大，代码量越来越多。一个页面上堆积了很多功能，也许有些功能很可能都用不到，但是一样下载加载到页面上，所以这里面肯定有优化空间。就如图片懒加载的理论。

### (2) import函数

javascript

```javascript
//import 命令
import { add } from './math';

console.log(add(16, 26));

//import函数
import("./math").then(math => {
  console.log(math.add(16, 26));
});
```

> 动态 `import()` 语法目前只是一个 ECMAScript (JavaScript) 提案， 而不是正式的语法标准。预计在不远的将来就会被正式接受。http://es6.ruanyifeng.com/#docs/module#import

### (3) import函数示例

下面是import一个示例：

在test文件夹下新建两个文件

![图片1](./images/1.png)

**图片1：**

test.html代码如下:

```html
<div id="root">
  页面无内容
</div>
<button id="btn">加载js</button>

<script>
  document.getElementById('btn').onclick=function(){
    import('./test.js').then(d=>{
      d.test()
    })
  }
</script>
```

test.js代码如下:

```javascript
function test(){
  document.getElementById('root')
  root.innerHTML='页面变的有内容了'
}
export {test}
```

![图片2](./images/2.png)

**图片2**

这时候打开web服务让页面以http的方式访问，http://192.168.1.2:8080/test.html

我们在chrome的开发者工具下的Network可以看到只请求了一个页面。

![图片3](./images/3.png)

**图片3**

但是当我们点击加载js，你会发现test.js会以动态的方式加入到代码中,同时执行了test函数，使页面的内容发生了变化。

![图片4](./images/4.png)

**图片4**

在`React.lazy`和常用的三方包`react-loadable`，都是使用了这个原理，然后配合webpack进行代码打包拆分达到异步加载，这样首屏渲染的速度将大大的提高。

由于`React.lazy`不支持服务端渲染，所以这时候`react-loadable`就是不错的选择。

## 2、如何使用React.lazy

下面示例代码使用create-react-app脚手架搭建：

```jsx
//OtherComponent.js 文件内容

import React from 'react'
const OtherComponent = ()=>{
  return (
    <div>
      我已加载
    </div>
  )
}
export default OtherComponent

// App.js 文件内容
import React from 'react';
import './App.css';

//使用React.lazy导入OtherComponent组件
const OtherComponent = React.lazy(() => import('./OtherComponent'));
function App() {
  return (
    <div className="App">
      <OtherComponent/>
    </div>
  );
}
export default App;
```

这是最简单的`React.lazy`，但是这样页面会报错。这个报错提示我们，在React使用了`lazy`之后，会存在一个加载中的空档期，React不知道在这个空档期中该显示什么内容，所以需要我们指定。接下来就要使用到`Suspense`。

![图片5](./images/5.png)

**图片5**

### (1) Suspense

如果在 `App` 渲染完成后，包含 `OtherComponent` 的模块还没有被加载完成，我们可以使用加载指示器为此组件做优雅降级。这里我们使用 `Suspense` 组件来解决。

这里将`App`组件改一改

```jsx
import React, { Suspense, Component } from 'react';
import './App.css';

//使用React.lazy导入OtherComponent组件
const OtherComponent = React.lazy(() => import('./OtherComponent'));

export default class App extends Component {
  state = {
    visible: false
  }
  render() {
    return (
      <div className="App">
        <button onClick={() => {
          this.setState({ visible: true })
        }}>
          加载OtherComponent组件
        </button>
        <Suspense fallback={<div>Loading...</div>}>
          {
            this.state.visible
              ?
              <OtherComponent />
              :
              null
          }
        </Suspense>
      </div>
    )
  }
}
```

我们指定了空档期使用Loading展示在界面上面，等`OtherComponent`组件异步加载完毕，把`OtherComponent`组件的内容替换掉Loading上。

![图片6](./images/6.gif)

**图片6**

![图片7](./images/7.png)

**图片7**

为了演示我把chrome网络调到`lower-end mobile`，不然看不到loading出现。

可以从上面图片看出，当点击加载的时候,页面的head会插入``这段代码，发出一个get请求，页面开始显示loading，去请求`2.chunk.js`文件。

请求结束返回内容就是`OtherComponent`组件的内容,只是文件名称和文件内容经过webpack处理过。

> 注意：`Suspense`使用的时候，`fallback`一定是存在且有内容的， 否则会报错。



# 十九、React Hooks

https://zh-hans.reactjs.org/docs/hooks-intro.html

在 React 的世界中，有容器组件和 UI 组件之分，在 React Hooks 出现之前，UI 组件我们可以使用函数，无状态组件来展示 UI，而对于容器组件，函数组件就显得无能为力，我们依赖于类组件来获取数据，处理数据，并向下传递参数给 UI 组件进行渲染。在我看来，使用 React Hooks 相比于从前的类组件有以下几点好处：

1. 代码可读性更强，原本同一块功能的代码逻辑被拆分在了不同的生命周期函数中，容易使开发者不利于维护和迭代，通过 React Hooks 可以将功能代码聚合，方便阅读维护
2. 组件树层级变浅，在原本的代码中，我们经常使用 HOC/render props 等方式来复用组件的状态，增强功能等，无疑增加了组件树层数及渲染，而在 React Hooks 中，这些功能都可以通过强大的自定义的 Hooks 来实现

React 在 v16.8 的版本中推出了 React Hooks 新特性，虽然社区还没有最佳实践如何基于 React Hooks 来打造复杂应用，凭借着阅读社区中大量的关于这方面的文章，下面我将通过十个案例来帮助你认识理解并可以熟练运用 React Hooks 大部分特性。

## 1、useState 保存组件状态

在类组件中，我们使用 `this.state` 来保存组件状态，并对其修改触发组件重新渲染。比如下面这个简单的计数器组件，很好诠释了类组件如何运行：

```jsx
import React from "react";
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
      name: "alife"
    };
  }
  render() {
    const { count } = this.state;
    return (
      <div>
        Count: {count}
        <button onClick={() => this.setState({ count: count + 1 })}>+</button>
        <button onClick={() => this.setState({ count: count - 1 })}>-</button>
      </div>
    );
  }
}
```

一个简单的计数器组件就完成了，而在函数组件中，由于没有 this 这个黑魔法，React 通过 useState 来帮我们保存组件的状态。

```jsx
import { useState } from 'react'
const App = () => {
  // 数组解构的语法让我们在调用 useState 时可以给 state 变量取不同的名字。

  // 定义了初始化状态，修改state的方式是通过 setCount 函数
  // 右侧书写状态的初始值
  const [ count, setCount ] = useState(10)

  // 一个组件可以引入多个 useState
  // const [age, setAge] = useState(42);
  // const [fruit, setFruit] = useState('banana');
  // const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);

  const add = () => {
    setCount((prevCount) => {
      // console.log(prevCount)
      return prevCount + 1
    })
  }
  return (
    <div>
      <h1>useState - 计数器案例</h1>
      <button onClick = {() => {
        setCount(count - 1)
      }}>-</button>
      { count }
      <button onClick = { add }>+</button>
    </div>
  )
}
export default App
```

通过传入 useState 参数后返回一个带有默认状态和改变状态函数的数组。通过传入新状态给函数来改变原本的状态值。**值得注意的是 useState 不帮助你处理状态，相较于 setState 非覆盖式更新状态，useState 覆盖式更新状态，需要开发者自己处理逻辑。(代码如上)**

似乎有个 useState 后，函数组件也可以拥有自己的状态了，但仅仅是这样完全不够。

## 2、useEffect 处理副作用

函数组件能保存状态，但是对于异步请求，副作用的操作还是无能为力，所以 React 提供了 useEffect 来帮助开发者处理函数组件的副作用，

```jsx
import { useState, useEffect } from 'react'
const App = () => {
  const [ count, setCount ] = useState(2)
  const [ proList, setProList ] = useState([])

  // 一直执行，componentDidMount  + componentDidUpdate
  // useEffect(() => {
  //   fetch('http://121.89.205.189/api/pro/list')
  //     .then(res => res.json())
  //     .then(res => {
  //       console.log(res)
  //       setProList(res.data)
  //     })
  // })

  // 只执行1次，相当于 componetDidMount
  useEffect(() => {
    fetch('http://121.89.205.189/api/pro/list')
      .then(res => res.json())
      .then(res => {
        console.log(res)
        setProList(res.data)
      })
    return () => {
      console.log(555) // 没有打印
    }
  }, [])

  const next = () => {
    fetch('http://121.89.205.189/api/pro/list?count=' + count)
      .then(res => res.json())
      .then(res => {
        console.log(res)
        setProList([...proList, ...res.data])
        setCount(count + 1)
      })
  }

  return (
    <div>
      <h1>useEffect - 副作用</h1>
      <button onClick = { next }>下一页</button>
      {
        proList && proList.map(item => (
          <div key= { item.proid }> { item.proname }</div>
        ))
      }
    </div>
  )
}
export default App
```

```jsx
import React, { Component } from 'react';
class Child extends Component {
  state = { 
    list: []
  }
  componentDidUpdate (prevProps) {
    console.log(222)
    console.log(prevProps, this.props)
    // fetch('http://121.89.205.189/api/pro/search?keyword=' + this.props.keyword)
    //   .then(res => res.json())
    //   .then(res => {
    //     console.log(res) // 一直执行
    //     this.setState({
    //       list: res.data
    //     })
    //   })

    // 依赖的数据发生改变才会继续重新执行
    if (prevProps.keyword !== this.props.keyword) {
      fetch('http://121.89.205.189/api/pro/search?keyword=' + this.props.keyword)
        .then(res => res.json())
        .then(res => {
          console.log(res) // 条件执行
          this.setState({
            list: res.data
          })
        })
    }
  }
  componentDidMount () {
    console.log('111')
    fetch('http://121.89.205.189/api/pro/search?keyword=' + this.props.keyword)
      .then(res => res.json())
      .then(res => {
        this.setState({
          list: res.data
        })
      })
  }
  render () {
    const { list } = this.state
    return (
      <>
        {
          list && list.map(item => (
            <div key = { item.proid }>{ item.proname }</div>
          ))
        }
      </>
    )
  }
}
export default class App extends Component {
  state = {
    keyword: '手机'
  }
  render() {
    return (
      <div>
        <input type="text" value={ this.state.keyword } onChange = { (event) => {
          this.setState({
            keyword: event.target.value
          })
        } }/>{ this.state.keyword }
        <Child keyword = { this.state.keyword }/>
      </div>
    );
  }
}

```



```jsx
const { useState, useEffect } = require("react")

const Child = (props) => {
  const [ list, setList ] = useState([])
  // useEffect(() => {
  //   function fetchData () {
  //     fetch('http://121.89.205.189/api/pro/search?keyword=' + props.keyword)
  //       .then(res => res.json())
  //       .then(res => {
  //         console.log(res) // 一直被执行
  //         setList(res.data)
  //       })
  //   }
  //   fetchData()

  //   return () => {
  //     console.log(5555) // 一直被调用
  //   }
  // })
  // useEffect(() => {
  //   function fetchData () {
  //     fetch('http://121.89.205.189/api/pro/search?keyword=' + props.keyword)
  //       .then(res => res.json())
  //       .then(res => {
  //         console.log(res) // 没有再次执行
  //         setList(res.data)
  //       })
  //   }
  //   fetchData()

  //   return () => {
  //     console.log(5555) // 没有执行
  //   }
  // }, [])

  useEffect(() => {
    function fetchData () {
      fetch('http://121.89.205.189/api/pro/search?keyword=' + props.keyword)
        .then(res => res.json())
        .then(res => {
          console.log(res) // 依赖项改变，代码重新执行
          setList(res.data)
        })
    }
    fetchData()

    // componentWillUnmount
    return () => { // 如果组件多次渲染（通常如此），则在执行下一个 effect 之前，上一个 effect 就已被清除
      console.log(5555) // 依赖项改变，代码重新执行
    }
  }, [props.keyword]) // 依赖项
  return (
    <>
      {
        list && list.map(item => (
          <div key = { item.proid }>{ item.proname }</div>
        ))
      }
    </>
  )
}
const App = () => {
  const [ keyword, setKeyword ] = useState('手机')
  return (
    <>
      <input type="text" value={ keyword } onChange = {
        (event) => {
          setKeyword(event.target.value)
        }
      }/>{ keyword }
      <Child keyword = { keyword }/>
    </>
  )
}

export default App
```

我们使用 useEffect 重写了上面的例子，**useEffect 第一个参数接收一个函数，可以用来做一些副作用比如异步请求，修改外部参数等行为，而第二个参数称之为dependencies，是一个数组，如果数组中的值变化才会触发 执行useEffect 第一个参数中的函数。返回值(如果有)则在组件销毁或者调用函数前调用**。

- 基于这个强大 Hooks，我们可以模拟封装出其他生命周期函数，比如 componentDidUpdate 代码十分简单

```source-js
function useUpdate(fn) {
    // useRef 创建一个引用
    const mounting = useRef(true);
    useEffect(() => {
      if (mounting.current) {
        mounting.current = false;
      } else {
        fn();
      }
    });
}
```

现在我们有了 useState 管理状态，useEffect 处理副作用，异步逻辑，学会这两招足以应对大部分类组件的使用场景。

## 3、useContext 减少组件层级

上面介绍了 useState、useEffect 这两个最基本的 API，接下来介绍的 useContext 是 React 帮你封装好的，用来处理多层级传递数据的方式，在以前组件树种，跨层级祖先组件想要给孙子组件传递数据的时候，除了一层层 props 往下透传之外，我们还可以使用 React Context API 来帮我们做这件事，举个简单的例子：

```jsx
import React, { Component } from 'react'
const ThemeContext = React.createContext()
const ColorContext = React.createContext()
ThemeContext.displayName = 'ThemeContext'
ColorContext.displayName = 'ColorContext'

const Third = () => {
  return (
    <>
      <ThemeContext.Consumer>
        {
          (themeVal) => {
            return (
              <div>
                <ColorContext.Consumer>
                  {
                    (colorVal) => {
                      return (
                        <div>
                          theme: { themeVal }
                          <hr />
                          color: { colorVal }
                        </div>
                      )
                    }
                  }
                </ColorContext.Consumer>
              </div>
            )
          }
        }
      </ThemeContext.Consumer>
    </>
  )
}


class Second extends Component {
  render() {
    return (
      <div>
        <Third/>
      </div>
    )
  }
}
export default class App extends Component {
  state = {
    theme: 'dark',
    color: '#f66'
  }
  render() {
    return (
      <ThemeContext.Provider value = { this.state.theme }>
        <ColorContext.Provider value = { this.state.color }>
          <div>
            <button onClick = {
              () => {
                this.setState({
                  theme: this.state.theme === 'dark' ? 'light': 'dark'
                })
              }
            }>切换主题</button>
            <input type="color" value={ this.state.color } onChange = { (e) => {
              this.setState({
                color: e.target.value
              })
            }}/>
            <Second theme = { this.state.theme }/>
          </div>
        </ColorContext.Provider>
      </ThemeContext.Provider>
    )
  }
}

```

通过 React createContext 的语法，在 APP 组件中可以跨组件 传递数据。而在 React Hooks 中，我们可以使用 useContext 进行改造。

`useContext(MyContext)` 只是让你能够*读取* context 的值以及订阅 context 的变化。你仍然需要在上层组件树中使用 `<MyContext.Provider>` 来为下层组件*提供* context。

```jsx
import React, { Component, useContext } from 'react'
const ThemeContext = React.createContext()
const ColorContext = React.createContext()
ThemeContext.displayName = 'ThemeContext'
ColorContext.displayName = 'ColorContext'

const Third = () => {
  const theme = useContext(ThemeContext)
  const color = useContext(ColorContext)
  return (
    <div>
      theme: { theme }
      <hr />
      color: { color }
    </div>
  )
}


class Second extends Component {
  render() {
    return (
      <div>
        <Third/>
      </div>
    )
  }
}
export default class App extends Component {
  state = {
    theme: 'dark',
    color: '#f66'
  }
  render() {
    return (
      <ThemeContext.Provider value = { this.state.theme }>
        <ColorContext.Provider value = { this.state.color }>
          <div>
            <button onClick = {
              () => {
                this.setState({
                  theme: this.state.theme === 'dark' ? 'light': 'dark'
                })
              }
            }>切换主题</button>
            <input type="color" value={ this.state.color } onChange = { (e) => {
              this.setState({
                color: e.target.value
              })
            }}/>
            <Second theme = { this.state.theme }/>
          </div>
        </ColorContext.Provider>
      </ThemeContext.Provider>
    )
  }
}

```

而使用 useContext 则变得十分简洁，可读性更强且不会增加组件树深度。 --- 根据开发者工具查看组件树结构



## 4、useReducer

useReducer 这个 Hooks 在使用上几乎跟 Redux/React-Redux 一模一样，唯一缺少的就是无法使用 redux 提供的中间件。我们将上述的计时器组件改写为 useReducer，

```jsx
const { useReducer } = require("react")

const initialState = {
  count: 10
}
const reducer = (state, action) => {
  switch (action.type) {
    case 'increment':
      return {...state, count: state.count + 1}
    case 'decrement':
      return {...state, count: state.count - 1}
    default:
      return state
  }
}
const App = () => {
  const [ state, dispatch ] = useReducer(reducer, initialState)
  return (
    <>
      <button onClick = {() => dispatch({ type : 'decrement' })} >-</button>
        { state.count }
      <button onClick = {() => dispatch({ type : 'increment' })} >+</button>
    </>
  )
}

export default App
```

> 如果遇到多个组件需要依赖于同一个状态，需要如何做,难点--是要解决 来自不同组件的行为需要变更同一个状态（难度系数太大）---- 以后建议直接使用 redux 传值

```jsx
import { useReducer, createContext, useContext } from 'react'
const MyContext = createContext()
const init = (initialCount) => {
  return {
    count: initialCount | 10
  }
}
const reducer = (state, action) => {
  switch (action.type) {
    case 'increment':
      return {...state, count: state.count + 1}
    case 'decrement':
      return {...state, count: state.count - 1}
    case 'reset':
      return init(action.payload)
    default:
      return new Error()
  }
}

const Child = ({ initialCount }) => {
  const count = useContext(MyContext)
  return (
    <div>
      { count }
    </div>
  )
}
const App = ({ initialCount }) => {
  console.log('initialCount', initialCount)
  const [ state, dispatch ] = useReducer(reducer, initialCount, init)
  console.log(state)
  return (
    <>
      <button onClick = {() => dispatch({ type : 'decrement' })} >-</button>
        { state.count }
      <button onClick = {() => dispatch({ type : 'increment' })} >+</button>
      <button onClick = {() => dispatch({ type : 'reset', payload: initialCount})} >重置</button>
      <hr></hr>
      <MyContext.Provider value={ state.count }>
        <Child/>
      </MyContext.Provider>
      
    </>
  )
}

export default App
```





> 问：redux 以及 useReducer之间的区别
>
> 1.React  在hooks中不使用 `state = initialState` 这一由 Redux 推广开来的参数约定。
>
> 2. https://www.cnblogs.com/lovevin/p/13528376.html



## 5、useCallback 记忆函数

在类组件中，我们经常犯下面这样的错误：

```source-js
class App {
    render() {
        return <div>
            <SomeComponent style={{ fontSize: 14 }} doSomething={ () => { console.log('do something'); }}  />
        </div>;
    }
}
```

这样写有什么坏处呢？一旦 App 组件的 props 或者状态改变了就会触发重渲染，即使跟 SomeComponent 组件不相关，**由于每次 render 都会产生新的 style 和 doSomething（因为重新render前后， style 和 doSomething分别指向了不同的引用）**，所以会导致 SomeComponent 重新渲染，倘若 SomeComponent 是一个大型的组件树，这样的 Virtual Dom 的比较显然是很浪费的，解决的办法也很简单，将参数抽离成变量。

```source-js
const fontSizeStyle = { fontSize: 14 };
class App {
    doSomething = () => {
        console.log('do something');
    }
    render() {
        return <div>
            <SomeComponent style={fontSizeStyle} doSomething={ this.doSomething }  />
        </div>;
    }
}
```

在类组件中，我们还可以通过 this 这个对象来存储函数，而在函数组件中没办法进行挂载了。所以函数组件在每次渲染的时候如果有传递函数的话都会重渲染子组件。

```source-js
function App() {
  const handleClick = () => {
    console.log('Click happened');
  }
  return <SomeComponent onClick={handleClick}>Click Me</SomeComponent>;
}
```

> 这里多说一句，一般把**函数式组件理解为class组件render函数的语法糖**，所以每次重新渲染的时候，函数式组件内部所有的代码都会重新执行一遍。所以上述代码中每次render，handleClick都会是一个新的引用，所以也就是说传递给SomeComponent组件的props.onClick一直在变(因为每次都是一个新的引用)，所以才会说这种情况下，函数组件在每次渲染的时候如果有传递函数的话都会重渲染子组件。

```jsx
import React, { useState, useCallback } from 'react';

const Child = () => {
  console.log('1') // 持续打印

  const handlerClick = () => {
    console.log('child')
  }
  return (
    <div onClick = { handlerClick }> textInComponent </div>
  );
}

const App = () => {
  const [ count, setCount ] = useState(100)

  const addHandler = useCallback(() => {
    setCount(count + 1)
  }, [])
  return (
    <div> 
      <button onClick = { addHandler }>add</button>{ count }
      <Child />
    </div>
  );

}
export default App

```



而有了 useCallback 就不一样了，你可以通过 useCallback 获得一个记忆后的函数。

```source-js
function App() {
  const memoizedHandleClick = useCallback(() => {
    console.log('Click happened')
  }, []); // 空数组代表无论什么情况下该函数都不会发生改变
  return <SomeComponent onClick={memoizedHandleClick}>Click Me</SomeComponent>;
}
```

老规矩，第二个参数传入一个数组，数组中的每一项一旦值或者引用发生改变，useCallback 就会重新返回一个新的记忆函数提供给后面进行渲染。

这样只要子组件继承了 PureComponent 或者使用 React.memo 就可以有效避免不必要的 VDOM 渲染。

## 6、useMemo 记忆组件

useCallback 的功能完全可以由 useMemo 所取代，如果你想通过使用 useMemo 返回一个记忆函数也是完全可以的。

```rust
useCallback(fn, inputs) is equivalent to useMemo(() => fn, inputs).
```

所以前面使用 useCallback 的例子可以使用 useMemo 进行改写：

```source-js
function App() {
  const memoizedHandleClick = useMemo(() => () => {
    console.log('Click happened')
  }, []); // 空数组代表无论什么情况下该函数都不会发生改变
  return <SomeComponent onClick={memoizedHandleClick}>Click Me</SomeComponent>;
}
```

唯一的区别是：**useCallback 不会执行第一个参数函数，而是将它返回给你，而 useMemo 会执行第一个函数并且将函数执行结果返回给你。**所以在前面的例子中，可以返回 handleClick 来达到存储函数的目的。

所以 useCallback 常用记忆事件函数，生成记忆后的事件函数并传递给子组件使用。而 useMemo 更适合经过函数计算得到一个确定的值，比如记忆组件。

```jsx
import { useState, useMemo } from "react"

const Child1 = () => {
  console.log('1111')
  return (
    <div> child1 </div>
  )
}

const Child2 = () => {
  console.log('2222')
  return (
    <div> child2 </div>
  )
}

const App = () => {
  const [ a, setA ] = useState(11)
  const [ b, setB ] = useState(22)

  const addA = () => {
    setA(a + 1)
  }
  const addB = () => {
    setB(b + 1)
  }

  // useMemo 之后  只有依赖项发生改变对应的组件才会重新渲染
  const myChild1 = useMemo(() => <Child1 a = { a }/>, [a])
  const myChild2 = useMemo(() => <Child2 b = { b }/>, [b])

  return (
    <>
      <button onClick = { addA }>add a</button>
      <button onClick = { addB }>add b</button>
      { myChild1 }
      { myChild2 }
    </>
  )
}

export default App
```

当 a/b 改变时，child1/child2 才会重新渲染。从例子可以看出来，只有在第二个参数数组的值发生变化时，才会触发子组件的更新。

## 7、useRef 保存引用值

useRef 跟 createRef 类似，都可以用来生成对 DOM 对象的引用，看个简单的例子：

```jsx
import React, { useState, useRef } from "react";
function App() {
  let [name, setName] = useState("吴大勋");
  let nameRef = useRef();
  const submitButton = () => {
    setName(nameRef.current.value);
  };
  return (
    <div className="App">
      <p>{name}</p>

      <div>
        <input ref={nameRef} type="text" />
        <button type="button" onClick={submitButton}>
          Submit
        </button>
      </div>
    </div>
  );
}
export default App
```

useRef 返回的值传递给组件或者 DOM 的 ref 属性，就可以通过 ref.current 值**访问组件或真实的 DOM 节点，重点是组件也是可以访问到的**，从而可以对 DOM 进行一些操作，比如监听事件等等。

当然 useRef 远比你想象中的功能更加强大，useRef 的功能有点像类属性，或者说您想要在组件中记录一些值，并且这些值在稍后可以更改。

利用 useRef 就可以绕过 Capture Value 的特性。可以认为 ref 在所有 Render 过程中保持着唯一引用，因此所有对 ref 的赋值或取值，拿到的都只有一个最终状态，而不会在每个 Render 间存在隔离。

React Hooks 中存在 Capture Value 的特性：

```jsx
function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    setTimeout(() => {
      alert("count: " + count);
    }, 3000);
  }, [count]);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>增加 count</button>
      <button onClick={() => setCount(count - 1)}>减少 count</button>
    </div>
  );
}
```

先点击增加button，后点击减少button，3秒后先alert 1，后alert 0，而不是alert两次0。这就是所谓的 capture value 的特性。而在**类组件**中 3 秒后输出的就是修改后的值，因为这时候 **message 是挂载在 this 变量上，它保留的是一个引用值**，对 this 属性的访问都会获取到最新的值。讲到这里你应该就明白了，useRef 创建一个引用，就可以有效规避 React Hooks 中 Capture Value 特性。

```jsx
import { useRef } from 'react'
function App() {
  const count = useRef(0); // 引用
  // vue3 

  const showCount = () => {
    alert("count: " + count.current);
  };

  const handleClick = number => {
    count.current = count.current + number;
    console.log(count.current)
    setTimeout(showCount, 3000);
  };

  return (
    <div>
      <p>You clicked {count.current} times</p>
      <button onClick={() => handleClick(1)}>增加 count</button>
      <button onClick={() => handleClick(-1)}>减少 count</button>
    </div>
  );
}

export default App
```

只要将赋值与取值的对象变成 useRef，而不是 useState，就可以躲过 capture value 特性，在 3 秒后得到最新的值。

## 8、useImperativeHandle 透传 Ref

通过 useImperativeHandle 用于让父组件获取子组件内的索引

```jsx
import React, { useRef, useEffect, useImperativeHandle, forwardRef } from "react";
function ChildInputComponent(props, ref) {
  const inputRef = useRef(null);
  useImperativeHandle(ref, () => inputRef.current);
  return <input type="text" name="child input" ref={inputRef} />;
}
const ChildInput = forwardRef(ChildInputComponent);
function App() {
  const inputRef = useRef(null);
  useEffect(() => {
    inputRef.current.focus();
  }, []);
  return (
    <div>
      <ChildInput ref={inputRef} />
    </div>
  );
}
```

通过这种方式，App 组件可以获得子组件的 input 的 DOM 节点。

## 9、useLayoutEffect 同步执行副作用

大部分情况下，使用 useEffect 就可以帮我们处理组件的副作用，但是如果想要同步调用一些副作用，比如对 DOM 的操作，就需要使用 useLayoutEffect，useLayoutEffect 中的副作用会在 DOM 更新之后同步执行。

```source-js
function App() {
  const [width, setWidth] = useState(0);
  useLayoutEffect(() => {
    const title = document.querySelector("#title");
    const titleWidth = title.getBoundingClientRect().width;
    console.log("useLayoutEffect");
    if (width !== titleWidth) {
      setWidth(titleWidth);
    }
  });
  useEffect(() => {
    console.log("useEffect");
  });
  return (
    <div>
      <h1 id="title">hello</h1>
      <h2>{width}</h2>
    </div>
  );
}
```

在上面的例子中，useLayoutEffect 会在 render，DOM 更新之后同步触发函数，会优于 useEffect 异步触发函数。

### (1) useEffect和useLayoutEffect有什么区别？

**简单来说就是调用时机不同，`useLayoutEffect`和原来`componentDidMount`&`componentDidUpdate`一致，在react完成DOM更新后马上**同步**调用的代码，会阻塞页面渲染。而`useEffect`是会在整个页面渲染完才会调用的代码。**

官方建议优先使用`useEffect`

> However, **we recommend starting with useEffect first** and only trying useLayoutEffect if that causes a problem.

在实际使用时如果想避免**页面抖动**（在`useEffect`里修改DOM很有可能出现）的话，可以把需要操作DOM的代码放在`useLayoutEffect`里。关于使用`useEffect`导致页面抖动。

不过`useLayoutEffect`在服务端渲染时会出现一个warning，要消除的话得用`useEffect`代替或者推迟渲染时机。

## 10 自定义hook

```jsx
import { useState, useEffect, useLayoutEffect } from "react"

const App = () => {
  const [ count, setCount ] = useState(10) 
  const add = () => {
    setCount(count + 1)
  }

  // useEffect(() => {
  //   document.title = count + '次'
  //   console.log(1111)
  // })
  useLayoutEffect(() => {
    document.title = count + '次'
  })
  return (
    <div>
      <button onClick = { add }>add</button> { count }
    </div>
  )
}

export default App
```

```jsx
// App.jsx
import useCount from './useCount'
import useTitle from './useTitle'
const App = () => {
  const { count, add } = useCount()
  useTitle(count)

  
  return (
    <div>
      <button onClick = { add }>add</button> { count }
    </div>
  )
}

export default App
```

```jsx
// useTitle.js
import { useLayoutEffect } from 'react' 
const useTitle = (count) => {
  useLayoutEffect(() => {
    document.title = count + '次'
  })
}
export default useTitle
```

```js
// useCount.js
import { useState } from 'react'
const useCount = () => {
  const [ count, setCount ] = useState(10) 
  const add = () => {
    setCount(count + 1)
  }

  return {
    count, 
    add
  }
}

export default useCount

```

自定义hooks库： https://ahooks.js.org/zh-CN

# 二十、Mobx

Mobx是一个功能强大，上手非常容易的状态管理工具。redux的作者也曾经向大家推荐过它，在不少情况下可以使用Mobx来替代掉redux。

![image-20210105091919079](proimg/mobx.png)

这张图来自于官网，把这张图理解清楚了。基本上对于mobx的理解就算入门了。

官网有明确的核心概念使用方法，并配有[egghead](<https://egghead.io/courses/manage-complex-state-in-react-apps-with-mobx>)的视频教程。这里就不一一赘述了。

要特别注意当使用 `mobx-react` 时可以定义一个新的生命周期钩子函数 `componentWillReact`。当组件因为它观察的数据发生了改变，它会安排重新渲染，这个时候 `componentWillReact` 会被触发。这使得它很容易追溯渲染并找到导致渲染的操作(action)。

- `componentWillReact` 不接收参数

- `componentWillReact` 初始化渲染前不会触发 (使用 `componentWillMount` 替代)

- `componentWillReact` 对于 mobx-react@4+, 当接收新的 props 时并在 `setState` 调用后会触发此钩子

- 要触发`componentWillReact`必须在render里面用到被观察的变量

- 使用Mobx之后不会触发`componentWillReceiveProps`




## 1、搭建环境

```bash
npx create-react-app mobx-app
```



## 2、Mobx 入门

### (1) observable可观察的状态

- map

```js
import {observable} from 'mobx'
// 声明
const map = observable.map({a: 1, b: 2});
// 设置
map.set('a', 11);
// 获取
console.log(map.get('a'));
console.log(map.get('b'));
// 删除
map.delete('a');
console.log(map.get('a'));
// 判断是否存在属性
console.log(map.has('a'));
```

- object

```js
import {observable} from 'mobx'
// 声明
const obj = observable({a: 1, b: 2});
// 修改
obj.a = 11;
// 访问
console.log(obj.a, obj.b);
```

- array

```js
import {observable} from 'mobx'
const arr = observable(['a', 'b', 'c', 'd']);
// 访问
console.log(arr[0], arr[10]);
// 操作
arr.pop();
arr.push('e');
```

- 基础类型

```js
import {observable} from 'mobx'/
const num = observable.box(10);
const str = observable.box('hello');
const bool = observable.box(true);
// 获得值
console.log(num.get(), str.get(), bool.get());
// 修改值
num.set(100);
str.set('hi');
bool.set(false);
console.log(num.get(), str.get(), bool.get());
```

### (2) observable装饰器

```js
import {observable} from 'mobx'

// observable这个函数可以识别当成普通函数调用还是装饰器调用
// 如果是装饰器，会自动识别数据类型，使用不同的包装转换方案。
class Store{
  @observable arr = [];
  @observable obj = {a: 1};
  @observable map = new Map();
  @observable str = 'hello';
  @observable num = 123;
  @observable bool = false;
}

const store = new Store();

console.log(store);
console.log(store.obj.a);
```

注意：vscode编译器中，js文件使用装饰器会报红。解决方式：

在根目录编写jsconfig.json

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es6",
    "experimentalDecorators": true
  },
  "include": ["src/**/*"]
}
```

### (3) 对 observables 作出响应

- 基础代码：

```js
import {observable} from 'mobx'
class Store{
  @observable arr = [];
  @observable obj = {a: 1};
  @observable map = new Map();
  @observable str = 'hello';
  @observable num = 123;
  @observable bool = false;
}
const store = new Store();
```

- computed

计算值是可以根据现有的状态或其它计算值衍生出的值, 跟vue中的computed非常相似。

```js
const result = computed(()=>store.str + store.num);
console.log(result.get());
// 监听数据的变化
result.observe((change)=>{
  console.log('result:', change);
})
//两次对store属性的修改都会引起result的变化
store.str = 'world';
store.num = 220;
```

computed可作为装饰器， 将result的计算添加到类中：

```js
class Store{
  @observable arr = [];
  @observable obj = {a: 1};
  @observable map = new Map();

  @observable str = 'hello';
  @observable num = 123;
  @observable bool = false;

  @computed get result(){
    return this.str + this.num;
  }  
}
```

- autorun

当你想创建一个响应式函数，而该函数本身永远不会有观察者时,可以使用 `mobx.autorun`

所提供的函数总是立即被触发一次，然后每次它的依赖关系改变时会再次被触发。

经验法则：如果你有一个函数应该自动运行，但不会产生一个新的值，请使用`autorun`。 其余情况都应该使用 `computed`。

```js
//aotu会立即触发一次
autorun(()=>{
  console.log(store.str + store.num);
})

autorun(()=>{
  console.log(store.result);
})
//两次修改都会引起autorun执行
store.num = 220;
store.str = 'world';
```

- when

```
when(predicate: () => boolean, effect?: () => void, options?)
```

`when` 观察并运行给定的 `predicate`，直到返回true。 一旦返回 true，给定的 `effect` 就会被执行，然后 autorunner(自动运行程序) 会被清理。 该函数返回一个清理器以提前取消自动运行程序。

对于以响应式方式来进行处理或者取消，此函数非常有用。

```js
when(()=>store.bool, ()=>{
  console.log('when function run.....');
})
store.bool = true;
```

- reaction

用法: `reaction(() => data, (data, reaction) => { sideEffect }, options?)`。

`autorun` 的变种，对于如何追踪 observable 赋予了更细粒度的控制。 它接收两个函数参数，第一个(*数据* 函数)是用来追踪并返回数据作为第二个函数(*效果* 函数)的输入。 不同于 `autorun` 的是当创建时*效果* 函数不会直接运行，只有在数据表达式首次返回一个新值后才会运行。 在执行 *效果* 函数时访问的任何 observable 都不会被追踪。

```js
// reaction
reaction(()=>[store.str, store.num], (arr)=>{
  console.log(arr.join('/'));
})
//只要[store.str, store.num]中任意一值发生变化，reaction第二个函数都会执行
store.num = 220;
store.str = 'world';
```

### (4) 改变 observables状态

- action

接上面案例，添加action到类中：

```js
class Store{
  @observable arr = [];
  @observable obj = {a: 1};
  @observable map = new Map();

  @observable str = 'hello';
  @observable num = 123;
  @observable bool = false;

  @computed get result(){
    return this.str + this.num;
  }

  @action bar(){
    this.str = 'world';
    this.num = 40;
  }
}
const store = new Store();

//调用action，只会执行一次
store.bar();
```

- action.bound

`action.bound` 可以用来自动地将动作绑定到目标对象。

```js
class Store{
  @observable arr = [];
  @observable obj = {a: 1};
  @observable map = new Map();

  @observable str = 'hello';
  @observable num = 123;
  @observable bool = false;

  @computed get result(){
    return this.str + this.num;
  }

  @action bar(){
    this.str = 'world';
    this.num = 40;
  }

  //this 永远都是正确的
  @action.bound foo(){
    this.str = 'world';
    this.num = 40;
  }
}

const store = new Store();
setInterval(store.foo, 1000)
```

- runInAction

`action` 只能影响正在运行的函数，而无法影响当前函数调用的异步操作。如果你使用async function来处理业务，那么我们可以使用 `runInAction` 这个API来解决这个问题。

```js
@action async fzz() {
  await new Promise((resolve) => { 
    setTimeout(() => {
      resolve({
        num: 220,
        str: 'world'
      })
    }, 1000) 
  })
  runInAction(()=>{
    store.num = 220
    store.str = 'world'
  })    
}
```

## 3、应用

### (1) 在react中使用mobx

```
cnpm i mobx mobx-react -S

```

```js
// src/store/modules/home.js
import { makeAutoObservable } from 'mobx'
class HomeStore {
  proList = []
  constructor (store) {
    makeAutoObservable(this)
    this.store = store
  }
  get len () { // 计算属性
    return this.proList.length
  }
  changeProList (payload) {
    console.log('6666', this.store.kind)
    this.proList = payload
  }
}
export default HomeStore
```

```js
// src/store/modules/kind.js
import { makeAutoObservable } from 'mobx'
class KindStore {
  categoryList = []
  constructor (store) {
    makeAutoObservable(this)
    this.store = store // 拿到全局的store
  }
  getCategoryListAction () {
    fetch('http://121.89.205.189/api/pro/categorylist')
      .then(res => res.json())
      .then(res => {
        this.changeCategoryList(res.data)
      })
  }
  changeCategoryList (payload) {
    this.categoryList = payload
  }
}
export default KindStore
```

```js
// src/store/index.js
import { makeAutoObservable } from 'mobx'
import HomeStore from "./modules/home"
import KindStore from "./modules/kind"

class Store {
  constructor () {
    makeAutoObservable(this) //将当前的这个类中的 所有的属性以及方法都被 当作是 可被观察变化的数据
    // 分模块处理
    // 传入的this 实际上是  整个store的实例，方便在模块内部访问全局的store
    this.home = new HomeStore(this)
    this.kind = new KindStore(this)
  }
}

export default new Store()
```

```js
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';
import { Provider } from 'mobx-react'
import store from './store'

ReactDOM.render(
  <React.StrictMode>
    <Provider store = { store }>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root')
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```

```js
// src/App.js
import React from 'react';
import Home from './views/Home'
import Kind from './views/Kind'

const App = (props) => {
  return (
    <div>
      <Home />
      <hr />
      <Kind />
    </div>
  )
}

export default App;
```

```js
// src/views/Home.js
import React, { useEffect } from 'react';
import { observer, inject } from 'mobx-react'

const App = (props) => {
  console.log('home', props)
  useEffect(() => {
    fetch('http://121.89.205.189/api/pro/list')
      .then(res => res.json())
      .then(res => {
        props.store.home.changeProList(res.data)
      })
  }, [props.store.home])
  return (
    <div>
      { props.store.home.len }
      { 
        props.store.home.proList && props.store.home.proList.map(item => {
          return (
            <div key = { item.proid }> {item.proname } </div>
          )
        })
      }
      {
        props.store.kind.categoryList && props.store.kind.categoryList.map((item, index) => {
          return (
            <div key = { index }>{ item }</div>
          )
        })
      }
    </div>
  )
}

export default inject('store')(observer(App)); // +++++++++++
```

```js
// src/views/Kind.js
import React, { useEffect } from 'react';
import { observer, inject } from 'mobx-react'

const App = (props) => {
  console.log('kind', props)
  useEffect(() => {
    props.store.kind.getCategoryListAction()
  }, [props.store.kind])
  return (
    <div>
      Kind
      {
        props.store.kind.categoryList && props.store.kind.categoryList.map((item, index) => {
          return (
            <div key = { index }>{ item }</div>
          )
        })
      }
    </div>
  )
}

export default inject('store')(observer(App));
```






# 附加

## 一、create-react-app 支持decorators

```shell
yarn add @babel/core @babel/plugin-proposal-decorators @babel/preset-env
```



**创建 .babelrc**

```json
{
    "presets": [
        "@babel/preset-env"
    ],
    "plugins": [
        [
            "@babel/plugin-proposal-decorators",
            {
                "legacy": true
            }
        ]
    ]
}
```

**创建config-overrides.js**

```javascript
const path = require('path')
const { override, addDecoratorsLegacy } = require('customize-cra')

function resolve(dir) {
    return path.join(__dirname, dir)
}

const customize = () => (config, env) => {
    config.resolve.alias['@'] = resolve('src')
    if (env === 'production') {
        config.externals = {
            'react': 'React',
            'react-dom': 'ReactDOM'
        }
    }

    return config
};


module.exports = override(addDecoratorsLegacy(), customize())
```

**安装依赖**

```
yarn add customize-cra react-app-rewired
```

**修改package.json**

```
...
"scripts": {
    "start": "react-app-rewired start",
    "build": "react-app-rewired build",
    "test": "react-app-rewired test",
    "eject": "react-app-rewired eject"
  },
...
```

# react重要版本的说明

## 16.8

2019年2月份更新，react16.8新增了 hooks api，让函数式组件拥有了状态

## 17

2020年10月更新

15.5

自 React v15.5 起，`React.PropTypes` 已移入另一个包中。请使用 [`prop-types` 库](https://www.npmjs.com/package/prop-types) 代替

1. 
