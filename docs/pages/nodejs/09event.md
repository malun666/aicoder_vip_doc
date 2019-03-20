# Node 的事件处理

Node中大量运用了事件回调，所以Node对事件做了单独的封装。所有能触发事件的对象都是 `EventEmitter` 类的实例,所以上一篇我们提到的文件操作的可读流、可写流等都是继承了 `EventEmitter`。当然我们也可以自定义具有事件行为的自定义对象，仅需要对其继承即可。

## 继承EventEmitter

node的`events`模块封装了`EventEmitter`类型，此类型里面封装了事件注册、触发等API。

```js
// 引入events模块
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {
  constructor(opt) {
    super(opt);
  }
}

// 创建事件对象实例。
const myEmitter = new MyEmitter();

// 注册event事件，event是事件名字，最好符合以驼峰命名规范。
myEmitter.on('event', () => {
  console.log('触发了一个事件！');
});

// 触发event事件
myEmitter.emit('event');

```

## 给回调函数传递参数

`emit()`方法触发事件的同时，还可以给回调函数传递参数。

```js
// 引入events模块
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {
  constructor(opt) {
    super(opt);
  }
}

// 创建事件对象实例。
const myEmitter = new MyEmitter();

// 注册event事件，event是事件名字，最好符合以驼峰命名规范。
myEmitter.on('event', (a, b) => {
  console.log('event: %s, %s', a, b);
});

// 触发event事件，并传递参数a、b
myEmitter.emit('event', 'aicoder.com', '全栈实习');

```

## 错误处理的约定

当 EventEmitter 实例中发生错误时，会触发一个 'error' 事件。 这在 Node.js 中是特殊情况。

如果 EventEmitter 没有为 'error' 事件注册至少一个监听器，则当 'error' 事件触发时，会抛出错误、打印堆栈跟踪、且退出 Node.js 进程。

```js
const myEmitter = new MyEmitter();
myEmitter.emit('error', new Error('whoops!'));
// 抛出错误，并使 Node.js 崩溃
```

为了防止 Node.js 进程崩溃，可以在 `process` 对象的 `uncaughtException` 事件上注册监听器.

```js
const myEmitter = new MyEmitter();

// 给nodejs的进程增加未捕获异常的处理，防止程序崩溃
process.on('uncaughtException', (err) => {
  console.error('有错误');
});

myEmitter.emit('error', new Error('whoops!'));
// 打印: 有错误
```

## 只处理事件一次

`on()`方法可以注册事件处理程序，而且是每次`emit()`触发事件，都会被执行。但是用`once()`注册的事件，仅执行一次。例如：

```js
// 引入events模块
const EventEmitter = require('events');

class MyEmitter extends EventEmitter {
  constructor(opt) {
    super(opt);
  }
}

const myEmitter = new MyEmitter();
let m = 0;
myEmitter.once('event', () => {
  console.log(++m);
});
myEmitter.emit('event');
// 打印: 1
myEmitter.emit('event');
// 忽略
```

## 实战案例

做一个应用时，我们需要在应用启动之前或者启动之后，给其他的开发人员提供一些可以注册处理程序的钩子，可以用事件的方式实现。其实本质就是发布订阅模式。

```js
'use strict';

const EventEmitter = require('events');

class Application extends EventEmitter {
  constructor(opt) {
    super(opt);
    this.on('error', err => {
      console.log('应用程序出错了！');
    });
  }

  init() {
    // 触发预初始化事件
    this.emit('preInit');

    // ... 默认的初始化代码

    // 初始化事件
    this.emit('init');
  }

  start() {
    // 初始化服务器
    this.init();
  }

}

/**/
var app = new Application();
app.on('init', () => {
  console.log('初始化！');
});
app.on('preInit', () => {
  console.log('pre init');
});
app.start();

```

## 总结

Node中的事件处理封装很简单易用，跟jQuery的事件系统非常类似。其实自己实现一套事件系统也不难，核心思想就是：发布订阅（观察者）模式。

---

[老马免费视频教程](https://qtxh.ke.qq.com)

[返回首页](../readme.md)
