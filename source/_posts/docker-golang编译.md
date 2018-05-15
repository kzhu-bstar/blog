---
title: docker golang编译
date: 2018-05-15 10:20:04
tags: [docker, golang]
---
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=186018&auto=1&height=66"></iframe>
//其中的width=298 height=52 均改为0就看不到了，依然可以播放音乐

### 基于scratch镜像

> Docker生态系统有一个特殊的镜像：scratch.这是一个空镜像。它不需要被创建或者下载，因为定义的就是空的。

#### 1. 在golang项目目录，创建Dockerfile文件

```bash
FROM scratch
COPY ./hello /hello
ENTRYPOINT ["/hello"]
```

#### 2. 在golang项目目录下，交叉编译构建项目

```
GOOS=linux GOARCH=amd64 go build -o hello *.go
```

#### 3. 在运行docker服务器上，创建镜像

```
chmod +x hello
docker build -t hello .
```

#### 4. 在运行docker服务器上，检查镜像是否生成成功并运行

```
docker images
docker run hello
```

----

### 参考

[Docker与Golang的巧妙结合](http://dockone.io/article/1712)