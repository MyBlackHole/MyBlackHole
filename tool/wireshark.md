# Wireshark

> Wireshark（前称Ethereal）是一个网络封包分析软件.网络管理员使用Wireshark来检测网络问题，网络安全工程师使用Wireshark来检查资讯安全相关问题，开发者使用Wireshark来为新的通讯协定除错，普通使用者使用Wireshark来学习网络协定的相关知识。@pdai

+   [Wireshark的背景](https://pdai.tech/md/develop/protocol/dev-protocol-tool-wireshark.html#wireshark%E7%9A%84%E8%83%8C%E6%99%AF)
+   [Wireshark的功能和使用](https://pdai.tech/md/develop/protocol/dev-protocol-tool-wireshark.html#wireshark%E7%9A%84%E5%8A%9F%E8%83%BD%E5%92%8C%E4%BD%BF%E7%94%A8)
    +   [界面功能](https://pdai.tech/md/develop/protocol/dev-protocol-tool-wireshark.html#%E7%95%8C%E9%9D%A2%E5%8A%9F%E8%83%BD)
    +   [过滤器表达式](https://pdai.tech/md/develop/protocol/dev-protocol-tool-wireshark.html#%E8%BF%87%E6%BB%A4%E5%99%A8%E8%A1%A8%E8%BE%BE%E5%BC%8F)
+   [使用：基本抓包](https://pdai.tech/md/develop/protocol/dev-protocol-tool-wireshark.html#%E4%BD%BF%E7%94%A8-%E5%9F%BA%E6%9C%AC%E6%8A%93%E5%8C%85)
+   [使用：Wireshark分析TCP协议的三次握手和四次挥手](https://pdai.tech/md/develop/protocol/dev-protocol-tool-wireshark.html#%E4%BD%BF%E7%94%A8-wireshark%E5%88%86%E6%9E%90tcp%E5%8D%8F%E8%AE%AE%E7%9A%84%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B%E5%92%8C%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B)
    +   [TCP报文首部回顾](https://pdai.tech/md/develop/protocol/dev-protocol-tool-wireshark.html#tcp%E6%8A%A5%E6%96%87%E9%A6%96%E9%83%A8%E5%9B%9E%E9%A1%BE)
    +   [回顾三次握手四次挥手](https://pdai.tech/md/develop/protocol/dev-protocol-tool-wireshark.html#%E5%9B%9E%E9%A1%BE%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B)
    +   [TCP 三次握手](https://pdai.tech/md/develop/protocol/dev-protocol-tool-wireshark.html#tcp-%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B)
    +   [TCP 四次挥手](https://pdai.tech/md/develop/protocol/dev-protocol-tool-wireshark.html#tcp-%E5%9B%9B%E6%AC%A1%E6%8C%A5%E6%89%8B)
+   [参考文章](https://pdai.tech/md/develop/protocol/dev-protocol-tool-wireshark.html#%E5%8F%82%E8%80%83%E6%96%87%E7%AB%A0)

## Wireshark的背景

Wireshark（前称Ethereal）是一个网络封包分析软件。网络封包分析软件的功能是撷取网络封包，并尽可能显示出最为详细的网络封包资料。Wireshark使用WinPCAP作为接口，直接与网卡进行数据报文交换。

在过去，网络封包分析软件是非常昂贵的，或是专门属于盈利用的软件。Ethereal的出现改变了这一切。在GNUGPL通用许可证的保障范围底下，使用者可以以免费的代价取得软件与其源代码，并拥有针对其源代码修改及客制化的权利。Ethereal是全世界最广泛的网络封包分析软件之一。

+   **使用Wireshark目的**

以下是一些使用Wireshark目的的例子：

网络管理员使用Wireshark来检测网络问题，网络安全工程师使用Wireshark来检查资讯安全相关问题，开发者使用Wireshark来为新的通讯协定除错，普通使用者使用Wireshark来学习网络协定的相关知识。当然，有的人也会“居心叵测”的用它来寻找一些敏感信息……

Wireshark不是入侵侦测系统（Intrusion Detection System,IDS）。对于网络上的异常流量行为，Wireshark不会产生警示或是任何提示。然而，仔细分析Wireshark撷取的封包能够帮助使用者对于网络行为有更清楚的了解。Wireshark不会对网络封包产生内容的修改，它只会反映出流通的封包资讯。 Wireshark本身也不会送出封包至网络上。

+   **参考网址**

官网地址：https://www.wireshark.org/

官网下载地址：https://www.wireshark.org/#download

## Wireshark的功能和使用

### 界面功能

WireShark 主要分为这几个界面

1.  Display Filter(显示过滤器)， 用于过滤
    
2.  Packet List Pane(封包列表)， 显示捕获到的封包， 有源地址和目标地址，端口号。 颜色不同，代表
    
3.  Packet Details Pane(封包详细信息), 显示封包中的字段
    
4.  Dissector Pane(16进制数据)
    
5.  Miscellanous(地址栏，杂项)
    

![](imgs/dev-network-wireshark-5.png)

+   **封包列表**(Packet List Pane)

封包列表的面板中显示，编号，时间戳，源地址，目标地址，协议，长度，以及封包信息。 你可以看到不同的协议用了不同的颜色显示。

你也可以修改这些显示颜色的规则， View ->Coloring Rules.

![](imgs/dev-network-wireshark-6.png)

+   **封包详细信息** (Packet Details Pane)

这个面板是我们最重要的，用来查看协议中的每一个字段。

![](imgs/dev-network-wireshark-7.png)

各行信息分别为:

Frame: 物理层的数据帧概况

Ethernet II: 数据链路层以太网帧头部信息

Internet Protocol Version 4: 互联网层IP包头部信息

Transmission Control Protocol: 传输层T的数据段头部信息，此处是TCP

Hypertext Transfer Protocol: 应用层的信息，此处是HTTP协议

### 过滤器表达式

显示过滤器表达式作用在在wireshark捕获数据包之后，从已捕获的所有数据包中显示出符合条件的数据包，隐藏不符合条件的数据包。

显示过滤表达示在工具栏下方的“显示过滤器”输入框输入即可生效

![](imgs/dev-network-wireshark-2.png)

#### 基本过滤表达式

一条基本的表达式由过滤项、过滤关系、过滤值三项组成。

比如ip.addr == 192.168.1.1，这条表达式中ip.addr是过滤项、==是过滤关系，192.168.1.1是过滤值（整条表达示的意思是找出所有ip协议中源或目标ip、等于、192.168.1.1的数据包）

+   **过滤项**

初学者感觉的“过滤表达式复杂”，最主要就是在这个过滤项上：一是不知道有哪些过滤项，二是不知道过滤项该怎么写。

这两个问题有一个共同的答案-----wireshark的过滤项是“协议“+”.“+”协议字段”的模式。以端口为例，端口出现于tcp协议中所以有端口这个过滤项且其写法就是tcp.port。

推广到其他协议，如eth、ip、udp、http、telnet、ftp、icmp、snmp等等其他协议都是这么个书写思路。当然wireshark出于缩减长度的原因有些字段没有使用协议规定的名称而是使用简写（比如Destination Port在wireshark中写为dstport）又出于简使用增加了一些协议中没有的字段（比如tcp协议只有源端口和目标端口字段，为了简便使用wireshark增加了tcp.port字段来同时代表这两个），但思路总的算是不变的。而且在实际使用时我们输入“协议”+“.”wireshark就会有支持的字段提示（特别是过滤表达式字段的首字母和wireshark在上边2窗口显示的字段名称首字母通常是一样的），看下名称就大概知道要用哪个字段了。wireshark支持的全部协议及协议字段可查看官方说明。

+   **过滤关系**

过滤关系就是大于、小于、等于等几种等式关系，我们可以直接看官方给出的表。注意其中有“English”和“C-like”两个字段，这个意思是说“English”和“C-like”这两种写法在wireshark中是等价的、都是可用的。

![](imgs/dev-network-wireshark-3.png)

+   **过滤值**

过滤值就是设定的过滤项应该满足过滤关系的标准，比如500、5000、50000等等。过滤值的写法一般已经被过滤项和过滤关系设定好了，只是填下自己的期望值就可以了。

#### 复合过滤表达示

所谓复合过滤表达示，就是指由多条基本过滤表达式组合而成的表达示。基本过滤表达式的写法还是不变的，复合过滤表达示多出来的东西就只是基本过滤表达示的“连接词”

我们依然直接参照官方给出的表，同样“English”和“C-like”这两个字段还是说明这两种写法在wireshark中是等价的、都是可用的。

![](imgs/dev-network-wireshark-4.png)

#### 常见用显示过滤需求及其对应表达式

+   **数据链路层**：

筛选mac地址为04:f9:38:ad:13:26的数据包----eth.src == 04:f9:38:ad:13:26

筛选源mac地址为04:f9:38:ad:13:26的数据包----eth.src == 04:f9:38:ad:13:26

+   **网络层**：

筛选ip地址为192.168.1.1的数据包----ip.addr == 192.168.1.1

筛选192.168.1.0网段的数据---- ip contains "192.168.1"

筛选192.168.1.1和192.168.1.2之间的数据包----ip.addr == 192.168.1.1 && ip.addr == 192.168.1.2

筛选从192.168.1.1到192.168.1.2的数据包----ip.src == 192.168.1.1 && ip.dst == 192.168.1.2

+   **传输层**：

筛选tcp协议的数据包----tcp

筛选除tcp协议以外的数据包----!tcp

筛选端口为80的数据包----tcp.port == 80

筛选12345端口和80端口之间的数据包----tcp.port == 12345 && tcp.port == 80

筛选从12345端口到80端口的数据包----tcp.srcport == 12345 && tcp.dstport == 80

+   **应用层**：

特别说明----http中http.request表示请求头中的第一行（如GET index.jsp HTTP/1.1），http.response表示响应头中的第一行（如HTTP/1.1 200 OK），其他头部都用http.header\_name形式。

筛选url中包含.php的http数据包----http.request.uri contains ".php"

筛选内容包含username的http数据包----http contains "username"

#### 抓取后过滤实例

```bash
过滤地址
ip.addr==192.168.10.10  或  ip.addr eq 192.168.10.10  #过滤地址
ip.src==192.168.10.10     #过滤源地址
ip.dst==192.168.10.10     #过滤目的地址
 
过滤协议，直接输入协议名
icmp 
http
 
过滤协议和端口
tcp.port==80
tcp.srcport==80
tcp.dstport==80
 
过滤http协议的请求方式
http.request.method=="GET"
http.request.method=="POST"
http.request.uri contains admin   #url中包含admin的
http.request.code==404    #http请求状态码的
 
连接符
&&  
||
and
or
 
通过连接符可以把上面的命令连接在一起，比如：
ip.src==192.168.10.10 and http.request.method=="POST"
```

## 使用：基本抓包

双击选择了网卡之后，就开始抓包了

![](imgs/dev-network-wireshark-8.png)

停止抓包后，我们可以选择保存抓取到的数据包。文件——> 另存为——>选择一个存储路径，然后就保存为后缀为 .pcap 格式的文件了，可以双击直接用wireshark打开。

![](imgs/dev-network-wireshark-9.png)

## 使用：Wireshark分析TCP协议的三次握手和四次挥手

### TCP报文首部回顾

首先看下TCP报文首部，和wireshark捕获到的TCP包中的每个字段如下图所示：

![](imgs/dev-network-wireshark-1.jpeg.jpg)

+   源端口号：数据发起者的端口号，16bit
+   目的端口号：数据接收者的端口号，16bit
+   序号：32bit的序列号，由发送方使用
+   确认序号：32bit的确认号，是接收数据方期望收到发送方的下一个报文段的序号，因此确认序号应当是上次已成功收到数据字节序号加1。
+   首部长度：首部中32bit字的数目，可表示15\*32bit=60字节的首部。一般首部长度为20字节。
+   保留：6bit, 均为0
+   紧急URG：当URG=1时，表示报文段中有紧急数据，应尽快传送。
+   确认比特ACK：ACK = 1时代表这是一个确认的TCP包，取值0则不是确认包。
+   推送比特PSH：当发送端PSH=1时，接收端尽快的交付给应用进程。
+   复位比特（RST）：当RST=1时，表明TCP连接中出现严重差错，必须释放连接，再重新建立连接。
+   同步比特SYN：在建立连接是用来同步序号。SYN=1， ACK=0表示一个连接请求报文段。SYN=1，ACK=1表示同意建立连接。
+   终止比特FIN：FIN=1时，表明此报文段的发送端的数据已经发送完毕，并要求释放传输连接。
+   窗口：用来控制对方发送的数据量，通知发放已确定的发送窗口上限。
+   检验和：该字段检验的范围包括首部和数据这两部分。由发端计算和存储，并由收端进行验证。
+   紧急指针：紧急指针在URG=1时才有效，它指出本报文段中的紧急数据的字节数。
+   选项：长度可变，最长可达40字节

### 回顾三次握手四次挥手

详解请[参考前文](https://pdai.tech/md/develop/protocol/dev-protocol-tcpip.html)

![](imgs/dev-network-tcpip-4.jpg)

> 注：以下例子来源于：https://blog.csdn.net/shengjie87/article/details/106095866, 同时我还推荐你看下这篇 [wireshark抓包分析——TCP/IP协议在新窗口打开](https://sq.163yun.com/blog/article/193066493804396544?tag=M_tg_144_70)

### TCP 三次握手

因为建立了连接，所以wireshark抓到数据包了

![](imgs/dev-network-wireshark-11.png)

+   当客户端向服务器发送请求连接的报文时：
    +   Seq序列号=x（这时客户端的序列号为0）
    +   SYN=1（表示发送连接请求）

![](imgs/dev-network-wireshark-12.png)

+   服务器端收到客户端发来的请求报文后，同意建立连接，则向客户端发送确认报文：
    +   Seq序列号=y（这时服务器也会产生一个序列号y）
    +   Ack确认号=1（Seq序列号x+1，表示确认收到了客户端的请求）
    +   ACK=1（表示这是条确认请求）
    +   SYN=1（同时也发送一个建立连接的请求）

![](imgs/dev-network-wireshark-13.png)

+   客户端进程收到服务端进程的确认后，还要向服务端给出确认，然后连接成功建立：
    +   Seq序列号=x+1（这时客户端的序号为1）
    +   Ack确认号=y+1（表示确认收到了服务器的连接请求）
    +   ACK=1（表示这是确认报文）

![](imgs/dev-network-wireshark-14.png)

### TCP 四次挥手

（这时我们将服务器也就是虚拟机关机，使得服务器主动要求断开连接，这时服务器和客户端身份对调）

![](imgs/dev-network-wireshark-16.png)

+   客户端进程发出连接释放报文，并停止发送数据：
    +   Seq序列号=x（客户端序列号）
    +   Ack确认号=673（产生一个初始确认号）
    +   ACK=1（表示发出确认报文）
    +   FIN=1（表示请求断开连接）

![](imgs/dev-network-wireshark-17.png)

+   服务器收到连接释放报文，发出确认报文：
    +   Seq序列号=y（这时服务器的序列号）
    +   Ack确认号=x+1（这时确认号为x+1=674，确认收到客户端的请求）
    +   ACK=1（表示这是确认请求）

![](imgs/dev-network-wireshark-18.png)

+   服务器将最后的数据发送完毕后，就向客户端发送连接释放报文：
    +   Seq序列号=z（服务器更新序列号为673）
    +   Ack确认号=674（这时还是x+1=674）
    +   ACK=1（表示这是确认请求）
    +   FIN=1（表示请求断开连接）

![](imgs/dev-network-wireshark-19.png)

+   客户端收到服务器的连接释放报文后，必须发出确认才能成功断开连接：
    +   Seq序列号=x+1（这时客户端序列号需要+1=674）
    +   Ack确认号=z+1（服务器的序列号+1=674，确认收到服务器的请求）
    +   ACK=1（表示这是确认请求）

![](imgs/dev-network-wireshark-20.png)

## 参考文章

+   https://www.cnblogs.com/auguse/p/11749466.html
+   https://mlog.club/article/620963
+   https://sq.163yun.com/blog/article/193066493804396544?tag=M\_tg\_144\_70