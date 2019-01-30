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