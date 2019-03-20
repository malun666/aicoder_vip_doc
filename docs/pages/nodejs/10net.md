# net网络模块

net模块是node对TCP或者IPC开发的封装，包括了客户端和服务器端相关API。对于阅读本文，请您有一定的网络编程的基础。
您需要已经了解了：
+ ip协议，会配置ip地址
+ 了解dns解析过程，了解dns的概念
+ 了解基本的TCP的协议的
+ 了解Socket的编程相关概念
+ 了解node的事件处理、流、文件处理等
+ 了解HTTP协议

本文，仅对部分API和TCP开发做一些简单介绍。

## 创建TCP服务器端

`net.Server` 类用于创建TCP的server，而且继承了`EventEmitter`。通过`net.createServer([options][, connectionListener])`方法创建此类型实例。

```js
const net = require('net');

// 创建服务器端的
const server = net.createServer();

// 监听异常错误事件
server.on('error', err => {
  // throw err;
  console.log(err);
});

// 监听客户端的连接事件，客户端连接上后，会自动执行回调函数，回调函数的参数就是指向客户端的socket
server.on('connection', clientSocket => {
  console.log('客户端：%s', clientSocket.remoteAddress);

  // 监听此客户端的end事件。
  clientSocket.on('end', () => {
    console.log('client disconnected');
  });

  // 监听此客户端发送数据的事件。
  clientSocket.on('data', data => {
    console.log('收到数据：%s', data);
  });

  // 向客户端发送数据
  clientSocket.write('Hi, aicoder.com ');

  // 2s后让客户端退出
  setTimeout(() => {
    // 通知客户端退出，并发送数据。
    clientSocket.end('bye！');
  }, 2000);
});

// 服务器开始监听60003端口（端口：0-65535之间的一个值）
server.listen(60003, () => {
  console.log('opened server on', server.address());
});

// 以下为关闭监听的实例
// setTimeout(() => {
//   console.log('服务器端退出！！！');
//   server.close();
// }, 5000);
```

`net.Server` 是对服务器端的Socket的封装，可以监听`close`事件、`error`事件、`connection` 事件、`listening` 事件。还可以通过`close()`方法关闭服务的监听。其他用法参考[官网文档](http://nodejs.cn/api/net.html#net_net_createserver_options_connectionlistener)。

`net.Socket` 类是对客户端Socket的封装，可以监听 `close` 事件、 `connect` 事件 、`data` 事件、`drain` 事件、`end` 事件、`error` 事件`、lookup` 事件、`timeout` 事件。可用的方法包括：`write()`发送数据、`edn()`结束连接等。其中可以同`data`事件来处理服务器端的数据。

## 创建TCP的客户端

`net.createConnection()`方法可以实现连接服务器端，并生成一个`net.Socket` 类实例，跟服务器端进行交互就是靠此实例。

```js
const net = require('net');

// 创建连接到服务器的客户端
let client = net.createConnection('60003', '127.0.0.1', () => {
  client.write('Hi， client, for aicoder.com');
  console.log('连接上服务器端！');
});

client.on('error', err => {
  console.log(err);
});

client.on('end', () => {
  console.log('结束连接！');
});

client.on('close', () => {
  console.log('退出');
});

client.on('data', data => {
  console.log('收到数据： %s', data);
});
```

## 通过Socket上传文件的例子

```js
const net = require('net');
const path = require('path');
const fs = require('fs');

// 创建服务器端的
const server = net.createServer();

// 监听异常错误事件
server.on('error', err => {
  console.log(err);
});

// 监听客户端的连接事件，客户端连接上后，会自动执行回调函数，回调函数的参数就是指向客户端的socket
server.on('connection', clientSocket => {
  console.log('客户端：%s', clientSocket.remoteAddress);

  // 监听此客户端的end事件。
  clientSocket.on('end', () => {
    console.log('client disconnected');
  });

  // 监听此客户端发送数据的事件。
  clientSocket.on('data', data => {
    console.log('收到数据：%s', data);
    let fileName = path.join(__dirname, 'b.html');
    let ws = fs.createWriteStream(fileName);
    ws.write(data, 'utf8');
  });

  // 向客户端发送数据
  clientSocket.write('Hi, aicoder.com ');
});

// 服务器开始监听60003端口（端口：0-65535之间的一个值）
server.listen(60003, () => {
  console.log('opened server on', server.address());
});

// 以下是客户端代码
const client = net.createConnection(60003, '127.0.0.1', () => {
  console.log('连接上服务器端！');
  let fileName = path.join(__dirname, 'a.html');
  let rs = fs.createReadStream(fileName, { encoding: 'utf8' });
  // socket本身是可读可写流，所以可以直接用管道。
  rs.pipe(client);
});

```

## 模拟一个WEB服务器软件

如果您已经了解了HTTP协议的话，而且已经掌握如何做TCP的发送数据和接受处理数据，再有您稍微掌握一点字符串处理的技巧，那么您就很容易做一个简单的静态web服务器出啦。当然这里是说用底层的API，不是用http模块。

限于篇幅，在此不再赘述，请直接看我的[github源码](https://github.com/malun666/aicoder_node/tree/master/demos/webserver)，仅仅是demo，不要用于生产环境中。

## 总结

node中对socket的封装，还是比较像node的开发风格的，可能跟其他平台的socket编程的风格不一致，但是原理和开发方式都是一样的。这里仅仅是简单介绍一下Node下面网络编程的基本方法，细节请参考官网文档。

---

[老马免费视频教程](https://qtxh.ke.qq.com)

## [回到nodejs知识列表首页](/pages/nodejs.md)
