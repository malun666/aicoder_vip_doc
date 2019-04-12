# koa2快速入门教程

`Koa` 是一个新的基于`node`平台的`web` 框架，由 `Express` 幕后的原班人马打造， 致力于成为 `web` 应用和 `API` 开发领域中的一个更小、更富有表现力、更健壮的基石。 通过利用 `async` 函数，`Koa` 帮你丢弃回调函数，并有力地增强错误处理。 `Koa` 并没有捆绑任何中间件， 而是提供了一套优雅的方法，帮助您快速而愉快地编写服务端应用程序。

因为`node.js`从 `v7.6.0`开始完全支持`async/await`，不需要加`flag`，所以`node.js`环境都要`7.6.0`以上

建议：直接安装`node.js 8+`：`node.js`官网地址 [https://nodejs.org](https://nodejs.org)

## 安装koa

```sh
$ npm i koa
```

## helloword

第一步： 创建项目目录和安装项目依赖

```sh
$ mkdir koademo
$ cd koademo
$ npm init -y
$ npm i -P koa@2.7.0
```

第二步： 创建`app.js`文件，并添加代码如下：

```js
// 引入koa框架
const Koa = require('koa');
// 创建koa的app实例，全局的应用程序是一个包含一组中间件函数的对象。它提供了注册中间件，缓存清理，代理支持和重定向等常见任务的方法
const app = new Koa();

// 注册一个中间件
app.use(async ctx => {
  ctx.body = 'Hello World , from aicoder.com'; // 设置最终的响应的内容
});

// 设置开启监听并设置监听端口为3006
app.listen(3006);
```

第三步： 启动运行

```sh
$ node app.js
```

第四步：打开浏览器测试

浏览器输入地址： `http://localhost:3006/`

如果输出：

```js
'Hello World , from aicoder.com'
```

## 关于app

`app`是koa的核心对象，提供了注册中间件，监听http等相关功能。

### app.listen(...)

Koa通过listen方法启动HTTP服务器监听，开启HTT服务。

```js
const Koa = require('koa');
const app = new Koa();
app.listen(3006);
```

### app.use(function)

将给定的中间件方法添加到此应用程序。中间件函数会收到两个参数`ctx`和`next`，两个参数的详细在下面。

### app.keys=

设置签名的 `Cookie` 密钥。

这些被传递给 KeyGrip，但是你也可以传递你自己的 KeyGrip 实例。

例如，以下是可以接受的：

```js
app.keys = ['im a newer secret', 'i like turtle'];
app.keys = new KeyGrip(['im a newer secret', 'i like turtle'], 'sha256');
```

这些密钥可以倒换，并在使用 { signed: true } 参数签名 Cookie 时使用。

```js
ctx.cookies.set('name', 'tobi', { signed: true });
```

### app.context

app.context 是从其创建 ctx 的原型。您可以通过编辑 app.context 为 ctx 添加其他属性。这对于将 ctx 添加到整个应用程序中使用的属性或方法非常有用，这可能会更加有效（不需要中间件）和/或 更简单（更少的 require()），而更多地依赖于ctx，这可以被认为是一种反模式。

例如，要从 ctx 添加对数据库的引用：

```js
app.context.db = db();

app.use(async ctx => {
  console.log(ctx.db);
});
```

>注意: ctx 上的许多属性都是使用 getter ，setter 和 Object.defineProperty() 定义的。你只能通过在 app.context 上使用 Object.defineProperty() 来编辑这些属性（不推荐）。

### 错误处理

默认情况下，将所有错误输出到 stderr，除非 app.silent 为 true。 当 err.status 是 404 或 err.expose 是 true 时默认错误处理程序也不会输出错误。 要执行自定义错误处理逻辑，如集中式日志记录，您可以添加一个 “error” 事件侦听器：

```js
app.on('error', err => {
  log.error('server error', err)
});
```

如果 req/res 期间出现错误，并且 _无法_ 响应客户端，Context实例仍然被传递：

```js
app.on('error', (err, ctx) => {
  log.error('server error', err, ctx)
});
```

当发生错误 _并且_ 仍然可以响应客户端时，也没有数据被写入 socket 中，Koa 将用一个 500 “内部服务器错误” 进行适当的响应。在任一情况下，为了记录目的，都会发出应用级 “错误”。

## context 上下文

`Koa` 提供一个 `Context` 对象，表示一次对话的上下文（包括HTTP请求req和响应res）。通过加工这个对象，获取请求的内容，还可以控制返回给用户的内容。

```js
// 引入koa框架
const Koa = require('koa');
// 创建koa的app实例，全局的应用程序是一个包含一组中间件函数的对象。它提供了注册中间件，缓存清理，代理支持和重定向等常见任务的方法
const app = new Koa();

// 注册一个中间件
app.use(async ctx => {
  ctx.body = 'Hello World , from aicoder.com'; // 设置最终的响应的内容
});
```

例如在上面这个例子中，`ctx`被传入到 `app.use(fn)`的中间件函数中。

我们可以获取请求的内容和响应的内容：

```js
app.use(async ctx => {
  // ctx.body = 'Hello World , from aicoder.com'; // 设置最终的响应的内容
  ctx.response.body = 'hi, aicoder.com'; // 设置响应内容
  console.log(ctx.request.url); // 获取请求的url地址。
});
```

### ctx.request

koa 的 Request 对象.见下文。

### ctx.response

koa 的 Response 对象.见下文。

### ctx.state

推荐的命名空间，用于通过中间件传递信息和你的前端视图。

```js
ctx.state.user = await User.find(id);
```

### ctx.app

应用程序实例引用。

### cookie操作

```js
ctx.cookies.get(name, [options])
```

通过 options 获取 cookie name:

```js
ctx.cookies.set(name, value, [options])
```

通过 options 设置 cookie name 的 value :

- maxAge 一个数字表示从 Date.now() 得到的毫秒数
- signed cookie 签名值
- expires cookie 过期的 Date
- path cookie 路径, 默认是'/'
- domain cookie 域名
- secure 安全 cookie
- httpOnly 服务器可访问 cookie, 默认是 true
- overwrite 一个布尔值，表示是否覆盖以前设置的同名的 cookie (默认是 false). 如果是 true, 在同一个请求中设置相同名称的所有 Cookie（不管路径或域）是否在设置此Cookie 时从 Set-Cookie 标头中过滤掉。

## HTTP Response 的类型

Koa 默认的返回类型是text/plain，如果想返回其他类型的内容，可以先用ctx.request.accepts判断一下，客户端希望接受什么数据（根据 HTTP Request 的Accept字段），然后使用ctx.response.type指定返回类型。请看下面的例子（完整代码看这里）

```js
const main = ctx => {
  if (ctx.request.accepts('xml')) {
    ctx.response.type = 'xml';
    ctx.response.body = '<data>Hello World</data>';
  } else if (ctx.request.accepts('json')) {
    ctx.response.type = 'json';
    ctx.response.body = { data: 'Hello World' };
  } else if (ctx.request.accepts('html')) {
    ctx.response.type = 'html';
    ctx.response.body = '<p>Hello World</p>';
  } else {
    ctx.response.type = 'text';
    ctx.response.body = 'Hello World';
  }
};
app.use(main);
```

> 其实可以直接通过ctx直接访问response的方法和属性,名字等价，例如：

```js
ctx.body
ctx.body=
ctx.status
ctx.status=
ctx.message
ctx.message=
ctx.length=
ctx.length
ctx.type=
ctx.type
ctx.headerSent
ctx.redirect()
ctx.attachment()
ctx.set()
ctx.append()
ctx.remove()
ctx.lastModified=
ctx.etag=
```

其他参考请参与文档： [https://koa.bootcss.com/](https://koa.bootcss.com/)

## HTTP Request 的类型

Koa Request 对象是在 node 的 vanilla 请求对象之上的抽象，提供了诸多对 HTTP 服务器开发有用的功能。

### request.header

获取，请求标头对象。

### request.method

获取请求的方法。

### request.querystring

获取请求的查询字符串

....

其他参考文档：[https://koa.bootcss.com/](https://koa.bootcss.com/)

## koa中间件机制

koa通过app.use方法注册一个中间件，中间件是一个函数。函数可以是async也可以是普通函数。可以接受两个参数`ctx`和`next`.
ctx是上下文， next是执行下一个中间件。


