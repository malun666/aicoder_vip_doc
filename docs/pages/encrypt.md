# JS 的加密库简介

作为前端，数据提交到后台之前，重要的数据要进行加密一下，虽然已经有 https 等技术，但是增加一道前端的加密还是相对更安全的。虽然，前端的加密很容破解，但是有总比没有强。

尤其是涉及到用户名和密码，最好加密后再进行发送 ajax 请求。

## 比较流行的前端加密库

- 斯坦福大学的[js 加密库](https://github.com/bitwiseshiftleft/sjcl)
- [crypto-js](https://github.com/brix/crypto-js)

## md5 加密

md5 加密算法是一种哈希算法，虽然已经被王小云博士找到了碰撞破解的方法，但是如果进行几次 md5 加密，破解难度就很高，所以目前依然可以使用。

以下是单独的 md5 加密帮助文件的使用：

- 第一步： [下载 md5 的 js 文件](http://files.cnblogs.com/mofish/md5.js)

- 第二步：引入 js 文件

- 第三步： 调用加密方法

```html
<script type="text/ecmascript" src="md5.js"></script>
<script type="text/javascript">  
  var hashHex = hex_md5("123dafd"); // 返回16进制的加密结果：a0deb4d124159da796c0e935ac8fbaa1
  var hashBase64 = b64_md5("123dafd"); // 返回 base64的加密结果：oN600SQVnaeWwOk1rI+6oQ
  var hashStr = str_md5("123dafd");  // 返回字符串的哈希结果： Þ´Ñ$§Àé5¬º¡
</script>  
```

## sh1 哈希加密

这个加密算法，非常出名，相对比较安全。建议使用。

- 第一步：[下载 sh1 加密 js](http://files.cnblogs.com/mofish/sha1.js)

- 第二步：页面中引入 sha1.js，调用方法为

- 第三步: 编写代码

```js
var shaHex = hex_sha1('mima123465'); // 07f804138ac308f552b17d7881105a9cb08758ca
var shaBase64 = b64_sha1('mima123465'); // B/gEE4rDCPVSsX14gRBanLCHWMo
var shaStr = str_sha1('mima123465'); // øÃõR±}xZ°XÊ
```

## base64 加密和解密

下载 [base64.js](https://files.cnblogs.com/files/mofish/base64.js)

```js
var b = new Base64();
var str = b.encode('admin:admin');
alert('base64 encode:' + str); //解密
str = b.decode(str);
alert('base64 decode:' + str);
```
