---
title: Android Studio下的Gradle版本更新
date: 2018-06-01 10:20:24
tags: [android, gradle]
categories: 笔记
---

Androd Studio升级之后，相应的gradle版本也需要升级，如果任由Android Studio程序自动更新gradle，由于网络的原因，下载最新的Gradle会很慢或者直接失败

### 下载Gradle

gradle的[官方下载地址](https://services.gradle.org/distributions/)，打开后下载对应的gradle-x.x-all.zip版本，也可以复制下载链接到迅雷中

### 替换本地gradle

将下载下来的gradle-x.x-all.zip压缩文件直接复制到用户目录```USER_HOME/.gradle/wrapper/dists/gradle-x.x-all/8bnwg5hd3w55iofp58khbp6yv/```目录下，该目录下的其它文件全部删除，8bnwg5hd3w55iofp58khbp6yv这个目录是由Android Studio自动生成的，不可更改。

重新打开Android Studio，会自动解压并生成文件，到此gradle更新完成。

