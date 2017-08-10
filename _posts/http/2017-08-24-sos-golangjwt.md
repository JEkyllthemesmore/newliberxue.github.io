layout: blog
istop: true
title: "Authorization认证和授权(authorization)http基本协议"
background-image: https://o243f9mnq.qnssl.com/2017/06/116099051.jpg
date:  2017-08-24 00:13:14
category: http
tags:
- sso
- http基本协议
- http
- HTTPS
- beego
- api
- jwt
- authentication
- authorization
---
# authentication和authorization区别

以前一直分不清authentication和authorization，其实很简单，举个例子来说： 

你要登机，你需要出示你的身份证和机票，身份证是为了证明你liberxue确实是你liberxue，这就是authentication；而机票是为了证明你liberxue确实买了票可以上飞机，这就是authorization。 


在computer science领域再举个例子： 

你要登陆论坛，输入用户名liberxue，密码013，密码正确，证明你liberxue确实是liberxue，这就是authentication；再一check用户张三是个版主，所以有权限加精删别人帖，这就是authorization。

# 什么是HTTP基本认证
桌面应用程序也通过HTTP协议跟Web服务器交互， 桌面应用程序一般不会使用cookie, 而是把 "用户名+冒号+密码"用BASE64编码的字符串放在http request 中的header Authorization中发送给服务端， 这种方式叫HTTP基本认证(Basic Authentication)

当浏览器访问使用基本认证的网站的时候， 浏览器会提示你输入用户名和密码，如下图

假如用户名密码错误的话， 服务器会返回401 

# HTTP基本认证的过程
第一步:  客户端发送http request 给服务器， 

第二步:  因为request中没有包含Authorization header,  服务器会返回一个401 Unauthozied 给客户端，并且在Response 的 header "WWW-Authenticate" 中添加信息。
第三步：客户端把用户名和密码用BASE64编码后，放在Authorization header中发送给服务器， 认证成功。

举个栗子:
最近在使用beego 开发jwt单点ss0登录
```
curl -H "Authorization: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJleHAiOjE1MDMzODU2NjksImlzcyI6IjAxMyJ9.nw2p6xZcEoHae-F1OLnpnfpQRkAK8RjSR1GHro4RJFk" http://localhost:8081/v1/user/auth
```
- header： "Authorization: tokenxxx" 

- url ：http://localhost:8081/v1/user/auth 

第四步：服务器将Authorization header中的用户名密码取出，进行验证， 如果验证通过，将根据请求，发送资源给客户端

使用Fiddler Inspectors 下的Auth 选项卡，可以很方便的看到用户名和密码:


# HTTP基本认证的优点
HTTP基本认证，简单明了。Rest API 就是经常使用基本认证的

 

# 每次都要进行认证
http协议是无状态的， 同一个客户端对 服务器的每个请求都要求认证

 

# HTTP基本认证和HTTPS
把 "用户名+冒号+密码" 用BASE64编码后的string虽然用肉眼看不出来， 但用程序很容易解密，上图可以看到Fiddler就直接给解密了。 所以这样的http request 在网络上，如果用HTTP传输是很不安全的。 一般都是会用HTTPS传输, HTTPS是加密的, 所以比较安全.

 

# HTTP OAuth认证
OAuth 对于Http来说，就是放在Authorization header中的不是用户名密码， 而是一个token.
微软的Skydrive 就是使用这样的方式， 
 


 
# 其他认证
除了基本认证(Basic Authentication), 还有摘要认证 digest authentication, WSSE(WS-Security)认证
 
 
# 客户端的使用
客户端如果要跟“使用基本认证的网站”交互。 非常很简单，把用户名密码 加在Authorization header中就可以了。
 
```Csharp
string url = "https://testsite";
HttpWebRequest req = (HttpWebRequest)WebRequest.Create(url);
NetworkCredential nc = new NetworkCredential("username", "password");
req.Credentials = nc;
 ```

Linux下的curl
```
curl -u username:password https://testsite/
```