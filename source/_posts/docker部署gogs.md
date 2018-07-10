---
title: docker部署gogs
date: 2018-07-10 10:34:25
tags:
categories:
---



利用docker部署gogs，利用nginx反向代理gogs，暴露80端口，配置域名git.xiazizhou.cn

### 1. Mysql数据库安装



```bash
docker run -d --name master01 -v /volume/mysql/data/master01:/var/lib/mysql -v /volume/mysql/master01:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=xzz520 -p 32769:3306 mysql:5.7
```



### 2. gogs安装



```bash
docker run -d --name=mygogs -p 10080:3000 -v /volume/gogs:/data gogs/gogs
```

### 3. nginx安装

```bash
docker run -p 80:80 --name nginx -v /volume/nginx/html:/usr/share/nginx/html -v /volume/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /volume/nginx/conf.d:/etc/nginx/conf.d -v /volume/nginx/logs:/var/log/nginx -d nginx

```

[Docker安装nginx](https://www.cnblogs.com/bingo1024/p/9022890.html)