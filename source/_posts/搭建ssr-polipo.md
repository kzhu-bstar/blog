---
title: 搭建ssr+polipo
date: 2018-06-27 13:40:35
tags:
categories: 笔记
---



https://github.com/storezhang/docker-ssr-client



docker run -d -name ssrp -p 1081:1081 -v /etc/ssrp/data:/ssr-data 



```bash
docker run -d --name ssrp -p 1081:1081 -v /etc/ssrp/data:/ssr-data storezhang/ssr-client
```

```
docker exec -i bb2 /bin/sh
```

```bash
docker pull breakwa11/shadowsocksr

docker run -itd -p 8888:8888 --env SERVER_PORT=8888 --env PASSWORD=$PASSWORD --name ssr breakwa11/shadowsocksr
```

