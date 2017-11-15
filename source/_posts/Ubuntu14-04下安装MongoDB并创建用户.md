---
title: Ubuntu14.04下安装MongoDB并创建用户
date: 2017-01-06 14:06:38
tags: [mongodb]
---

在 Ubuntu14 下安装 MongoDB 并创建用户开启鉴权

由于阿里云通知我，我的MongoDB服务器有安全漏洞，是因为之前没有设置权限，所以特写次文章

<!--more-->

## 安装Mongodb

##### 1. 下载最新版 MongoDB
```shell
$ wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-ubuntu1404-3.4.1.tgz
```

##### 2. 解压下载好的 taz 包, 并重命名
```shell
$ tar -zxvf mongodb-linux-x86_64-ubuntu1404-3.4.1.tgz
$ mv mongodb-linux-x86_64-ubuntu1404-3.4.1.tgz mongodb
```

##### 3. 编写配置文件 mongodb.conf
```
# 数据库存放路径（这里写的是相对路径）
dbpath=data/db
# 端口号
port=27017
# 先把auth关闭，添加用户后再开启
auth=false
# 日志文件存放路径
logpath=mongodb.log
# 日志是否追加
logappend=true
# 后台进程启动
fork=true
```

##### 4. 创建数据库存放目录，并修改权限
```shell
$ mkdir -p data/db
$ chmod -R 777 data/db/
```

##### 5. 启动服务
```shell
$ ./bin/mongod -f mongodb.conf
```

##### 6. 连接到 mongodb
```
./bin/mongo
MongoDB shell version v3.4.1
connecting to: mongodb://127.0.0.1:27017
MongoDB server version: 3.4.1
> 
```

> 请自行设置 $PATH
> 这时候我们看一下log日志后有3个警告信息分别为，暂时我还不会解决...
> 1). WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
> 2). WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'. We suggest setting it to 'never'
> 3). WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'. We suggest setting it to 'never'


## 创建用户并使用
> 注意：MongoDB 2.6 版本之前使用 db.addUser() 创建用户, 2.6 版本之后使用 db.createUser() 创建用户

```
addUser() 参数只有两个 1. 用户名 2. 密码
createUser() 有多个参数, 分别为
 	{
		user: <name>,
		pwd: <password>,
		customData: <用户说明>,
		roles: [
			{
				role: <角色类型>,
				db: <创建在哪个数据库>
			}
		]
	}

数据库角色类型分别有:
	(
		read: 只读
		readWrite: 读写
		dbAdmin: 数据库管理权限
		dbOwner: 前三者的集合体，有前三者所有的权限
		userAdmin: 用户管理
	)

了解了这么多，接下来就要创建用户了
```

##### 1. 创建用户

> 创建一个admin账户用来管理所有的用户，
> 再创建一个个人账号用来管理所有的数据库

```
# 创建一个admin，给予userAdminAnyDatabase的权限，可以管理所有的用户：
> db.createUser({user: 'useradmin', pwd: 'useradmin', roles: [{role: 'userAdminAnyDatabase', db: 'admin'}]})
# 创建一个个人用户，给予dbAdminAnyDatabase的权限，可以管理所有数据库：
> db.createUser({user: 'zail', pwd: 'zail', roles: [{role: 'dbAdminAnyDatabase', db: 'admin'}]})
# 之后就可以通过 admin 账户来管理用户， 通过自己的账号来管理所有的数据库
```

##### 2. 开启鉴权认证
```
# 关闭 MongoDB
> use admin
> db.shutdownServer()

# 修改配置文件 mongodb.conf 中的 auth=true
# 保存
# 重启 MongoDB
$ ./bin/mongod -f mongodb.conf

# 之后就可以使用账号链接 MongoDB 了
$ ./bin/mongo admin -u admin -p admin
$ ./bin/mongo admin -u zail -p zail
```