# PostgreSQL入门

## centos7 安装

以下是centos7的配置过程，仅供参考。详细安装官网教程[地址](https://www.postgresql.org/download/linux/redhat/)。

第一步： 下载rpm包

```sh
yum install https://download.postgresql.org/pub/repos/yum/11/redhat/rhel-7-x86_64/pgdg-centos11-11-2.noarch.rpm
```

第二步： 安装客户端

```sh
yum install postgresql11
```

第三步： 安装服务器端的包

```sh
yum install postgresql11-server
```

第四步： 初始化数据库和设置开机启动

```sh
/usr/pgsql-11/bin/postgresql-11-setup initdb
systemctl enable postgresql-11
systemctl start postgresql-11
```

> 注意PostegreSQL 默认用的端口是5432，请设置防火墙打开5432端口。

测试是否监听

```sh
#查看Centos端口命令：
netstat -lntp #查看监听(Listen)的端口

# netstat -antp #查看所有建立的TCP连接
```

## 使用PostgreSQL控制台登录

首先，新建一个Linux新用户，可以取你想要的名字，这里为dbuser。

```sh
sudo adduser dbuser
passwd dbuser  # 修改密码，两次输入一致。
```

然后，切换到postgres用户。安装PostgreSQL自动创建的用户。

```sh
sudo su - postgres
```

下一步，使用psql命令登录PostgreSQL控制台。

```sh
sudo su - postgres
psql
```

这时相当于系统用户postgres以同名数据库用户的身份，登录数据库，这是不用输入密码的。如果一切正常，系统提示符会变为"postgres=#"，表示这时已经进入了数据库控制台。以下的命令都在控制台内完成。

第一件事是使用\password命令，为postgres用户设置一个密码。

```sh
\password postgres
```

第二件事是创建数据库用户dbuser（刚才创建的是Linux系统用户），并设置密码。

```sql
CREATE USER dbuser WITH PASSWORD 'password';
```

第三件事是创建用户数据库，这里为exampledb，并指定所有者为dbuser。

```sql
CREATE DATABASE exampledb OWNER dbuser;
```

第四件事是将exampledb数据库的所有权限都赋予dbuser，否则dbuser只能登录控制台，没有任何数据库操作权限。

```sql
GRANT ALL PRIVILEGES ON DATABASE exampledb to dbuser;
```

最后，使用\q命令退出控制台（也可以直接按ctrl+D）。

```sh
\q
```

然后就可以使用dbuser登录并创建数据库，并添加表，添加数据等操作了。

## 创建表和初始化数据

```sh
su - dbuser
psql -d exampledb

# 登录成功后，创建表和数据
CREATE TABLE UserInfo(Id SERIAL PRIMARY KEY, UserName VRACHAR(32) NULL, Del SMALLINT
NULL, SubOn DATE NULL, Mail VRACHAR(128) NULL, Phone VRACHAR(32) NULL, Remark VRACHAR(64) NULL);

# 插入数据

INSERT INTO UserInfo(UserName,Del, SubOn, Mail, Phone, Remark) VALUES('laoma', 0, '20
19-05-16', 'ss@dfs.com', '333', '3333');

# 查询数据

select * from UserInfo
```

## 查询配置文件所在

首先进入 postgres用户的plsq的命令行运行命令。

```sh
sudo su - postgres
psql
```

```sql
select name,setting from pg_settings where category='File Locations';
```

输出：

```sh
       name        |                setting
-----------------------------------------------------------
 config_file       | /var/lib/pgsql/11/data/postgresql.conf
 data_directory    | /var/lib/pgsql/11/data
 external_pid_file |
 hba_file          | /var/lib/pgsql/11/data/pg_hba.conf
 ident_file        | /var/lib/pgsql/11/data/pg_ident.conf
```

那么配置文件就是： `/var/lib/pgsql/11/data/postgresql.conf`

## 修改PostgreSQL的配置文件*允许远程连接*

第一步：修改postgresql.conf，监听本机的所有的ip地址。

```sh
vim /var/lib/pgsql/11/data/postgresql.conf`
```

修改如下：

```diff
-- #listen_addresses = 'localhost'
++ listen_addresses = '*'  
```

第二步： 修改本机的PostgreSQL允许远程连接的ip

编辑$PGDATA/pg_hba.conf, 例如我的文件就是：

```sh
vim /var/lib/pgsql/11/data/pg_hba.conf
```

修改如下：

```diff
#local   all             all                                     peer
-- local   all             all                                    peer
++ local   all             all                                    md5

...

# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
++ host    all             all             0.0.0.0/0               md5
```

- 允许特定ip主机连接

```sh
host    all             all             192.168.1.112/32 md5
```

- 允许特定网段的主机连接

```sh
host all all 192.168.1.0/24 md5
```

```sh
# 重启动 PostgreSQL
systemctl restart postgresql-11

netstat -lntp #查看监听(Listen)的端口

# 如果监听的ip变为 0.0.0.0 则可以被其他服务器连接并访问。
0 0.0.0.0:5432
```

## 客户端推荐

- [Admin4](http://www.admin4.org/)

- [Barman](https://www.pgbarman.org/index.html)

## 参考

1.[PostgreSQL新手入门](http://www.ruanyifeng.com/blog/2013/12/getting_started_with_postgresql.html)
1.[PostgreSQL官网](https://www.postgresql.org/download/linux/redhat/)
1.[PostgreSQL 配置文件postgresql.conf](https://blog.csdn.net/syshzbtt/article/details/80953963)
