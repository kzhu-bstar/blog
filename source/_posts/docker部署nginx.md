---
title: Docker部署nginx
date: 2018-08-05 15:11:04
tags: [docker]
categories: 笔记
---

Nginx (engine x) 是一个高性能的HTTP和反向代理服务，也是一个IMAP/POP3/SMTP服务。利用Docker安装nginx步骤和注意事项

> 系统：CentOS 7.0

## CentOS安装Docker

### 拉取nginx镜像

```
docker pull nginx
```

### 启动容器

```
docker run -p 80:80 -p 443:443 --name nginx \
 -v /volume/nginx/html:/usr/share/nginx/html \
 -v /volume/nginx/conf.d:/etc/nginx/conf.d \
 -v /volume/nginx/logs:/var/log/nginx \
 -d nginx
```

### nginx配置

```
log_format proxyformat "$remote_addr $request_time $http_x_readtime [$time_local] \"$request_method http://$host$request_uri\" $status $body_bytes_sent \"$http_referer\" \"$upstream_addr\" \"$http_user_agent\" \"$upstream_response_time\" \"$request_time\"";


server {
  listen 80;
  server_name ali.hellowood.com.cn;
  location / {
	proxy_pass http://172.17.0.1:8080;
	proxy_set_header Host $http_host;                    
	proxy_set_header X-Real-IP $remote_addr;                    
	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
  }
}
```

### 查询宿主机 IP

```
docker inspect --format '{{ .NetworkSettings.IPAddress }}' <container-ID> 

# 或
docker inspect <container id> 

# 或
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id
```

 -v /volume/nginx/nginx.conf:/etc/nginx/nginx.conf:ro \