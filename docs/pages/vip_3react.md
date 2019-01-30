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