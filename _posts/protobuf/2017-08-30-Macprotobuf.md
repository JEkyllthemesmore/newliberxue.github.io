---
layout: blog
istop: true
title:  "Mac编译安装protobuf环境"
date:   2017-08-30 11:19:54
category: protobuf
background-image: http://ot1cc1u9t.bkt.clouddn.com/17-8-30/20585985.jpg
tags: 
- protobuf
- protobuf 2.6.1
- Mac
---

# 下载protobuf2.6.1
```
wget https://github.com/google/protobuf/releases/download/v2.6.1/protobuf-2.6.1.tar.gz

```
# 解压下载的文件
```
 cd protobuf-2.6.1 目录
```
## Configure脚本配置
```
 ./configure

```
# 安装 
 

## make检测
```
make
```
## 环境以及源码等进行检测
```
make check
```
## sudo安装
```
sudo make install （需要接着输入密码）
```
## which查找protoc
```
which protoc
```
## 显示which查找路径

```
/Users/Liberxue/www/Go/bin/protoc
```
## 安装完成查看版本
```
protoc --version
```
### 显示版本

```
libprotoc 3.4.0
```
# 安装完毕测试下

1. 新建一个目录protos

```
mkdir liberxue/protos
```

2. 拷贝一个写好的proto文件

```
cdliberxue/protos目录
```
3. protoc --proto_path=./  --java_out=./  example.proto


---------会生成你proto文件中包名相关的文件包结构----------