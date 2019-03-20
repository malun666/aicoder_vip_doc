# http模块

[Ryan Dahl](https://github.com/ry)开发node的初衷就是：把`Nginx`非阻塞IO功能和一个高度封装的WEB服务器结合在一起的东东。所以Node初衷就是为了高性能的Web服务器去的，所以：Node的HTTP模块也是核心的核心。

本文需要您了解的前置知识点：

+ HTTP协议
+ Web请求模型：请求→处理→响应
+ Node的流、事件

## http模块的客户端

要使用 HTTP 服务器与客户端，需要 `require('http')`模块。http模块提供了两个函数`http.request()`和`http.get()`,帮助程序向服务器端发送请求。

我们可以通过`http.request
()`方法创建一个发送请求的`http.ClientRequest`类实例，请求创建后，并不会立即发送请求，我们还可以继续访问请求头：`setHeader(name, value)`、`getHeader(name)` 和 `removeHeader(name)` API 进行修改。实际的请求头会与第一个数据块一起发送或当调用 `request.end()` 时发送。

### http.ClientRequest类

+ `http.ClientRequest`类继承了`EventEmitter`,它内部定义了以下事件。

事件|说明
---|---
'abort' |当请求已被客户端终止时触发。 该事件仅在首次调用 abort() 时触发。
'connect' |每当服务器响应 CONNECT 请求时触发。 如果该事件未被监听，则接收到 CONNECT 方法的客户端会关闭连接。
'continue' |当服务器发送了一个 100 Continue 的 HTTP 响应时触发，通常是因为请求包含 Expect: 100-continue。 这是客户端将要发送请求主体的指令。
'response' |当请求的响应被接收到时触发。 该事件只触发一次。如果没有添加 'response' 事件处理函数，则响应会被整个丢弃。 如果添加了 'response' 事件处理函数，则必须消耗完响应对象的数据，可通过调用 response.read()、或添加一个 'data' 事件处理函数、或调用 .resume() 方法。 数据被消耗完时会触发 'end' 事件。 在数据被读取完之前会消耗内存，可能会造成 'process out of memory' 错误。
'socket' |当 socket 被分配到请求后触发。
'timeout'|当底层 socket 超时的时候触发。该方法只会通知空闲的 socket。请求必须手动停止。
'upgrade' |每当服务器响应 upgrade 请求时触发。 如果该事件未被监听，则接收到 upgrade 请求头的客户端会关闭连接。

+ `http.ClientRequest`类还提供了一些方法供我们进行请求和返回响应的处理。

方法|参数|说明
---|---|---
`request.end([data[, encoding]][, callback])`|①`data`发送的数据  ②`encoding`编码 ③callback回调函数|结束发送请求。如果部分请求主体还未被发送，则会刷新它们到流中。如果请求是分块的，则会发送终止字符 '0\r\n\r\n'。如果指定了 data，则相当于调用 request.write(data, encoding) 之后再调用 request.end(callback)。如果指定了 callback，则当请求流结束时会被调用。
`request.flushHeaders()`|无|刷新请求头。出于效率的考虑，Node.js 通常会缓存请求头直到 request.end() 被调用或第一块请求数据被写入。 然后 Node.js 会将请求头和数据打包成一个单一的 TCP 数据包。
`request.getHeader(name)`|①name ②返回字符串 |读出请求头，注意:参数name是大小写敏感的
`request.removeHeader(name)`|name 字符串|移除一个已经在 headers 对象里面的 header。
`request.setHeader(name, value)`|①name是header的key②value|为 headers 对象设置一个单一的 header 值。如果该 header 已经存在了，则将会被替换。这里使用一个字符串数组来设置有相同名称的多个 headers。
`request.setSocketKeepAlive([enable][, initialDelay])`|①enable类型boolean②initialDelay|一旦 socket 被分配给请求且已连接，socket.setKeepAlive() 会被调用。
`request.setTimeout(timeout[, callback])`|①timeout请求被认为是超时的毫秒数。②callback 可选的函数,当超时发生时被调用。|等同于绑定到 timeout 事件。一旦socket被分配给请求且已连接，socket.setTimeout() 会被调用。
`request.write(chunk[, encoding][, callback])`|①chunk发送的请求数据。②encoding：编码；③callback回调函数|发送请求主体的一个数据块。 通过多次调用该方法，一个请求主体可被发送到一个服务器，在这种情况下，当创建请求时，建议使用 ['Transfer-Encoding', 'chunked'] 请求头。

### 发送GET请求

```js
// 引入http模块
const http = require('http');

// 创建一个请求
let request = http.request(
  {
    protocol: 'http:',     // 请求的协议
    host: 'aicoder.com',   // 请求的host
    port: 80,              // 端口
    method: 'GET',         // GET请求
    timeout: 2000,         // 超时时间
    path: '/'              // 请求路径
  },
  res => {  // 连接成功后，接收到后台服务器返回的响应，回调函数就会被调用一次。
    // res => http.IncomingMessage : 是一个Readable Stream
    res.on('data', data => {
      console.log(data.toString('utf8')); // 打印返回的数据。
    });
  }
);

// 设置请求头部
request.setHeader('Cache-Control', 'max-age=0');


// 真正的发送请求
request.end();
```

### 发送get请求的另外一个办法

http模块还提供了`http.get(options,callback)`，用来更简单的处理GET方式的请求，它是`http.request()`的简化版本，唯一的区别在于http.get自动将请求方法设为GET请求，同时不需要手动调用req.end(); 

```js
http.get('http://aicoder.com', res => {
  res.on('data', data => {
    console.log(data.toString('utf8'));
  });
});
```


### 发送post请求

且看一个发送post请求的例子。

```js
const http = require('http');

let request = http.request(
  {
    protocol: 'http:',
    host: 'aicoder.com',
    port: 80,
    method: 'POST',
    timeout: 2000,
    path: '/'
  },
  res => {
    res.on('data', data => {
      console.log(data.toString('utf8'));
    });
  }
);
// 发送请求的数据。
request.write('id=3&name=aicoder');
request.end();

```


## HTTP服务器端

`http.Server`实现了简单的web服务器，并把请求和响应也做了封装。

### http.server对象的事件

`http.server`是一个基于事件的HTTP服务器，所有的请求都被封装到独立的事件当中，我们只需要对他的事件编写相应的行数就可以实现HTTP服务器的所有功能，它继承自EventEmitter,提供了以下的事件： 
1. `request`：当客户端请求到来的时候，该事件被触发，提供两个参数request和response，分别是http.ServerRequest和http.ServerResponse表示请求和响应的信息。 
2. `connection`：当TCP建立连接的时候，该事件被触发，提供了一个参数socket，为net.socket的实例(底层协议对象) 
3. `close`：当服务器关闭的时候会被触发 
4. 除此之外还有`checkContinue`、`upgrade`、`clientError`等事件 

我们最常用的还是`request`事件，http也给这个事件提供了一个捷径：`http.createServer([requestListener])`
下面我们来简单的看一下两个案例： 

第一个：使用request事件的：

```
const http = require('http');

let server = new http.Server();
server.on('request', (req, res) => {
  console.log(req.url);
  //设置应答头信息
  res.writeHead(200, { 'Content-Type': 'text/html' });
  res.write('hello we are family<br>');
  res.end('server already end\n');
});
//显示了三次这也证明了TCP的三次握手
server.on('connection', () => {
  console.log('握手');
});
server.on('close', () => {
  console.log('server will close');
});
//关闭服务为了触发close事件
server.close();
server.listen(8080);
```

第二个：利用`http.createServer`创建服务器实例代码：

```js
const http = require('http');

http.createServer(function(req,res){
    res.writeHead(200,{'Content-Type':'text/plain'})
    res.write("hi, from aicoder.com");
    res.end();

}).listen(3000);
```

### http.ServerRequset请求信息

我们都知道HTTP请求分为两部分：请求头和请求体，如果请求的内容少的话就直接在请求头协议完成之后立即读取，请求体可能相对较长一点，需要一定的时间传输。因此提供了三个事件用于控制请求体传输. 

1.`data`：当请求体数据到来时，该事件被触发，该事件一共一个参数chunk，表示接受到的数据。 
1.`end`：当请求体数据传输完成时，该事件被触发，此后将不会再有数据到来。 
1.`close`：用户当前请求结束时，该事件被触发，不同于end，如果用户强制终止了传输，也会触发close 

ServerRequest的属性

名称 | 含义
---|---
ccomplete| 客户端请求是否已经发送完成
httpVersion| HTTP协议版本，通常是1.0或1.1
method|  HTTP请求方法，如：GET,POST
url| 原始的请求路径
headers| HTTP请求头
trailers|  HTTP请求尾(不常见)
connection|  当前HTTP连接套接字，为net.Socket的实例
socket|  connection属性的别名
client | client属性的别名

```
http.createServer(function(req,res){
    console.log(req.httpVersion);
    //console.log(req.socket);
    console.log(req.headers);
    console.log(req.method);
    res.writeHead(404,{'Content-Type':'text/plain'})
    res.write("we are is content");
    res.end();
}).listen(8080);

```

### 获取GET请求内容

由于GET请求直接被嵌入在路径中,URL完整的请求路径，包括了?后面的部分，因此你可以手动解析后面的内容作为GET的参数，Nodejs的url模块中的parse函数提供了这个功能。

```js
const http = require('http');
const url = require('url');
const util = require('util');

http
  .createServer((req, res) => {
    //利用url模块去解析客户端发送过来的URL
    res.write(util.inspect(url.parse(req.url, true)));
    res.end();
  })
  .listen(8080);
```

### 获得POST请求内容

POST请求的内容全部都在请求体中，·http.ServerRequest·并没有一个属性内容为请求体，原因是等待请求体传输可能是一件耗时的工作。譬如上传文件。恶意的POST请求会大大消耗服务器的资源。所以Nodejs是不会解析请求体，当你需要的时候，需要手动来做。 

简单的看一下代码：
```js
// 获取post请求数据
const http = require('http');
const util = require('util');
const querystring = require('querystring');

http
  .createServer((req, res) => {
    let post = '';
    req.on('data', chunk => {
      post += chunk;
    });
    req.on('end', () => {
      post = querystring.parse(post);
      res.end(util.inspect(post));
    });
  })
  .listen(60004);
```

### http.ServerResponse返回客户端信息

`http.ServerResponse`这个类实现了（而不是继承自）可写流 接口。继承了`EventEmitter`。它用来给用户发送响应结果，它是由`http.Server`的`request`事件发送的，作为第二个参数传递。一般为response或res 
主要的三个函数： 
+ `response.writeHead(statusCode,[headers])`：向请求的客户端发送响应头。 
  - `statusCode`是HTTP的状态码，如200为成功，404未找到等。 
  - `headers`是一个类似关联数组的对象，表示响应头的每个属性。 
+ `response.write(data,[encoding])` 向请求客户端发送相应内容，data是buffer或字符串，encoding为编码 
+ `response.end([data],[encoding])` 结束响应，告知用户所有发送已经完成，当所有要返回的内容发送完毕，该函数必须被调用一次，如果不调用，客户端永远处于等待状态


## 总结

真正开发环境，不会用这么底层的API去做web网站或者微服务，一般会选择KOA或者EXPRESS等框架。不过，通过底层的API也可以感受一下NODE原生的开发的快乐。
˜
---------
参考：

1.https://blog.csdn.net/woshinannan741/article/details/51357464
1.http://nodejs.cn/api/http.html#http_http_createserver_requestlistener

---

[老马免费视频教程](https://qtxh.ke.qq.com)

## [回到nodejs知识列表首页](/pages/nodejs.md)
