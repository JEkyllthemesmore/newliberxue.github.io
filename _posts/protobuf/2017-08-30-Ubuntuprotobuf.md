---
layout: blog
istop: true
title:  "ubuntu下编译安装protobuf环境"
date:   2017-08-30 11:19:54
category: protobuf
background-image: http://ot1cc1u9t.bkt.clouddn.com/17-8-30/49137437.jpg
tags: 
- protobuf
- protobuf 2.6.1
- ubuntu
- libprotocbuf
---

# 下载protobuf
```
wget https://github.com/google/protobuf/releases/download/v2.6.1/protobuf-2.6.1.tar.gz

```
# 编译
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

## 我在执行./configure时出现如下问题
```
Ubuntu: configure error: C++ preprocessor "/lib/cpp" fails sanity check  
```
## 解决办法
```
sudo apt-get install build-essential
```
## 检查是否安装成功
```
protoc --version

```
如果安装成功,会出现版本号 如
```
libprotoc 2.6.1
```
如果有问题，会输出错误内容

## 抱歉一个错误
```
protoc: error while loading shared libraries: libprotocbuf.so.9: cannot open shared

```
## 错误原因

protobuf的默认安装路径是``/usr/local/lib``,而``/usr/local/lib``不在ubuntu体系默认的``LD_LIBRARY_PATH``里,所以就找不到``lib``

## 解决办法

 ```
 cd /etc/ld.so.conf.d/
 ```
  
 ```
  vim 
  
  ```
 
```
/usr/local/lib
```
在 ``/etc/ld.so.conf.d/``目录下创建文件``bprotobuf.conf``文件,文件内容如下
```
/usr/local/lib
```
# 输入命令

sudo ldconfig