# let 与 const 增强变量声明

ES6 新增了`let`命令，用来声明局部变量。它的用法类似于`var`，但是所声明的变量，只在`let`命令所在的代码块内有效，而且有暂时性死区的约束。

先看个`var`的常见变量提升的面试题目：

```js
题目1：
var a = 99;            // 全局变量a
f();                   // f是函数，虽然定义在调用的后面，但是函数声明会提升到作用域的顶部。
console.log(a);        // a=>99,  此时是全局变量的a
function f() {
  console.log(a);      // 当前的a变量是下面变量a声明提升后，默认值undefined
  var a = 10;
  console.log(a);      // a => 10
}

// 输出结果：
undefined
10
99
```

如果以上题目有理解困难的童鞋，请系统的看一下老马的[**免费 JS 高级视频教程**](http://qtxh.ke.qq.com/)。

## ES6 可以用 let 定义块级作用域变量

在 ES6 之前，我们都是用 var 来声明变量，而且 JS 只有函数作用域和全局作用域，没有块级作用域，所以`{}`限定不了 var 声明变量的访问范围。
例如：

```js
{
  var i = 9;
}
console.log(i); // 9
```

ES6 新增的`let`，可以声明块级作用域的变量。

```js
{
  let i = 9; // i变量只在 花括号内有效！！！
}
console.log(i); // Uncaught ReferenceError: i is not defined
```

## let 配合 for 循环的独特应用

`let`非常适合用于 `for`循环内部的块级作用域。JS 中的 for 循环体比较特殊，每次执行都是一个全新的独立的块作用域，用 let 声明的变量传入到 for 循环体的作用域后，不会发生改变，不受外界的影响。看一个常见的面试题目：

```js
for (var i = 0; i <10; i++) {  
  setTimeout(function() {  // 同步注册回调函数到 异步的 宏任务队列。
    console.log(i);        // 执行此代码时，同步代码for循环已经执行完成
  }, 0);
}
// 输出结果
10   共10个
// 这里面的知识点： JS的事件循环机制，setTimeout的机制等

// 解决以上问题
for (var i = 0; i < 10; i++) {
  ((index) => {
    setTimeout(() => {
      console.log(index);
    }, 4);
  })(i);
}
// 输出结果：
0  1  2  3  4  5  6  7  8 9
```

如果把 `var`改成 `let`声明：

```js
// i虽然在全局作用域声明，但是在for循环体局部作用域中使用的时候，变量会被固定，不受外界干扰。
for (let i = 0; i < 10; i++) {
  setTimeout(function() {
    console.log(i);    //  i 是循环体内局部作用域，不受外界影响。
  }, 0);
}
// 输出结果：
0  1  2  3  4  5  6  7  8 9
```

## let 没有变量提升与暂时性死区

用`let`声明的变量，不存在变量提升。而且要求必须 等`let`声明语句执行完之后，变量才能使用，不然会报`Uncaught ReferenceError`错误。
例如：

```js
console.log(aicoder); // 错误：Uncaught ReferenceError ...
let aicoder = 'aicoder.com';
// 这里就可以安全使用aicoder
```

> ES6 明确规定，如果区块中存在 let 和 const 命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。
> 总之，在代码块内，使用 let 命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。

## let 变量不能重复声明

let 不允许在相同作用域内，重复声明同一个变量。否则报错：`Uncaught SyntaxError: Identifier 'XXX' has already been declared`

例如：

```js
let a = 0;
let a = 'sss';
// Uncaught SyntaxError: Identifier 'a' has already been declared
```

## 块接作用域嵌套

```js
// 块接作用域嵌套
{
  let k = 9;
  {
    console.log(k); // => 9
    let m = 10;
    console.log(m); // => 10
  }
  console.log(m); //ReferenceError: m is not defined
}
```

这样原先我们的立即执行函数就可以被块级作用域取代了。

## let 小结

ES6 的 let 让 js 真正拥有了块级作用域，也是向这更安全更规范的路走，虽然加了很多约束，但是都是为了让我们更安全的使用和写代码。

## const 常量声明

`const` 声明一个只读的常量。一旦声明，常量的值就不能改变,并且一旦声明常量，就必须立即初始化，不能留到以后赋值。。

对于一般的简单类型的常量我们一般用大写的字符组成的单词来做名字，比如：

```js
const SITE_URL = 'http://aicoder.com';

// 修改常量会报错。
SITE_URL = 'hamkd.com'; // TypeError: Assignment to constant variable.


const PI; // SyntaxError: Missing initializer in const declaration
PI = 3.1415926; // 不允许声明后改动
```

## const 与 let 同样都属于块级作用域

const 的作用域与 let 命令相同：只在声明所在的块级作用域内有效。

```js
{
  const S_URL = 'http://aicoder.com';
}
console.log(S_URL); // ReferenceError: S_URL is not defined
```

## const 也存在暂时性死区

```js
console.log(M_URL); // ReferenceError: M_URL is not defined
{
  const M_URL = 'http://aicoder.com';
}
```

## const 与复杂类型

const 能确保简单类型的值不能被修改。
如果用 const 声明的复杂类型的变量，那么此复杂类型的变量指向内存地址的指针就不能修改了，但是指向的那块内存地址的内容是可以修改的。

```js
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop; // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```

## let 和 const 声明的变量不属于顶层对象的属性

为了保持兼容性，var 命令和 function 命令声明的全局变量，依旧是顶层对象的属性；let 命令、const 命令、class 命令声明的全局变量，不属于顶层对象的属性。

```js
const a = 9;
window.a; // => undefined
let c = 10;
window.c; // => undefined
var b = 9;
window.b; // => 9
```

## 总结

es6 的语法的确简单方便，要勇敢的拥抱 es6 而不是排斥。

## 参考

[《ECMAScript 6 入门》](http://es6.ruanyifeng.com/)

# [回到ES6知识列表首页](/pages/vip_2ES6.md)