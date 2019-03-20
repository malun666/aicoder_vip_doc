# NodeJs的文件处理

`Node`的文件处理涉及到前面说的`ptah`模块,以及`fs`文件系统、`stream`流处理、`Buffer`缓冲器等模块。内容可能比较多，相关内容请以官网文档为主，此处主要以案例讲解为主，分享给大家一些常用的经典案例。细节就不展开了。

## fs文件系统

`fs`模块提供了很多文件操作相关的api，比如：监控文件夹、文件，文件重命名，文件读写，文件修改权限、文件读写流等。

在此，我们仅以几个案例的方式来驱动学习Node的文件系统，细节请详细阅读Node的api文档或者源码。

案例：

+ 如何监控文件夹的变化？

+ 如何读取一个文件？

+ 如何把内容写入另外一个文件？

+ 文件件读取、文件重命名、移动等各种功能

## 如何监控文件夹的变化？

`fs`模块提供了`FSWatcher`类辅助我们进行监控文件夹，可以通过`fs.watch()`方法返回此类型实例。然后通过注册相关的事件回调函数达到对文件变化的监控。不废话，直接上代码，详细内容请参考[官方文档](http://nodejs.cn/api/fs.html#fs_class_fs_fswatcher)。

```js
// 引入fs模块
const fs = require('fs');

// 通过fs.watch方法可以创建一个fs.FSWatcher类的实例。
let watcher = fs.watch(
  __dirname,                  // 监控的文件夹，我这里用了一个模块的变量，当前js文件所在的目录
  { recursive: true },        // 是否监控子文件夹，还可以设置编码，具体参考官网文档
  (eventName, fileName) => {  // 回调函数接受两个参数：事件名字和文件名
    console.log(`事件名字：${eventName}, 文件名字： ${fileName}`);
  }
);

// 还可以单独注册事件，回调函数跟watch方法一致。还可以监听：error事件。
watcher.on('change', (eventType, fileName) => {
  console.log('事件名：%s , 文件名： %s', eventType, fileName);
});

// 设置13秒中后，退出监控文件夹
setTimeout(() => {
  // 关闭监控。
  watcher.close(function(err) {
    if (err) {
      console.error(err);
    }
    console.log('关闭watch');
  });
}, 13000);

/********
以下是输出的结果：当js文件修改的时候
事件名字：change, 文件名字： 05filewatch.js
事件名：change , 文件名： 05filewatch.js
事件名字：change, 文件名字： 05filewatch.js
事件名：change , 文件名： 05filewatch.js
*/
```

## 如何读取一个文件？

读取文件可以分为小文件读取和大文件，如果小文件可以一次性的把文件内容读取出来然后再处理，对于比较大的文件用文件流处理。

### 一次性读取小文件的方法

`fs.readFile()`方法可以帮助我们一次性的把文件里面内容读取出来。[文档](http://nodejs.cn/api/fs.html)

```js
const fs = require('fs');
const path = require('path');

// 把当前js所在的目录中的a.html文件的路径赋值给 fileName
let fileName = path.join(__dirname, 'a.html');

// 读取a.html文件，按照utf8的编码方式读取。回调函数第一个参数是err(这个是一个默认的约定规范，大多数node
// 的回调函数第一个参数都是异常的err，如果为空则表示没有错误。)第二个参数是文件的所有内容。
fs.readFile(fileName, { encoding: 'utf8' }, (err, data) => {
  if (err) throw err; // 判断是否读取错误
  console.log(data);  // 文件内容读取并打印到控制台。
});

//readFileSync方法是readFile的同步版本，没有回调函数，函数的返回值就是文件内容。
// 以下代码是同步读取，不使用回调函数，此方法不是用的： libuv 的线程池的线程执行，所以慎用！！
let fileContent = fs.readFileSync(fileName, { encoding: 'utf8' });
console.log(fileContent);
```

> 注意：此方法只适合比较小的文件，不适合大文件读取。
> 同步方法尽量少用，异步的读取文件都是利用了libuv 的线程池的线程读取文件，所以读取文件等待期间不会阻塞主线程的事件循环。

### 读取大文件

使用stream读取大文件。当然你可以自定义可读流，也可以用node内置的创建可读流的api。

```js
const fs = require('fs');
const path = require('path');

// 把当前js所在的目录中的a.html文件的路径赋值给 fileName
let fileName = path.join(__dirname, 'a.html');

// 创建可读流
let readStream = fs.createReadStream(fileName, {
  flags: 'r',       // 设置文件只读模式打开文件
  encoding: 'utf8'  // 设置读取文件的内容的编码
});

// 打开文件流的事件。
readStream.on('open', fd => {
  console.log('文件可读流已打开，句柄：%s', fd);
});

// 可读流打开后，会源源不断的触发此事件方法，回调函数参数就是读取的数据。
readStream.on('data', data => {
  console.log(data);
});

readStream.on('close', () => {
  console.log('文件可读流结束！');
});
```

## 如何写入文件

写入文件也是一样的，如果是比较少的内容可以一次性写入文件。其他情况可以用流、管道等方式解决。

### 一次性写入少量内容到文件

```js
const fs = require('fs');
const path = require('path');

// 把当前js所在的目录中的a.html文件的路径赋值给 fileName
let fileName = path.join(__dirname, 'a.html');

// 写入文件
fs.writeFile(
  fileName,                                            // 文件名    
  '<h1>aicoder.com 全栈实习不8000就业不还实习费</h1>',     // 写入文件内容
  err => {                                             // 写入成功后的回调函数
    if (err) throw err;
    console.log('文件内容已经写入！');
  }
);
```


### 写入大量数据到文件

写入大量文件的方式，可以用流的方式，可以用管道的方式，使用基本类似。且看代码。

+ 流的方式写入文件。

语法: `fs.createWriteStream(path, [options])`
参数:
  + path 文件路径
  + [options] flags:指定文件操作，默认'w',；encoding,指定读取流编码；start指定写入文件的位置

```js
const fs = require('fs');
const path = require('path');

// 把当前js所在的目录中的a.html文件的路径赋值给 fileName
let fileName = path.join(__dirname, 'msg.log');

// 创建议文件的可写流。
let ws = fs.createWriteStream(fileName, { start: 0 });

ws.on('open', function(fd) {
  console.log('要写入的数据文件已经打开，文件描述符是： ' + fd);
});

// 监听写入异常事件
ws.on('error', err => {
  console.log(err);
});

// 监听写入完成的事件
ws.on('finish', () => {
  console.log('写入结束！');
});

// 写入100个字符串。
for (let i = 0; i < 100; i++) {
  // write方法可以把内容写入到可写流中。
  let w_flag = ws.write('aicoder.com  全栈实习 \r\n');
  //当缓存区写满时，输出false
  console.log(w_flag);
}

ws.end('结束写入！');
```


+ 大文件的复制（综合运用文件流）

大文件复制，可以用一个可读流和一个可写流进行复制，由于读取文件一般比写入文件要快。所以要避免缓冲区不可写入的时候，暂停文件流的读取，可以继续写入的时候，再继续读取数据。

```js
const fs = require('fs');
const path = require('path');

let fileNameSrc = path.join(__dirname, 'jdk.dmg');      // 复制的源文件
let fileNameDist = path.join(__dirname, 'jdk1.dmg');    // 拷贝完的目标文件名

// 创建可读流
let rs = fs.createReadStream(fileNameSrc, { start: 0 });
// 创建可写流
let ws = fs.createWriteStream(fileNameDist, { start: 0 });

rs.on('data', function(chunk) {
  if (ws.write(chunk) === false) {
    // 判断数据流是否已经写入目标了
    rs.pause();
  }
});

// 当可读流结束的时候，让可写流结束。
rs.on('end', function() {
  ws.end(); // 结束可写流
  console.log('文件复制成功');
});

ws.on('drain', function() {
  rs.resume(); // 继续启动读取数据流
});
```

+ 使用管道的方式大文件的复制。

```js
const fs = require('fs');
const path = require('path');

let fileNameSrc = path.join(__dirname, 'jdk.dmg');      // 复制的源文件
let fileNameDist = path.join(__dirname, 'jdk1.dmg');    // 拷贝完的目标文件名  

let rs = fs.createReadStream(fileNameSrc, { start: 0 });
let ws = fs.createWriteStream(fileNameDist, { start: 0 });

rs.on('end', () => {
  console.log('读取完毕');
});
ws.on('finish', () => {
  console.log('写入成功！');
});

// 可读流直接跟可写流建立管道。可读流的数据 直接通过管道流向 可写流
rs.pipe(ws);
/*******************

|----------|
|----------|
|---rs  ---|-----------\
|----------|           |
|__________|           |
                       |
                    |  |       |
                    |  |       |
                    |----------|
                    |  ws      |
                    |__________|

********************/
```

## 文件相关的其他操作

### 重命名

语法：`fs.rename(oldPath, newPath, callback);`
参数：
* oldPath, 原目录/文件的完整路径及名；
* newPath, 新目录/文件的完整路径及名；如果新路径与原路径相同，而只文件名不同，则是重命名
* [callback(err)], 操作完成回调函数；err操作失败对象

```js
fs.rename(__dirname + '/test', __dirname + '/fsDir', function (err) {
  if(err) {
    console.error(err);
    return;
  }
  console.log('重命名成功')
});
```

### 修改文件或目录的操作权限

语法：`fs.utimes(path, mode, callback);`
参数：
* path, 要查看目录/文件的完整路径及名；
* mode, 指定权限，如：0666 8进制，权限：所有用户可读、写，
* [callback(err)], 操作完成回调函数；err操作失败对象
 
```js
fs.chmod(__dirname + '/fsDir', 0666, function (err) {
  if(err) {
    console.error(err);
    return;
  }
  console.log('修改权限成功')
});
```

### 查看文件与目录的是否存在

语法：`fs.exists(path, callback);`
参数：
* path, 要查看目录/文件的完整路径及名；
* [callback(exists)], 操作完成回调函数；exists true存在，false表示不存在

```js
fs.exists(__dirname + '/te', function (exists) {
  var retTxt = exists ? retTxt = '文件存在' : '文件不存在';
  console.log(retTxt);
});
```

## 文件夹读取操作

```js
const fs = require('fs');
const path = require('path');

// 读取文件夹
fs.readdir(__dirname, function(err, files) {
  if (err) {
    console.error(err);
    return;
  }
  // 对文件夹中的所有路径做处理
  files.forEach(file => {
    let filePath = path.join(__dirname, file);
    // 读取文件的信息，判断是文件还是路径
    fs.stat(filePath, function(err, stat) {
      if (err) throw err;
      console.log(filePath, stat.isFile() ? ' is: file' : ' is: dir');
    });
  });
});
```

## 后续的学习

后续，可以实现一下自定义的可写流、可读流，多运用一些管道的方法，多看一下官方的文档，相信您已经可以掌握了文件模块相关的内容。

---
参考：
1. [node.js之fs模块](https://www.jianshu.com/p/5683c8a93511)
1. [Node.js 文件系统fs模块](https://www.cnblogs.com/pingfan1990/p/4707317.html)

---

[老马免费视频教程](https://qtxh.ke.qq.com)

[返回首页](../readme.md)










