# Vue 入门之数据绑定

## 什么是双向绑定？

Vue 框架很核心的功能就是双向的数据绑定。
双向是指：HTML 标签数据 绑定到 Vue 对象，另外反方向数据也是绑定的。通俗点说就是，Vue 对象的改变会直接影响到 HTML 的标签的变化，而且标签的变化也会反过来影响 Vue 对象的属性的变化。  
这样以来，就彻底变革了之前 Dom 的开发方式，之前 Dom 驱动的开发方式尤其是以 jQuery 为主的开发时代，都是 dom 变化后，触发 js 事件，然后在事件中通过 js 代码取得标签的变化，再跟后台进行交互，然后根据后台返回的结果再更新 HTML 标签，异常的繁琐。有了 Vue 这种双向绑定，让开发人员只需要关心 json 数据的变化即可，Vue 自动映射到 HTML 上，而且 HTML 的变化也会映射回 js 对象上，开发方式直接变革成了前端由数据驱动的
开发时代，远远抛弃了 Dom 开发主导的时代了。

![vue 双向绑定](imgs/02vue双向绑定.jpg)

## Vue 绑定文本

数据绑定最常见的形式就是使用 “Mustache” 语法（双大括号）的文本插值，比如模板引擎：handlebars 中就是用的`{{}}`.  
创建的 Vue 对象中的 data 属性就是用来绑定数据到 HTML 的。参考如下代码：

```html
<span>Message: {{ msg }}</span>
<script>
  var app = new Vue({         // 创建Vue对象。Vue的核心对象。
    el: '#app',               // el属性：把当前Vue对象挂载到 div标签上，#app是id选择器
    data: {                   // data: 是Vue对象中绑定的数据
      msg: 'Hello Vue!'   // message 自定义的数据
    }
  });
</script>
```

## 绑定数据中使用 JavaScript 表达式

对于所有的数据绑定， Vue.js 都提供了完全的 JavaScript 表达式支持。

```html
<span>Message: {{ msg + ' - ' + name }}</span>
<script>
  var app = new Vue({         // 创建Vue对象。Vue的核心对象。
    el: '#app',               // el属性：把当前Vue对象挂载到 div标签上，#app是id选择器
    data: {                   // data: 是Vue对象中绑定的数据
      msg: 'Hi',              // message 自定义的数据
      name: 'flydragon'       // name自定义的属性，vue可以多个自定义属性，属性类型也可是复杂类型
    }
  });
</script>
```

结果：

```html
Hi - flydragon
```

当然 Vue 还可以支持表达中的任何计算、函数处理等。参考下面的综合点的案例。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Vue入门之数据绑定-表达式运算</title>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
</head>
<body>
  <div id="app">
    {{ msg + ' - ' + name }}
    <p>
      {{ isOk ? '123' : '456' }}
    </p>
    <p>我的年龄是： {{ age *2 }}</p>
  </div>

  <script>
  var app = new Vue({         // 创建Vue对象。Vue的核心对象。
    el: '#app',               // el属性：把当前Vue对象挂载到 div标签上，#app是id选择器
    data: {                   // data: 是Vue对象中绑定的数据
      msg: 'Hi',              // message 自定义的数据
      name: 'flydragon',
      isOk: true,
      age: 18
    }
  });
  </script>
</body>
</html>
```

## Vue 属性绑定

Vue 中不能直接使用`{{ expression }}` 语法进行绑定 html 的标签属性进行绑定，而是用它特有的 v-bind 指令（就是一种写法，先按照格式走，具体指令是什么可以后续再了解）。

绑定的语法结构：

```html
<标签 v-bind:属性名="要绑定的Vue对象的data里的属性名"></标签>
例如:
<span v-bind:id="menuId">{{ menuName }}</span>
```

参考如下代码案例：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Vue入门之数据绑定--属性绑定</title>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
</head>
<body>
  <div id="app">
    <div v-bind:id="MenuContaineId">
      <a href="#" v-bind:class="MenuClass">首页</a>
      <a href="#" v-bind:class="MenuClass">产品</a>
      <a href="#" v-bind:class="MenuClass">服务</a>
      <a href="#" v-bind:class="MenuClass">关于</a>
    </div>
  </div>

  <script>
    var app = new Vue({
      el: '#app',
      data: {                   // data: 是Vue对象中绑定的数据
        MenuClass: 'top-menu',
        MenuContaineId: 'sitemenu'
      }
    });
  </script>
</body>
</html>
```

## 属性绑定简写

由于`v-bind` 使用非常频繁，所以 Vue 提供了简单的写法，可以去掉 v-bind 直接使用`:`即可。

```html
例如：
<div :id="MenuContaineId">
等价于
<div v-bind:id="MenuContaineId">
```

## 输出纯 HTML

由于 Vue 对于输出绑定的内容做了提前 encode，保障在绑定到页面上显示的时候不至于被 xss 攻击。但某些场景下，我们确保后台数据是安全的，那么我们就要在网页中显示原生的 HTML 标签。Vue 提供了`v-html`指令。

```html
<div id="app">
  <div v-bind:id="MenuContaineId" v-html="MenuBody">
  </div>
</div>
<script>
  var app = new Vue({
    el: '#app',
    data: {                   // data: 是Vue对象中绑定的数据
      MenuContaineId: 'menu',
      MenuBody: '<p>这里是菜单的内容</p>'
    }
  });
</script>
```

结果：

```html
<div id="app">
  <div id="menu">
    <p>这里是菜单的内容</p>
  </div>
</div>
```

## 布尔类型值用于属性绑定

标签的布尔类型的特性（属性），比如： `disabled`特性。这类属性特点只要存在就表示 `true`，`v-bind` 应用于这类属性的时候，如果绑定值为真，则输出挣钱的对应属性。如果为假值，则不会渲染此特性。

``` html
<button v-bind:disabled="isButtonDisabled">按钮</button>
```

如果 `isButtonDisabled` 的值是 `null`、`undefined` 或 `false`，则 `disabled` 特性甚至不会被包含在渲染出来的 `<button>` 元素中。
如果为真值：`<button disabled="disabled">按钮</button>`

## 样式绑定

对于普通的属性的绑定，只能用上面的讲的绑定属性的方式。而 Vue 专门加强了 class 和 style 的属性的绑定。可以有复杂的对象绑定、数组绑定样式和类。

### 绑定样式对象

经常我们需要对样式进行切换，比如：div 的显示和隐藏，某些标签 active 等。Vue 提供的对象绑定样式的方式就很容做这些事情。

```html
代码：
<div v-bind:class="{ active: isActive }"></div>
解释：
当 isActive为 true时， div就会具有了active样式类，如果 isActive为false，那么div就去掉active样式类。
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Vue入门之绑定样式类</title>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
  <style>
  .active {
    background-color: #ccc;
  }
  </style>
</head>
<body>
  <div id="app">
    <div v-bind:id="MenuContaineId" v-bind:class="{ active: isActive }">
      绑定颜色类
    </div>
  </div>
  <script>
    var app = new Vue({
      el: '#app',
      data: {                   // data: 是Vue对象中绑定的数据
        MenuContaineId: 'menu',
        isActive: true
      }
    });
  </script>
</body>
</html>
```

### 混合普通的 HTML 标签样式类及绑定样式对象

v-bind:class 指令可以与普通的 class 属性共存。

```html
<div id="app">
  <div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
  </div>
</div>
<script>
  var app = new Vue({
    el: '#app',
    data: {          // data: 是Vue对象中绑定的数据
      isActive: true,
      hasError: false
    }
  });
</script>
```

结果：

```html
<div id="app">
  <div class="static active">
  </div>  
</div>
```

### 绑定 data 中的样式对象

直接在 html 属性中的双引号内写对象，还是很不爽，也没有智能提示，很容易写错。
Vue 可以让我们直接把绑定的 class 字符串指向 data 的一个对象，这样就非常方便了，既可以有智能提示，又可以很复杂进行编辑，不用担心烦人的`""`了。

```html
<div id="app">
  <div class="static"
     v-bind:class="classObject">
  </div>
</div>
<script>
  var app = new Vue({
    el: '#app',
    data: {
      classObject: {
        active: true,
        'text-danger': false
      }
    }
  });
</script>
```

结果：

```html
<div id="app">
  <div class="static active">
  </div>
</div>
```

### 绑定样式数组

其实绑定数组，就是绑定样式对象的延续，看官网的例子代码吧。

```html
<div v-bind:class="[activeClass, errorClass]">

data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

当然还有很多其他很有趣的支持，就不赘述了。

```html
例如:
<div v-bind:class="[isActive ? activeClass : '', errorClass]">
<div v-bind:class="[{ active: isActive }, errorClass]">
```

### 内联样式绑定

内联样式的绑定，非常类似于样式类的操作。v-bind:style 的对象语法十分直观——看着非常像 CSS ，其实它是一个 JavaScript 对象。 CSS 属性名可以用驼峰式（camelCase）或短横分隔命名（kebab-case）。

看个例子：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Vue入门之htmlraw</title>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
</head>
<body>
  <div id="app">
    <div v-bind:style="{fontSize: size + 'px', backgroundColor: bgcolor, width: width}">
      vue 入门系列教程
    </div>
  </div>
  <script>
    var app = new Vue({
      el: '#app',
      data: {
        size: 19,
        width: 200,
        bgcolor: 'red'
      }
    });
  </script>
</body>
</html>
```

自动添加前缀  
当 v-bind:style 使用需要特定前缀的 CSS 属性时，如 transform ，Vue.js 会自动侦测并添加相应的前缀。

## 计算属性

在做数据的绑定的时候,数据要进行处理之后才能展示到 html 页面上，虽然 vue 提供了非常好的表达式绑定的方法，但是只能应对低强度的需求。比如： 把一个日期按照规定格式进行输出，可能就需要我们对日期对象做一些格式化的出来，表达式可能就捉襟见肘了。

Vue 对象提供的 computed 属性，可以让我们开发者在里面可以放置一些方法，协助我们绑定数据操作，这些方法可以跟 data 中的属性一样用，注意这些方法用的时候不要加`()`。
例子来了：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Vue入门之htmlraw</title>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
</head>
<body>
  <div id="app">
    <table>
      <tr>
        <!-- computed里面的函数可以直接当成data里面的属性用，非常方便，注意没有括号！！！-->
        <td>生日</td><td>{{ getBirthday }}</td>
      </tr>
      <tr>
        <td>年龄</td><td>{{ age }}</td>
      </tr>
      <tr>
        <td>地址</td><td>{{ address }}</td>
      </tr>
    </table>
  </div>
  <script>
    var app = new Vue({
      el: '#app',
      data: {
        birthday: 914228510514,     // 这是一个日期对象的值：1998年11月1日
        age: 19,
        address: '北京昌平区龙泽飞龙'
      },
      computed: {
        // 把日期换成 常见规格格式的字符串。
        getBirthday: function () {
          var m = new Date(this.birthday);
          return m.getFullYear() + '年' + m.getMonth() +'月'+ m.getDay()+'日';
        }
      }
    });
  </script>
</body>
</html>
```

## 绑定的数据过滤器

过滤器本质就是数据在呈现之前先进行过滤和筛选。官网上写的不错，我就不再赘述，下面是官网的描述。

Vue.js 允许你自定义过滤器，可被用于一些常见的文本格式化。过滤器可以用在两个地方：**双花括号插值和 `v-bind` 表达式** (后者从 2.1.0+ 开始支持)。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符号指示：

``` html
<!-- 在双花括号中 -->
{{ message | capitalize }}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>
```

你可以在一个组件的选项中定义本地的过滤器：

``` js
filters: {
  capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  }
}
```

或者在创建 Vue 实例之前全局定义过滤器：

``` js
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})

new Vue({
  // ...
})
```

下面这个例子用到了 `capitalize` 过滤器：

``` html
<div id="example-1" class="demo">
  <input type="text" v-model="message">
  <p>{{ message | capitalize }}</p>
</div>
<script>
  new Vue({
    el: '#example-1',
    data: function () {
      return {
        message: 'john'
      }
    },
    filters: {
      capitalize: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
      }
    }
  })
</script>
```

过滤器函数总接收表达式的值 (之前的操作链的结果) 作为第一个参数。在上述例子中，`capitalize` 过滤器函数将会收到 `message` 的值作为第一个参数。

过滤器可以串联：

``` html
{{ message | filterA | filterB }}
```

在这个例子中，`filterA` 被定义为接收单个参数的过滤器函数，表达式 `message` 的值将作为参数传入到函数中。然后继续调用同样被定义为接收单个参数的过滤器函数 `filterB`，将 `filterA` 的结果传递到 `filterB` 中。

过滤器是 JavaScript 函数，因此可以接收参数：

``` html
{{ message | filterA('arg1', arg2) }}
```

这里，`filterA` 被定义为接收三个参数的过滤器函数。其中 `message` 的值作为第一个参数，普通字符串 `'arg1'` 作为第二个参数，表达式 `arg2` 的值作为第三个参数。

## 核心：自动响应对象的变化到 HTML 标签

上面的例子都是 数据对象是写死在创建的 Vue 对像上，那如果数据（data）发生改变时会怎样呢？
让我们用 chrome 把上面例子的页面打开，并打开发者工具控制台,输入：`app.age = 20` 会有什么情况发生呢？

---

![响应](imgs/03vue响应.png)

在页面中添加一个按钮，动态的增加年龄：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Vue入门之htmlraw</title>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
</head>
<body>
  <div id="app">
    <table>
      <tr>
        <!-- computed里面的函数可以直接当成data里面的属性用，非常方便，注意没有括号！！！-->
        <td>生日</td><td>{{ getBirthday }}</td>
      </tr>
      <tr>
        <td>年龄</td><td>{{ age }}</td>
      </tr>
      <tr>
        <td>地址</td><td>{{ address }}</td>
      </tr>
    </table>
  </div>

  <!-- 添加下面这行代码，动态增加 年龄，页面会有怎样的变化呢？？ -->
  <button type="button" onclick="app.age+=1;" >加加</button>
  <script>
    var app = new Vue({
      el: '#app',
      data: {
        birthday: 914228510514,     // 这是一个日期对象的值：1998年11月1日
        age: 19,
        address: '北京昌平区龙泽飞龙'
      },
      computed: {
        // 把日期换成 常见规格格式的字符串。
        getBirthday: function () {
          var m = new Date(this.birthday);
          return m.getFullYear() + '年' + m.getMonth() +'月'+ m.getDay()+'日';
        }
      }
    });
  </script>
</body>
</html>
```

## 双向数据绑定

上面的例子我们大多讲的是单向的 js 对象向 HTML 数据进行绑定，那 HTML 怎样向 js 进行反馈数据呢？
HTML 中只有表达能接受用户的输入，最简单的演示双向绑定的就是文本框了。

Vue 提供了一个新的指令：v-model 进行双向数据的绑定，注意不是 v-bind。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Vue入门之htmlraw</title>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
</head>
<body>
  <div id="app">
    <!-- v-model可以直接指向data中的属性，双向绑定就建立了 -->
    <input type="text" name="txt" v-model="msg">
    <p>您输入的信息是：{{ msg }}</p>
  </div>
  <script>
    var app = new Vue({
      el: '#app',
      data: {
        msg: '双向数据绑定的例子'
      }
    });
  </script>
</body>
</html>
```

最终的结果就是：你改变 input 文本框的内容的时候，p 标签中的内容会跟着进行改变，哇是不是很神奇呢...

关于其他表单的绑定的语法我就不赘述了，还是参考官网吧，我这里大部分例子也是来自[官网](https://cn.vuejs.org/v2/guide/forms.html#基础用法)。

## 数据绑定总结

vue 提供了大量的绑定的语法和方法，非常方便我们进行数据的绑定，尤其它是双向的数据绑定，极大的减少了我们 dom 操作的麻烦程度。可能你越来越喜欢它了吧...

## 表单绑定详解

你可以用 `v-model` 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 `v-model` 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

<p class="tip">`v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 `data` 选项中声明初始值。</p>

<p class="tip" id="vmodel-ime-tip">对于需要使用[输入法](https://zh.wikipedia.org/wiki/%E8%BE%93%E5%85%A5%E6%B3%95) (如中文、日文、韩文等) 的语言，你会发现 `v-model` 不会在输入法组合文字过程中得到更新。如果你也想处理这个过程，请使用 `input` 事件。</p>

### 文本

``` html
<div id="example-1">
  <input v-model="message" placeholder="edit me">
  <p>Message is: {{ message }}</p>
</div>
<script>
new Vue({
  el: '#example-1',
  data: {
    message: ''
  }
})
</script>
```

### 多行文本

``` html
<div id="example-textarea">
  <span>Multiline message is:</span>
  <p style="white-space: pre-line;">{{ message }}</p>
  <br>
  <textarea v-model="message" placeholder="add multiple lines"></textarea>
</div>
<script>
new Vue({
  el: '#example-textarea',
  data: {
    message: ''
  }
})
</script>
```

<p class="tip">在文本区域插值 (&lt;textarea&gt;{{text}}&lt;/textarea&gt;) 并不会生效，应用 `v-model` 来代替。</p>

### 复选框

单个复选框，绑定到布尔值：

``` html
<div id="example-2">
  <input type="checkbox" id="checkbox" v-model="checked">
  <label for="checkbox">{{ checked }}</label>
</div>
<script>
new Vue({
  el: '#example-2',
  data: {
    checked: false
  }
})
</script>
```

多个复选框，绑定到同一个数组：

``` html
<div id="example-3">
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>Checked names: {{ checkedNames }}</span>
</div>
<script>
new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
</script>
```

### 单选按钮

``` html
<div id="example-4">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>
<script>
new Vue({
  el: '#example-4',
  data: {
    picked: ''
  }
})
</script>
```

### 选择框

单选时：

``` html
<div id="example-5">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
<script>
new Vue({
  el: '#example-5',
  data: {
    selected: ''
  }
})
</script>
```

<p class="tip">如果 `v-model` 表达式的初始值未能匹配任何选项，`select` 元素将被渲染为“未选中”状态。在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 change 事件。因此，更推荐像上面这样提供一个值为空的禁用选项。</p>

多选时 (绑定到一个数组)：

``` html
<div id="example-6">
  <select v-model="selected" multiple style="width: 50px;">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <span>Selected: {{ selected }}</span>
</div>
<script>
new Vue({
  el: '#example-6',
  data: {
    selected: []
  }
})
</script>
```

用 `v-for` 渲染的动态选项：

``` html
<div id="example-7">
  <select v-model="selected">
    <option v-for="option in options" v-bind:value="option.value">
      {{ option.text }}
    </option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
<script>
new Vue({
  el: '#example-7',
  data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
})
</script>
```

## 值绑定

对于单选按钮，复选框及选择框的选项，`v-model` 绑定的值通常是静态字符串 (对于复选框也可以是布尔值)：

``` html
<!-- 当选中时，`picked` 为字符串 "a" -->
<input type="radio" v-model="picked" value="a">

<!-- `toggle` 为 true 或 false -->
<input type="checkbox" v-model="toggle">

<!-- 当选中第一个选项时，`selected` 为字符串 "abc" -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```

但是有时我们可能想把值绑定到 Vue 实例的一个动态属性上，这时可以用 `v-bind` 实现，并且这个属性的值可以不是字符串。

### 复选框

``` html
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no"
>
```

``` js
// 当选中时
vm.toggle === 'yes'
// 当没有选中时
vm.toggle === 'no'
```

<p class="tip">这里的 `true-value` 和 `false-value` 特性并不会影响输入控件的 `value` 特性，因为浏览器在提交表单时并不会包含未被选中的复选框。如果要确保表单中这两个值中的一个能够被提交，(比如“yes”或“no”)，请换用单选按钮。</p>

### 单选按钮

``` html
<input type="radio" v-model="pick" v-bind:value="a">
```

``` js
// 当选中时
vm.pick === vm.a
```

### 选择框的选项

``` html
<select v-model="selected">
    <!-- 内联对象字面量 -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```

``` js
// 当选中时
typeof vm.selected // => 'object'
vm.selected.number // => 123
```

## 修饰符

### `.lazy`

在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 (除了[上述](#vmodel-ime-tip)输入法组合文字时)。你可以添加 `lazy` 修饰符，从而转变为使用 `change` 事件进行同步：

``` html
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg" >
```

### `.number`

如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符：

``` html
<input v-model.number="age" type="number">
```

这通常很有用，因为即使在 `type="number"` 时，HTML 输入元素的值也总会返回字符串。

### `.trim`

如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符：

```html
<input v-model.trim="msg">
```

# [回到vue.js知识列表首页](/pages/vip_2vue.md)