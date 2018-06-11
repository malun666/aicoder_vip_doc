# jQuery EasyUI 详解

## EasyUI 简介

`easyui` 是一种基于 `jQuery` 的用户界面插件集合。

`easyui` 为创建现代化，互动，JavaScript 应用程序，提供必要的功能。

使用 `easyui` 你不需要写很多代码，你只需要通过编写一些简单 HTML 标记，就可以定义用户界面。

`easyui` 是个完美支持 HTML5 网页的完整框架。

`easyui` 节省您网页开发的时间和规模。

`easyui` 很简单但功能强大的。

官网地址：[http://www.jeasyui.com/index.php](http://www.jeasyui.com/index.php)

文档地址：

- [中文文档](http://www.jeasyui.net/tutorial/)
- [英文文档](http://www.jeasyui.com/documentation/index.php)

## 快速入门 弹出对话框 demo

**第一步： 下载** [Jquery EasyUI](https://files.cnblogs.com/files/fly_dragon/jquery-easyui-1.5.5.2.zip)

你在使用和进行开发时，请遵守官方相应的条款，尊重他们的知识版权。

目前官方最新版本是：jQuery EasyUI 1.5，官方提供了两个版本供下载，GPL 版本和商业版本，你根据自己的需要下载

- GPL 版本
  GPL 版本在 GPl 协议下有效，你能在任何遵循 GPl 协议的项目下使用它。
- 商业版本
  商业版在 Commercial 协议下有效，你能在任何非 GPL/专有的协议下使用。

**第二步：创建 html 文件，并添加相关引用**

创建项目的文件夹

```dir
easydemo                            // 项目根目录
├── index.html                      // 我们的测试页面
└── lib                             // 第三方组件
    └── jquery-easyui-1.5.5.2       // 下载的easyui的压缩包解压后的文件夹
        ├── easyloader.js           // easyui的动态加载组件的js
        ├── jquery.easyui.min.js    // 压缩后的包！！！关键！！
        ├── jquery.easyui.mobile.js
        ├── jquery.min.js           // 依赖的jq的文件
        ├── locale                  // 本地语言的文件夹
        ├── plugins                 // 拆分的组件
        └── themes                  // 样式主题文件夹
```

**第三步： 修改 index.html 文件**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <!-- easyui的样式，可以选择其他主题 -->
  <link rel="stylesheet" href="./lib/jquery-easyui-1.5.5.2/themes/bootstrap/easyui.css">
  <!-- easyui的图标css文件 -->
  <link rel="stylesheet" href="./lib/jquery-easyui-1.5.5.2/themes/icon.css">
  <!-- easyui 依赖jq -->
  <script src="./lib/jquery-easyui-1.5.5.2/jquery.min.js"></script>
  <!-- jq easyui的js脚本 -->
  <script src="./lib/jquery-easyui-1.5.5.2/jquery.easyui.min.js"></script>
  <!-- 引用语言包 -->
  <script src="./lib/jquery-easyui-1.5.5.2/locale/easyui-lang-zh_CN.js"></script>

  <title>AICODER jQuery EasyUI Demos</title>
</head>
<body>
  <!-- 功能：点击按钮弹出模态对话框 -->
  <input type="button" value="弹出模态对话框" id="btnOpenDialog">

  <!-- 设置弹出来的对话框div，首先设置为隐藏 -->
  <div id="addDialog" style="display: none;">
    <h3>添加的对话框</h3>
  </div>
  <script>
    $(function () {
      $('#btnOpenDialog').on('click', function () {
        // 弹出对话框
        $('#addDialog').dialog({
          modal: true,  // 是否是模态对话框
          title: 'AICODER全栈实习--添加用户！',  // 弹出来的窗口的标题
          width: 400, // 窗口的宽度
          height: 400, // 窗口的高度
        });
      });
    });
  </script>
</body>
</html>
```

结果如下：

![弹出dialog](..//images/easyui_dialog.png)

## easyui 的布局

jq easyui 把网页分成了 `上`、`下`、`左`、`中`、`右`，分别对应：`North`、`South`、`West`、`Center`、`East`。

easyui 增加了自定义的属性：`data-options`，通过它可以设置 easyui 组件的选项。

```html
<body class="easyui-layout">
  <div data-options="region:'north',split:true" style="height:100px;"></div>
  <div data-options="region:'south',split:true" style="height:100px;"></div>
  <div data-options="region:'east',title:'East',split:true" style="width:100px;"></div>
  <div data-options="region:'west',title:'West',split:true" style="width:100px;"></div>
  <div data-options="region:'center',title:'center title'" style="padding:5px;background:#eee;">
    <input type="button" value="弹出模态对话框" id="btnOpenDialog">
  </div>
</body>
```

### 布局区域选项说明

| 选项          | 类型      | 说明                                                | 默认值   |
|-------------|---------|---------------------------------------------------|-------|
| region      | String  | 所处的方位，可取值：`North`、`South`、`West`、`Center`、`East`  | null  |
| title       | String  | 区域的标题                                             | null  |
| split       | Boolean | 是否跟其他区域进行分离（增加外边距)                                | false |
| href        | String  | 从后台获取 html，并显示在此区域                                | null  |
| collapsible | Boolean | 是否显示可折叠按钮                                         | true  |
| iconCls     | string  | An icon CSS class to show a icon on panel header. | null  |
| minWidth    | Number  | 区域的最小宽度                                           | 10    |
| minHeight   | Number  | 区域的最小高度                                           | 10    |
| maxWidth    | Number  | 区域的最大宽度                                           | 10000 |
| maxHeight   | Number  | 区域的最大高度                                           | 10000 |

### 布局的方法

| 方法名      | 参数      | 描述                                                                          |
|----------|---------|-----------------------------------------------------------------------------|
| resize   | param   | 改变布局的大小. 参数 `param` 对象可以设置以下属性: <br>**width**: 布局的宽度.<br>**height**: 布局的高度. |
| collapse | region  | 折叠区域, `region` 参数可以取值：`north`,`south`,`east`,`west`.                        |
| expand   | region  | 展开区域面板,  `region` 参数可以取值：`north`,`south`,`east`,`west`.                     |
| add      | options | 添加一个面板                                                                      |
| remove   | region  | 移除一个区域面板, 参数 `region` 可以取值:`north`,`south`,`east`,`west`.                   |
| split    | region  | 移除区域        参数 `region` 可以取值:`north`,`south`,`east`,`west`                  |
| unsplit  | region  | 取消移除区域   参数 `region` 可以取值:`north`,`south`,`east`,`west`                     |
例如：

```js
// 改变大小
$('#cc').layout('resize', {
  width: '80%',
  height: 300
});

// 折叠区域
$('#btnCloseEast').click(function () {
  $(document.body).layout('collapse', 'east');
});

// 展开区域
$('#btnExpandEast').click(function () {
  $(document.body).layout('expand', 'east');
});
```

### 布局的事件

| 事件名        | 参数     | 描述         |
|------------|--------|------------|
| onCollapse | region | 当折叠区域的时候触发 |
| onExpand   | region | 当展开区域的时候触发 |
| onAdd      | region | 当添加区域的时候触发 |
| onRemove   | region | 当移除区域的时候触发 |

```js
// 注册监听事件
$(document.body).layout({
  onCollapse: function (region) {
    $.messager.alert('消息标题', '消息内容：折叠了面板：' + region, 'info');
  },
  onExpand: function (region) {
    $.messager.alert('消息标题', '消息内容：展开了面板：' + region, 'warning');
  }
});
```

## easyui 的消息组件

easyui提供了丰富的弹出消息框的方法。

### 弹出消息框

`$.messager.alert` 方法提供了弹出消息的功能，类似`window.alert`的功能。

此方法接受的参数：

| 参数名   | 说明                                                       |
|-------|----------------------------------------------------------|
| title | 显示消息框的标题                                                 |
| msg   | 消息内容.                                                    |
| icon  | 消息的内容前面的图标，可以用图标名为: `error`,`question`,`info`,`warning`. |
| fn    | 点击ok按钮后的回调函数                                             |

两种调用模式

```js
// 第一种： 传入三个字符串参数
$.messager.alert('My Title','Here is a info message!','info');

// 第二种： 传入对象参数
$.messager.alert({
  title: 'My Title',
  msg: 'Here is a message!',
  fn: function(){
    //...
  }
});
```

### 弹出确认对话框

`$.messager.confirm` 方法提供了弹出消息的功能，类似`window.confirm`的功能。

此方法接受的参数：

| 参数名   | 说明           |
|-------|--------------|
| title | 显示消息框的标题     |
| msg   | 消息内容.        |
| fn    | 点击ok按钮后的回调函数 |

两种调用模式

```js
// 第一种： 传入三个字符串参数
$.messager.confirm('Confirm', 'Are you sure to exit this system?', function(r){
  if (r){ // 如果用户点击确认，那么  r就是true，否则fals
    // exit action;
  }
});

// 第二种： 传入对象参数
$.messager.confirm({
  title: 'My Title',
  msg: 'Are you confirm this?',
  fn: function(r){
    if (r){  // 如果用户点击确认，那么  r就是true，否则fals
      alert('confirmed: '+r);
    }
  }
});
```

## easyui 的树组件

easyui 树形菜单（Tree）也可以定义在 `<ul>` 元素中。

初始化树有两种方式：

- 通过标签初始化

- 通过js初始化

以下是通过js初始化的案例

```js
$('#tt').tree({
  checkbox: true, // 是否显示多选框
  data: [{
    id: 1,
    text: '北京',
    state: 'open',
    attributes: {
      url: "/demo/book/abc",
      price: 100
    },
    children: [{
      id: 7,
      text: "昌平",
      checked: true
    }, {
      id: 8,
      text: "朝阳",
      state: "closed"
    }]
  }, {
    id: 2,
    text: '山东',
    state: 'open',
    attributes: {
      url: "/demo/book/abc",
      price: 100
    },
    children: [{
      id: 9,
      text: "潍坊",
      checked: true
    }, {
      id: 10,
      text: "青岛",
      state: "closed"
    }]
  },],
  animate: true,  // 节点折叠和展开是否使用动画
  lines: true, // 是否显示 节点之间的线条
  dnd: true, // 是否可拖拽
});
```

结果：

![tree](../images/easyui_tree.png)

## easyui 表格组件

表格是easyui里面使用最广的组件。

以下为demo：

```js
$('#tt').datagrid({
  url: '/UserInfo/GetAllUserInfos',//rows:一页有多少条，page：请求当前页
  title: '用户信息列表',
  width: 700,
  height: 400,
  fitColumns: true,
  idField: 'ID',
  loadMsg: '正在加载用户的信息...',
  pagination: true,
  singleSelect: false,
  pageSize: 10,
  pageNumber: 1,
  pageList: [10, 20, 30],
  queryParams: queryParam,//让表格在加载数据的时候，额外传输的数据。
  columns: [[
    { field: 'ck', checkbox: true, align: 'left', width: 50 },
    { field: 'ID', title: '用户的编号', width: 80 },
    { field: 'UName', title: '用户名', width: 120 },
    { field: 'Pwd', title: '密码', width: 120 },
    { field: 'Remark', title: '备注', width: 120 },
    {
      field: 'SubTime', title: '提交时间', width: 80, align: 'right',
      formatter: function (value, row, index) {
        return (eval(value.replace(/\/Date\((\d+)\)\//gi, "new Date($1)"))).pattern("yyyy-M-d h:m:s");
      }
    },
    {
      field: 'ModfiedOn', title: '操作', width: 120, formatter: function (value, row, index) {
        var str = "";
        str += "<a href='javascript:void(0)' class='editLink' uid='" + row.ID + "'>修改</a> &nbsp;&nbsp;";
        str += "<a href='javascript:void(0)' class='deletLink' uid='" + row.ID + "'>删除</a>";
        return str;
      }
    }
  ]],
  toolbar: [{
    id: 'btnDownShelf',
    text: '添加',
    iconCls: 'icon-add',
    handler: function () {
      //alert("dd");
      //弹出一个添加的对话框
      addClickEvent();
    }
  }, {
    id: 'btnDelete',
    text: '删除',
    iconCls: 'icon-cancel',
    handler: function () {
      deleteEvent();
    }
  }, {
    id: 'btnEdit',
    text: '修改',
    iconCls: 'icon-edit',
    handler: function () {
      //校验你是否只选中一个 用户
      var selectedRows = $('#tt').datagrid("getSelections");
      if (selectedRows.length != 1) {
        //error,question,info,warning.
        $.messager.alert("错误提醒", "请选中1行要修改数据！", "question");
        return;
      }

      editEvent(selectedRows[0].ID);
    }
  }, {
    id: 'btnSetRole',
    text: '设置角色',
    iconCls: 'icon-redo',
    handler: function () {
      //判断是否选中一个用户进行角色设置。弹出一个设置角色的对话框出来。
      setRole();
    }
  }, {
    id: 'btnSetAction',
    text: '设置特殊权限',
    iconCls: 'icon-redo',
    handler: function () {
      //判断是否选中一个用户进行角色设置。弹出一个设置角色的对话框出来。
      setAction();
    }
  }],
  onHeaderContextMenu: function (e, field) {

  },
  onLoadSuccess: function (data) {
    $(".editLink").click(function () {//
      editEvent($(this).attr("uid"));
      return false;
    });
  }
});

```

## easyui 的 Tab 组件

tab可以直接通过html标签创建。

```html
<div id="tt" class="easyui-tabs" style="height:250px;" data-options="fit: true">
  <div title="Tab1" style="padding:20px;display:none;">
    tab1
  </div>
  <div title="Tab2" data-options="closable:true" style="overflow:auto;padding:20px;display:none;">
    tab2
  </div>
  <div title="Tab3" data-options="iconCls:'icon-reload',closable:true" style="display:none;">
    tab3
  </div>
</div>
```

其他常用的方法：

- 通过js控制添加tab标签

```js
$('#tt').tabs('add',{
    title:'New Tab',
    content:'Tab Body',
    closable:true,
    tools:[{
        iconCls:'icon-mini-refresh',
        handler:function(){
            alert('refresh');
        }
    }]
});
```

- 判断tab是存在

```js
// exists 可以接受一个 tab的索引，或者是tab的title的字符串
$('#tt').tabs('exists', 1);
$('#tt').tabs('exists', 'tab1');
```

- 选中某个tab页签

```js
$('#tt').tabs('select', 1);
$('#tt').tabs('select', 'tab1');
```

- 获取选中的tab页签

```js
$('#tt').tabs('getSelected');  // 返回tab的索引
```

