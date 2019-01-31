# react 教程

## react 一个UI框架

`React`用于构建用户界面的 `JavaScript` 库.

### react 特点

- mvvm
- 声明式
- 组件化
- 生态强大

## 学习前置基础

- [html\css\js](https://qtxh.ke.qq.com/?tuin=1eb4a0a4)免费视频教程
- [es6+](https://ke.qq.com/course/318503?tuin=1eb4a0a4)
- [webpack教程](https://ke.qq.com/course/321174?tuin=1eb4a0a4)
- [nodejs基础|sass|ajax|...](https://ke.qq.com/course/294595?tuin=1eb4a0a4)

## 快速入门

快速构建一个React的前端项目最好的就是用脚手架快速生成一个项目架构目录，并做好基础的配置。建议使用[`Create React App`](https://github.com/facebook/create-react-app)。

### 安装`Create React App`

```sh
# 建议全局安装
$ npm install -g create-react-app

# yarn
$ yarn global add create-react-app
```

测试是否安装成功：

```sh
$ create-react-app -V
2.1.3
```

### 快速初始化一个react项目

```sh
npx create-react-app myapp
cd myapp
npm start
```

![](https://camo.githubusercontent.com/29765c4a32f03bd01d44edef1cd674225e3c906b/68747470733a2f2f63646e2e7261776769742e636f6d2f66616365626f6f6b2f6372656174652d72656163742d6170702f323762343261632f73637265656e636173742e737667)

此时打开`http://localhost:3000/`就能看到基本的一个简单的web页面。

### 释放webpack的配置文件

由于create-react-app脚手架生成的项目所有的配置都内置在代码中，我们看不到webpack配置的细节，需要通过一个命令，把所有配置都显示的展现在项目中。

```sh
npm run eject
```

> 除非您对webpack已经非常熟悉，请不要这么操作！

### 其他的构建辅助脚本

```sh
# 构建项目
npm run build

yarn build

# 运行测试
npm run test
yarn test

# 另外一种构建方式
# required npm 6.0+
npm init react-app my-app

yarn create react-app my-app

```

## HelloWorld

项目的默认目录：

```sh
├── package.json
├── public                  # 这个是webpack的配置的静态目录
│   ├── favicon.ico
│   ├── index.html          # 默认是单页面应用，这个是最终的html的基础模板
│   └── manifest.json
├── src
│   ├── App.css             # App根组件的css
│   ├── App.js              # App组件代码
│   ├── App.test.js
│   ├── index.css           # 启动文件样式
│   ├── index.js            # 启动的文件（开始执行的入口）！！！！
│   ├── logo.svg
│   └── serviceWorker.js
└── yarn.lock

```

`index.js`就是项目启动启动的入口。

```js
import React from 'react';                           // 引入react核心库（必须）
import ReactDOM from 'react-dom';                    // 引入react在浏览器上运行的需要的支持库
import './index.css';
import App from './App';                             // 引入App组件
import * as serviceWorker from './serviceWorker';    // 注册serviceWork

// 此行代码的意思：把App组件的内容经过Ract的编译生成最终的html挂载到 root的dom节点生。
ReactDOM.render(<App />, document.getElementById('root'));  // !!!核心代码

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: http://bit.ly/CRA-PWA
serviceWorker.unregister();
```

核心代码就是下面一行：把App组件的内容经过Ract的编译生成最终的html挂载到 root的dom节点生。

`ReactDOM.render(<App />, document.getElementById('root'));  // !!!核心代码`

那么我们看一下App组件的代码：

```js
import React, { Component } from 'react';   // 引入react的组件根对象
import logo from './logo.svg';
import './App.css';

class App extends Component {              // 创建App组件类型，继承Component
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <p>
            Edit <code>src/App.js</code> and save to reload.
          </p>
          <a
            className="App-link"
            href="https://reactjs.org"
            target="_blank"
            rel="noopener noreferrer"
          >
            Learn React
          </a>
        </header>
      </div>
    );
  }
}
export default App;
```

> 您需要有[es6](https://ke.qq.com/course/318503?tuin=1eb4a0a4)的语法的基础。

在App.js中就做了以下几件事：

- 引入React库
- 定义App类型（继承自React.Component)
- 在App类中定义render方法
- 在render方法中返回要渲染的html（jsx语法）

然后我们修改如下App.js为：

```js
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h1>Hi, aicoder.com</h1>
      </div>
    );
  }
}

export default App;
```

此时页面会自动刷新为：

```sh
Hi, aicoder.com
```

## JSX语法

JSX， 一种 JavaScript 的语法扩展。JSX 用来声明 React 当中的元素。

比如定义一个变量：

```js
// jsx语法是js和html的组合，本质还是js，最终会编译成js
const element = <h1>Hello, world!</h1>;
```

JSX 的基本语法规则：遇到 HTML 标签（以 < 开头），就用 HTML 规则解析；遇到代码块（以 { 开头），就用 JavaScript 规则解析。

### JSX中使用表达式

如果JSX中的代码超过一行，我们一般用一个()进行分组处理，遇到html一般都会单独写在一个新行。

```js
const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);
```

再比如：

```js
// 用{}可以直接展示数据内容个，类似es6模板字符串中的 ${}
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {user}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### JSX 属性与{}

你可以使用引号来定义以字符串为值的属性：

```js
const element = <div tabIndex="0"></div>;
```

也可以使用大括号来定义以 JavaScript 表达式为值的属性：

```js
const element = <img src={user.avatarUrl} />;
```

### JSX 防注入攻击

你可以放心地在 JSX 当中使用用户输入：

```js
const title = <span>你好！</span>;
// 直接使用是安全的：
const element = <h1>{title}</h1>;
```

React DOM 在渲染之前默认会 过滤 所有传入的值。它可以确保你的应用不会被注入攻击。所有的内容在渲染之前都被转换成了字符串。这样可以有效地防止 XSS(跨站脚本) 攻击。

### 数组的展示

变量是一个数组，则会展开这个数组的所有成员。

```js
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    var arr = [
      <h1>hi, aicoder.com</h1>,
      <h2>React is awesome</h2>,
    ];
    return (
      <div className="App">
        {arr}
      </div>
    );
  }
}
export default App;
```

最终结果：

```sh
hi, aicoder.com
React is awesome
```

### 数组map输出一个列表

App.js如下：

```js
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    var arr = ['aicoder.com', 'hamkd.com']
    return (
      <div className="App">
        <h1>aicoder.com</h1>
        <ul>
          {
            arr.map((item, index) =>
              <li>{ index +1 } - { item }</li>
            )
          }
        </ul>
      </div>
    );
  }
}
export default App;

```

最终结果：

```sh
aicoder.com
1 - aicoder.com
2 - hamkd.com
```

### JSX的最终归宿

JSX 本质会被编译成JS，Babel 转译器会把 JSX 转换成一个名为 `React.createElement()` 的方法调用。

下面两种代码的作用是完全相同的：

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

```js
React.createElement() 这个方法首先会进行一些避免bug的检查，之后会返回一个类似下面例子的对象：

// 注意: 以下示例是简化过的（不代表在 React 源码中是这样）
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world'
  }
};
```

这样的对象被称为 “React 元素”。它代表所有你在屏幕上看到的东西。React 通过读取这些对象来构建 DOM 并保持数据内容一致。

## React组件

组件可以将UI切分成一些独立的、可复用的部件，这样你就只需专注于构建每一个单独的部件。

组件从概念上看就像是函数，它可以接收任意的输入值（称之为“props”），并返回一个需要在页面上展示的React元素。

### 函数定义/类定义组件

定义一个组件最简单的方式是使用JavaScript函数：

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

该函数是一个有效的React组件，它接收一个单一的“props”对象并返回了一个React元素。我们之所以称这种类型的组件为函数定义组件，是因为从字面上来看，它就是一个JavaScript函数。

你也可以使用 [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) 来定义一个组件:

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

上面两个组件在React中是相同的。

### 组件渲染

在前面，我们遇到的React元素都只是DOM标签：

```js
const element = <div />;
```

然而，React元素也可以是用户自定义的组件：

```js
const element = <Welcome name="Sara" />;
```

当React遇到的元素是用户自定义的组件，它会将JSX属性作为单个对象传递给该组件，这个对象称之为“props”。

例如,这段代码会在页面上渲染出"Hello,Sara":

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// ...
class App extends Component {
  render() {
    return (
      <div className="App">
        <Welcome name="Sara" />
      </div>
    );
  }
}
// ...
```

我们来回顾一下在这个例子中发生了什么：

1. 我们对`<Welcome name="Sara" />`元素调用了`ReactDOM.render()`方法。
2. React将`{name: 'Sara'}`作为props传入并调用`Welcome`组件。
3. `Welcome`组件将`<h1>Hello, Sara</h1>`元素作为结果返回。
4. 将DOM更新为`<h1>Hello, Sara</h1>`。

>**警告:**
>
>组件名称必须以大写字母开头。
>
>例如，`<div />` 表示一个DOM标签，但 `<Welcome />` 表示一个组件，并且在使用该组件时你必须定义或引入它。

### 组合组件

组件可以在它的输出中引用其它组件，这就可以让我们用同一组件来抽象出任意层次的细节。在React应用中，按钮、表单、对话框、整个屏幕的内容等，这些通常都被表示为组件。

例如，我们可以创建一个`App`组件，用来多次渲染`Welcome`组件：

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
```

通常，一个新的React应用程序的顶部是一个`App`组件。但是，如果要将React集成到现有应用程序中，则可以从下而上使用像`Button`这样的小组件作为开始，并逐渐运用到视图层的顶部。

>**警告:**
>
>组件的返回值只能有一个根元素。这也是我们要用一个`<div>`来包裹所有`<Welcome />`元素的原因。

### 提取组件

你可以将组件切分为更小的组件，这没什么好担心的。

例如，来看看这个`Comment`组件：

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

这个组件接收`author`(对象)、`text`(字符串)、以及`date`(Date对象)作为props，用来描述一个社交媒体网站上的评论。

这个组件由于嵌套，变得难以被修改，可复用的部分也难以被复用。所以让我们从这个组件中提取出一些小组件。

首先，我们来提取`Avatar`组件：

```js
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

`Avatar`作为`Comment`的内部组件，不需要知道是否被渲染。因此我们将`author`改为一个更通用的名字`user`。

我们建议从组件自身的角度来命名props，而不是根据使用组件的上下文命名。

现在我们可以对`Comment`组件做一些小小的调整：

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

提取组件一开始看起来像是一项单调乏味的工作，但是在大型应用中，构建可复用的组件完全是值得的。当你的UI中有一部分重复使用了好几次（比如，`Button`、`Panel`、`Avatar`），或者其自身就足够复杂（比如，`App`、`FeedStory`、`Comment`），类似这些都是抽象成一个可复用组件的绝佳选择，这也是一个比较好的做法。

### Props的只读性

无论是使用[函数或是类](#函数定义/类定义组件)来声明一个组件，它决不能修改它自己的props。来看这个`sum`函数：

```js
function sum(a, b) {
  return a + b;
}
```

类似于上面的这种函数称为[“纯函数”](https://en.wikipedia.org/wiki/Pure_function)，它没有改变它自己的输入值，当传入的值相同时，总是会返回相同的结果。

与之相对的是非纯函数，它会改变它自身的输入值：

```js
function withdraw(account, amount) {
  account.total -= amount;
}
```

React是非常灵活的，但它也有一个严格的规则：

**所有的React组件必须像纯函数那样使用它们的props。**

当然，应用的界面是随时间动态变化的，后面会介绍一种称为“state”的新概念，State可以在不违反上述规则的情况下，根据用户操作、网络响应、或者其他状态变化，使组件动态的响应并改变组件的输出。

## 组件的声明周期

React组件的生命周期图解

![React 生命周期图解](https://upload-images.jianshu.io/upload_images/4118241-d979d05af0b7d4db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/488/format/webp)

组件会随着组件的props和state改变而发生变化，它的DOM也会有相应的变化。一个组件就是一个状态机：对于特定的输入，它总会返回一致的输出。

React组件提供了生命周期的钩子函数去响应组件不同时刻的状态，组件的生命周期如下：

- 实例化阶段
- 存在期阶段
- 销毁期阶段

![react 生命周期](https://segmentfault.com/img/bVCOam?w=800&h=652)

### 实例化阶段

首次调用组件时，实例化阶段有以下方法会被调用（注意顺序，从上到下先后执行）：

#### `getDefaultProps`

这个方法是用来设置组件默认的props，组件生命周期只会调用一次。但是只适合`React.createClass`直接创建的组件，使用ES6/ES7创建的这个方法不可使用，ES6/ES7可以使用下面方式：

```js
//es7
class Component {
  static defaultProps = {}
}
```

#### `getInitialState`

设置state初始值，在这个方法中你已经可以访问到this.props。getDefaultProps只适合React.createClass使用。使用ES6初始化state方法如下：

```js
class Component extends React.Component {
  constructor(props){
    super(props);
    this.state = {
      render: true,
    }
  }
}
// 或者这样, es6 stage3

class Component extends React.Component{
  state = {
    render: true
  }
  render(){return false;}
}
```

#### `componentWillMount`

此方法会在组件首次渲染之前调用，这个是在render方法调用前可修改state的最后一次机会。这个方法很少用到。

#### `render`

这个方法以后大家都应该会很熟悉，JSX通过这里，解析成对应的虚拟DOM，渲染成最终效果。格式大致如下：

```js
class Component extends React.Component{
  render(){
    return (
       <div></div>
    )
  }
}
```

#### `componentDidMount`

这个方法在首次真实的DOM渲染后调用（仅此一次）当我们需要访问真实的DOM时，这个方法就经常用到。如何访问真实的DOM这里就不想说了。当我们需要请求外部接口数据，一般都在这里处理。

### 存在期

实例化后，当props或者state发生变化时，下面方法依次被调用：

#### `componentWillReceiveProps`

没当我们通过父组件更新子组件props时（这个也是唯一途径），这个方法就会被调用。

`componentWillReceiveProps(nextProps){}`

#### `shouldComponentUpdate`

字面意思，是否应该更新组件，默认返回true。当返回false时，后期函数就不会调用，组件不会在次渲染。

`shouldComponentUpdate(nextProps,nextState){}`

#### `componentWillUpdate`

字面意思组件将会更新，props和state改变后必调用。

#### `render`

跟实例化时的render一样，不多说

#### `componentDidUpdate`

这个方法在更新真实的DOM成功后调用，当我们需要访问真实的DOM时，这个方法就也经常用到。

### 销毁期

销毁阶段，只有一个函数被调用：

#### `componentWillUnmount`

没当组件使用完成，这个组件就必须从DOM中销毁，此时该方法就会被调用。当我们在组件中使用了setInterval，那我们就需要在这个方法中调用clearTimeout。

参考：[React组件生命周期](https://segmentfault.com/a/1190000006792687)

## 将函数转换为类

你可以通过5个步骤将函数组件 `Clock` 转换为类

1. 创建一个名称扩展为 `React.Component` 的[ES6 类](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

2. 创建一个叫做`render()`的空方法

3. 将函数体移动到 `render()` 方法中

4. 在 `render()` 方法中，使用 `this.props` 替换 `props`

5. 删除剩余的空函数声明

```js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

[在 CodePen 中尝试](http://codepen.io/gaearon/pen/zKRGpo?editors=0010)

`Clock` 现在被定义为一个类而不只是一个函数

使用类就允许我们使用其它特性，例如局部状态、生命周期钩子

## 为一个类添加局部状态

我们会通过3个步骤将 `date` 从属性移动到状态中：

1) 在 `render()` 方法中使用 `this.state.date` 替代 `this.props.date`

```js{6}
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

2) 添加一个[类构造函数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor)来初始化状态 `this.state`

```js{4}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

注意我们如何传递 `props` 到基础构造函数的：

```js{2}
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
```

类组件应始终使用`props`调用基础构造函数。

3) 从 `<Clock />` 元素移除 `date` 属性：

```js{2}
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

稍后将定时器代码添加回组件本身。

结果如下：

```js{2-5,11,18}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[在 CodePen 中尝试](http://codepen.io/gaearon/pen/KgQpJd?editors=0010)

接下来，我们将使`Clock`设置自己的计时器并每秒更新一次。

## 将生命周期方法添加到类中

在具有许多组件的应用程序中，在销毁时释放组件所占用的资源非常重要。

每当`Clock`组件第一次加载到DOM中的时候，我们都想[生成定时器](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval)，这在React中被称为`挂载`

同样，每当`Clock`生成的这个DOM被移除的时候，我们也会想要[清除定时器](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/clearInterval)，这在React中被称为`卸载`。


我们可以在组件类上声明特殊的方法，当组件挂载或卸载时，来运行一些代码：

```js{7-9,11-13}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  }

  componentWillUnmount() {

  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

这些方法被称作`生命周期钩子`。

当组件输出到 DOM 后会执行 `componentDidMount()` 钩子，这是一个建立定时器的好地方：

```js{2-5}
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```

注意我们是将定时器ID保存在 `this` 中的。

尽管 `this.props` 是由React本身安装的以及`this.state` 有特殊的含义，如果你需要存储的东西不在数据流中，你可以随意手动向类中添加其他字段（比如定时器ID）。

我们将在 `componentWillUnmount()`生命周期钩子中卸载计时器：

```js{2}
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
```

最后，我们实现了每秒钟执行的 `tick()` 方法。

它将使用 `this.setState()` 来更新组件局部状态：

```js{18-22}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[在 CodePen 中尝试](http://codepen.io/gaearon/pen/amqdNA?editors=0010)

现在时钟每秒钟都会执行。

让我们快速回顾一下发生了什么以及调用方法的顺序：

1) 当 `<Clock />` 被传递给 `ReactDOM.render()` 时，React 调用 `Clock` 组件的构造函数。 由于 `Clock` 需要显示当前时间，所以使用包含当前时间的对象来初始化 `this.state` 。 我们稍后会更新此状态。

2) React 然后调用 `Clock` 组件的 `render()` 方法。这是 React 了解屏幕上应该显示什么内容，然后 React 更新 DOM 以匹配 `Clock` 的渲染输出。

3) 当 `Clock` 的输出插入到 DOM 中时，React 调用 `componentDidMount()` 生命周期钩子。 在其中，`Clock` 组件要求浏览器设置一个定时器，每秒钟调用一次 `tick()`。

4) 浏览器每秒钟调用 `tick()` 方法。 在其中，`Clock` 组件通过使用包含当前时间的对象调用 `setState()` 来调度UI更新。 通过调用 `setState()` ，React 知道状态已经改变，并再次调用 `render()` 方法来确定屏幕上应当显示什么。 这一次，`render()` 方法中的 `this.state.date` 将不同，所以渲染输出将包含更新的时间，并相应地更新DOM。

5) 一旦`Clock`组件被从DOM中移除，React会调用`componentWillUnmount()`这个钩子函数，定时器也就会被清除。

## 正确地使用状态

关于 `setState()` 这里有三件事情需要知道

### 不要直接更新状态

例如，此代码不会重新渲染组件：

```js
// Wrong
this.state.comment = 'Hello';
```

应当使用 `setState()`:

```js
// Correct
this.setState({comment: 'Hello'});
```

构造函数是唯一能够初始化 `this.state` 的地方。

### 状态更新可能是异步的

React 可以将多个`setState()` 调用合并成一个调用来提高性能。

因为 `this.props` 和 `this.state` 可能是异步更新的，你不应该依靠它们的值来计算下一个状态。

例如，此代码可能无法更新计数器：

```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

要修复它，请使用第二种形式的 `setState()` 来接受一个函数而不是一个对象。 该函数将接收先前的状态作为第一个参数，将此次更新被应用时的props做为第二个参数：

```js
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
```

上方代码使用了[箭头函数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，但它也适用于常规函数：

```js
// Correct
this.setState(function(prevState, props) {
  return {
    counter: prevState.counter + props.increment
  };
});
```

### 状态更新合并

当你调用 `setState()` 时，React 将你提供的对象合并到当前状态。

例如，你的状态可能包含一些独立的变量：

```js{4,5}
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

你可以调用 `setState()` 独立地更新它们：

```js{4,10}
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

这里的合并是浅合并，也就是说`this.setState({comments})`完整保留了`this.state.posts`，但完全替换了`this.state.comments`。

## 数据自顶向下流动

父组件或子组件都不能知道某个组件是有状态还是无状态，并且它们不应该关心某组件是被定义为一个函数还是一个类。

这就是为什么状态通常被称为局部或封装。 除了拥有并设置它的组件外，其它组件不可访问。

组件可以选择将其状态作为属性传递给其子组件：

```js
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
```

这也适用于用户定义的组件：

```js
<FormattedDate date={this.state.date} />
```

`FormattedDate` 组件将在其属性中接收到 `date` 值，并且不知道它是来自 `Clock` 状态、还是来自 `Clock` 的属性、亦或手工输入：

```js
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

[在 CodePen 中尝试](http://codepen.io/gaearon/pen/zKRqNB?editors=0010)

这通常被称为`自顶向下`或`单向`数据流。 任何状态始终由某些特定组件所有，并且从该状态导出的任何数据或 UI 只能影响树中`下方`的组件。

如果你想象一个组件树作为属性的瀑布，每个组件的状态就像一个额外的水源，它连接在一个任意点，但也流下来。

为了表明所有组件都是真正隔离的，我们可以创建一个 `App` 组件，它渲染三个`Clock`：

```js{4-6}
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[在 CodePen 中尝试](http://codepen.io/gaearon/pen/vXdGmd?editors=0010)

每个 `Clock` 建立自己的定时器并且独立更新。

在 React 应用程序中，组件是有状态还是无状态被认为是可能随时间而变化的组件的实现细节。 可以在有状态组件中使用无状态组件，反之亦然。
