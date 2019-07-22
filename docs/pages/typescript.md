# TypeScript 入门教程

## 安装TypeScript

有两种主要的方式来获取`TypeScript`工具：

- 通过`npm`（`Node.js`包管理器）
- 安装`Visual Studio`的`TypeScript`插件

> Visual Studio 2017和Visual Studio 2015 Update 3默认包含了TypeScript。 如果你的Visual Studio还没有安装TypeScript，你可以[下载](https://www.tslang.cn/#download-links)它。

针对使用`npm`的用户：

```sh
npm install -g typescript
```

## 构建你的第一个`TypeScrip`t文件

### 编写第一个ts文件

在编辑器，将下面的代码输入到`greeter.ts`文件里：

```js
function greeter(person) {
    return "Hello, " + person;
}

let user:String = "Jane User"; // 此时设定user变量为String类型

document.body.innerHTML = greeter(user);
```

### 编译代码

在命令行上，运行TypeScript编译器：

```sh
tsc greeter.ts
```

输出结果为一个`greeter.js`文件，它包含了和输入文件中相同的`JavsScript`代码。 一切准备就绪，我们可以运行这个使用`TypeScript`写的`JavaScript`应用了！

> 其实，TypeScript完全兼容ES6的语法，所以使用TS完全可以无缝从JS切入。
