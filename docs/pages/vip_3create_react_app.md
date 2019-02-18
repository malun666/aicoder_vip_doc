# creat-react-app入门教程

`Create React App`是FaceBook的React团队官方出的一个构建`React`单页面应用的脚手架工具。它本身集成了`Webpack`，并配置了一系列内置的`loader`和默认的npm的脚本，可以很轻松的实现零配置就可以快速开发React的应用。

## Quick Start（快如入门）

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

## 构建React项目的其他方式

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
