---
title: MongoDB 安装和配置
description: MongoDB 是一个开源的文档数据库，目前大家推荐的是使用 Docker 安装，然后需要设置管理员账号。
date:  2022/04/16 03:29:39
updated: 2022/06/19 02:07:16
categories:
  - Tool
  - MongoDB
tags:
  - MongoDB
---

## 使用Docker安装

执行下列命令创建`MongoDB`容器：

```shell
docker run -d --restart=always -p 27017:27017 --name mymongo -v /data/mongo:/data/db mongo --auth
```

>   `-d` 表示后台启动
>
>   `--restart` 表示`docker`启动时自启动相关容器
>
>   `-p` 映射端口，格式为**主机端口:容器端口**
>
>   `--name` 指定容器名称
>
>   `-v` 挂载目录，格式为**主机目录:容器目录**
>
>   `--auth` 开启`MongoDB`的**授权模式**
>
>   网上其他文章说先不开启此选项，等设置管理员后，再重启并设置此选项对docker来说过于麻烦

## 添加管理员

首先进入容器

```shell
docker exec -it mymongo /bin/bash
```

根据阿里云帮助文档，~~在未开启认证的环境下~~，先输入`mongo`，进入命令模式，然后切换到`admin`数据库，并创建管理员账号，管理员账号将在`system.users`中。

```shell
# mongo
MongoDB shell version v5.0.5
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
...
> use admin
switched to db admin
> db.createUser({user:"root",pwd:"root",roles:["root"]})
Successfully added user: { "user" : "root", "roles" : [ "root" ] }
> exit
bye
```
