# Vue 事件处理

## 原生的js事件处理

原生的js事件处理，可以分为：直接内联执行代码，或者绑定事件函数。

在内联的事件处理函数内部或者事件绑定的方法内部的作用域中的this都是指向当前的dom对象。如何在vue绑定的元素中通过this访问到vue的实例呢？vue框架做了很多工作，很方便我们进行事件的绑定处理开发。

## 监听事件的 Vue 处理

Vue 提供了协助我们为标签绑定时间的方法，当然我们可以直接用 dom 原生的方式去绑定事件。Vue 提供的指令进行绑定也是非常方便，而且能让 ViewModel 更简洁，逻辑更彻底。所以还是推荐大家使用的。

Vue 提供了`v-on`指令帮助我们进行事件的绑定。
基本的内联事件处理方法[官方 demo]：

``` html
<div id="example-1">
  <!-- 为按钮绑定点击事件，执行counter += 1的任务。 -->
  <button v-on:click="counter += 1">增加 1</button>
  <p>这个按钮被点击了 {{ counter }} 次。</p>
</div>
<script>
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
</script>
```

## 事件处理方法集成到 Vue 对象

内联的方式绑定的事件，只能处理简单的事件的处理逻辑。复杂的情况还是封装到 js 中最方便，也不容易出错。
Vue 对象中可以添加 methods 属性，开发者可以把事件处理函数的逻辑放到 methods 中。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Vue入门之动态显示表格</title>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
</head>
<body>
  <div id="app">
    <p>{{ number }}</p>
    <input type="button" name="btnGetNumber" value="增加[绑定事件处理器]" v-on:click="getNumber">
    <input type="button" name="btnGetNumber" value="增加[内联方法调用]" v-on:click="getNumber()">
  </div>
  <script>
    var app = new Vue({
      el: '#app',
      data: {
        number: 1
      },
      methods: {
        // 事件响应方法的逻辑代码
        getNumber: function (e) {
          this.number += 1;   // 不管是内联方法调用，还是绑定事件处理器两种方式执行事件响应方法的时候 this都是指向 app
        }
      }
    });
  </script>
</body>
</html>
```

## 事件修饰符

在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

为了解决这个问题，Vue.js 为 `v-on` 提供了**事件修饰符**。之前提过，修饰符是由点开头的指令后缀来表示的。

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`
- `.passive`

``` html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```

<p class="tip">使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止**所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击。</p>

> 2.1.4 新增

``` html
<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```

不像其它只能对原生的 DOM 事件起作用的修饰符，`.once` 修饰符还能被用到自定义的[组件事件](components-custom-events.html)上。如果你还没有阅读关于组件的文档，现在大可不必担心。

> 2.3.0 新增

Vue 还对应 [`addEventListener` 中的 `passive` 选项](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener#Parameters)提供了 `.passive` 修饰符。

``` html
<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

这个 `.passive` 修饰符尤其能够提升移动端的性能。

<p class="tip">不要把 `.passive` 和 `.prevent` 一起使用，因为 `.prevent` 将会被忽略，同时浏览器可能会向你展示一个警告。请记住，`.passive` 会告诉浏览器你*不*想阻止事件的默认行为。</p>

## 按键修饰符

在监听键盘事件时，我们经常需要检查常见的键值。Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符：

``` html
<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">
```

记住所有的 `keyCode` 比较困难，所以 Vue 为最常用的按键提供了别名：

``` html
<!-- 同上 -->
<input v-on:keyup.enter="submit">

<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

全部的按键别名：

- `.enter`
- `.tab`
- `.delete` (捕获“删除”和“退格”键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

可以通过全局 `config.keyCodes` 对象[自定义按键修饰符别名](../api/#keyCodes)：

``` js
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

### 自动匹配按键修饰符

> 2.5.0 新增

你也可直接将 [`KeyboardEvent.key`](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/key/Key_Values) 暴露的任意有效按键名转换为 kebab-case 来作为修饰符：

```html
<input @keyup.page-down="onPageDown">
```

在上面的例子中，处理函数仅在 `$event.key === 'PageDown'` 时被调用。

<p class="tip">有一些按键 (`.esc` 以及所有的方向键) 在 IE9 中有不同的 `key` 值, 如果你想支持 IE9，它们的内置别名应该是首选。</p>

## 系统修饰键

> 2.1.0 新增

可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

> 注意：在 Mac 系统键盘上，meta 对应 command 键 (⌘)。在 Windows 系统键盘 meta 对应 Windows 徽标键 (⊞)。在 Sun 操作系统键盘上，meta 对应实心宝石键 (◆)。在其他特定键盘上，尤其在 MIT 和 Lisp 机器的键盘、以及其后继产品，比如 Knight 键盘、space-cadet 键盘，meta 被标记为“META”。在 Symbolics 键盘上，meta 被标记为“META”或者“Meta”。

例如：

```html
<!-- Alt + C -->
<input @keyup.alt.67="clear">

<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```

<p class="tip">请注意修饰键与常规按键不同，在和 `keyup` 事件一起用时，事件触发时修饰键必须处于按下状态。换句话说，只有在按住 `ctrl` 的情况下释放其它按键，才能触发 `keyup.ctrl`。而单单释放 `ctrl` 也不会触发事件。如果你想要这样的行为，请为 `ctrl` 换用 `keyCode`：`keyup.17`。</p>

### `.exact` 修饰符

> 2.5.0 新增

`.exact` 修饰符允许你控制由精确的系统修饰符组合触发的事件。

``` html
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>
```

### 鼠标按钮修饰符

> 2.2.0 新增

- `.left`
- `.right`
- `.middle`

这些修饰符会限制处理函数仅响应特定的鼠标按钮。

## 事件绑定总结

Vue 为了方便大家进行开发，提供了事件的相关的封装，可以让我们方便我们用 Vue 对事件进行开发，尤其是 v-on 指令的非常方便的跟 Vue 对象中 methods 进行配合进行复杂的事件处理，非常方便。另外事件的事件修饰符和按键修饰符也可以让 Vue 事件这块锦上添花。


# [回到vue.js知识列表首页](/pages/vip_2vue.md)