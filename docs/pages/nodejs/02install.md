# Node 安装

官网下载地址： [https://nodejs.org/en/download/](https://nodejs.org/en/download/)

## 安装方式

* windows 下安装

建议直接选择：`Windows Installer (.msi)`下载进行傻瓜化安装，由于网络环境及 Windows 的特点，其他方式都不够稳定，建议直接下载最新版本的安装包直接安装。

* mac 下安装

第一种：直接下载 pkg 文件安装，跟 windows 一致。不再赘述。

第二种：推荐使用 brew 进行安装

```shell
$ brew install node
```

* linux 下安装

参考官方各种系统下的安装方式：https://nodejs.org/en/download/package-manager/

## 测试是否安装成功

打开命令行工具（windows ： cmd.exe | powershell), mac|linux 打开终端。

```shell
# 通过cd命令进入任意目录下

cd 任意目录下
$ node -v
v9.9.0
$ npm -v
5.6.0

# 以上命令正常运行说明安装ok
```

---
参考：
https://nodejs.org/en/download/package-manager/
https://nodejs.org/

---

[老马免费视频教程](https://qtxh.ke.qq.com)

[返回首页](../readme.md)
