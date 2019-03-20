# global - 全局变量

全局对象（global object）,不要和 全局的对象（ global objects ）或称标准内置对象混淆。这里说的全局的对象是说在全局作用域里的内的对象。全局作用域包含了全局对象的属性，还有它继承来的属性。

**注意浏览器下的全局对象跟 nodejs 中的全局对象不一致**

* 浏览器环境下的全局对象就是`window`
* Node 的全局对象是 `global`

## JS 语言标准的全局的内置对象

JS 语言规范中的全局的内置对象在 Nodejs 中都有效，以下简单过一下，不熟悉请查[MDN 文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)

* 全局属性
  * Infinity
  * NaN
  * undefined
  * null 字面量
* 函数属性
  * eval()
  * uneval()
  * isFinite()
  * isNaN()
  * parseFloat()
  * parseInt()
  * decodeURI()
  * decodeURIComponent()
  * encodeURI()
  * encodeURIComponent()
  * escape()
  * unescape()
* 基本对象与其他内容
  * Object
  * Function
  * Boolean
  * Symbol
  * Error
  * Number
  * Math
  * Date
  * String
  * RegExp
  * Array
  * Map
  * Set
  * JSON
  * ArrayBuffer
  * Promise
  * Generator
  * GeneratorFunction
  * Reflect
  * Proxy
  * arguments

## nodejs 中的全局变量

### 关于模块的补充

这里先简单补充一下模块的概念，后续我们还会更深入讲解一下，笔者不想让复杂的内容让初学者分心，只是想让您能快速先建立学习 Nodejs 的信心。NodeJs 中把不同功能的 api 封装成不同的模块，避免了不同功能的代码相互冲突。当然 NodeJS 也支持开发人员写的 Nodejs 代码模块化。如果你是 Java、DotNet 的程序员，那 Nodejs 的模块就是 DotNet 的程序集或者 Java 的包。

Node 中所有的代码都会被 node 编译器自动包装成模块后再进行解析执行，具体细节后面会有详解。

### console(控制台)

`console` 模块提供了一个简单的调试控制台，类似于 Web 浏览器提供的 JavaScript 控制台。

> 注意：全局的 console 对象的方法既不总是同步的（如浏览器中类似的 API）

全局的`console`对象可以再 node 中任何地方直接调用。接下来看看它的常用方法。

1.  打印日志 console.log

语法：`console.log([data][, ...args])`

打印到输出控制台，并**带上换行符**。 可以传入多个参数，第一个参数作为主要信息（字符串类型），其他参数作为代替值。

```js
var count = 5;
console.log('count: %d', count);
// 打印: count: 5 到 stdout
console.log('count:', count);
// 打印: count: 5 到 stdout
console.log('count:', count, '-222', '-999');
// 打印： count:5-222-999。  log方法可以把多个参数连接一块输出。
```

log 方法的第一个参数是一个字符串，包含零个或多个占位符。 每个占位符会被对应参数转换后的值所替换。 支持的占位符有

```
%s - 字符串。
%d - 数值（整数或浮点数）。
%i - Integer.
%f - Floating point value.
%j - JSON。如果参数包含循环引用，则用字符串 '[Circular]' 替换。
...
```

更多请参考:[Node 文档](http://nodejs.cn/api/util.html#util_util_format_format_args)


> console.info() 函数是 console.log() 的一个别名。也就是说你用log方法跟info方法一致。

2. 打印错误消息和警告信息

语法：`console.error([data][, ...args])`

error方法的使用同 log方法，所以不赘述，我们一般用此方法打印错误消息，一般用log方法打印普通消息。

```js
console.error('error');
```

另外打印警告信息使用warn方法，此方法仅仅提示开发人员一些警示信息，用法同log方法。

语法：`console.warn([data][, ...args])`

3. 打印对象结构

语法： `console.dir(obj[, options])`

参数说明：
* 第一个参数obj，就是要打印属性的对象。

* 第二个参数options是设置打印的配置项：
  - showHidden - 如果为 true，则该对象中的不可枚举属性和 symbol 属性也会显示。默认为 false。

  - depth - 告诉 util.inspect() 函数当格式化对象时要递归多少次。 这对于检查较大的复杂对象很有用。 默认为 2。 设为 null 可无限递归。

  - colors - 如果为 true，则输出会带有 ANSI 颜色代码。 默认为 false。 颜色是可定制的，详见定制 util.inspect() 颜色。

```js
var a = { age: 134, t: { name: '33', mm: '333' } };
console.dir(a);
console.dir(a, {
  colors: true,
  showHidden: true
});

```

> dir方法非常有用，在可以辅助我们调试时查看对象内的属性和继承关系，也是一个非常好的学习js的手段。

4. 时间计时

同浏览器一致，也封装了计时的两个方法：`console.time(label)`和`console.timeEnd(label)`;

time方法启动一个定时器，用以计算一个操作的持续时间。 定时器由一个唯一的 label 标识。 当调用 console.timeEnd() 时，可以使用相同的 label 来停止定时器，并以毫秒为单位将持续时间输出到 stdout。 定时器持续时间精确到亚毫秒。

```js
console.time('lb1');
for (let i = 0; i < 100; i++) {}
console.timeEnd('lb1');
// 打印 lb1: 23.43ms
```

5. 其他方法

说明|实例
---|---
打印堆栈跟踪在代码 | `console.trace([message][, ...args])`
计数器的显示标签|`console.count([label])` 
重置指定 label 的内部计数器。|`console.countReset([label='default'])`
清空控制台| `console.clear()`
断言|`console.assert(value[, message][, ...args])`


console的方法还比较多，这里就不一一列举了，有兴趣的可以直接参考[中文文档](http://nodejs.cn/api/console.html)


### 定时器

node提供了全局的3个设置定时器方法：
* `setImmediate(callback[, ...args])`
* `setInterval(callback, delay[, ...args])`
* `setTimeout(callback, delay[, ...args])`

另外对应3个取消定时器的方法
* `clearImmediate(immediate)`
* `clearInterval(timeout)`
* `clearTimeout(timeout)`

三个方法，使用上基本都表现一致，这里只说一种情况，其他类比。

```
参数说明：
  callback <Function> 当定时器到点时要调用的函数。
  delay <number> 调用 callback 之前要等待的毫秒数。
  ...args <any> 当调用 callback 时要传入的可选参数。
```

```js
var timer = setTimeout(()=>{
  console.log(123);
}, 100);

// .... js代码

clearTimeout(timer);

```

> callback 可能不会精确地在 delay 毫秒被调用。 Node.js 不能保证回调被触发的确切时间，也不能保证它们的顺序。 回调会在尽可能接近所指定的时间上调用。
> 注意：当 delay 大于 2147483647 或小于 1 时，delay 会被设为 1。

### 其他全局变量

另外全局还提供了 `Buffer`、模块相关变量、`process`等全局变量。

这些内容，等我们后续章节再详细介绍。

到此为止，您如果熟悉前端开发的话，这不就是前端js移到了nodejs上执行了吗？的确如此，但是这运行的环境已经脱离了浏览器，是直接运行在操作系统之上了。

那么nodejs背后是怎样运行的？模块机制是怎么搞到的？且看下回再解。

---

参考：

1. [NodeJs 官网文档](https://nodejs.org/)
1. [MDN 文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)

---

[老马免费视频教程](https://qtxh.ke.qq.com)

## [回到nodejs知识列表首页](/pages/nodejs.md)
