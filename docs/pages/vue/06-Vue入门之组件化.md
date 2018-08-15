# Vue 入门之组件化开发

组件其实就是一个拥有样式、动画、js 逻辑、HTML 结构的综合块。前端组件化确实让大的前端团队更高效的开发前端项目。而作为前端比较流行的框架之一，Vue 的组件和也做的非常彻底，而且有自己的特色。尤其是她单文件组件开发的方式更是非常方便，而且第三方工具支持也非常丰富，社区也非常活跃，第三方组件也呈井喷之势。当然学习和使用 Vue 的组件也是我们的最重要的目标。

## 全局扩展方法`Vue.extend`

Vue 提供了一个全局的 API，`Vue.extend`可以帮助我们对 Vue 实例进行扩展，扩展完了之后，就可以用此扩展对象创建新的 Vue 实例了。
类似于继承的方式。

``` html
语法：Vue.extend( options )

参数：

{Object} options
用法：

使用基础 Vue 构造器，创建一个“子类”。参数是一个包含组件选项的对象[后面会细讲]。

data 选项是特例，需要注意 - 在 Vue.extend() 中它必须是函数
```

下面是一个官网 demo：

```html
<div id="mount-point"></div>
<script>
// 创建构造器
var Profile = Vue.extend({
  // 新的对象的模板，所有子实例都会拥有此模板
  template: '<p>{{firstName}} {{lastName}} aka {{alias}}</p>',
  data: function () {   // 创建的Vue实例时，data可以是Object 也可以是Function，但是在扩展
    return {            // 的时候，data必须是一个函数，而且要返回值奥。
      firstName: 'Walter',
      lastName: 'White',
      alias: 'Heisenberg'
    }
  }
})
</script>


// 创建 Profile 实例，并挂载到一个元素上。
new Profile().$mount('#mount-point')
// .$mount() 方法跟设置 el属性效果是一致的。

结果如下：

<p>Walter White aka Heisenberg</p>
```

综合案例代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Vue入门之extend全局方法</title>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
</head>
<body>
  <div id="app">
  </div>
  <script>
    var myVue = Vue.extend({
      template: '<div>{{ name }} - {{ age }} - {{ mail }}</div>',
      data: function () {
        return {
          name: 'malun',
          age: '19',
          mail: 'flydragonml@gmail.com'
        };
      }
    });
    var app = new myVue({
      el: '#app'
    });
  </script>
</body>
</html>
```

## 创建组件和注册组件

当然上面的方式只是能让我们继承 Vue 实例做一些扩展的动作。看 Vue 中如何创建一个组件并注册使用。

Vue 提供了一个全局注册组件的方法：Vue.component。

```html
语法： Vue.component( id, [definition] )

参数：
  {string} id    组件的名字，可以当HTML标签用，注意组件的名字都是小写，而且最好有横线和字母组合。
  {Function | Object} [definition]   组件的设置

用法：
注册或获取全局组件。注册还会自动使用给定的id设置组件的名称

// 注册组件，传入一个扩展过的构造器
Vue.component('my-component', Vue.extend({ /* ... */ }))
// 注册组件，传入一个选项对象（自动调用 Vue.extend）
Vue.component('my-component', { /* ... */ })
// 获取注册的组件（始终返回构造器）
var MyComponent = Vue.component('my-component')
```

因为组件是可复用的 Vue 实例，所以它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。仅有的例外是像 `el` 这样根实例特有的选项。

简单 demo：

```html
<div id="example">
  <!--组件直接跟普通的标签一样的使用。-->
  <my-component></my-component>
</div>
```

```js
// 注册一个组件
Vue.component('my-component', {
  // 模板选项设置当前组件，最终输出的html模板。注意：有且只有一个根元素。
  template: '<div>A custom component!</div>'
});
// 创建根实例
new Vue({
  el: '#example'
});
```

那么我们注册一个组件自动帮我生成 label 和 radiobutton 组合。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Vue入门之extend全局方法</title>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
</head>
<body>
  <div id="app">
    <!--组件名直接可以当标签使用。-->
    <radio-tag rid="rBas" txt="篮球" val="1"></radio-tag>

    <!--组件的属性也可以使用Vue的绑定的语法，下面是动态绑定数据给子组件-->
    <radio-tag :rid="demoId" :txt="demoText" :val="demoVal"></radio-tag>
  </div>
  <script>
    // 定义组件模板，模板必须有且只有一个根元素。
    var temp = '<div><label v-bind:for="rid">{{ txt }}</label><input :id="rid" type="radio" v-bind:value="val"></div>';
    // 注册一个全局的组件
    Vue.component('radio-tag', {       // 组件的名字不能有大写字母，跟React的曲别啊。另外组件名最好是小写字母加横线组合。
      template: temp,
      props: ['rid', 'txt', 'val'],   // 设置组件的属性有哪些，定义标签的属性一致。
      data: function () {             // 注意属性名都得是小写，不然会不认的。
        return {                      // 在组件的定义中data必须是函数，而且必须有返回值。
          age: 19,                    // 此地方的 age 和 emial都是演示，并么有有到。
          email: 'flydragonml@gmail.com'
        }
      }
    });

    // 初始化一个Vue实例
    var app = new Vue({
      el: '#app',
      data: {
        demoId: 'ft',
        demoText: '足球',
        demoVal: 2
      }
    });
  </script>
</body>
</html>
```

注意结果点

- 组件是可以复用的，跟普通的html标签一样使用。
- 组件模板内只能有一个根元素，不能并列多个子元素。
- 组件的名字都必须是小写【[其实是非必须，但是为了不麻烦就强制吧](https://cn.vuejs.org/v2/guide/components.html#组件命名约定)】！！！而且建议是小写字母和横线的组合比如： my-radiobtn
- 注册组件的时候，可以传入一个选项对象进行配置。其中`props`是设置当前组件的属性，属性也都必须小写。属性是连接父容器和子组件的桥梁。
- 注意：属性名和组件的名字都要小写啊，不然 vue 不会认的。
- 编写组件代码最好配合 Vue 的 chrome 插件：[vue-devtool](https://github.com/vuejs/vue-devtools)
- 组件可以返还自己的数据，但是必须是函数。data 必须是 Function

## 局部注册组件

全局注册组件就是使用全局 API `Vue.componet(id, {....})`就行了，当然我们有时候需要注册一个局部模块的自己用的组件。那么就可以用下面的方式了。

```js
var Child = {
  template: '<div>A custom component!</div>'
};
new Vue({
  // ...
  components: {
    // <my-component> 将只在父模板可用
    'my-component': Child
  }
});
```

<p class="tip">
如果是使用webpack配合vue的话，全局注册的组件总会打包到最终的生成的js文件中，如果组件没有被用到，也会被打包到了最终产品中。所以：建议使用局部组件。
</p>

## 组件的`data` 必须是一个函数

**一个组件的 `data` 选项必须是一个函数**，因此每个实例可以维护一份被返回对象的独立的拷贝：

```js
data: function () {
  return {
    count: 0
  }
}
```

如果 Vue 没有这条规则，点击一个按钮就可能会像如下代码一样影响到*其它所有实例*：

```html
<div id="components-demo3" class="demo">
  <button-counter2></button-counter2>
  <button-counter2></button-counter2>
  <button-counter2></button-counter2>
</div>
<script>
var buttonCounter2Data = {
  count: 0
}
Vue.component('button-counter2', {
  data: function () {
    return buttonCounter2Data
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
new Vue({ el: '#components-demo3' })
</script>
```

<p class="tip">
多个组件，指向同一个data对象的时候，会造成多个组件同时追踪一个对象的变化，造成了组件之间相互影响。
</p>

## 通过 Prop 向子组件传递数据

Prop 是你可以在组件上注册的一些自定义特性。当一个值传递给一个 prop 特性的时候，它就变成了那个组件实例的一个属性。

```js
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```

一个组件默认可以拥有任意数量的 prop，任何值都可以传递给任何 prop。在上述模板中，你会发现我们能够在组件实例中访问这个值，就像访问 `data` 中的值一样。

一个 prop 被注册之后，你就可以像这样把数据作为一个自定义特性传递进来：

```html
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
<blog-post title="Why Vue is so fun"></blog-post>
```

### Prop 的大小写 (camelCase vs kebab-case)

HTML 中的特性名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。这意味着当你使用 DOM 中的模板时，camelCase (驼峰命名法) 的 prop 名需要使用其等价的 kebab-case (短横线分隔命名) 命名：

``` js
Vue.component('blog-post', {
  // 在 JavaScript 中是 camelCase 的
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
```

``` html
<!-- 在 HTML 中是 kebab-case 的 -->
<blog-post post-title="hello!"></blog-post>
```

> 重申一次，如果你使用字符串模板，那么这个限制就不存在了。

### props的类型限定

通常你希望每个 prop 都有指定的值类型。这时，你可以以对象形式列出 prop，这些属性的名称和值分别是 prop 各自的名称和类型：

```js
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object
}
```

#### Prop 验证

我们可以为组件的 prop 指定验证要求，例如你知道的这些类型。如果有一个需求没有被满足，则 Vue 会在浏览器控制台中警告你。这在开发一个会被别人用到的组件时尤其有帮助。

为了定制 prop 的验证方式，你可以为 `props` 中的值提供一个带有验证需求的对象，而不是一个字符串数组。例如：

``` js
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 匹配任何类型)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组且一定会从一个工厂函数返回默认值
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

当 prop 验证失败的时候，(开发环境构建版本的) Vue 将会产生一个控制台的警告。

<p class="tip">注意那些 prop 会在一个组件实例创建**之前**进行验证，所以实例的属性 (如 `data`、`computed` 等) 在 `default` 或 `validator` 函数中是不可用的。</p>

#### 类型检查

`type` 可以是下列原生构造函数中的一个：

- `String`
- `Number`
- `Boolean`
- `Array`
- `Object`
- `Date`
- `Function`
- `Symbol`

额外的，`type` 还可以是一个自定义的构造函数，并且通过 `instanceof` 来进行检查确认。例如，给定下列现成的构造函数：

```js
function Person (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}
```

你可以使用：

```js
Vue.component('blog-post', {
  props: {
    author: Person
  }
})
```

来验证 `author` prop 的值是否是通过 `new Person` 创建的。

### 非 Prop 的特性

一个非 prop 特性是指传向一个组件，但是该组件并没有相应 prop 定义的特性。

因为显式定义的 prop 适用于向一个子组件传入信息，然而组件库的作者并不总能预见组件会被用于怎样的场景。这也是为什么组件可以接受任意的特性，而这些特性会被添加到这个组件的根元素上。

例如，想象一下你通过一个 Bootstrap 插件使用了一个第三方的 `<bootstrap-date-input>` 组件，这个插件需要在其 `<input>` 上用到一个 `data-date-picker` 特性。我们可以将这个特性添加到你的组件实例上：

``` html
<bootstrap-date-input data-date-picker="activated"></bootstrap-date-input>
```

然后这个 `data-date-picker="activated"` 特性就会自动添加到 `<bootstrap-date-input>` 的根元素上。

## 组件的 slot

使用组件的时候，经常需要在父组件中为子组件中插入一些标签等。当然其实可以通过属性等操作，但是比较麻烦，直接写标签还是方便很多。
那么 Vue 提供了 slot 协助子组件对父容器写入的标签进行管理。

当父容器写了额外的内容时， 如果子组件恰好有一个 slot 标签，那边子容器的 slot 标签会被父容器写入的内容替换掉。

比如下面的例子：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Vue入门之extend全局方法</title>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
</head>
<body>
  <div id="app">
    <!--父容器输入标签-->
    <my-slot>
      <h3>这里是父容器写入的</h3>
    </my-slot>

    <!--父容器绑定数据到子容器的slot,这里的作用域是父容器的啊。-->
    <my-slot>{{ email }}</my-slot>

    <!--父容器什么都不传内容-->
    <my-slot></my-slot>
  </div>
  <script>
    // 反引号：可以定义多行字符串。
    var temp = `
      <div>
        <h1>这里是子组件</h1>
        <hr>
        <slot>slot标签会被父容器写的额外的内容替换掉，如果父容器没有写入任何东西，此标签将保留！</slot>
      </div>
    `;
    Vue.component('MySlot', {          // 如果定义的组件为MySlot，那么用组件的时候：<my-slot></my-slot>
      template: temp,
    });
    // 初始化一个Vue实例
    var app = new Vue({
      el: '#app',
      data: {
       email: 'flydragon@gmail.com'
      }
    });
  </script>
</body>
</html>
```

最终结果：

```html
<div id="app">
  <div>
    <h1>这里是子组件</h1>
    <hr>
    <h3>这里是父容器写入的</h3>
  </div>

  <div>
    <h1>这里是子组件</h1>
    <hr> flydragon@gmail.com
  </div>

  <div>
    <h1>这里是子组件</h1>
    <hr> slot标签会被父容器写的额外的内容替换掉，如果父容器没有写入任何东西，此标签将删除！
  </div>
</div>
```

Vue 实现了一套内容分发的 API，这套 API 基于当前的 [Web Components 规范草案](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md)，将 `<slot>` 元素作为承载分发内容的出口。

它允许你像这样合成组件：

``` html
<navigation-link url="/profile">
  Your Profile
</navigation-link>
```

然后你在 `<navigation-link>` 的模板中可能会写为：

``` html
<a
  v-bind:href="url"
  class="nav-link"
>
  <slot></slot>
</a>
```

当组件渲染的时候，这个 `<slot>` 元素将会被替换为“Your Profile”。插槽内可以包含任何模板代码，包括 HTML：

``` html
<navigation-link url="/profile">
  <!-- 添加一个 Font Awesome 图标 -->
  <span class="fa fa-user"></span>
  Your Profile
</navigation-link>
```

甚至其它的组件：

``` html
<navigation-link url="/profile">
  <!-- 添加一个图标的组件 -->
  <font-awesome-icon name="user"></font-awesome-icon>
  Your Profile
</navigation-link>
```

如果 `<navigation-link>` **没有**包含一个 `<slot>` 元素，则任何传入它的内容都会被抛弃。

### 具名插槽

有些时候我们需要多个插槽。例如，一个假设的 `<base-layout>` 组件多模板如下：

``` html
<div class="container">
  <header>
    <!-- 我们希望把页头放这里 -->
  </header>
  <main>
    <!-- 我们希望把主要内容放这里 -->
  </main>
  <footer>
    <!-- 我们希望把页脚放这里 -->
  </footer>
</div>
```

对于这样的情况，`<slot>` 元素有一个特殊的特性：`name`。这个特性可以用来定义额外的插槽：

``` html
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

在向具名插槽提供内容的时候，我们可以在一个父组件的 `<template>` 元素上使用 `slot` 特性：

```html
<base-layout>
  <template slot="header">
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template slot="footer">
    <p>Here's some contact info</p>
  </template>
</base-layout>
```

另一种 `slot` 特性的用法是直接用在一个普通的元素上：

``` html
<base-layout>
  <h1 slot="header">Here might be a page title</h1>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <p slot="footer">Here's some contact info</p>
</base-layout>
```

我们还是可以保留一个未命名插槽，这个插槽是**默认插槽**，也就是说它会作为所有未匹配到插槽的内容的统一出口。上述两个示例渲染出来的 HTML 都将会是：

``` html
<div class="container">
  <header>
    <h1>Here might be a page title</h1>
  </header>
  <main>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </main>
  <footer>
    <p>Here's some contact info</p>
  </footer>
</div>
```

### 插槽的默认内容

有的时候为插槽提供默认的内容是很有用的。例如，一个 `<submit-button>` 组件可能希望这个按钮的默认内容是“Submit”，但是同时允许用户覆写为“Save”、“Upload”或别的内容。

你可以在 `<slot>` 标签内部指定默认的内容来做到这一点。

```html
<button type="submit">
  <slot>Submit</slot>
</button>
```

如果父组件为这个插槽提供了内容，则默认的内容会被替换掉。

### 编译作用域

当你想在插槽内使用数据时，例如：

``` html
<navigation-link url="/profile">
  Logged in as {{ user.name }}
</navigation-link>
```

该插槽可以访问跟这个模板的其它地方相同的实例属性 (也就是说“作用域”是相同的)。但这个插槽**不能**访问 `<navigation-link>` 的作用域。例如尝试访问 `url` 是不会工作的。牢记一条准则：

> 父组件模板的所有东西都会在父级作用域内编译；子组件模板的所有东西都会在子级作用域内编译。

### 作用域插槽

> 2.1.0+ 新增

有的时候你希望提供的组件带有一个可从子组件获取数据的可复用的插槽。例如一个简单的 `<todo-list>` 组件的模板可能包含了如下代码：

```html
<ul>
  <li
    v-for="todo in todos"
    v-bind:key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>
```

但是在我们应用的某些部分，我们希望每个独立的待办项渲染出和 `todo.text` 不太一样的东西。这也是作用域插槽的用武之地。

为了让这个特性成为可能，你需要做的全部事情就是将待办项内容包裹在一个 `<slot>` 元素上，然后将所有和其上下文相关的数据传递给这个插槽：在这个例子中，这个数据是 `todo` 对象：

```html
<ul>
  <li
    v-for="todo in todos"
    v-bind:key="todo.id"
  >
    <!-- 我们为每个 todo 准备了一个插槽，-->
    <!-- 将 `todo` 对象作为一个插槽的 prop 传入。-->
    <slot v-bind:todo="todo">
      <!-- 回退的内容 -->
      {{ todo.text }}
    </slot>
  </li>
</ul>
```

现在当我们使用 `<todo-list>` 组件的时候，我们可以选择为待办项定义一个不一样的 `<template>` 作为替代方案，并且可以通过 `slot-scope` 特性从子组件获取数据：

```html
<todo-list v-bind:todos="todos">
  <!-- 将 `slotProps` 定义为插槽作用域的名字 -->
  <template slot-scope="slotProps">
    <!-- 为待办项自定义一个模板，-->
    <!-- 通过 `slotProps` 定制每个待办项。-->
    <span v-if="slotProps.todo.isComplete">✓</span>
    {{ slotProps.todo.text }}
  </template>
</todo-list>
```

> 在 2.5.0+，`slot-scope` 不再限制在 `<template>` 元素上使用，而可以用在插槽内的任何元素或组件上。

### 解构 `slot-scope`

如果一个 JavaScript 表达式在一个函数定义的参数位置有效，那么这个表达式实际上就可以被 `slot-scope` 接受。也就是说你可以在支持的环境下 ([单文件组件](single-file-components.html)或[现代浏览器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#浏览器兼容))，在这些表达式中使用 [ES2015 解构语法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#解构对象)。例如：

```html
<todo-list v-bind:todos="todos">
  <template slot-scope="{ todo }">
    <span v-if="todo.isComplete">✓</span>
    {{ todo.text }}
  </template>
</todo-list>
```

这会使作用域插槽变得更干净一些。

## 单文件组件的使用方式介绍

通过上面我们定义组件的方式，就已经感觉很不爽了，尤其是模板的定义，而且样式怎么处理也没有很好的进行规整。
Vue 可以通过 Webpack 等第三方工具实现单文件的开发的方式。当然这里会牵扯到很多 es6 的语法、第三方工具实现前端模块化等很多知识，
我们大概看一眼指导 Vue 的组件可以直接写一个文件中，其他地方就可以直接导入这个模块了。后面做项目的时候我还会再讲一下怎么用。

```html
<template>
  <div>
    <nav class="navbar navbar-dark navbar-fixed-top">
    </nav>
     <div class="col-md-3 sidebar">
      <ul>
        <li v-for="item in list" >
          <router-link :to="{ path: item.url }">{{ item.name }}</router-link>
        </li>
      </ul>
    </div>
    <div class="container-fluid content">
      <router-view></router-view>
    </div>
    </div>
  </div>
</template>

<script>
// 这里怎么回事
import Axios from 'axios'
export default {
  name: 'app',
  components: {
  },
  data: function () {
    return {
      list: []
    }
  },
  mounted: function () {          // 挂在完成后
    this.$nextTick(function () {
      Axios.get('/api/menulist', {
        params: {
        }
      }).then(function (res) {
        this.list = res.data
      }.bind(this))
    })
  }
}
</script>

<style>
ul, li {
  list-style: none;
}
.router-link-active {
  background-color: #f6f6f6;
}

.navbar {
  height: 50px;
  background-color: #303030;
}
.content {
  margin-top: 50px;
  padding-left: 210px;
}

.sidebar {
  background-color: #f5f5f5;
  border-right: 1px solid #eee;
  width: 200px;
}

@media (min-width: 768px) {
  .sidebar {
    position: fixed;
    top: 51px;
    bottom: 0;
    left: 0;
    z-index: 1000;
    display: block;
    padding: 20px;
    overflow-x: hidden;
    overflow-y: auto; /* Scrollable contents if viewport is shorter than content. */
    background-color: #f5f5f5;
    border-right: 1px solid #eee;
  }
}
</style>
```

单文件书写组件的方式必须要配合 webpack 之类的工具才行，所以这里暂时不讲解如何做，后面到项目阶段的时候再详细讲解。
不过你可以参考：[Vue 官网单文件组件](https://cn.vuejs.org/v2/guide/single-file-components.html)

## 组件的事件

### 事件名

跟组件和 prop 不同，事件名不存在任何自动化的大小写转换。而是触发的事件名需要完全匹配监听这个事件所用的名称。举个例子，如果触发一个 camelCase 名字的事件：

```js
this.$emit('myEvent')
```

则监听这个名字的 kebab-case 版本是不会有任何效果的：

```html
<my-component v-on:my-event="doSomething"></my-component>
```

跟组件和 prop 不同，事件名不会被用作一个 JavaScript 变量名或属性名，所以就没有理由使用 camelCase 或 PascalCase 了。并且 `v-on` 事件监听器在 DOM 模板中会被自动转换为全小写 (因为 HTML 是大小写不敏感的)，所以 `v-on:myEvent` 将会变成 `v-on:myevent`——导致 `myEvent` 不可能被监听到。

因此，我们推荐你**始终使用 kebab-case 的事件名**。

### 通过事件实现子组件向父组件传递数据

```html
 <div id="app">
    <p>{{childData}}</p>
    <ls @gg="childData=$event"></ls>
  </div>
  <script>
    Vue.component('ls', {
      template: `<div><p @click="emitGGEvent">点击子组件传递数据</p></div>`,
      methods: {
        emitGGEvent() {
          this.$emit('gg', 'aicoder.com')
        }
      }
    })
    let vm = new Vue({
      el: '#app',
      data: {
        childData: ''
      }
    });
  </script>
```

## 在动态组件上使用 `keep-alive`

`is` 特性来切换不同的动态的组件：

```html
<component v-bind:is="currentTabComponent"></component>
```

当在这些组件之间切换的时候，你有时会想保持这些组件的状态，以避免反复重渲染导致的性能问题。例如我们来展开说一说这个多标签界面：

你会注意到，如果你选择了一篇文章，切换到 *Archive* 标签，然后再切换回 *Posts*，是不会继续展示你之前选择的文章的。这是因为你每次切换新标签的时候，Vue 都创建了一个新的 `currentTabComponent` 实例。

重新创建动态组件的行为通常是非常有用的，但是我们更希望那些标签的组件实例能够被在它们第一次被创建的时候缓存下来。为了解决这个问题，我们可以用一个 `<keep-alive>` 元素将其动态组件包裹起来。

``` html
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

## 异步组件

在大型应用中，我们可能需要将应用分割成小一些的代码块，并且只在需要的时候才从服务器加载一个模块。为了简化，Vue 允许你以一个工厂函数的方式定义你的组件，这个工厂函数会异步解析你的组件定义。Vue 只有在这个组件需要被渲染的时候才会触发该工厂函数，且会把结果缓存起来供未来重渲染。例如：

``` js
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    // 向 `resolve` 回调传递组件定义
    resolve({
      template: '<div>I am async!</div>'
    })
  }, 1000)
})
```

如你所见，这个工厂函数会收到一个 `resolve` 回调，这个回调函数会在你从服务器得到组件定义的时候被调用。你也可以调用 `reject(reason)` 来表示加载失败。这里的 `setTimeout` 是为了演示用的，如何获取组件取决于你自己。一个推荐的做法是将异步组件和 [webpack 的 code-splitting 功能](https://webpack.js.org/guides/code-splitting/)一起配合使用：

``` js
Vue.component('async-webpack-example', function (resolve) {
  // 这个特殊的 `require` 语法将会告诉 webpack
  // 自动将你的构建代码切割成多个包，这些包
  // 会通过 Ajax 请求加载
  require(['./my-async-component'], resolve)
})
```

你也可以在工厂函数中返回一个 `Promise`，所以把 webpack 2 和 ES2015 语法加在一起，我们可以写成这样：

``` js
Vue.component(
  'async-webpack-example',
  // 这个 `import` 函数会返回一个 `Promise` 对象。
  () => import('./my-async-component')
)
```

当使用[局部注册](components.html#本地注册)的时候，你也可以直接提供一个返回 `Promise` 的函数：

``` js
new Vue({
  // ...
  components: {
    'my-component': () => import('./my-async-component')
  }
})
```

<p class="tip">如果你是一个 <strong>Browserify</strong> 用户同时喜欢使用异步组件，很不幸这个工具的作者[明确表示](https://github.com/substack/node-browserify/issues/58#issuecomment-21978224)异步加载“并不会被 Browserify 支持”，至少官方不会。Browserify 社区已经找到了[一些变通方案](https://github.com/vuejs/vuejs.org/issues/620)，这些方案可能会对已存在的复杂应用有帮助。对于其它的场景，我们推荐直接使用 webpack，以拥有内建的被作为第一公民的异步支持。</p> 

### 处理加载状态

> 2.3.0+ 新增

这里的异步组件工厂函数也可以返回一个如下格式的对象：

``` js
const AsyncComponent = () => ({
  // 需要加载的组件 (应该是一个 `Promise` 对象)
  component: import('./MyComponent.vue'),
  // 异步组件加载时使用的组件
  loading: LoadingComponent,
  // 加载失败时使用的组件
  error: ErrorComponent,
  // 展示加载时组件的延时时间。默认值是 200 (毫秒)
  delay: 200,
  // 如果提供了超时时间且组件加载也超时了，
  // 则使用加载失败时使用的组件。默认值是：`Infinity`
  timeout: 3000
})
```

> 注意如果你希望在 [Vue Router](https://github.com/vuejs/vue-router) 的路由组件中使用上述语法的话，你必须使用 Vue Router 2.4.0+ 版本。

## 访问元素 & 组件

在绝大多数情况下，我们最好不要触达另一个组件实例内部或手动操作 DOM 元素。不过也确实在一些情况下做这些事情是合适的。

### 访问根实例

在每个 `new Vue` 实例的子组件中，其根实例可以通过 `$root` 属性进行访问。例如，在这个根实例中：

```js
// Vue 根实例
new Vue({
  data: {
    foo: 1
  },
  computed: {
    bar: function () { /* ... */ }
  },
  methods: {
    baz: function () { /* ... */ }
  }
})
```

所有的子组件都可以将这个实例作为一个全局 store 来访问或使用。

```js
// 获取根组件的数据
this.$root.foo

// 写入根组件的数据
this.$root.foo = 2

// 访问根组件的计算属性
this.$root.bar

// 调用根组件的方法
this.$root.baz()
```

<p class="tip">对于 demo 或非常小型的有少量组件的应用来说这是很方便的。不过这个模式扩展到中大型应用来说就不然了。因此在绝大多数情况下，我们强烈推荐使用 <a href="https://github.com/vuejs/vuex">Vuex</a> 来管理应用的状态。</p>

### 访问父级组件实例

和 `$root` 类似，`$parent` 属性可以用来从一个子组件访问父组件的实例。它提供了一种机会，可以在后期随时触达父级组件，以替代将数据以 prop 的方式传入子组件的方式。

<p class="tip">在绝大多数情况下，触达父级组件会使得你的应用更难调试和理解，尤其是当你变更了父级组件的数据的时候。当我们稍后回看那个组件的时候，很难找出那个变更是从哪里发起的。</p>

另外在一些*可能*适当的时候，你需要特别地共享一些组件库。举个例子，在和 JavaScript API 进行交互而不渲染 HTML 的抽象组件内，诸如这些假设性的 Google 地图组件一样：

```html
<google-map>
  <google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
</google-map>
```

这个 `<google-map>` 组件可以定义一个 `map` 属性，所有的子组件都需要访问它。在这种情况下 `<google-map-markers>` 可能想要通过类似 `this.$parent.getMap` 的方式访问那个地图，以便为其添加一组标记。你可以在[这里](https://jsfiddle.net/chrisvfritz/ttzutdxh/)查阅这种模式。

请留意，尽管如此，通过这种模式构建出来的那个组件的内部仍然是容易出现问题的。比如，设想一下我们添加一个新的 `<google-map-region>` 组件，当 `<google-map-markers>` 在其内部出现的时候，只会渲染那个区域内的标记：

```html
<google-map>
  <google-map-region v-bind:shape="cityBoundaries">
    <google-map-markers v-bind:places="iceCreamShops"></google-map-markers>
  </google-map-region>
</google-map>
```

那么在 `<google-map-markers>` 内部你可能发现自己需要一些类似这样的 hack：

```js
var map = this.$parent.map || this.$parent.$parent.map
```

很快它就会失控。这也是我们针对需要向任意更深层级的组件提供上下文信息时推荐[依赖注入](#依赖注入)的原因。

### 访问子组件实例或子元素

尽管存在 prop 和事件，有的时候你仍可能需要在 JavaScript 里直接访问一个子组件。为了达到这个目的，你可以通过 `ref` 特性为这个子组件赋予一个 ID 引用。例如：

```html
<base-input ref="usernameInput"></base-input>
```

现在在你已经定义了这个 `ref` 的组件里，你可以使用：

```js
this.$refs.usernameInput
```

来访问这个 `<base-input>` 实例，以便不时之需。比如程序化地从一个父级组件聚焦这个输入框。在刚才那个例子中，该 `<base-input>` 组件也可以使用一个类似的 `ref` 提供对内部这个指定元素的访问，例如：

```html
<input ref="input">
```

甚至可以通过其父级组件定义方法：

```js
methods: {
  // 用来从父级组件聚焦输入框
  focus: function () {
    this.$refs.input.focus()
  }
}
```

这样就允许父级组件通过下面的代码聚焦 `<base-input>` 里的输入框：

```js
this.$refs.usernameInput.focus()
```

当 `ref` 和 `v-for` 一起使用的时候，你得到的引用将会是一个包含了对应数据源的这些子组件的数组。

<p class="tip"><code>$refs</code> 只会在组件渲染完成之后生效，并且它们不是响应式的。这只意味着一个直接的子组件封装的“逃生舱”——你应该避免在模板或计算属性中访问 <code>$refs</code>。</p>


## 组件的生命周期

根组件和子组件的生命周期执行的过程如下：

- 根组件 (beforeCreate、created、beforeMount)
- 组件 (beforeCreate、created、beforeMount)
- 组件 mounted
- 根组件 mounted
- nextTick

## 组件总结

Vue 的组件化还是做的比较彻底的。不像 Angular1.0 中的模块那么鸡肋。组件化确实让前端模块化开发更加容易实现，
Vue 的单文件开发组件的方式也是 Vue 的一大创新，也发非常好用。

# [回到vue.js知识列表首页](/pages/vip_2vue.md)