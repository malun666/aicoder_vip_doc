# 宝洁SFA后台接口说明文档

## 登陆相关

### 登陆获取token

类型|说明
---|---
接口地址|`http://域名/login`  <br>例如：`http://aicoder.com/login`
请求方式|`POST`
数据类型|`application/x-www-form-urlencoded`

#### 请求参数

序号|字段|类型|性质|说明
---|---|---|---|---
1|Name|String|必填|用户名
2|Passwd|String|必填|密码

#### 返回值

序号|字段|类型|说明
---|---|---|---
1|user|Object|登陆成功的用户对象信息
2|code|Number|登陆成功的编码，1成功， -1失败。
3|token|String|token密钥。所有的后续请求都需要添加此密钥到header中。

#### 返回实例

```js
{
  "user": {
      "isDel": false,
      "_id": "5b9621a6944ecd21c43dff52",
      "Name": "laoma",
      "Passwd": "2344",
      "__v": 0
  },
  "code": 1,
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc0RlbCI6ZmFsc2UsIl9pZCI6IjViOTYyMWE2OTQ0ZWNkMjFjNDNkZmY1MiIsIk5hbWUiOiJsYW9tYSIsIlBhc3N3ZCI6IjIzNDQiLCJfX3YiOjB9.6bWNq5j_pUxlMUpiCSIE1_3iuqim1nQIQX7Qxo0KMG0"
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