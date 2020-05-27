

>ping (Packet Internet Groper)，因特网包探索器，用于测试网络连接量的程序。Ping发送一个`ICMP`，回声请求消息给目的地并报告是否收到所希望的ICMP echo （ICMP回声应答）。它是用来检查网络是否通畅或者网络连接速度的命令

## ping 原理

`ICMP`协议是“Internet Control Message Ptotocol”（因特网控制消息协议）的缩写，它是TCP/IP协议族的一个子协议，用于在IP主机、路由器之间传递控制消息。

这个“Ping”的过程实际上就是ICMP协议工作的过程。还有其他的网络命令比如跟踪路由的Tracert命令也是基于ICMP协议的。IP协议是一种无连接的，不可靠的数据包协议。在Unix/Linux,序号从0开始计数，依次递增。而Windows ping程序的ICMP序列号是没有规律。

ping向指定的网络地址发送一定长度的数据包，按照约定，若指定网络地址存在的话，会返回同样大小的数据包，当然，若在特定时间内没有返回，就是“超时”，会被认为指定的网络地址不存在。

**ping和ICMP的关系：ping命令发送数据使用的是ICMP协议。**

##  用法

```java
用法: ping [-t] [-a] [-n count] [-l size] [-f] [-i TTL] [-v TOS]
            [-r count] [-s count] [[-j host-list] | [-k host-list]]
            [-w timeout] [-R] [-S srcaddr] [-c compartment] [-p]
            [-4] [-6] target_name
```

## ping命令详细参数

**注意：Windows和Linux参数有一点区别**

以下是windows的参数：

```java
    -t             Ping 指定的主机，直到停止。
                   若要查看统计信息并继续操作，请键入 Ctrl+Break；
                   若要停止，请键入 Ctrl+C。
    -a             将地址解析为主机名。
    -n count       要发送的回显请求数。
    -l size        发送缓冲区大小。
    -f             在数据包中设置“不分段”标记(仅适用于 IPv4)。
    -i TTL         生存时间。
    -v TOS         服务类型(仅适用于 IPv4。该设置已被弃用，
                   对 IP 标头中的服务类型字段没有任何
                   影响)。
    -r count       记录计数跃点的路由(仅适用于 IPv4)。
    -s count       计数跃点的时间戳(仅适用于 IPv4)。
    -j host-list   与主机列表一起使用的松散源路由(仅适用于 IPv4)。
    -k host-list    与主机列表一起使用的严格源路由(仅适用于 IPv4)。
    -w timeout     等待每次回复的超时时间(毫秒)。
    -R             同样使用路由标头测试反向路由(仅适用于 IPv6)。
                   根据 RFC 5095，已弃用此路由标头。
                   如果使用此标头，某些系统可能丢弃
                   回显请求。
    -S srcaddr     要使用的源地址。
    -c compartment 路由隔离舱标识符。
    -p             Ping Hyper-V 网络虚拟化提供程序地址。
    -4             强制使用 IPv4。
    -6             强制使用 IPv6。
```

## 常用命令

### ping -t

windows下可以指定 `-t` 直到管理员中断（Ctrl+C），而Linux不用加`-t`，默认一直ping

![image-20200527212955700](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200527212955700.png)

最后可以通过丢包率看到网络的情况，这样就说明网络很好：

![image-20200527213221708](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200527213221708.png)

### **ping -a**

`ping -a` 可以解析计算机名与NetBios名。 

![image-20200527213939019](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200527213939019.png)

但是这个和你本地设置的host有关，并不是都能解析的。

### ping -n

默认情况下，windows发送4个默认包，Linux默认一直ping，可以指定发送10个：

![image-20200527213559285](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200527213559285.png)

### **ping -l **

Linux是 `ping -s` Linux默认是64Bytes，windows默认是32Bytes，两者最大能发送65500Bytes。

当一次发送的数据包大于或等于65500byt时，将可能导致接收方计算机宕机。所以微软限制了这一数值；这个参数配合其它参数以后危害非常强大，比如攻击者可以结合-t参数实施DOS攻击。（所以它具有危险性，不要轻易向别人计算机使用）。

### **批量Ping**

比如说，公司局域网有 `192.168.1.1~192.168.1.255`  共计255个IP，我想测试一下每个IP的情况，可以使用批量ping命令：

```cmd
for /L %D in (1,1,255) do ping 192.168.1.%D -l 32
```

当输入批量命令后，那么它就自动把网段内所有的ip地址都ping完为止。

`(1,1,255)`  第一个参数表示起始IP，即 `192.168.1.1` ；第二个表示间隔，即每次递增1；第三个表示末IP，这里即`192.168.1.255`

![image-20200527215051171](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200527215051171.png)

#### 例子：

例如我ping 百度，ping 4次，发送32Byte的包，超时时间10秒，每1秒发送一次，Linux可以这样写：

```cmd
ping baidu.com -c 4 -s 32 -w 10 -i 1
```

![image-20200527210745451](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200527210745451.png)

在windows可以这样写：(DOS下没有延时，设置不了？还是linux的单位是秒，windows是毫秒？留个疑问)

```
ping baidu.com -n 4 -l 32 -w 10
```

![image-20200527210831773](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200527210831773.png)



## 结果解释

![image-20200527211239934](https://images-1253198264.cos.ap-guangzhou.myqcloud.com/image-20200527211239934.png)

`字节`：即Bytes，发送数据包大小。

`时间` 即time，响应时间，时间越小， 访问这个地址速度越快。

`TTL`Time To Live, 表示DNS记录在DNS服务器上存在的时间，它是IP协议包的一个值，告诉路由器该数据包何时需要被丢弃。

一般情况下，Linux系统的TTL值为64或255，Windows NT/2000/XP系统的TTL值为128，Windows 98系统的TTL值为32，UNIX主机的TTL值为255；但是这个值可以修改。



参考：

https://blog.csdn.net/hebbely/article/details/54965989

https://www.cnblogs.com/operationhome/p/9848138.html