---
title: 阿里云ECS搭建Docker
date: 2018-10-16 10:11:04
tags: [docker]
categories: 笔记
---

云服务器Elastic Compute Service（ECS）是阿里云提供的一种基础云计算服务。在其上搭建Docker步骤和注意事项

> 系统：CentOS 7.0

## CentOS安装Docker

### 添加yum源

```
yum install epel-release –y
yum clean all
yum list
```

### 安装并运行Docker

```
yum install docker-io –y
systemctl start docker
```

### 检查安装结果

```
docker info
```

### 镜像加速器设置

安装完成之后，`docker pull` 发现无法拉取镜像，需要设置镜像加速器

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://opcl0w45.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```
[阿里云控制端容器镜像服务](
https://cr.console.aliyun.com/cn-shenzhen/mirrors)

```
docker run -p 80:80 -p 443:443 --name nginx \
 -v /volume/nginx/html:/usr/share/nginx/html \
 -v /volume/nginx/conf.d:/etc/nginx/conf.d \
 -v /volume/nginx/logs:/var/log/nginx \
 -d nginx
```

 -v /volume/nginx/nginx.conf:/etc/nginx/nginx.conf:ro \