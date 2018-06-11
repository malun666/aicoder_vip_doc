## art-template 模板使用

官网： https://aui.github.io/art-template/zh-cn/index.html

第一步： 引入 art-template 的包

```shell
npm install --save art-template
npm install --save express-art-template
```

第二步：项目中设置 express 的应用 art-template 模板引擎

```js
const art_express = require('express-art-template');

const app = express(); // 创建app对象。

// 设置art的模板引擎
app.engine('art', art_express);

app.get('/user/list', (req, res) => {
  res.render('users/userlist2.art', {
    title: '你好啊！',
    users: userService.getUsers()
  });
});
```

### 语法

- 输出标准语法

```html
{{value}}
{{data.key}}
{{data['key']}}
{{a ? b : c}}
{{a || b}}
{{a + b}}
```

- 原始语法

```html
<%= value %>
<%= data.key %>
<%= data['key'] %>
<%= a ? b : c %>
<%= a || b %>
<%= a + b %>
```

模板一级特殊变量可以使用 `$data` 加下标的方式访问：

`{{$data['user list']}}`

- 原文输出标准语法

`{{@ value }}`
原始语法

`<%- value %>`

> 原文输出语句不会对 HTML 内容进行转义处理，可能存在安全风险，请谨慎使用。

- 条件标准语法

```html
{{if value}} ... {{/if}}
{{if v1}} ... {{else if v2}} ... {{/if}}
```

原始语法

```html
<% if (value) { %> ... <% } %>
<% if (v1) { %> ... <% } else if (v2) { %> ... <% } %>
```

- 循环标准语法

```html
{{each target}}
{{$index}} {{$value}}
{{/each}}
```

原始语法

```html
<% for(var i = 0; i < target.length; i++){ %>
<%= i %> <%= target[i] %>
<% } %>
```

`target` 支持 `array` 与 `object` 的迭代，其默认值为 $data。
`$value` 与 `$index` 可以自定义：`{{each target val key}}`。变量标准语法

`{{set temp = data.sub.content}}`
原始语法

`<% var temp = data.sub.content; %>`

- 模板继承标准语法

```html
{{extend './layout.art'}}
{{block 'head'}} ... {{/block}}
```

原始语法

```html
<% extend('./layout.art') %>
<% block('head', function(){ %> ... <% }) %>
```

模板继承允许你构建一个包含你站点共同元素的基本模板“骨架”。范例：

```html
<!--layout.art-->
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <title>{{block 'title'}}My Site{{/block}}</title>

    {{block 'head'}}
    <link rel="stylesheet" href="main.css">
    {{/block}}

</head>
<body>
    {{block 'content'}}{{/block}}
</body>
</html>
<!--index.art-->
{{extend './layout.art'}}

{{block 'title'}}{{title}}{{/block}}

{{block 'head'}}

<link rel="stylesheet" href="custom.css">
{{/block}}

{{block 'content'}}

<p>This is just an awesome page.</p>
{{/block}}
```

渲染 index.art 后，将自动应用布局骨架。

- 子模板标准语法

```html
{{include './header.art'}}
{{include './header.art' data}}
```

原始语法

```html
<% include('./header.art') %>
<% include('./header.art', data) %>
```

`data` 数默认值为 `$data`；标准语法不支持声明 `object` 与 `array`，只支持引用变量，而原始语法不受限制。
`art-template` 内建 HTML 压缩器，请避免书写 HTML 非正常闭合的子模板，否则开启压缩后标签可能会被意外“优化。过滤器注册过滤器

`template.defaults.imports.dateFormat = function(date, format){/_[code..]_/};`
`template.defaults.imports.timestamp = function(value){return value \* 1000};`

- 过滤器函数第一个参数接受目标值。

标准语法

```html
{{date | timestamp | dateFormat 'yyyy-MM-dd hh:mm:ss'}}
{{value | filter}} 过滤器语法类似管道操作符，它的上一个输出作为下一个输入。
```

原始语法

```html
<%= $imports.dateFormat($imports.timestamp(date), 'yyyy-MM-dd hh:mm:ss') %>
```
