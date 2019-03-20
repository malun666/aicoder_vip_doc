# 第一个 Nodejs 程序

本教程仅适合您已经有一定的JS编程的基础或者是后端语言开发的基础。
如果您是零基础，建议您先学一下老马的[前端免费视频教程](https://qtxh.ke.qq.com)

## 第一步：创建项目文件夹

首先创建 demos 文件夹。然后在此文件夹下创建`01_hello.js`文件

```shell
# 以下是linux/mac下使用终端用命令行创建文件，windows下请直接用资源管理可视化鼠标操作
$ mkdir demos && cd demos
# 创建 01_hello.js文件
$ touch 01_hello.js
```

## 第二步：编写 nodejs 的第一个程序文件

然后用编辑器（推荐使用：vscode 或者 sublime）打开文件：`01_hello.js`，并添加代码如下：

```js
// 以下是常规的JS语法，如果您还不熟悉js，请您移步老马的
// 免费在线js全套视频教程: https://qtxh.ke.qq.com
// console是控制台的意思，node把浏览器端的控制台做了迁移整合，可以直接使用。log是往控制台打印文字的方法。
console.log('Hi, aicoder.com! Hello, world!');
```

保存文件，并用 node 执行此 js 文件。

## 第三步：编译和运行 JS 文件

打开系统的命令行工具（mac|linux 为终端，windows 下为 cmd 或 powershell），用 cd 命令进入 demos 文件夹。运行编译和执行 js 文件的命令：

```shell
# 进入demos目录
$ cd demos
$ node ./01_hello.js
Hi, aicoder.com! Hello, world!
```

至此，您的第一个 node 程序就已经运行成功了，也就是您的 nodejs 环境已经没有问题了。

---

[老马免费视频教程](https://qtxh.ke.qq.com)

## [回到nodejs知识列表首页](/pages/nodejs.md)
