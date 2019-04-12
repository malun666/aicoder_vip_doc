# koa2快速入门教程

`Koa` 是一个新的基于`node`平台的`web` 框架，由 `Express` 幕后的原班人马打造， 致力于成为 `web` 应用和 `API` 开发领域中的一个更小、更富有表现力、更健壮的基石。 通过利用 `async` 函数，`Koa` 帮你丢弃回调函数，并有力地增强错误处理。 `Koa` 并没有捆绑任何中间件， 而是提供了一套优雅的方法，帮助您快速而愉快地编写服务端应用程序。

因为`node.js`从 `v7.6.0`开始完全支持`async/await`，不需要加`flag`，所以`node.js`环境都要`7.6.0`以上

建议：直接安装`node.js 8+`：`node.js`官网地址 [https://nodejs.org](https://nodejs.org)

## 安装koa

```sh
$ npm i koa
```

## helloword

```js
// 引入koa框架
const Koa = require('koa');
// 创建koa的app实例，全局的应用程序是一个包含一组中间件函数的对象。它提供了注册中间件，缓存清理，代理支持和重定向等常见任务的方法
const app = new Koa();

// 注册一个中间件
app.use(async ctx => {
  ctx.body = 'Hello World , from aicoder.com'; // 设置最终的响应的内容
});

app.listen(3000);
```
