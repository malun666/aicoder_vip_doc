# webpack 入门教程

## webpack是什么？

本质上，webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

![webpack](../images/webpack.png)

## 快速了解几个基本的概念

### mode 开发模式

webpack提供 mode 配置选项，配置webpack 相应模式的内置优化。

```diff
// webpack.production.config.js
module.exports = {
+  mode: 'production',
}
```

### 入口文件(entry)

入口文件，类似于其他语言的起始文件。比如：c语言的main函数所在的文件。

入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

可以在webpack的配置文件中配置入口，配置节点为： `entry`,当然可以配置一个入口，也可以配置多个。

### 输出(output)

output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件。

```js
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```

### loader

loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。

### 插件(plugins)

loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。插件接口功能极其强大，可以用来处理各种各样的任务。

## webpack的安装

请确保安装了 `Node.js` 的最新版本。而且已经在您的项目根目录下已经初始化好了最基本的`package.json`文件

### 本地安装webpack

```sh
$ npm install --save-dev webpack

# 如果你使用 webpack 4+ 版本，你还需要安装 CLI。
npm install --save-dev webpack-cli
```

安装完成后，可以添加`npm`的`script`脚本

```js
// package.json
"scripts": {
    "start": "webpack --config webpack.config.js"
}
```

### 全局安装webpack（不推荐)

将使 webpack 在全局环境下可用：

```sh
npm install --global webpack
```

> 注意：不推荐全局安装 webpack。这会将你项目中的 webpack 锁定到指定版本，并且在使用不同的 webpack 版本的项目中，可能会导致构建失败。

## 快速入门完整demo

- 第一步：创建项目结构

首先我们创建一个目录，初始化 npm，然后 在本地安装 webpack，接着安装 webpack-cli（此工具用于在命令行中运行 webpack）：

```sh
mkdir webpack-demo && cd webpack-demo
npm init -y
npm install webpack webpack-cli --save-dev
```

项目结构

```diff
  webpack-demo
+ |- package.json
+ |- /dist
+   |- index.html
+ |- /src
+   |- index.js
```

- 第二步：安装loadash依赖和编写js文件

```sh
npm install --save lodash
```

编写：src/index.js 文件

```js
import _ from 'lodash';

function createDomElement() {
  let dom = document.createElement('div');
  dom.innerHTML = _.join(['aicoder', '.com', ' wow'], '');
  return dom;
}

document.body.appendChild(createDomElement());
```

index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>起步</title>
</head>
<body>
  <script src="./main.js"></script>
</body>
</html>
```

- 第三步：编写webpack配置文件

根目录下添加 `webpack.config.js`文件。

```diff
  webpack-demo
  |- package.json
+ |- webpack.config.js
  |- /dist
    |- index.html
  |- /src
    |- index.js
```

webpack.config.js 内容如下：

```js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'main.js',
    path: path.resolve(__dirname, './dist')
  }
}
```

- 执行构建任务

直接执行构建任务：

```sh
npx webpack
```

打开： dist/index.html 可以查看到页面的结果。

## 加载非js文件

webpack 最出色的功能之一就是，除了 JavaScript，还可以通过 loader 引入任何其他类型的文件

### 加载CSS文件

第一步： 安装css和style模块解析的依赖 `style-loader` 和 `css-loader`

```sh
npm install --save-dev style-loader css-loader
```

