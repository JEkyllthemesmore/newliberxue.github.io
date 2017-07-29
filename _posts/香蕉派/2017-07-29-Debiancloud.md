---
layout: blog
banana: true
category: 香蕉派
title:  BananaPi M2U在Debian安装私有云owncloud
date:   2017-07-29 22:49:14
background-image: http://ot1cc1u9t.bkt.clouddn.com/17-7-29/75653305.jpg
tags:
- BananaPiM2U
- Debian
- apache2
- owncloud
- 香蕉派
---
# 安装环境：
 
- 香蕉派： BananaPi M2U
- BananaPi M2U 运行系统Debian7 32位
- Mac 10.12.6 Beta (16G24b)

## 第一、更新Debian系统组件

运行升级命令来确保我们的系统组件各方面都是最新的。

```
apt-get update
apt-get upgrade --show-upgraded

```
## 安装Apache Web服务器的当前版本（在2.x系列），执行命令如下：
```
apt-get install apache2
```
大部分应用程序网站都会用到路径重写（伪静态）功能，默认APACHE是没有安装的，我们需要运行脚本支持rewrite
```
a2enmod rewrite
```
## 启动rewrite 。
编辑/etc/apache2/apache2.conf文件配置让系统运行更加优化（测试机器基于2GB内存BananapiM2U）
```
<IfModule mpm_prefork_module>
StartServers 2
MinSpareServers 6
MaxSpareServers 12
MaxClients 80
MaxRequestsPerChild 3000
</IfModule>
```
这一步我们也可以默认，等网站运行情况再进行调整以对比有何不同，目前也没搞明白这里面的参数对应的性能区别，以前我在用MAPN环境时候MYSQL占用太大，然后调整后占用率是低很多。
配置完毕之后，我们下面就需要配置域名、子域名添加站点。

# 第二、配置虚拟主机、绑定域名

在/etc/apache2/sites-available/的文件夹，用来存放所有站点的站点域名配置文件，设置站点时候用域名命名.conf这样站点多的时候也可以看到对应的站点。比如我们这里要创建2个站点，就需要配置2个conf文件，如下：
## 站点A - /etc/apache2/sites-available/liberxue.com.conf
```
<VirtualHost *:80>
ServerAdmin admin@liberxue.com
ServerName liberxue.com
ServerAlias www.liberxue.com
DocumentRoot /var/www/liberxue.com/public_html/
ErrorLog /var/www/liberxue.com/logs/error.log
CustomLog /var/www/liberxue.com/logs/access.log combined
</VirtualHost>
```
## 站点B - /etc/apache2/sites-available/liberxue2.com.conf
```
<VirtualHost *:80>
ServerAdmin webmaster@liberxue2.com
liberxue2.com
ServerAlias www.liberxue2.com
DocumentRoot /var/www/liberxue2.com/public_html/
ErrorLog /var/www/liberxue2.com/logs/error.log
CustomLog /var/www/liberxue2.com/logs/access.log combined
</VirtualHost>
```
按照上面的示范，我们有几个站点就搭建几个.CONF文件，然后对其进行路径的配置。同样的上述牵涉到的几个目录我们也需要创建。
## 多站点公共目录和日志
```
mkdir -p /var/www/liberxue.com/public_html
mkdir /var/www/liberxue.com/logs
mkdir -p /var/www/idcxen.com/public_html
mkdir /var/www/liberxue2.com/logs
```
## 执行命令启动站点
a2ensite liberxue.com.conf
a2ensite liberxue2.com.conf
# 启动Apache
```
service apache2 restart
```
* 备注：如果我们希望取消这个站点运行，那就用这个命令取消这个站点
```
a2dissite liberxue.com.conf
```
# 第三、安装和配置MySQL数据库

## 安装MYSQL
```
apt-get install mysql-server
```
在执行过程中需要我们输入MYSQL的ROOT用户密码，这个要稍微复杂些。数据库配置文件在/etc/mysql/my.cnf，如果我们需要调整尽量先备份一个。
## 配置MySQL建立数据库
```
mysql_secure_installation
```
需要我们输入上面设置的MYSQL数据库ROOT密码才可以进入，首次进入会问是否需要修改，以及其他的各种设置，我们根据需要选择n/y选项。
```
mysql -u root -p
create database laozuoorg;
grant all on laozuoorg.* to 'laozuouser' identified by 'liberxue.com';
```
利用root权限进入MYSQL数据库，输入我们之前设置的密码进入，然后建立laozuoorg数据库名，laozuouser数据表，以及liberxue.com设置数据库密码。
DEBIAN LAMP数据库添加
创建完毕之后输入 quit 退出MYSQL设置。
## 第四、安装和设置PHP环境
```
apt-get install php5 php-pear
```
安装之后我们需要配置php.ini文件（/etc/php5/apache2/php.ini）可以默认不改
```
max_execution_time = 30
memory_limit = 128M
error_reporting = E_COMPILE_ERROR|E_RECOVERABLE_ERROR|E_ERROR|E_CORE_ERROR
display_errors = Off
log_errors = On
error_log = /var/log/php.log
register_globals = Off
max_input_time = 30
```
## 创建日志目录且设置权限
```
mkdir /var/log/php
chown www-data /var/log/php
```
如果我们需要MySQL的PHP支持
## 安装PHP5 MySQL包下面的命令：
```
apt-get install php5-mysql
```
## 启动apache
```
service apache2 restart
```
这样，通过上面的四步，就可以搭建站点、数据库，后面我们就只需要到/var/www/liberxue2.com/public_html上传网页程序，然后根据提示安装就可以了。
唯一需要注意根目录权限需要可写.htaccess或者手工创建伪静态文件，这样后台的固定连接设置之后才生效。
```
chown -R www-data:www-data /var/www/html
```
# Debiana 安装 owncloud

## 下载owncloud
这个百度一下就不写 了。。。

## scp本地上传m2u香蕉派

```
scp -p 22 /home/owncloud.zip root@192.168.13.13:/var/www/html/

```
 
## BananaPi M2U在 Debian 解压

``
unzip -rf owncloud.zip  /var/www/html/

``
## owncloud需要注意地方

- php gd
- php curl
- owncloud／config文件和apps需要写入权限

> phpgd 安装 apt-get install php5 gd
 
>  phpcurl 安装 apt-get install php5 curl