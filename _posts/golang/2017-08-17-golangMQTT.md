 ---
layout: blog
istop: true
title: "golang语言实现的MQTT"
background-image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1502911386359&di=3d6ee92a0e96f80e296ace5e1be53d01&imgtype=0&src=http%3A%2F%2Fpic.w2bc.com%2Fupload%2F201703%2F23%2F201703231530048636.jpg
date:  2017-03-07
category: git
tags:
- golang
- go
- MQTT
---


mqtt.git github链接  https://github.com/jeffallen/mqtt.git

# 协议分析

MQTT协议中文版 by mcxiaoke 
最新版本：v1.0.3 2016.02.06 
文档地址
MQTT协议中文版 https://mcxiaoke.gitbooks.io/mqtt-cn/content/
PDF和ePub文件下载 https://www.gitbook.com/book/mcxiaoke/mqtt-cn/details
中文翻译项目 https://github.com/mcxiaoke/mqtt
概述 MQTT是一个客户端服务端架构的发布/订阅模式的消息传输协议。它的设计思想是轻巧，开放，简单，规范，易于实现。这些特点使得它对很多场景来说都是很好的选择，特别是对于受限的环境如机器与机器的通信（M2M）以及物联网环境（IOT）。
说明
MQTT英文原版协议提供了字格式和HTML格式，我翻译的时候用的字文档，之前一直提供的是Word中文档转换的HTML和PDF共浏览和下载，最近花时间整理了降价版本，可以更方便的分章节在线浏览了，转换为降价后部分表格的格式不太对，会逐步用图片代替。
目录发现任何翻译问题或格式问题欢迎提PR帮忙完善。
说明
前言
目录
第一章 - MQTT介绍
第二章 - MQTT控制报文格式
第三章 - MQTT控制报文
3.1 CONNECT - 连接服务端
3.2 CONNACK - 确认连接请求
3.3发布 - 发布消息
3.4 PUBACK - 发布确认
3.5 PUBREC - 发布收到（QoS 2，第一步）
3.6 PUBREL - 发布释放（QoS 2，第二步）
3.7 PUBCOMP - 发布完成（QoS 2，第三步）
3.8订阅 - 订阅主题
3.9 SUBACK - 订阅确认
3.10 UNSUBSCRIBE - 取消订阅
3.11 UNSUBACK - 取消订阅确认
3.12 PINGREQ - 心跳请求
3.13 PINGRESP - 心跳响应
3.14 DISCONNECT - 断开连接
第四章 - 操作行为
第五章 - 安全
第六章 - 使用WebSocket
第七章 - 一致性目标
附录B - 强制性规范声明



上面所列举go实现的有关说明：

mqtt MQTT客户端，服务器和负载测试器在Go中
，请参阅：http : //godoc.org/github.com/jeffallen/mqtt
有关此代码的一些讨论，请参阅：http： //blog.nella.org/mqtt-code-golf 
限制目前，以下限制适用：
仅QoS级别0; 消息仅存储在RAM中
保留工作，但在QoS级别0。服务器重新启动时，保留的消息将丢失。
最后的消息不会实现。
Keepalive和超时未实现。
服务器示例MQTT服务器位于目录mqttsrv和smqttsrv（使用TLS保护）。
基准测试工具要使用基准测试工具，请进入pingtest，loadtest或许多，然后键入“go build”。这些工具具有合理的默认值，但您也可以使用-help标志来查找可以调整的内容。
所有的基准吸吮，而这三个吸吮不同的方式。
pingtest模拟一些客户端，他们之间尽可能快地弹跳消息。它旨在通过系统负载测量消息的延迟。
负载测试模拟一对客户端，其中一个客户端尽可能快地发送到另一个客户端。实际上，这样做最终将测试系统对消息进行解码和排队的能力，因为读取器调度中的任何轻微的不平衡会导致来自写入器的消息堆积在服务器的喉部。
许多人模拟了大量发送低交易率的客户端。目标是最终在一个服务器中实现1百万（和更多？）并发，活动的MQTT会话。到目前为止，mqttsrv已经从许多并发连接中获得了40k的负载。

