# path 模块详解

`path` 模块提供了一些工具函数，用于处理文件与目录的路径。由于 windows 和其他系统之间路径不统一，`path`模块还专门做了相关处理，屏蔽了彼此之间的差异。

## 可移植操作系统接口(POSIX)

可移植操作系统接口（英语：Portable Operating System Interface，缩写为 POSIX），是 IEEE 为要在各种 UNIX 操作系统上运行软件，而定义 API 的一系列互相关联的标准的总称，其正式称呼为 IEEE Std 1003，而国际标准名称为 ISO/IEC 9945。此标准源于一个大约开始于 1985 年的项目。POSIX 这个名称是由理查德·斯托曼应 IEEE 的要求而提议的一个易于记忆的名称。它基本上是 Portable Operating System Interface（可移植操作系统接口）的缩写，而 X 则表明其对 Unix API 的传承。

Linux 基本上逐步实现了 POSIX 兼容，但并没有参加正式的 POSIX 认证。

微软的 Windows NT 声称部分实现了 POSIX 标准。

当前的 POSIX 主要分为四个部分：Base Definitions、System Interfaces、Shell and Utilities 和 Rationale。

**综述：目前主流的类 Unix 操作系统：Unix、Linux 都会兼容 POSIX 的标准，而 Windows 只是部分实行了 POSIX 标准，所以后面我们说 POSIX 系统是指类 Unix 系统**

## windows 系统和类 Unix 系统的路径的区别

### 目录结构的区别

可能大家比较熟悉 windows 资源管理系统，windows 是分不同的磁盘，然后磁盘下面都是树状结构的文件和文件夹。

而类 Unix（Unix、Linux）系统中是不分盘符的，只有一个根目录 `/`， 都是都是这个下面的子目录或者文件，当然也是树状的机构。

Linux 的目录结构

![linux的目录结构](../img/03.jpeg);

### 路径的区别

除了目录结构有区别外，路径也是有区别的。windows 是用反斜杠`\`分割目录或者文件的，而在类 Unix 的系统中是用的`/`。

```
windows的路径： C:\temp\myfile.html
类Unix的路径：  /tmp/myfile.html
```

## path 模块获取路径中的文件名

语法：`path.basename(path[, ext])`

参数：

* path <string> 完整文件名路径
* ext <string> 可选的文件扩展名
* 返回: <string> 文件名

例如：

```js
path.basename('/foo/bar/baz/asdf/quux.html');
// 返回: 'quux.html'

path.basename('/foo/bar/baz/asdf/quux.html', '.html');
// 返回: 'quux'
```

> 注意：如果 path 不是一个字符串或提供了 ext 但不是一个字符串，则抛出 TypeError。

完整实例：

```js
const path = require('path'); // 引入path模块
let linuxPath = '/Users/aicoder/abc.html';
let name = path.basename(linuxPath);
console.log(name);

let winPath = 'c:\\temp\\abc.html';
let winName = path.basename(linuxPath);
console.log(winName);

console.log(path.basename(linuxPath, '.html')); // => abc,去掉后缀输出文件名

// 输出结果
abc.html;
abc.html;
abc;
```

## node 提供了 win32 和 posix 兼容的 api

默认情况下，node 会根据不同的系统做相关兼容处理，力保输出的结果在不同平台下是一致的，但是某些情况下还是不能完美的兼容所有的情况。所以，node 提供了`win32`和`posix`各自对应 path 的所有的 api。也就是说：`path`模块的 api 都可以通过`path.win32` 或者 `path.posix`调用。

要想在任何操作系统上处理 Windows 文件路径时获得一致的结果，可以使用 `path.win32`

```js
path.win32.basename('C:\\temp\\myfile.html');
// 返回: 'myfile.html'
```

要想在任何操作系统上处理 POSIX 文件路径时获得一致的结果，可以使用 `path.posix`

```js
path.posix.basename('/tmp/myfile.html');
// 返回: 'myfile.html'
```

其他 api 也是一致的，不再赘述。

## 获取路径的文件夹

`path.dirname()` 方法返回一个 path 的目录名。

语法： `path.dirname(path)`
参数：

* `path <string>` ，要返回路径的变量
* 返回: `<string>`

```js
path.dirname('/foo/bar/baz/asdf/quux');
// 返回: '/foo/bar/baz/asdf'
```

## 获取路径的扩展名

`path.extname()` 方法返回 path 的扩展名，即从 path 的最后一部分中的最后一个 .（句号）字符到字符串结束。  
如果 path 的最后一部分没有 `.` 或 path 的文件名的第一个字符是 `.`，则返回一个空字符串。

语法： `path.extname(path)`

* 参数： `path` 是 `<string>`类型。
* 返回: <string>

```js
path.extname('index.html');
// 返回: '.html'
path.extname('/etc/a/index.html');
// 返回: '.html'

path.extname('index.coffee.md');
// 返回: '.md'

path.extname('index.');
// 返回: '.'

path.extname('index');
// 返回: ''

path.extname('.index');
// 返回: ''
```

## 格式化一个路径

`path.format()` 方法会从一个对象返回一个路径字符串。

语法：`path.format(pathObject)`

* pathObject <Object> 要转换成路径字符串的设置对象
  * dir <string> 所在目录，提供了 pathObject.dir，则 pathObject.root 会被忽略
  * root <string> 根目录
  * base <string> 文件全名。如果`pathObject.base` 存在，则 `pathObject.ext` 和 `pathObject.name` 会被忽略
  * name <string> 文件名字（不带后缀）
  * ext <string> 文件后缀
* 返回: <string> 返回完整路径字符串

```js
path.format({
  dir: '/home/user/dir',
  base: 'file.txt'
});
// 返回: '/home/user/dir/file.txt'

path.format({
  root: '/',
  name: 'file',
  ext: '.txt'
});
// 返回: '/file.txt'
```

## 把路径字符串转换成对象

`path.parse()` 方法返回一个对象，对象的属性表示 path 的元素。

`parse`方法跟 `format`方法正好相反，所以不赘述。直接看例子：

```js
let pathObj = path.parse('/users/home/aicoder/a.html');
console.dir(pathObj);

// 输出如下
{ root: '/',
  dir: '/users/home/aicoder',
  base: 'a.html',
  ext: '.html',
  name: 'a' }
```

## 连接多个路径**重点**

`path.join()` 方法使用平台特定的分隔符把全部给定的 path 片段连接到一起，并规范化生成的路径。  
长度为零的 path 片段会被忽略。 如果连接后的路径字符串是一个长度为零的字符串，则返回 '.'，表示当前工作目录。

参数说明：  
`...paths <string>` 一个路径片段的序列。返回: <string>

```js
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..');
// 返回: '/foo/bar/baz/asdf'
path.join('/foot', __filename); // __filename是模块内的变量，代表当前js文件
// 返回：/foot/xxx.js

// 以下路径拼接的方式不推荐：
var strPath = '/foot/' + 'xxx.js'; //虽然跟上面实现的方式一致，但是不推荐。
```

> 注意：不推荐路径直接进行字符串拼接，毕竟 win 和 POSIX 系统路径有区别。

## 获取相对路径

path.relative() 方法返回从 from 到 to 的相对路径（基于当前工作目录）。  
如果 from 和 to 各自解析到同一路径（调用 path.resolve()），则返回一个长度为零的字符串。  
如果 from 或 to 传入了一个长度为零的字符串，则当前工作目录会被用于代替长度为零的字符串。

语法： `path.relative(from, to)`
参数：

* from <string> 求相对路径的原始路径。
* to <string> 求相对路径的最终路径。
* 返回: <string> 返回相对于 from 的 to 的相对路径。

```js
path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb');
// 返回: '../../impl/bbb'
```

## 智能解析绝对路径

path.resolve() 方法会把一个路径或路径片段的序列解析为一个绝对路径。

```
规则：
1. 给定的路径的序列是从右往左被处理的，后面每个 path 被依次解析，直到构造完成一个绝对路径。
2. 如果处理完全部给定的 path 片段后还未生成一个绝对路径，则当前工作目录会被用上。
3. 生成的路径是规范化后的，且末尾的斜杠会被删除，除非路径被解析为根目录。
4. 长度为零的 path 片段会被忽略。
5. 如果没有传入 path 片段，则 path.resolve() 会返回当前工作目录的绝对路径。
```

```js
path.resolve('/foo/bar', './baz');
// 返回: '/foo/bar/baz'
path.resolve('/foo/bar', '/tmp/file/');
// 返回: '/tmp/file'
path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif');
// 如果当前工作目录为 /home/myself/node，
// 则返回 '/home/myself/node/wwwroot/static_files/gif/image.gif'
```

## 对路径字符串进行规范化

path.normalize() 方法会规范化给定的 path，并解析 '..' 和 '.' 片段。  
当发现多个连续的路径分隔符时（如 POSIX 上的 / 与 Windows 上的 \ 或 /），它们会被单个的路径分隔符（POSIX 上是 /，Windows 上是 \）替换。 末尾的多个分隔符会被保留。  
如果 path 是一个长度为零的字符串，则返回 '.'，表示当前工作目录。

语法： `path.normalize(path)`

* path <string> 要进行规范的路径字符串
* 返回: <string> 规范后的路径字符串

```js
path.normalize('/foo/bar//baz/asdf/quux/..');
// 返回: '/foo/bar/baz/asdf

// windows 上
path.normalize('C:\\temp\\\\foo\\bar\\..\\');
// 返回: 'C:\\temp\\foo\\'
```

## 平台兼容的分隔符

### 路径片段分隔符：

Windows 上是 `\`
POSIX 上是 `/`

为了兼容不同平台，node 提供了一个 path 的辅助属性`path.sep`来兼容不同平台下的路径片段分隔符。

```js
console.log(path.sep); // POSIX: /     windows:  \

// 在 POSIX 上：
'foo/bar/baz'.split(path.sep);
// 返回: ['foo', 'bar', 'baz']

//在 Windows 上：
'foo\\bar\\baz'.split(path.sep);
// 返回: ['foo', 'bar', 'baz']
```

### 路径分隔符

平台路径分隔符是不同的：
`Windows` 上是 `;`
`POSIX` 上是 `:`

node 也做了兼容处理，提供了`path.delimiter`来实现平台兼容。

例如，我们常见的 path 环境变量上做分割处理：

```js
//在 POSIX 上：
console.log(process.env.PATH);
// 输出: '/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin'
process.env.PATH.split(path.delimiter);
// 返回: ['/usr/bin', '/bin', '/usr/sbin', '/sbin', '/usr/local/bin']

//在 Windows 上：
console.log(process.env.PATH);
// 输出: 'C:\Windows\system32;C:\Windows;C:\Program Files\node\'
process.env.PATH.split(path.delimiter);
// 返回: ['C:\\Windows\\system32', 'C:\\Windows', 'C:\\Program Files\\node\\']
```

## 判断是否是绝对路径

`path.isAbsolute(path)`此方法接受一个字符串，返回 boolean 类型。

```js
// POSIX
path.isAbsolute('/foo/bar'); // true
path.isAbsolute('qux/'); // false
// Windows
path.isAbsolute('C:\\foo\\..'); // true
path.isAbsolute('bar\\baz'); // false
```

## 总结

node 的 path 模块使用非常简单，而且老马简单看了一下 node 的源码，写的非常精彩，对于多种情况的处理都很恰到好处，推荐大家看 node 的 path 模块源码： [/lib/path.js](https://github.com/nodejs/node/blob/master/lib/path.js)。

---

[老马免费视频教程](https://qtxh.ke.qq.com)

[返回首页](../readme.md)
