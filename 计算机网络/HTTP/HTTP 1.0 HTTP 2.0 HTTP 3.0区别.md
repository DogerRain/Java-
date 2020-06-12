> 众所周知，建立起一个TCP连接需要经过“三次握手”，而HTTP协议是建立在TCP协议之上的一种应用。



TCP建立连接时三次握手有1.5个RTT（round-trip time）的延迟，为了避免每次请求的都经历握手带来的延迟，应用层会选择不同策略的http长链接方案；TCP在建立连接的初期有慢启动（slow start）的特性，所以连接的重用总是比新建连接性能要好。

## HTTP 1.0

在 HTTP/1.0 中，HTTP 1.0规定浏览器与服务器只保持短暂的连接，一个服务器在发送完一个 HTTP 响应后，会断开 TCP 链接。但是这样每次请求都会重新建立和断开 TCP 连接，代价过大。

## HTTP 1.1

HTTP 1.1支持持久连接（HTTP/1.1的默认模式使用带流水线的持久连接），在一个TCP连接上可以传送多个HTTP请求和响应。

HTTP 1.1的 request和reponse头中都有可能出现一个connection的头，此header的含义是当client和server通信时对于长链接如何进行处理。

Connection请求头的值为Keep-Alive时，客户端通知服务器返回本次请求结果后保持连接；Connection请求头的值为close时，客户端通知服务器返回本次请求结果后关闭连接。HTTP 1.1还提供了与身份认证、状态管理和Cache缓存等机制相关的请求头和响应头。HTTP 1.1还支持文件断点续传，传送文件不用从头开始。

## HTTP 2.0

HTTP2.0中所有加强性能的核心是二进制传输，在HTTP1.x中，我们是通过文本的方式传输数据。

HTTP2.0 使用了HPACK（HTTP2头部压缩算法）压缩格式对传输的header进行编码，减少了header的大小。

HTTP2.0，可以在一个连接里，客户端和服务端都可以同时发送多个请求或回应，而且不用按照顺序一对一对应。

![HTTP2.0与HTTP3.0](F:\笔记\yudianxxJavaNote\计算机网络\HTTP\picture\aaatcp.jpg)





参考：

[https://zhuanlan.zhihu.com/p/61423830](https://zhuanlan.zhihu.com/p/61423830)

[https://www.jianshu.com/p/52d86558ca57](https://www.jianshu.com/p/52d86558ca57)

[https://mp.weixin.qq.com/s/MHYMOYHqhrAbQ0xtTkV2ig](https://mp.weixin.qq.com/s/MHYMOYHqhrAbQ0xtTkV2ig)