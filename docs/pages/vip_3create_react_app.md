# create-react-app入门教程

`Create React App`是FaceBook的React团队官方出的一个构建`React`单页面应用的脚手架工具。它本身集成了`Webpack`，并配置了一系列内置的`loader`和默认的npm的脚本，可以很轻松的实现零配置就可以快速开发React的应用。

## Quick Start（快如入门）

### 全局安装

首先确保你电脑上安装最新的

```sh
# 全局安装
npm install -g create-react-app
# 构建一个my-app的项目
npx create-react-app my-app
cd my-app

# 启动编译当前的React项目，并自动打开 http://localhost:3000/
npm start
```

以上命令执行完成后，则自动打开： ` http://localhost:3000/ `

> 如果你不能确保最新版本，可以先尝试卸载： `npm uninstall -g create-react-app`,然后再全局安装。

### 构建React项目的其他方式

- npm

```sh
# npm init <initializer> is available in npm 6+
npm init react-app my-app
```

- Yarn

```sh
# yarn create is available in Yarn 0.25+
yarn create react-app my-app
```

### 项目目录

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

### 默认的npm的脚本

启动开发

```sh
npm start
# or
yarn start
```

启动测试

```sh
npm test
#or
yarn test
```

构建生产版本

```sh
npm run build
#or
yarn build
```

解压默认的webpack配置到配置文件中

```sh
npm run eject
```

## 启用`sass`

> react-scripts@2.0.0 以上版本才适用。

### 安装依赖

要使用Sass必须首先安装`node-sass`

```sh
$ npm install node-sass --save
$ # or
$ yarn add node-sass
```

安装完之后，我们就可以直接把原来的CSS文件后缀直接改为 `.scss` 或者`.sass`,然后组件中导入的文件不再是 css文件而给我scss文件即可。

```js
import React, {Component} from 'react';
import store from './Store/Index';
import {AddNumCreator, MinusNumCreator} from './Store/ActionCreaters';
import './App.scss';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <h1>潇洒</h1>
        </header>
      </div>
    );
  }
}

export default App;
```

### 在sass文件中引入其他sass文件

- 引入src中的scss文件 `@import 'styles/_colors.scss'; `

- 引入`node_modules`中的样式： `@import '~nprogress/nprogress';`

> ~ 就代表： node_modules

## CSS Modules支持

导入CSS文件或者Sass文件的时候，可以用一个变量接收一下返回值。那么就可以直接通过它来访问CSS或者Sass中的内部样式类了。比如：

- Button.module.css

```css
.error {
  background-color: red;
}
```

- Button.js

```js
import React, { Component } from 'react';
import styles from './Button.module.css'; // Import css modules stylesheet as styles

class Button extends Component {
  render() {
    // reference as a js object
    return <button className={styles.error}>Error Button</button>;
  }
}
```

结果：

```html
<!-- This button has red background -->
<button class="Button_error_ax7yz">Error Button</button>
```

了解更多关于[CSS模块化](https://css-tricks.com/css-modules-part-1-need/)

## 公共目录

公共目录里面的内容不会被webpack进行处理，仅仅会拷贝到最终生成的项目的根目录下。

### HTML模板修改

在`public`目录中有个`index.html`是单页面应用的基本模板，所有react生成的代码都会注入到此HTML中。所以此处可以添加一些cdn脚本或者全局的html。

### 添加全局的资源（图片、字体、svg、视频等）

在公共目录下，你可以放字体文件、图片、svg等文件，访问这些文件最好添加 `%PUBLIC_URL%`作为根目录。

`<link rel="shortcut icon" href="%PUBLIC_URL%/favicon.ico">`

## 环境变量

巧妙的使用环境变量可以帮我们在项目中区分开生产环境还是编译环境，从而执行不同的代码。

### 命令行直接添加环境变量

- Windows (cmd.exe)

```sh
set "REACT_APP_NOT_SECRET_CODE=abcdef" && npm start
```

- Windows (Powershell)

```sh
($env:REACT_APP_NOT_SECRET_CODE = "abcdef") -and (npm start)
```

- Linux, macOS (Bash)

```sh
REACT_APP_NOT_SECRET_CODE=abcdef npm start
```

### 添加自定义环境变量文件`.env`

项目根目录下创建`.env`文件，文件内部添加 `key=value`的配置可以直接应用于项目的编译中。

```sh
REACT_APP_NOT_SECRET_CODE=abcdef
```

>Note: 如果创建自定义的环境变量必须以`REACT_APP_`开头. 

### 在项目中使用环境变量

在项目中可以直接用`process.env.XXX`访问我们的自定义的环境变量。比如：

- js中使用

```js
render() {
  return (
    <div>
      <small>You are running this application in <b>{process.env.NODE_ENV}</b> mode.</small>
      <form>
        <input type="hidden" defaultValue={process.env.REACT_APP_NOT_SECRET_CODE} />
      </form>
    </div>
  );
}
```

再比如：判断是否是生产环境

```js
if (process.env.NODE_ENV !== 'production') {
  analytics.disable();
}
```

- HTML中使用

`<title>%REACT_APP_WEBSITE_NAME%</title>`

## 配合TypeScript

第一种方式：创建项目的时候直接配置好`TypeScript`.

```sh
npx create-react-app my-app --typescript
#or
yarn create react-app my-app --typescript
```

第二种方式：为现有的React项目添加`TypeScript`

```sh
npm install --save typescript @types/node @types/react @types/react-dom @types/jest
# or
yarn add typescript @types/node @types/react @types/react-dom @types/jest
```

## 配置代理

### `package.json`配置代理

配置简单代理，直接在package.json文件中添加proxy节点即可：

```json
{
  ...
  "proxy": "http://localhost:4000",
  ...
}
```

### 自定义配置代理

第一步：安装 `http-proxy-middleware`

```sh
$ npm install http-proxy-middleware --save
$ # or
$ yarn add http-proxy-middleware
```

第二步：创建： `src/setupProxy.js` 添加如下内容:

```js
const proxy = require('http-proxy-middleware');
module.exports = function(app) {
  app.use(proxy('/api', { target: 'http://localhost:5000/' }));
};

## Visual Studio Code配置React开发环境

### React集成VSCode测试

第一步:
首先安装：[`Debugger for Chrome`](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)插件。
第二步： 项目根目录下创建 `.vscode`文件夹。
第三步：创建`launch.json`文件
文件内容：

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Chrome",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:3000",
      "webRoot": "${workspaceFolder}/src",
      "sourceMapPathOverrides": {
        "webpack:///src/*": "${webRoot}/*"
      }
    }
  ]
}
```

此时可以`F5`直接启动测试了。

### 插件

- Reactjs code snippets
- Prettier - Code formatter
- ES7 React/Redux/GraphQL/React-Native snippets
- eslint

## HTTPS托管静态站

有时候需要用HTTPS进行调试相关结构，所以需要把静态站也做成HTTPS的，那么以下配置：

配置`HTTPS`的环境变量为`true`然后再用`npm start`启动`dev server`就是HTTPS的了。

- Windows (cmd.exe)

```sh
set HTTPS=true&&npm start
```

- Windows (Powershell)

```sh
($env:HTTPS = "true") -and (npm start)
```

- Linux, macOS (Bash)

```sh
HTTPS=true npm start
```

浏览器可能有安全警告，不用管，直接测试，enjoy it！

## 分析包结构的大小

`Source map explorer`可以帮助我们分析代码最终打包的`bundles`的代码来自哪里，

安装：

```sh
npm install --save source-map-explorer
#or
yarn add source-map-explorer
```

添加分析脚本到`package.json`的`scripts`中：

```diff
   "scripts": {
+    "analyze": "source-map-explorer build/static/js/main.*",
     "start": "react-scripts start",
     "build": "react-scripts build",
     "test": "react-scripts test",
```

那么就可以运行以下命令进行分析最终打包的情况了：

```sh
npm run build
npm run analyze
```

## 其他react的默认配置

- 直接可以使用sass（安装node-sass模块后）
- 直接可以使用css（import）
- 直接可以导入 图片、svg、字体文件等，已经配置好相应的loader
- ES67代码直接可以用
  - class
  - 箭头函数
  - 私用变量
  - 静态属性
  - 继承
  - 装饰器（XXX不能用）

## 参考

1. [官网文档](https://facebook.github.io/create-react-app/docs/documentation-intro)