# Node的最后补充

到此为止，Node的大部分的主要内容我们都涉及到，但是还有一些核心模块需要你自己去探索，以下文章内容仅仅以案例补充的形式帮大家了解一下Node的用法。

## UDP开发

以下是UDP的服务器端的例子：

```js
// 引入数据报模块
const dgram = require('dgram');
// 创建服务器端的Socket对象，继承了EventEmitter
const server = dgram.createSocket('udp4');

// 监听服务器端关闭监听的事件
server.on('close', () => {
  console.log('socket已关闭');
});

// 异常处理
server.on('error', err => {
  console.log(err);
});

server.on('listening', () => {
  console.log('socket正在监听中...');
});

// 收到客户端消息后触发此事件。
server.on('message', (msg, rinfo) => {
  console.log(`receive :${msg} from ${rinfo.address}:${rinfo.port}`);
});

// 绑定端口，并开始进行广播。
// 第一个参数：端口，第二个参数：服务器本机IP地址
server.bind('58060', '192.168.0.104', () => {
  server.setBroadcast(true); //开启广播
  // server.setTTL(128);
  setInterval(() => {
    console.log('发送消息');
    var msg = Buffer.from('大家好啊，我是服务端.');
    // 广播消息，注意最后一个IP是广播网段的IP，可以设置分组广播。
    server.send(msg, 0, msg.length, 48061, '192.168.0.255');
  }, 1500);
});
```

以下是UDP客户端的例子：

```js
const dgram = require('dgram');
// 创建客户端的socket，其实不分服务端和客户端，仅仅是我们人为定义的。
const client = dgram.createSocket('udp4');

client.on('close', () => {
  console.log('socket已关闭');
});

client.on('error', err => {
  console.log('===>', err);
});
client.on('listening', () => {
  console.log('socket正在监听中...');
});

// 收到服务器消息。
client.on('message', (msg, rinfo) => {
  console.log(`client => receive :${msg} from ${rinfo.address}:${rinfo.port}`);
});

// 绑定接受消息的端口，不绑定IP，它会帮助我们尽可能的接收各个网段的对应的端口的消息。
client.bind(48061);

setInterval(() => {
  var msg = Buffer.from('你好');
  console.log('发送消息');
  // 对单个服务器发送消息。
  var msg = Buffer.from('大家好啊，我是client.');
  client.send(msg, 0, msg.length, 58060, '192.168.0.104');
}, 1500);
```

## 进程管理

`process` 对象是一个全局变量，它提供当前 Node.js 进程的有关信息，以及控制当前 Node.js 进程。 因为是全局变量，所以无需使用 require()。

### 应用程序未捕获异常补救

`process对象`的'uncaughtException' 事件会在Node进程出现代码中有未处理的异常时触发，不至于让程序出现未处理异常直接崩溃。所以一般一个健壮的Node应用都要用此事件做最后的日志记录，用它在进程结束前执行一些已分配资源（比如文件描述符，句柄等等）的同步清理操作或者应用重启等操作，最好不要让程序继续执行，最好是重启应用。比如极端情况：持续性出异常，可能导致内存暴增或者其他问题就可能让服务器瘫痪。

```js
process.on('uncaughtException', (err) => {
  fs.writeSync(1, `捕获到异常：${err}\n`);
});
```

### 环境变量

`process.env`属性返回一个包含用户环境信息的对象。

```js
// a.js
console.log(process.env);
// 运行： node a.js
// 输出如下内容：  
{ 
  SSH_AUTH_SOCK: '/privatexxbin',
  LSCOLORS: 'Gxfxcxdxbxegedabagacad',
  SHLVL: '1',
  ZSH: '/Users/x/.oh-my-zsh',
  _: '/usr/local/bin/node',
  TERM_PROGRAM: 'vscode',
  TERM_PROGRAM_VERSION: '1.22.2',
  LANG: 'en_US.UTF-8',
  TERM: 'xterm-256color'
}
```

当然我们执行node程序的时候，还可以临时设置环境变量，`process.env`大量运用于我们的自动化部署和编译的工具中环境判断：比如webpack、gulp等。

例如：
```js

process.env.NODE_ENV = 'production'
// a.js
console.log(process.env);
```

```shell
# 运行a.js文件，并给设置环境变量Age
$ Age=18 node a.js

# 输出如下内容, 多了一个Age=18的环境变量参数：  

{ 
  SSH_AUTH_SOCK: '/private/',
  PATH: '/Users/x/Desktop/xxxxxx',
  LSCOLORS: 'Gxfxcxdxbxegedabagacad',
  SHLVL: '1',
  ZSH: '/Users/x/.oh-my-zsh',
  _: '/usr/local/bin/node',
  TERM_PROGRAM: 'vscode',
  TERM_PROGRAM_VERSION: '1.22.2',
  LANG: 'en_US.UTF-8',
  TERM: 'xterm-256color',
  NODE_ENV: 'production',
  Age: 18
}
```

### 进程退出

`process.exit([code])`方法以结束状态码code指示Node.js同步终止进程。 

```js
// 异常退出。
process.exit(1);
```

状态码列表：

状态码|说明
---|---
1| 未捕获异常 - 有一个未被捕获的异常, 并且没被一个 domain 或 an 'uncaughtException' 事件处理器处理。
2| - 未被使用 (Bash为防内部滥用而保留)
3| 内部JavaScript 分析错误 - Node.js的内部的JavaScript源代码 在引导进程中导致了一个语法分析错误。 这是非常少见的, 一般只会在开发Node.js本身的时候出现。
4| 内部JavaScript执行失败 - 引导进程执行Node.js的内部的JavaScript源代码时，返回函数值失败。 这是非常少见的, 一般只会在开发Node.js本身的时候出现。
5| 致命错误 - 在V8中有一个致命的错误. 比较典型的是以FATALERROR为前缀从stderr打印出来的消息。
6| 非函数的内部异常处理 - 发生了一个内部异常，但是内部异常处理函数 被设置成了一个非函数，或者不能被调用。
7| 内部异常处理运行时失败 - 有一个不能被捕获的异常。 在试图处理这个异常时，处理函数本身抛出了一个错误。 这是可能发生的, 比如, 如果一个 'uncaughtException' 或者 domain.on('error') 处理函数抛出了一个错误。
8| - 未被使用. 在之前版本的Node.js, 退出码8有时候表示一个未被捕获的异常。
9| - 不可用参数 - 也许是某个未知选项没有确定，或者没给必需要的选项填值。
10| 内部JavaScript运行时失败 - 调用引导函数时， 引导进程执行Node.js的内部的JavaScript源代码抛出错误。 这是非常少见的, 一般只会在开发Node.js本身的时候出现。
12| 不可用的调试参数 - --inspect 和/或 --inspect-brk 选项已设置，但选择的端口号无效或不可用。
大于128| 退出信号 - 如果Node.js的接收信号致命诸如 SIGKILL 或 SIGHUP，那么它的退出代码将是 128 加上信号的码值。 这是POSIX的标准做法，因为退出码被定义为7位整数，并且信号退出设置高位，然后包含信号码值。


### 其他用法

+ `process.memoryUsage()`  获取当前内存的使用量

```json
{ rss: 20783104,
  heapTotal: 7159808,
  heapUsed: 4905696,
  external: 8608 }
```

+ 当前事件循环的延后处理

`process.nextTick(callback[, ...args])`方法将 callback 添加到"next tick 队列"。
一旦当前事件轮询队列的任务全部完成，在next tick队列中的所有callbacks会被依次调用。

> 这种方式不是setTimeout(fn, 0)的别名。它更加有效率。事件轮询随后的ticks 调用，会在任何I/O事件（包括定时器）之前运行。

```js
console.log('start');
process.nextTick(() => {
  console.log('nextTick callback');
});
console.log('scheduled');
// Output:
// start
// scheduled
// nextTick callback
```

+ 获取当前进程工作目录`process.cwd()`

+ 关闭进程`process.kill(pid[, signal])`

## 总结

其实还有很多其他的东西可以慢慢展开：比如node的vm模块、v8模块、加密模块等，都是非常有意思的东西。但是老马不展开了，毕竟文档还是最详细的。

















