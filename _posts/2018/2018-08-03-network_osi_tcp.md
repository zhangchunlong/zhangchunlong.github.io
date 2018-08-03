---
layout: post
title: TCP/IP模型和OSI参考模型
categories:
- 技术
tags:
- network
---
### 1. OSI参考模型
OSI RM（Open System Interconnection Reference Model，开放系统互连参考模型）将数据通信划分成七个层次，如下图:  
![osi_rm](/media/pic/osi_rm.png 'osi_rm')  
七个层次的功能如下：  
* 物理层：用以建立、维护和拆除物理连接，在设备之间传输比特流，规定通信设备机械的、电气的、功能的和过程的特性，如规定了电平、速度、电缆针脚等；  
* 数据链路层：在物理层提供比特流的基础上，将比特组合成字节，再将字节组合成帧，并使用MAC地址来访问设备，同时进行差错控制；  
* 网络层：控制分组传送系统的路由选择、网络互连等功能，提供逻辑地址（IP地址），供三层设备(如，路由器)确定路径；  
* 传输层：为上层提供端到端的、透明的数据传输服务，可提供面向连接或非面向连接的数据传递以及拥塞控制、差错检测；  
* 会话层：提供两进程之间建立、维护和结束会话连接的功能
* 表示层：提供各种用于应用层数据的编码、转换、格式化功能，确保一个系统的应用层发送的数据能被另一个系统的应用层识别；  
* 应用层：为应用程序提供网络服务，常见的应用层协议包括FTP,HTTP，Telnet等。
### 2. TCP/IP模型
TCP/IP模型也是采用分层结构，分为四层，跟OSI RM对应关系如下：  
![TCPIP](/media/pic/TCPIP.png 'TCPIP')   
注意TCP/IP模型不是TCP和IP这两个协议的合称，而是指因特网整个TCP/IP协议族。
当前大家为了理解和学习一般按照如下模型进行学习和研究：  
![osi_tcp](/media/pic/osi_tcp.png 'osi_tcp')  
### 3. 观察数据包  
组网如下：  
![tcp_network](/media/pic/tcp_network.PNG 'tcp_network')  
http客户端配置:  
![httpclient_config](/media/pic/httpclient_config.PNG 'httpclient_config')  
![httpclient_config2](/media/pic/httpclient_config2.PNG 'httpclient_config2')    
![httpclient](/media/pic/httpclient.PNG 'httpclient')  
http服务器端：  
![httpserver_config](/media/pic/httpserver_config.PNG 'httpserver_config')  
![httpserver](/media/pic/httpserver.PNG 'httpserver')  
在交换机的GE0/0/1抓包如下:  
![package](/media/pic/package.PNG 'package')  
我们看一下上图中第八个HTTP的数据包:  
![http_package](/media/pic/http_package.PNG 'http_package')  
从中可以看到数据包是分层的，包含Hypertext Transfer Protocol、TCP、IP、Ethernet数据头，每个头部包含相应的数据，例如TCP数据头部包含源端口、目的端口；IP头部包含源IP和目的IP;以太网帧包含源MAC地址、目的MAC地址；  
此外从抓取的包中可以看出TCP建立连接的三次握手及断开时的四次握手。