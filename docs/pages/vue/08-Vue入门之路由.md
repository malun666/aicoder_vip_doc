# Vue 路由详解

对于前端来说，其实浏览器配合超级连接就很好的实现了路由功能。但是对于单页面应用来说，浏览器和超级连接的跳转方式已经不能适用，所以各大框架纷纷给出了单页面应用的解决路由跳转的方案。

Vue 框架的兼容性非常好，可以很好的跟其他第三方的路由框架进行结合。当然官方也给出了路由的方案： `vue-router`;

建议还是用官方的最好，使用量也是最大，相对来说 Vue 框架的升级路由组件升级也会及时跟上，所以为了以后的维护和升级方便还是使用 Vue 自家的东西最好。

## Vue-router 的版本对应

> 注意:
> vue-router@3.0+ 依赖 vue@2.5+  
> vue-router@2.x 只适用于 Vue 2.x 版本。  
> vue-router@1.x 对应于 Vue1.x 版本。  

- 的 Github 地址：[vue-router](https://github.com/vuejs/vue-router)
- [文档地址](https://router.vuejs.org/zh-cn/)

## vue-router 的安装使用

- CDN 连接方式

`https://unpkg.com/vue-router/dist/vue-router.js`

- npm 安装

```shell
npm install vue-router
```

## 相关的概念

路由相关的对象和组件：

- [**Router实例**](https://router.vuejs.org/zh/api/)：配置路由规则和控制路由跳转及配置路由钩子的实例，由VueRouter构造函数构建，并由根vue实例注入到所有的子组件。
- [**Route对象**](https://router.vuejs.org/zh/api/)：一个**路由对象 (route object)** 表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息，还有 URL 匹配到的**路由记录 (route records)**。
- [**router-link组件**](https://router.vuejs.org/zh/api/)：`<router-link>` 组件支持用户在具有路由功能的应用中（点击）导航。通过 `to` 属性指定目标地址，默认渲染成带有正确链接的 `<a>` 标签，可以通过配置 `tag` 属性生成别的标签.。另外，当目标路由成功激活时，链接元素自动设置一个表示激活的 CSS 类名。
- [**router-view组件**](https://router.vuejs.org/zh/api/)：`<router-view>` 组件是一个 functional 组件，渲染路径匹配到的视图组件。`<router-view>` 渲染的组件还可以内嵌自己的 `<router-view>`，根据嵌套路径，渲染嵌套组件。

## vue-router 入门 demo

vue-router 开发的步骤：

- 第一步： 引入 vue 和 vue-router 包。
  > 可以使用 cdn 的方式或者 npm 的方式。如果配合 npm 和 webpack 的话可以直接作为一个模块导入即可。但是作为初学入门的话建议还是
  > 直接使用 cdn 包的形式，先学会怎么用路由。

```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
```

- 第二步： 定义路由跳转的组件

```js
// 1. 定义（路由）组件。
const Foo = { template: '<div>foo</div>' };
const Bar = { template: '<div>bar</div>' };
```

- 第三步： 定义路由规则对象

```js
// 每个路由path应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
];

// 创建路由对象
const router = new VueRouter({
  routes // （缩写）相当于 routes: routes，es6的新语法
});
```

- 第四步： 创建 Vue 对象，并加重上面创建的路由对象

```js
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
  router
}).$mount('#app');
```

- 第五步： 在模板中编写路由跳转链接

```html
<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- 使用 router-link 组件来导航. -->
    <!-- 通过传入 `to` 属性指定链接. -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
```

最终的代码

```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- 使用 router-link 组件来导航. -->
    <!-- 通过传入 `to` 属性指定链接. -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
<script>
// 1. 定义（路由）组件。
// 可以从其他文件 import 进来
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }

// 2. 定义路由
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]

// 3. 创建 router 实例，然后传 `routes` 配置
// 你还可以传别的配置参数, 不过先这么简单着吧。
const router = new VueRouter({
  routes // （缩写）相当于 routes: routes
})

// 4. 创建和挂载根实例。
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
  router
}).$mount('#app')
</script>
```

## 使用 vue-router 的综合实例

下面是一个综合的例子, 页面上有几个导航的按钮，然后通过点击不同的按钮，可以在当前页面切换不同的组件。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Vue入门之extend全局方法</title>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
  <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
  <style>
  ul, li { list-style: none; }
  ul { overflow: hidden; }
  li { float: left; width: 100px; }
  h2 { background-color: #903;}
  </style>
</head>
<body>
  <div id="app">
    <top-bar> </top-bar>
    <hr>
      <p>email to: {{ email }}</p>
    <hr>
    <router-view class="view one"></router-view>
    <footer-bar></footer-bar>
  </div>
  <script>
    var topbarTemp = `
      <nav>
        <ul>
          <li v-for="item in NavList">
            <router-link :to="item.url">{{ item.name }}</router-link>
          </li>
        </ul>
      </nav>
    `;
    // 定义组件：topbar
    Vue.component('top-bar', {
      template: topbarTemp,
      data: function () {
        return {
          NavList: [
            { name: '首页', url: '/home'},
            { name: '产品', url: '/product'},
            { name: '服务', url: '/service'},
            { name: '关于', url: '/about'}
          ]
        }
      }
    });

    Vue.component('footer-bar', {  // 定义组件 footerbar
      template: `
        <footer>
          <hr/>
          <p>版权所有@flydragon<p>
        </footer>
      `
    });

    // 创建home模块
    var home = {
      template: `<div> <h2>{{ msg }}<h2></div>`,
      data: function () {
        return { msg: 'this is home view' }
      }
    };

    // 创建product 模块
    var product = {
      template: `<div> {{ msg }}</div>`,
      data: function () {
        return { msg: 'this is product view' }
      }
    }

    // 定义路由对象
    var router = new VueRouter({
      routes: [
        { path: '/home', component: home },
        { path: '/product', component: product }
      ]
    });

    // 初始化一个Vue实例
    var app = new Vue({
      el: '#app',
      data: {
       email: 'flydragon@gmail.com'
      },
      router: router
    });
  </script>
</body>
</html>
```

## 动态路由匹配

我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件。例如，我们有一个 `User` 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。那么，我们可以在  `vue-router` 的路由路径中使用『动态路径参数』（dynamic segment）来达到这个效果：

``` js
const User = {
  template: '<div>User</div>'
}

const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
```

现在呢，像 `/user/foo` 和 `/user/bar` 都将映射到相同的路由。

一个『路径参数』使用冒号 `:` 标记。当匹配到一个路由时，参数值会被设置到
 `this.$route.params`，可以在每个组件内使用。于是，我们可以更新 `User` 的模板，输出当前用户的 ID：

``` js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
```

你可以看看这个[在线例子](https://jsfiddle.net/yyx990803/4xfa2f19/)。

你可以在一个路由中设置多段『路径参数』，对应的值都会设置到 `$route.params` 中。例如：

| 模式 | 匹配路径 | $route.params |
|---------|------|--------|
| /user/:username | /user/evan | `{ username: 'evan' }` |
| /user/:username/post/:post_id | /user/evan/post/123 | `{ username: 'evan', post_id: 123 }` |

除了 `$route.params` 外，`$route` 对象还提供了其它有用的信息，例如，`$route.query`（如果 URL 中有查询参数）、`$route.hash` 等等。你可以查看 [API 文档](https://router.vuejs.org/zh/api/) 的详细说明。

### 响应路由参数的变化

提醒一下，当使用路由参数时，例如从 `/user/foo` 导航到 `/user/bar`，**原来的组件实例会被复用**。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。**不过，这也意味着组件的生命周期钩子不会再被调用**。

复用组件时，想对路由参数的变化作出响应的话，你可以简单地 watch（监测变化） `$route` 对象：

``` js
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}
```

或者使用 2.2 中引入的 `beforeRouteUpdate` 守卫：

``` js
const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
    // react to route changes...
    // don't forget to call next()
  }
}
```

### 高级匹配模式

`vue-router` 使用 [path-to-regexp](https://github.com/pillarjs/path-to-regexp) 作为路径匹配引擎，所以支持很多高级的匹配模式，例如：可选的动态路径参数、匹配零个或多个、一个或多个，甚至是自定义正则匹配。查看它的 [文档](https://github.com/pillarjs/path-to-regexp#parameters) 学习高阶的路径匹配，还有 [这个例子 ](https://github.com/vuejs/vue-router/blob/next/examples/route-matching/app.js) 展示 `vue-router` 怎么使用这类匹配。

### 匹配优先级

有时候，同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序：谁先定义的，谁的优先级就最高。

## 路由参数获取

定义路由路径的时候，可以指定参数。参数需要通过路径进行标识：`/user/:id`就是定义了一个规则，/user 开头，然后后面的就是 id 参数的值。
比如：

```html
路由规则：  /user/:id
/user/9   =>  id = 9
/user/8   =>  id = 8
/user/1   =>  id = 1
```

然后在跳转后的 vue 中可以通过`this.$route.params.参数名`获取对应的参数。
比如代码：

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>Vue入门之extend全局方法</title>
  <script src="https://unpkg.com/vue/dist/vue.js"></script>
  <script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>
</head>

<body>
  <div id="app">
    <nav>
      <router-link to="/user/9">用户</router-link>
      <router-link to="/stu/malun">学生</router-link>
      <hr>
    </nav>
    <router-view></router-view>
  </div>
  <script>
    var user = {
      template: `
        <div>user id is : {{ $route.params.id }}</div>
      `
    };

    var stu = {
      template: `
        <div>
          <h2>{{ getName }}</h2>
        </div>
      `,
      computed: {
        getName: function () {
          return this.$route.params.name;
        }
      }
    };
    var router = new VueRouter({
      routes: [
        { path: '/user/:id', component: user },
        { path: '/stu/:name', component: stu }
      ]
    });
    var app = new Vue({
      el: '#app',
      router: router
    });
  </script>
</body>
</html>
```

## js 控制路由跳转

上面我们演示的都是通过 router-link 进行跳转。 其实我们还可以通过 js 编程的方式进行路由的跳转。

我们可以在任何组件内通过 this.$router 访问路由器，也可以通过 this.$route 访问当前路由.

当你点击 `<router-link>` 时，这个方法会在内部调用，所以说，点击 `<router-link :to="...">` 等同于调用 `router.push(...)`。

| 声明式 | 编程式 |
|-------------|--------------|
| `<router-link :to="...">` | `router.push(...)` |

```js
// 当前路由的view跳转到 /home
this.$router.push('home');

// 对象,  跳转到/home
this.$router.push({ path: 'home' });

// 命名的路由
this.$router.push({ name: 'user', params: { userId: 123 } });

// 带查询参数，变成 /register?plan=private
this.$router.push({ path: 'register', query: { plan: 'private' } });
```

**注意：如果提供了 `path`，`params` 会被忽略，上述例子中的 `query` 并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的 `name` 或手写完整的带有参数的 `path`：**

```js
const userId = 123
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```

同样的规则也适用于 `router-link` 组件的 `to` 属性。

### `router.replace(location, onComplete?, onAbort?)`

跟 `router.push` 很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录。

| 声明式 | 编程式 |
|-------------|--------------|
| `<router-link :to="..." replace>` | `router.replace(...)` |

### `router.go(n)`

这个方法的参数是一个整数，意思是在 history 记录中向前或者后退多少步，类似 `window.history.go(n)`。

例子

``` js
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)

// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
router.go(-100)
router.go(100)
```

### 操作 History

你也许注意到 `router.push`、 `router.replace` 和 `router.go` 跟 [`window.history.pushState`、 `window.history.replaceState` 和 `window.history.go`](https://developer.mozilla.org/en-US/docs/Web/API/History)好像， 实际上它们确实是效仿 `window.history` API 的。

因此，如果你已经熟悉 [Browser History APIs](https://developer.mozilla.org/en-US/docs/Web/API/History_API)，那么在 vue-router 中操作 history 就是超级简单的。

还有值得提及的，vue-router 的导航方法 （`push`、 `replace`、 `go`） 在各类路由模式（`history`、 `hash` 和 `abstract`）下表现一致。

## 命名路由

有时候，通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。你可以在创建 Router 实例的时候，在 `routes` 配置中给某个路由设置名称。

``` js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```

要链接到一个命名路由，可以给 `router-link` 的 `to` 属性传一个对象：

``` html
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```

这跟代码调用 `router.push()` 是一回事：

``` js
router.push({ name: 'user', params: { userId: 123 }})
```

这两种方式都会把路由导航到 `/user/123` 路径。

完整的例子请[移步这里](https://github.com/vuejs/vue-router/blob/next/examples/named-routes/app.js)。

## 嵌套路由

嵌套路由跟普通路由基本没有什么区别。但是可以让 vue 开发变的非常灵活。
官网这块写的也非常好，我就直接拷贝了（原谅我吧。）
实际生活中的应用界面，通常由多层嵌套的组件组合而成。同样地，URL 中各段动态路径也按某种结构对应嵌套的各层组件，例如：

```sh
/user/foo/profile                     /user/foo/posts
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Profile      | |  +------------>  | | Posts       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
借助 vue-router，使用嵌套路由配置，就可以很简单地表达这种关系。
```

```html
<div id="app">
  <router-view></router-view>
</div>
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}

const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})
这里的 <router-view> 是最顶层的出口，渲染最高级路由匹配到的组件。同样地，一个被渲染组件同样可以包含自己的嵌套 <router-view>。例如，在 User 组件的模板添加一个 <router-view>：

const User = {
  template: `
    <div class="user">
      <h2>User {{ $route.params.id }}</h2>
      <router-view></router-view>
    </div>
  `
}
要在嵌套的出口中渲染组件，需要在 VueRouter 的参数中使用 children 配置：

const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User,
      children: [
        {
          // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
        },
        {
          // 当 /user/:id/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})
```

要注意，以 / 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。
你会发现，children 配置就是像 routes 配置一样的路由配置数组，所以呢，你可以嵌套多层路由。

此时，基于上面的配置，当你访问 /user/foo 时，User 的出口是不会渲染任何东西，这是因为没有匹配到合适的子路由。如果你想要渲染点什么，可以提供一个 空的 子路由：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id',
      component: User,
      children: [
        // 当 /user/:id 匹配成功，
        // UserHome 会被渲染在 User 的 <router-view> 中
        { path: '', component: UserHome }

        // ...其他子路由
      ]
    }
  ]
});
```

## 命名视图

有时候想同时（同级）展示多个视图，而不是嵌套展示，例如创建一个布局，有 `sidebar`（侧导航） 和 `main`（主内容） 两个视图，这个时候命名视图就派上用场了。你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。如果 `router-view` 没有设置名字，那么默认为 `default`。

``` html
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 `components` 配置（带上 s）：

``` js
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

以上案例相关的可运行代码请[移步这里](https://jsfiddle.net/posva/6du90epg/)。

### 嵌套命名视图

我们也有可能使用命名视图创建嵌套视图的复杂布局。这时你也需要命名用到的嵌套 `router-view` 组件。我们以一个设置面板为例：

```
/settings/emails                                       /settings/profile
+-----------------------------------+                  +------------------------------+
| UserSettings                      |                  | UserSettings                 |
| +-----+-------------------------+ |                  | +-----+--------------------+ |
| | Nav | UserEmailsSubscriptions | |  +------------>  | | Nav | UserProfile        | |
| |     +-------------------------+ |                  | |     +--------------------+ |
| |     |                         | |                  | |     | UserProfilePreview | |
| +-----+-------------------------+ |                  | +-----+--------------------+ |
+-----------------------------------+                  +------------------------------+
```

- `Nav` 只是一个常规组件。
- `UserSettings` 是一个视图组件。
- `UserEmailsSubscriptions`、`UserProfile`、`UserProfilePreview` 是嵌套的视图组件。

**注意**：_我们先忘记 HTML/CSS 具体的布局的样子，只专注在用到的组件上_

`UserSettings` 组件的 `<template>` 部分应该是类似下面的这段代码：

```html
<!-- UserSettings.vue -->
<div>
  <h1>User Settings</h1>
  <NavBar/>
  <router-view/>
  <router-view name="helper"/>
</div>
```

_嵌套的视图组件在此已经被忽略了，但是你可以在[这里](https://jsfiddle.net/posva/22wgksa3/)找到完整的源代码_

然后你可以用这个路由配置完成该布局：

```js
{
  path: '/settings',
  // 你也可以在顶级路由就配置命名视图
  component: UserSettings,
  children: [{
    path: 'emails',
    component: UserEmailsSubscriptions
  }, {
    path: 'profile',
    components: {
      default: UserProfile,
      helper: UserProfilePreview
    }
  }]
}
```

一个可以工作的示例的 demo 在[这里](https://jsfiddle.net/posva/22wgksa3/)。

## 路由组件传参

在组件中使用 `$route` 会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的 URL 上使用，限制了其灵活性。

使用 `props` 将组件和路由解耦：

**取代与 `$route` 的耦合**

``` js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})
```

**通过 `props` 解耦**

``` js
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```

这样你便可以在任何地方使用该组件，使得该组件更易于重用和测试。

### 布尔模式

如果 `props` 被设置为 `true`，`route.params` 将会被设置为组件属性。

### 对象模式

如果 `props` 是一个对象，它会被按原样设置为组件属性。当 `props` 是静态的时候有用。

``` js
const router = new VueRouter({
  routes: [
    { path: '/promotion/from-newsletter', component: Promotion, props: { newsletterPopup: false } }
  ]
})
```

### 函数模式

你可以创建一个函数返回 `props`。这样你便可以将参数转换成另一种类型，将静态值与基于路由的值结合等等。

``` js
const router = new VueRouter({
  routes: [
    { path: '/search', component: SearchUser, props: (route) => ({ query: route.query.q }) }
  ]
})
```

URL `/search?q=vue` 会将 `{query: 'vue'}` 作为属性传递给 `SearchUser` 组件。

请尽可能保持 `props` 函数为无状态的，因为它只会在路由发生变化时起作用。如果你需要状态来定义 `props`，请使用包装组件，这样 Vue 才可以对状态变化做出反应。

更多高级用法，请查看[例子](https://github.com/vuejs/vue-router/blob/dev/examples/route-props/app.js)。

## 导航守卫

>（译者：『导航』表示路由正在发生改变。）

正如其名，`vue-router` 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。有多种机会植入路由导航过程中：全局的, 单个路由独享的, 或者组件级的。

记住**参数或查询的改变并不会触发进入/离开的导航守卫**。你可以通过[观察 `$route` 对象](../essentials/dynamic-matching.md#响应路由参数的变化)来应对这些变化，或使用 `beforeRouteUpdate` 的组件内守卫。

### 全局守卫

你可以使用 `router.beforeEach` 注册一个全局前置守卫：

``` js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 **等待中**。

每个守卫方法接收三个参数：

- **`to: Route`**: 即将要进入的目标 [路由对象](../api/route-object.md)

- **`from: Route`**: 当前导航正要离开的路由

- **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。

  - **`next()`**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** （确认的）。

  - **`next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了（可能是用户手动或者浏览器后退按钮），那么 URL 地址会重置到 `from` 路由对应的地址。

  - **`next('/')` 或者 `next({ path: '/' })`**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 [`router-link` 的 `to` prop](../api/router-link.md) 或 [`router.push`](../api/router-instance.md#方法) 中的选项。

  - **`next(error)`**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](../api/router-instance.html#方法) 注册过的回调。

**确保要调用 `next` 方法，否则钩子就不会被 resolved。**

### 全局解析守卫

> 2.5.0 新增

在 2.5.0+ 你可以用 `router.beforeResolve` 注册一个全局守卫。这和 `router.beforeEach` 类似，区别是在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后**，解析守卫就被调用。

### 全局后置钩子

你也可以注册全局后置钩子，然而和守卫不同的是，这些钩子不会接受 `next` 函数也不会改变导航本身：

``` js
router.afterEach((to, from) => {
  // ...
})
```

### 路由独享的守卫

你可以在路由配置上直接定义 `beforeEnter` 守卫：

``` js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

这些守卫与全局前置守卫的方法参数是一样的。

### 组件内的守卫

最后，你可以在路由组件内直接定义以下路由导航守卫：

- `beforeRouteEnter`
- `beforeRouteUpdate` (2.2 新增)
- `beforeRouteLeave`

``` js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

`beforeRouteEnter` 守卫 **不能** 访问 `this`，因为守卫在导航确认前被调用,因此即将登场的新组件还没被创建。

不过，你可以通过传一个回调给 `next`来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。

``` js
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

注意 `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫。对于 `beforeRouteUpdate` 和 `beforeRouteLeave` 来说，`this` 已经可用了，所以**不支持**传递回调，因为没有必要了。

```js
beforeRouteUpdate (to, from, next) {
  // just use `this`
  this.name = to.params.name
  next()
}
```

这个离开守卫通常用来禁止用户在还未保存修改前突然离开。该导航可以通过 `next(false)` 来取消。

```js
beforeRouteLeave (to, from , next) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
```

### 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用离开守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 用创建好的实例调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数。

## 过渡动效

`<router-view>` 是基本的动态组件，所以我们可以用 `<transition>` 组件给它添加一些过渡效果：

``` html
<transition>
  <router-view></router-view>
</transition>
```

[`<transition>` 的所有功能](https://cn.vuejs.org/guide/transitions.html) 在这里同样适用。

### 单个路由的过渡

上面的用法会给所有路由设置一样的过渡效果，如果你想让每个路由组件有各自的过渡效果，可以在各路由组件内使用 `<transition>` 并设置不同的 name。

``` js
const Foo = {
  template: `
    <transition name="slide">
      <div class="foo">...</div>
    </transition>
  `
}

const Bar = {
  template: `
    <transition name="fade">
      <div class="bar">...</div>
    </transition>
  `
}
```

### 基于路由的动态过渡

还可以基于当前路由与目标路由的变化关系，动态设置过渡效果：

``` html
<!-- 使用动态的 transition name -->
<transition :name="transitionName">
  <router-view></router-view>
</transition>
```

``` js
// 接着在父组件内
// watch $route 决定使用哪种过渡
watch: {
  '$route' (to, from) {
    const toDepth = to.path.split('/').length
    const fromDepth = from.path.split('/').length
    this.transitionName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
  }
}
```

查看完整例子请[移步这里](https://github.com/vuejs/vue-router/blob/next/examples/transitions/app.js)。

## 总结

其实作为入门的话，暂时先掌握这些知识，后续

# [回到vue.js知识列表首页](/pages/vip_2vue.md)