---
title: Docker学习笔记
date: 2020-12-18 09:40:33
tags:
---
## docker 安装mysql

1. `docker pull mysql:5.7` 冒号后加版本，在https://hub.docker.com/_/mysql查看

2. 启动

   <!--more-->
   
   ```bash
   docker run -p 3306:3306 --name mysql-3306 \
   -v /mydata/mysql-3306/log:/var/log/mysql \
   -v /mydata/mysql3306/data:/var/lib/mysql \
   -v /mydata/mysql3306/conf:/etc/mysql \
   -e MYSQL_ROOT_PASSWORD=Qq1234 \
   -d mysql:5.7
   ```
   
   - -p设置端口及映射端口  linux端口:docker容器端口
   - --name 设置容器名称
   - -v设置挂载
   - -e MYSQL_ROOT_PASSWORD=root 设置MySQL密码

## docker安装redis

1. `docker pull redis`
2. 启动

```bash
docker run -p 6379:6379 --name redis -v /mydata/redis/data:/data \
-v /mydata/redis/conf/redis.conf:/etc/redis/redis.conf \
-d redis redis-server /etc/redis/redis.conf
```

