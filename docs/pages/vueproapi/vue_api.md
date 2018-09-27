# 宝洁SFA后台接口说明文档

## 综述

只有登录相关的接口不需要添加认证的token,其余所有的接口请求都必须在请求头中添加 `Authorization`.

所有的请求对应模型都默认提供了: 添加、修改、查询、复杂查询和删除操作的api。

### 接口统一地址模型

接口地址符合`RestFul`风格。

+ 添加

标题|说明|备注
---|---|---
地址| `POST /api/模型名字`|`header`中必须添加 `Authrization`对应的jwt的token.

例如:

```js
$.ajax({
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:3002/api/user",
  "method": "POST",
  "headers": {
    "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1MzcxODU3OTk5NzgsIm5hbWUiOiJzZGZmZmYifQ.WLVdU-GESQR6kJfUdLCBpNWUZMGAW6VsTk6lfAXC1xM"
  },
  "data": {
    "Name": "laomsds",
    "Passwd": "aaafc44445555",
    "isDel": "false"
  }
}).done(function (response) {
  console.log(response);
});
```

+ 修改

标题|说明|备注
---|---|---
地址|`PUT /api/模型名字/:id`|`header`中必须添加 `Authrization`对应的jwt的token.
支持`PATCH协议`

```js
$.ajax({
  "async": true,
  "crossDomain": true,
  "url": "http://%E4%BF%AE%E6%94%B9",
  "method": "PUT",
  "headers": {
    "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1MzcxODU3OTk5NzgsIm5hbWUiOiJzZGZmZmYifQ.WLVdU-GESQR6kJfUdLCBpNWUZMGAW6VsTk6lfAXC1xM",
    "Content-Type": "application/x-www-form-urlencoded"
  },
  "data": {
    "Name": "laoma2333"
  }
}).done(function (response) {
  console.log(response);
});
```

+ 删除

标题|说明|备注
---|---|---
地址|`DELETE /api/模型名字/:id`|`header`中必须添加 `Authrization`对应的jwt的token.

```js
$.ajax({
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:3002/api/user/5b9621e7cad9bf1c60e1d51f",
  "method": "DELETE",
  "headers": {
    "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1MzcxODU3OTk5NzgsIm5hbWUiOiJzZGZmZmYifQ.WLVdU-GESQR6kJfUdLCBpNWUZMGAW6VsTk6lfAXC1xM"
  }
}).done(function (response) {
  console.log(response);
});
```

+ 查询id

标题|说明|备注
---|---|---
地址|`GET /api/模型名字/:id`|`header`中必须添加 `Authrization`对应的jwt的token.

```js
$.ajax({
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:3002/api/user/5b9621e7cad9bf1c60e1d51f",
  "method": "GET",
  "headers": {
    "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1MzcxODU3OTk5NzgsIm5hbWUiOiJzZGZmZmYifQ.WLVdU-GESQR6kJfUdLCBpNWUZMGAW6VsTk6lfAXC1xM"
  }
}).done(function (response) {
  console.log(response);
});
```

### 复合查询

标题|说明
---|---
地址|`GET /api/模型名字`
注意事项|`header`中必须添加 `Authrization`对应的jwt的token.
分页| 当前页请求参数中,添加 `page`, 默认不分页.一页大小,请在请求参数中添加`limit`或`pageSize`. 例如:   `page=9&limit=10`,一页10条,第9页.
排序| 升序: `sort_asc=排序字段名`, 降序:`sort_desc=排序字段名`
等于过滤| `字段名_eq=等于的值`, 查询某个字段必须等于什么...
模糊查询过滤|`字段名_like=模糊查询的值`

```js
$.ajax({
  "async": true,
  "crossDomain": true,
  "url": "http://localhost:3002/api/user?sort_asc=Passwd&limit=5&page=1",
  "method": "GET",
  "headers": {
    "Authorization": "Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1MzcxODU3OTk5NzgsIm5hbWUiOiJzZGZmZmYifQ.WLVdU-GESQR6kJfUdLCBpNWUZMGAW6VsTk6lfAXC1xM"
  }
}).done(function (response) {
  console.log(response);
});
```

## 登陆相关

### 登陆获取token

用户登录WebApp时，首先对用户进行鉴权。

类型|说明
---|---
接口地址|`http://域名/login`  <br>例如：`http://aicoder.com/login`
请求方式|`POST`
数据类型|`application/x-www-form-urlencoded`
特殊要求|后台限制同一指纹浏览器,在1分钟内只能请求5次,超过次数认为是攻击,则禁止登录.

#### 请求参数

序号|字段|类型|性质|说明
---|---|---|---|---
1|Name|String|必填|用户名
2|Passwd|String|必填|密码

#### 返回值

序号|字段|类型|说明
---|---|---|---
1|user|Object|登陆成功的用户对象信息
2|code|Number|登陆成功的编码，1成功， 0失败。
3|token|String|token密钥。
4|msg|String|消息内容。

> 登录成功后续请求都需要添加token密钥到header的Authorization中。

#### 返回实例

```js
// 登录成功消息
{
  "user": {
      "isDel": false,
      "_id": "5b9621a6944ecd21c43dff52",
      "Name": "laoma",
      "Passwd": "2344",
      ...
  },
  "code": 1,
  "msg": "登录成功!",
  "token": "eyJ0eXAiOiJKV1QiLCJh"
}

// 登录失败消息
{
  code: 0,
  msg: '登录验证失败'
}
```

## 用户接口相关

### 用户ID查询

类型|说明
---|---
接口地址|`http://域名/user/:id`<br>例如：`http://aicoder.com/user/29`
请求方式|`GET`
数据类型|`application/x-www-form-urlencoded`

#### 请求参数

序号|字段|类型|性质|说明
---|---|---|---|---
1|id|String|必填|这个是路由参数，必须放入地址中
2|token|String|必填|这个是登陆后，用户的登陆token。数据请放到请求的`Authorization`

#### 返回值

序号|字段|类型|说明
---|---|---|---
1|_id|String|自动生成的主键
2|Name|String|用户名
3|Passwd|String|密码

#### 返回实例

```js
{
  "_id": "5b9621e7cad9bf1c60e1d51f",
  "isDel": false,
  "Name": "laomsds",
  "Passwd": "2344"
}
```

### 用户删除

### 用户修改接口

### 用户修改密码

### 用户复杂查询

## 订单相关接口

### 获取店铺订单

### 提交订单

### 取消订单

### 查询订单

### 查询店铺订单

## 店铺相关接口

### 查询店铺信息

### 设置店铺信息（新增门店)

## 消息相关接口

### 发布消息

### 获取消息

### 阅读消息

## 公告接口

### 获取公司公告

### 阅读公司公告

## 商品相关接口

### 查询商品详情

### 同步库存

## 部门相关接口

### 获取部门数据

### 获取部门员工

## 签到接口

### 商店签到

### 商店上传图片

## 地图接口相关
