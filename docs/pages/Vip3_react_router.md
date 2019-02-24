# React Router教程

React项目的可用的路由库是`React-Router`,当然这也是官方支持的。它也分为：

- react-router 核心组件
- react-router-dom 应用于浏览器端的路由库（单独使用包含了react-router的核心部分）
- react-router-native 应用于native端的路由

> 以下教程我们都以Web端为主，所以所有的教程内容都是默认关于`react-router-dom`的介绍。

进行网站（将会运行在浏览器环境中）构建，我们应当安装`react-router-dom`。`react-router-dom`暴露出`react-router`中暴露的对象与方法，因此你只需要安装并引用`react-router-dom`即可。

## Installation | 安装

安装：

```sh
yarn add react-router-dom
# 或者，不使用 yarn
npm install react-router-dom
```

## 路由的基本概念

现在的React Router版本中已不需要路由配置，现在一切皆组件。

ReactRouter中提供了以下三大组件：

- Router是所有路由组件共用的底层接口组件，它是路由规则制定的最外层的容器。
- Route路由规则匹配，并显示当前的规则对应的组件。
- Link路由跳转的组件

当然每个组件下又会有几种不同的子类组件实现。比如：
Router组件就针对不同功能和平台对应用：

- `<BrowserRouter>`  浏览器的路由组件
- `<HashRouter>`    URL格式为Hash路由组件
- `<MemoryRouter>`  内存路由组件
- `<NativeRouter>`  Native的路由组件
- `<StaticRouter>` 地址不改变的静态路由组件

三大组件使用的关系：

![](../images/react_router.png)

如果说我们的应用程序是一座小城的话，那么Route就是一座座带有门牌号的建筑物，而Link就代表了到某个建筑物的路线。有了路线和目的地，那么就缺一位老司机了，没错Router就是这个老司机。

## 第一个Demo

现在你可以复制任意的示例代码，并粘贴到 `src/App.js`。如下：

```jsx
import React, { Component } from 'react';
import { HashRouter as Router, Link, Route } from 'react-router-dom';
import './App.css';

const Home = () => (
  <div>
    <h2>Home</h2>
  </div>
)

const About = () => (
  <div>
    <h2>About</h2>
  </div>
)

const Product = () => (
  <div>
    <h2>Product</h2>
  </div>
)


class App extends Component {
  render() {
    return (
      <Router>
        <div className="App">
          <Link to="/">Home</Link>
          <Link to="/About">About</Link>
          <Link to="/Product">Product</Link>
          <hr/>
          <Route path="/" exact component={Home}></Route>
          <Route path="/about" component={About}></Route>
          <Route path="/product" component={Product}></Route>
        </div>
      </Router>
    );
  }
}

export default App;
```

## Router组件

### BrowserRouter组件

`BrowserRouter`主要使用在浏览器中，也就是WEB应用中。它利用HTML5 的history API来同步URL和UI的变化。当我们点击了程序中的一个链接之后,`BrowserRouter`就会找出与这个`URL`匹配的`Route`，并将他们对应的组件渲染出来。 `BrowserRouter`是用来管理我们的组件的，那么它当然要被放在最顶级的位置，而我们的应用程序的组件就作为它的一个子组件而存在。

```js
import * as React from 'react';
import * as ReactDOM from 'react-dom';
import { BrowserRouter } from 'react-router-dom';

ReactDOM.render(
    <BrowserRouter>
        <App/>
    </BrowserRouter>,
    document.body);

```

`BrowserRouter`组件提供了四个属性。

- `basename`: 字符串类型，路由器的默认根路径
- `forceRefresh`: 布尔类型，在导航的过程中整个页面是否刷新
- `getUserConfirmation`: 函数类型，当导航需要确认时执行的函数。默认是：`window.confirm`
- `keyLength`: 数字类型`location.key` 的长度。默认是 6

#### basename 属性

当前位置的基准 URL。如果你的页面部署在服务器的二级（子）目录，你需要将 `basename` 设置到此子目录。正确的 URL 格式是前面有一个前导斜杠，但不能有尾部斜杠。

例如：有时候我们的应用只是整个系统中的一个模块，应用中的URL总是以 http://localhost/admin/ 开头。这种情况下我们总不能每次定义Link和Route的时候都带上admin吧？react-router已经考虑到了这种情况，所以为我们提供了一个basename属性。为BrowserRouter设置了basename之后，Link中就可以省略掉admin了，而最后渲染出来的URL又会自动带上admin。

```js
<BrowserRouter basename="/admin"/>
    ...
    <Link to="/home"/> // 被渲染为 <a href="/admin/home">
    ...
</BrowserRouter>
```

#### getUserConfirmation: func

当导航需要确认时执行的函数。默认使用 [`window.confirm`](https://developer.mozilla.org/en-US/docs/Web/API/Window/confirm)。

```js
// 使用默认的确认函数
const getConfirmation = (message, callback) => {
  const allowTransition = window.confirm(message)
  callback(allowTransition)
}

<BrowserRouter getUserConfirmation={getConfirmation}/>
```

#### forceRefresh: bool

当设置为 `true` 时，在导航的过程中整个页面将会刷新。
只有当浏览器不支持 [HTML5 的 history API](http://caniuse.com/#feat=history) 时，才设置为 `true`。

```js
const supportsHistory = 'pushState' in window.history
<BrowserRouter forceRefresh={!supportsHistory}/>
```

#### keyLength: number

`location.key` 的长度。默认是 6。

```js
<BrowserRouter keyLength={12}/>
```

#### children: node

渲染[单一子组件（元素）](https://facebook.github.io/react/docs/react-api.html#react.children.only)。

### HashRouter

`HashRouter` 使用 URL 的 hash (例如：`window.location.hash`) 来保持 UI 和 URL 的同步。

>**注意：** 使用 hash 的方式记录导航历史不支持 `location.key` 和`location.state`。在以前的版本中，我们为这种行为提供了 shim，但是仍有一些问题我们无法解。任何依赖此行为的代码或插件都将无法正常使用。由于该技术仅用于支持传统的浏览器，因此在用于浏览器时可以使用 `<BrowserHistory>` 代替。

跟`BrowserRouter`类似，它也有：`basename`、`getUserConfirmation`、`children`属性，而且是一样的。

#### hashType: string

`window.location.hash` 使用的 hash 类型。有如下几种：

- `"slash"` - 后面跟一个斜杠，例如 `#/` 和 `#/sunshine/lollipops`
- `"noslash"` - 后面没有斜杠，例如 `#` 和 `#sunshine/lollipops`
- `"hashbang"` - Google 风格的 ["ajax crawlable"](https://developers.google.com/webmasters/ajax-crawling/docs/learn-more)，例如 `#!/` 和 `#!/sunshine/lollipops`

默认为 `"slash"`。

### MemoryRouter 

主要用在ReactNative这种非浏览器的环境中，因此直接将URL的history保存在了内存中。
StaticRouter 主要用于服务端渲染。

## Link组件

Link就像是一个个的路牌，为我们指明组件的位置。Link使用声明式的方式为应用程序提供导航功能，定义的Link最终会被渲染成一个a标签。Link使用to这个属性来指明目标组件的路径，可以直接使用一个字符串，也可以传入一个对象。

```js
// 字符串参数
<Link to="/query">查询</Link>

// 对象参数
<Link to={{
  pathname: '/query',
  search: '?key=name',
  hash: '#hash'
}}>查询</Link>
```

Link提供的功能并不多，好在我们还有NavLink可以选择。NavLink是一个特殊版本的Link，可以使用activeClassName来设置Link被选中时被附加的class，使用activeStyle来配置被选中时应用的样式。此外，还有一个exact属性,此属性要求location完全匹配才会附加class和style。这里说的匹配是指地址栏中的URl和这个Link的to指定的location相匹配。

```js
// 选中后被添加class selected
<NavLink to={'/'} exact activeClassName='selected'>Home</NavLink>
// 选中后被附加样式 color:red
<NavLink to={'/gallery'} activeStyle={{color:red}}>Gallery</NavLink>
```

## Route组件

Route应该是react-route中最重要的组件了，它的作用是当location与Route的path匹配时渲染Route中的Component。如果有多个Route匹配，那么这些Route的Component都会被渲染。

与Link类似，Route也有一个exact属性，作用也是要求location与Route的path绝对匹配。

```js
// 当location形如 http://location/时，Home就会被渲染。
// 因为 "/" 会匹配所有的URL，所以这里设置一个exact来强制绝对匹配。
<Route exact path="/" component={Home}/>
<Route path="/about" component={About}/>
```

### Route的三种渲染方式

1. component: 这是最常用也最容易理解的方式，给什么就渲染什么。
2. render: render的类型是function，Route会渲染这个function的返回值。因此它的作用就是附加一些额外的逻辑。

```js
<Route path="/home" render={() => {
    console.log('额外的逻辑');
    return (<div>Home</div>);
    }/>
```

3. children: 这是最特殊的渲染方式。一、它同render类似,是一个function。不同的地方在于它会被传入一个match参数来告诉你这个Route的path和location匹配上没有。二、第二个特殊的地方在于，即使path没有匹配上，我们也可以将它渲染出来。秘诀就在于前面一点提到的match参数。我们可以根据这个参数来决定在匹配的时候渲染什么，不匹配的时候又渲染什么。

```js
// 在匹配时，容器的calss是light，<Home />会被渲染
// 在不匹配时，容器的calss是dark，<About />会被渲染
<Route path='/home' children={({ match }) => (
    <div className={match ? 'light' : 'dark'}>
    {match ? <Home/>:<About>}
    </div>
  )}/>
```

所有路由中指定的组件将被传入以下三个 props 。

- match.
- location.
- history.

这里主要说下match.params.透过这个属性，我们可以拿到从location中解析出来的参数。当然，如果想要接收参数，我们的Route的path也要使用特殊的写法。

如下示例，三个Link是一个文章列表中三个链接，分别指向三篇id不同的文章。而Route用于渲染文章详情页。注意path='/p/:id' ，location中的对应的段会被解析为id=1 这样的键值。最终这个键值会作为param的键值存在。Route中的组件可以使用this.props.match.params.id来获取，示例中使用了结构赋值。

```js
<Link to='/p/1' />
<Link to='/p/2' />
<Link to='/p/3' />
......
<Route path='/p/:id' render={(match)=<h3>当前文章ID:{match.params.id}</h3>)} />
```

## Redirect组件

当这个组件被渲染是，location会被重写为Redirect的to指定的新location。它的一个用途是登录重定向，比如在用户点了登录并验证通过之后，将页面跳转到个人主页。

```js
<Redirect to="/new"/>
```

Router中常用的组件基本上都介绍了一遍，不过也只是蜻蜓点水而已。如果想更透彻的理解路由系统，建议还是去翻看官方文档并且试着去用一用。文中给出的示例也是非常精简的片段，仅仅作为参考。