## 网络协议 - DNS 相关详解

> DNS的核心工作就是将域名翻译成计算机IP地址, 它是基于UDP协议实现的，本文将具体阐述DNS相关的概念，解析，调度原理（负载均衡和区域调度）等DNS相关的所有知识点。 @pdai

+   [DNS简介](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns%E7%AE%80%E4%BB%8B)
    +   [域名层级结构](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E5%9F%9F%E5%90%8D%E5%B1%82%E7%BA%A7%E7%BB%93%E6%9E%84)
    +   [域名服务器](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E5%9F%9F%E5%90%8D%E6%9C%8D%E5%8A%A1%E5%99%A8)
+   [DNS 解析流程](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns-%E8%A7%A3%E6%9E%90%E6%B5%81%E7%A8%8B)
    +   [为什么DNS通常基于UDP](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E4%B8%BA%E4%BB%80%E4%B9%88dns%E9%80%9A%E5%B8%B8%E5%9F%BA%E4%BA%8Eudp)
+   [DNS 查询](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns-%E6%9F%A5%E8%AF%A2)
    +   [dig 查询](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dig-%E6%9F%A5%E8%AF%A2)
    +   [host查询](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#host%E6%9F%A5%E8%AF%A2)
    +   [nslookup查询](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#nslookup%E6%9F%A5%E8%AF%A2)
    +   [whois查询](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#whois%E6%9F%A5%E8%AF%A2)
    +   [在线工具查询](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E5%9C%A8%E7%BA%BF%E5%B7%A5%E5%85%B7%E6%9F%A5%E8%AF%A2)
+   [DNS 调度原理](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns-%E8%B0%83%E5%BA%A6%E5%8E%9F%E7%90%86)
    +   [地理位置调度不准确](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E5%9C%B0%E7%90%86%E4%BD%8D%E7%BD%AE%E8%B0%83%E5%BA%A6%E4%B8%8D%E5%87%86%E7%A1%AE)
    +   [规则变更生效时间不确定](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E8%A7%84%E5%88%99%E5%8F%98%E6%9B%B4%E7%94%9F%E6%95%88%E6%97%B6%E9%97%B4%E4%B8%8D%E7%A1%AE%E5%AE%9A)
    +   [高可用](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E9%AB%98%E5%8F%AF%E7%94%A8)
+   [DNS 安全相关](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns-%E5%AE%89%E5%85%A8%E7%9B%B8%E5%85%B3)
    +   [什么是DNS劫持](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E4%BB%80%E4%B9%88%E6%98%AFdns%E5%8A%AB%E6%8C%81)
    +   [什么是DNS污染](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E4%BB%80%E4%B9%88%E6%98%AFdns%E6%B1%A1%E6%9F%93)
    +   [为什么要DNS流量监控](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81dns%E6%B5%81%E9%87%8F%E7%9B%91%E6%8E%A7)
    +   [DNS 流量监控](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns-%E6%B5%81%E9%87%8F%E7%9B%91%E6%8E%A7)
    +   [DNS 服务器监控](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns-%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%9B%91%E6%8E%A7)
+   [参考文章](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E5%8F%82%E8%80%83%E6%96%87%E7%AB%A0)

## DNS简介

域名系统并不像电话号码通讯录那么简单，通讯录主要是单个个体在使用，同一个名字出现在不同个体的通讯录里并不会出现问题，但域名是群体中所有人都在用的，必须要保持唯一性。为了达到唯一性的目的，因特网在命名的时候采用了层次结构的命名方法。每一个域名（本文只讨论英文域名）都是一个标号序列（labels），用字母（A-Z，a-z，大小写等价）、数字（0-9）和连接符（-）组成，标号序列总长度不能超过255个字符，它由点号分割成一个个的标号（label），每个标号应该在63个字符之内，每个标号都可以看成一个层次的域名。级别最低的域名写在左边，级别最高的域名写在右边。域名服务主要是基于UDP实现的，服务器的端口号为53。

### 域名层级结构

![](imgs/dev-network-dns-9.png)

> 注意：最开始的域名最后都是带了点号的，比如 `www.kernel.org.` ，最后面的点号表示根域名服务器，后来发现所有的网址都要加上最后的点，就简化了写法，干脆所有的都不加即`www.kernel.org`，但是你在网址后面加上点号也是可以正常解析的。

### 域名服务器

有域名结构还不行，还需要有一个东西去解析域名，手机通讯录是由通讯录软件解析的，域名需要由遍及全世界的域名服务器去解析，域名服务器实际上就是装有域名系统的主机。由高向低进行层次划分，可分为以下几大类：

+   **根域名服务器**：最高层次的域名服务器，也是最重要的域名服务器，本地域名服务器如果解析不了域名就会向根域名服务器求助。全球共有13个不同IP地址的根域名服务器，它们的名称用一个英文字母命名，从a一直到m。这些服务器由各种组织控制，并由 ICANN（互联网名称和数字地址分配公司）授权，由于每分钟都要解析的名称数量多得令人难以置信，所以实际上每个根服务器都有镜像服务器，每个根服务器与它的镜像服务器共享同一个 IP 地址，中国大陆地区内只有6组根服务器镜像（F，I（3台），J，L）。当你对某个根服务器发出请求时，请求会被路由到该根服务器离你最近的镜像服务器。所有的根域名服务器都知道所有的顶级域名服务器的域名和地址，如果向根服务器发出对 “pdai.tech” 的请求，则根服务器是不能在它的记录文件中找到与 “pdai.tech” 匹配的记录。但是它会找到 "tech" 的顶级域名记录，并把负责 "tech" 地址的顶级域名服务器的地址发回给请求者。
+   **顶级域名服务器**：负责管理在该顶级域名服务器下注册的二级域名。当根域名服务器告诉查询者顶级域名服务器地址时，查询者紧接着就会到顶级域名服务器进行查询。比如还是查询"pdai.tech"，根域名服务器已经告诉了查询者"tech"顶级域名服务器的地址，"tech"顶级域名服务器会找到 “pdai.tech”的域名服务器的记录，域名服务器检查其区域文件，并发现它有与 “pdai.tech” 相关联的区域文件。在此文件的内部，有该主机的记录。此记录说明此主机所在的 IP 地址，并向请求者返回最终答案。
+   **权限域名服务器**：负责一个区的域名解析工作
+   **本地域名服务器**：当一个主机发出DNS查询请求的时候，这个查询请求首先就是发给本地域名服务器的。

![](imgs/dev-network-dns-11.png)

## DNS 解析流程

> 网上找了个比较好的例子

.com.fi国际金融域名DNS解析的步骤一共分为9步，如果每次解析都要走完9个步骤，大家浏览网站的速度也不会那么快，现在之所以能保持这么快的访问速度，其实一般的解析都是跑完第4步就可以了。除非一个地区完全是第一次访问（在都没有缓存的情况下）才会走完9个步骤，这个情况很少。

+   1、本地客户机提出域名解析请求，查找本地HOST文件后将该请求发送给本地的域名服务器。
+   2、将请求发送给本地的域名服务器。
+   3、当本地的域名服务器收到请求后，就先查询本地的缓存。
+   4、如果有该纪录项，则本地的域名服务器就直接把查询的结果返回浏览器。
+   5、如果本地DNS缓存中没有该纪录，则本地域名服务器就直接把请求发给根域名服务器。
+   6、然后根域名服务器再返回给本地域名服务器一个所查询域（根的子域）的主域名服务器的地址。
+   7、本地服务器再向上一步返回的域名服务器发送请求，然后接受请求的服务器查询自己的缓存，如果没有该纪录，则返回相关的下级的域名服务器的地址。
+   8、重复第7步，直到找到正确的纪录。
+   9、本地域名服务器把返回的结果保存到缓存，以备下一次使用，同时还将结果返回给客户机。

![](imgs/dev-network-dns-12.png)

注意事项：

**递归查询**：在该模式下DNS服务器接收到客户机请求，必须使用一个准确的查询结果回复客户机。如果DNS服务器本地没有存储查询DNS信息，那么该服务器会询问其他服务器，并将返回的查询结果提交给客户机。

**迭代查询**：DNS所在服务器若没有可以响应的结果，会向客户机提供其他能够解析查询请求的DNS服务器地址，当客户机发送查询请求时，DNS服务器并不直接回复查询结果，而是告诉客户机另一台DNS服务器地址，客户机再向这台DNS服务器提交请求，依次循环直到返回查询的结果为止。

### 为什么DNS通常基于UDP

使用基于UDP的DNS协议只要一个请求、一个应答就好了

而使用基于TCP的DNS协议要三次握手、发送数据以及应答、四次挥手

明显基于TCP协议的DNS更浪费网络资源！

当然以上只是从数据包的数量以及占有网络资源的层面来进行的分析，那数据一致性层面呢？

DNS数据包不是那种大数据包，所以使用UDP不需要考虑分包，如果丢包那么就是全部丢包，如果收到了数据，那就是收到了全部数据！所以只需要考虑丢包的情况，那就算是丢包了，重新请求一次就好了。而且DNS的报文允许填入序号字段，对于请求报文和其对应的应答报文，这个字段是相同的，通过它可以区分DNS应答是对应的哪个请求

> DNS通常是基于UDP的，但当数据长度大于512字节的时候，为了保证传输质量，就会使用基于TCP的实现方式

## DNS 查询

### dig 查询

> 用`dig`可以查看整个过程，看下下面的返回就能理解的

+   dig www.sina.com

```bash
pdaiMbp:/ pdai$ dig www.sina.com

; <<>> DiG 9.10.6 <<>> www.sina.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15304
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;www.sina.com.			IN	A

;; ANSWER SECTION:
www.sina.com.		32	IN	CNAME	us.sina.com.cn.
us.sina.com.cn.		32	IN	CNAME	spool.grid.sinaedge.com.
spool.grid.sinaedge.com. 32	IN	A	115.238.190.240

;; Query time: 20 msec
;; SERVER: 192.168.3.1#53(192.168.3.1)
;; WHEN: Fri Jan 31 14:53:39 CST 2020
;; MSG SIZE  rcvd: 108
```

+   dig +trace www.sina.com // 分级查询

```bash
pdaiMbp:/ pdai$ dig +trace www.sina.com

; <<>> DiG 9.10.6 <<>> +trace www.sina.com
;; global options: +cmd
.			52722	IN	NS	f.root-servers.net.
.			52722	IN	NS	k.root-servers.net.
.			52722	IN	NS	m.root-servers.net.
.			52722	IN	NS	l.root-servers.net.
.			52722	IN	NS	d.root-servers.net.
.			52722	IN	NS	i.root-servers.net.
.			52722	IN	NS	c.root-servers.net.
.			52722	IN	NS	e.root-servers.net.
.			52722	IN	NS	a.root-servers.net.
.			52722	IN	NS	g.root-servers.net.
.			52722	IN	NS	b.root-servers.net.
.			52722	IN	NS	h.root-servers.net.
.			52722	IN	NS	j.root-servers.net.
;; Received 228 bytes from 192.168.3.1#53(192.168.3.1) in 3 ms

com.			172800	IN	NS	a.gtld-servers.net.
com.			172800	IN	NS	b.gtld-servers.net.
com.			172800	IN	NS	c.gtld-servers.net.
com.			172800	IN	NS	d.gtld-servers.net.
com.			172800	IN	NS	e.gtld-servers.net.
com.			172800	IN	NS	f.gtld-servers.net.
com.			172800	IN	NS	g.gtld-servers.net.
com.			172800	IN	NS	h.gtld-servers.net.
com.			172800	IN	NS	i.gtld-servers.net.
com.			172800	IN	NS	j.gtld-servers.net.
com.			172800	IN	NS	k.gtld-servers.net.
com.			172800	IN	NS	l.gtld-servers.net.
com.			172800	IN	NS	m.gtld-servers.net.
com.			86400	IN	DS	30909 8 2 E2D3C916F6DEEAC73294E8268FB5885044A833FC5459588F4A9184CF C41A5766
com.			86400	IN	RRSIG	DS 8 1 86400 20200213050000 20200131040000 33853 . zMeZpKg/LGzpVjlBUJRfkmk8tSvZW+L0UFHnzSn8agztJ8sMGU+knBLW 5LLoPoh6iG7exLV5wVIJZVh+0ISk3AG85VJXZ3HSTWcHZfjMOYI7JXpe pv/5JqT9Eai0ScEJAowDa1qctGOE/LHdNwr30VF8U0LoZL0iXVN3KQ4k iKnl0S0hB41KH+BHFcNpWqxKHRK2piMZRNe8+8Nu9I4GilfW/D90e69p SgG7puU3J3srarhccj0OS5WcLi6nsMf/2k0C6rQMe+WD7aOVZXoLts93 /thoNSWIprseKrYze2STnuG+T/VxzZRJ3fjoZARGHtDf3gTibHC2syXL xaXz5w==
;; Received 1172 bytes from 198.97.190.53#53(h.root-servers.net) in 54 ms

sina.com.		172800	IN	NS	ns1.sina.com.cn.
sina.com.		172800	IN	NS	ns2.sina.com.cn.
sina.com.		172800	IN	NS	ns3.sina.com.cn.
sina.com.		172800	IN	NS	ns1.sina.com.
sina.com.		172800	IN	NS	ns2.sina.com.
sina.com.		172800	IN	NS	ns4.sina.com.
sina.com.		172800	IN	NS	ns3.sina.com.
CK0POJMG874LJREF7EFN8430QVIT8BSM.com. 86400 IN NSEC3 1 1 0 - CK0Q1GIN43N1ARRC9OSM6QPQR81H5M9A  NS SOA RRSIG DNSKEY NSEC3PARAM
CK0POJMG874LJREF7EFN8430QVIT8BSM.com. 86400 IN RRSIG NSEC3 8 2 86400 20200207054811 20200131043811 56311 com. N15f7ia8A0pd2A5iWM/8t+T6gs8mQJaOWe/aj3bs4cWxpG7WmCaquZp7 6gfbfotFmss+DuBm9MAd6bwe2fm9m60FQgROWGOZwGRrvZqawy/5eDeV sLIJqhnwM0lT1PuDgNe2SFYsV506melwC4cEtR8M6gkX3nwYMCf6Frus anO+4Lufi229N5Y00N4x9vrlO3zsGBR1yg2xBki9Ni379A==
TGAG8VMC6NS5VVK68CIGRJ6Q414N2KB2.com. 86400 IN NSEC3 1 1 0 - TGAGRAEN3DVBS761O1PSQ1TU0407EVHO  NS DS RRSIG
TGAG8VMC6NS5VVK68CIGRJ6Q414N2KB2.com. 86400 IN RRSIG NSEC3 8 2 86400 20200206061700 20200130050700 56311 com. TMrk1/56Wa+isS5Y6OFQz9OZWMAAbt2TOEzaBp1Uuj9z1Eg4uio92+ff sWRB6vACYSBAJJLy4NPJfqxYpue39hvaDgFRYAZreDuCC0x+p9yi7yQ8 JaN2mS7W8Mbv0iEV0AUyzZGyhYq83DA58slNSGRhZfcvLYBAETURyH0X bJp+Hgq0bqXOqGyi/lAAv8/2mr+tiramb/pNst1MBBPaig==
;; Received 791 bytes from 192.48.79.30#53(j.gtld-servers.net) in 215 ms

www.sina.com.		60	IN	CNAME	us.sina.com.cn.
us.sina.com.cn.		60	IN	CNAME	spool.grid.sinaedge.com.
;; Received 103 bytes from 180.149.138.199#53(ns2.sina.com.cn) in 30 ms
```

域名与IP之间的对应关系，称为"记录"（record）。根据使用场景，"记录"可以分成不同的类型（type），前面已经看到了有A记录和NS记录。

常见的DNS记录类型如下。

+   **A**：地址记录（Address），返回域名指向的IP地址。
+   **NS**：域名服务器记录（Name Server），返回保存下一级域名信息的服务器地址。该记录只能设置为域名，不能设置为IP地址。
+   **MX**：邮件记录（Mail eXchange），返回接收电子邮件的服务器地址。
+   **CNAME**：规范名称记录（Canonical Name），返回另一个域名，即当前查询的域名是另一个域名的跳转，详见下文。
+   **PTR**：逆向查询记录（Pointer Record），只用于从IP地址查询域名，详见下文。

一般来说，为了服务的安全可靠，至少应该有两条NS记录，而A记录和MX记录也可以有多条，这样就提供了服务的冗余性，防止出现单点失败。

CNAME记录主要用于域名的内部跳转，为服务器配置提供灵活性，用户感知不到。

### host查询

```bash
pdaiMbp:/ pdai$ host www.sina.com
www.sina.com is an alias for us.sina.com.cn.
us.sina.com.cn is an alias for spool.grid.sinaedge.com.
spool.grid.sinaedge.com has address 115.238.190.240
spool.grid.sinaedge.com has IPv6 address 240e:f7:a000:221::75:71
```

### nslookup查询

```bash
pdaiMbp:/ pdai$ nslookup
> www.sina.com
Server:		192.168.3.1
Address:	192.168.3.1#53

Non-authoritative answer:
www.sina.com	canonical name = us.sina.com.cn.
us.sina.com.cn	canonical name = spool.grid.sinaedge.com.
Name:	spool.grid.sinaedge.com
Address: 115.238.190.240
```

### whois查询

```bash
pdaiMbp:/ pdai$ whois www.sina.com
% IANA WHOIS server
% for more information on IANA, visit http://www.iana.org
% This query returned 1 object

refer:        whois.verisign-grs.com

domain:       COM

organisation: VeriSign Global Registry Services
address:      12061 Bluemont Way
address:      Reston Virginia 20190
address:      United States

contact:      administrative
name:         Registry Customer Service
organisation: VeriSign Global Registry Services
address:      12061 Bluemont Way
address:      Reston Virginia 20190
address:      United States
phone:        +1 703 925-6999
fax-no:       +1 703 948 3978
e-mail:       info@verisign-grs.com

contact:      technical
name:         Registry Customer Service
organisation: VeriSign Global Registry Services
address:      12061 Bluemont Way
address:      Reston Virginia 20190
address:      United States
phone:        +1 703 925-6999
fax-no:       +1 703 948 3978
e-mail:       info@verisign-grs.com

nserver:      A.GTLD-SERVERS.NET 192.5.6.30 2001:503:a83e:0:0:0:2:30
nserver:      B.GTLD-SERVERS.NET 192.33.14.30 2001:503:231d:0:0:0:2:30
nserver:      C.GTLD-SERVERS.NET 192.26.92.30 2001:503:83eb:0:0:0:0:30
nserver:      D.GTLD-SERVERS.NET 192.31.80.30 2001:500:856e:0:0:0:0:30
nserver:      E.GTLD-SERVERS.NET 192.12.94.30 2001:502:1ca1:0:0:0:0:30
nserver:      F.GTLD-SERVERS.NET 192.35.51.30 2001:503:d414:0:0:0:0:30
nserver:      G.GTLD-SERVERS.NET 192.42.93.30 2001:503:eea3:0:0:0:0:30
nserver:      H.GTLD-SERVERS.NET 192.54.112.30 2001:502:8cc:0:0:0:0:30
nserver:      I.GTLD-SERVERS.NET 192.43.172.30 2001:503:39c1:0:0:0:0:30
nserver:      J.GTLD-SERVERS.NET 192.48.79.30 2001:502:7094:0:0:0:0:30
nserver:      K.GTLD-SERVERS.NET 192.52.178.30 2001:503:d2d:0:0:0:0:30
nserver:      L.GTLD-SERVERS.NET 192.41.162.30 2001:500:d937:0:0:0:0:30
nserver:      M.GTLD-SERVERS.NET 192.55.83.30 2001:501:b1f9:0:0:0:0:30
ds-rdata:     30909 8 2 E2D3C916F6DEEAC73294E8268FB5885044A833FC5459588F4A9184CFC41A5766

whois:        whois.verisign-grs.com

status:       ACTIVE
remarks:      Registration information: http://www.verisigninc.com

created:      1985-01-01
changed:      2017-10-05
source:       IANA

No match for domain "WWW.SINA.COM".
>>> Last update of whois database: 2020-01-31T07:03:29Z <<<
```

### 在线工具查询

[https://www.nslookuptool.com/chs/在新窗口打开](https://www.nslookuptool.com/chs/)

![](imgs/dev-network-dns-10.png)

## DNS 调度原理

> 本节转自：[【网易MC】DNS 调度原理解析在新窗口打开](https://segmentfault.com/a/1190000010787338)

现在，大部分应用和业务都采用域名作为服务的入口，因此用 DNS 来负载均衡和区域调度是非常普遍的做法，网易云也有着一套基于 DNS 的调度系统。某些用户在进行直播推流时用的并不是网易云的直播 SDK，而是一些第三方的推流软件，如obs，这样就不能使用我们的 GSLB 全局调度服务器来调度。对于这些用户，我们使用 DNS 调度的方式，对不同地域的请求返回不同解析结果，将请求调度到离用户最近的服务器节点，从而减少延迟访问。

![](imgs/dev-network-dns-2.jpeg.jpg)

咋一看，DNS 调度这么简单方便，那为什么不让所有的用户都走 DNS 调度呢？想知道原因？来，我们继续讲。

### 地理位置调度不准确

在 DNS 解析过程中，与权威服务器通信的只有 DNS 缓存服务器，所以权威服务器只能根据 DNS 缓存服务器的IP来进行调度。因此 DNS 调度有一个前提：假定用户使用的缓存DNS与用户本身在同个网络内，即至少在同一个 AS(自治域)内，在该前提下，DNS 的解析才是准确的。通常情况下，用户使用 ISP 提供的本地缓存(简称 local DNS)，local DNS 一般与用户在同个网络内，这时候 DNS 调度是有效的。

但近些年，不少互联网厂商推广基于 BGP Anycast 的公共 DNS (Public DNS)，而这些Anycaset IP 的节点一般是远少于各个ISP的节点，例如可能广州电信用户使用了某公共 DNS，但该公共 DNS 里用户最近的是上海电信节点，甚至更极端的如 Google DNS 8.8.8.8，在中国大陆没有节点(最近的是台湾)。而不幸的是国内有不少用户使用了 Google DNS，这其实降低了他们的网络访问体验。总的来说，**使用公共 DNS，实际上破坏了上文的前提，导致 DNS 区域调度失效，用户以为得到了更快更安全的 DNS 解析，但实际得到了错误的解析，增加了网络访问延迟**。

传统 DNS 协议的区域调度过程示例如下图，假定某业务以 foo.163.com 对外提供服务，在北京和东京各有一个节点，业务期望国内大陆的用户访问北京节点，而非大陆用户则访问东京节点。因为权威是根据 DNS 缓存来决定返回的结果，所以当用户使用不用的 DNS 缓存时，可能会解析到不同的结果。

![](imgs/dev-network-dns-3.jpeg.jpg)

2011 年，Google 为首的几家公司在提出了一个 DNS 的扩展方案 edns-client-subnet (以下简称 ECS)，该扩展方案的核心思想是通过在 DNS 请求报文里加入原始请求的 IP(即 client 的 IP)，使得权威能根据该信息返回正确的结果。目前，该方案仍处于草案阶段。该方案很好地解决了上述提到的 remote DNS 导致解析不准确的问题，但也带了一些问题：

+   至少需要 cache 和权威都支持，才能完成完整的 ECS 解析
+   ECS 给 cache 增加了很大的缓存压力，因为理论上可能需要为每个IP段分配空间去缓存解析结果

![](imgs/dev-network-dns-4.jpeg.jpg)

### 规则变更生效时间不确定

当缓存服务器向权威服务器查询得到记录之后，会将其缓存起来，在缓存有效期内，如果收到相同记录的查询，缓存服务器会直接返回给客户端，而不需要再次向权威查询，当有效期过后，缓存则是需要再次发起查询。这个缓存有效期即是 TTL。

虽然 DNS 的缓存机制在大多数情况下缩短了客户端的记录解析时间，但缓存也意味着生效同步的延迟。当权威服务器的记录变更时，需要等待一段时间才能让所有客户端能解析到新的结果，因为很可能缓存服务器还缓存着旧的记录。

我们将权威的记录变更到全网生效这个过程称为 propagation，它的时间是不确定的，理论上的最大值即是 TTL 的值，对于记录变更或删除，这个时间是记录原本的 TTL，对于记录新增则是域的 nTTL 值。

如果一个域名记录原本的 TTL 是 18000，可以认为，变更该记录理论上需要等待 5 个小时才能保证记录能生效到全网。假设该域名的业务方希望缩短切换的时间，正确的做法是，至少提前5个小时修改记录，仅改小 TTL，例如改为5分钟，等待该变更同步到全网之后，再进行修改指向的操作，确认无误再将 TTL 修改为原本的值。

虽然 DNS 协议标准里建议缓存服务器应该记住或者缩短 TTL 的值，但实际上，有一些DNS缓存会修改权威服务器的 TTL，将其变大，这在国内几大运营商中是很常见的。例如，某域记录的 TTL 值实际上设置为 60，但在运营商的 DNS 缓存上，却变成 600 或者更大的值，甚至还有一些 DNS 缓存是不遵循 TTL 机制。这些都会影响域名的实际生效时间。

### 高可用

为避免受 DNS 缓存的影响，需要保证 DNS 中 A 记录的 IP 节点高可用性。对此，网易云DNS 调度系统采用的方案是在同一区域的多台直播服务器节点之间做负载均衡，对外只暴露一个虚 IP，这样，即使某台服务器宕机，负载均衡能迅速感知到，排除故障节点，而对 DNS 而言，因为虚 IP 不变而不受影响。

![](imgs/dev-network-dns-5.jpeg.jpg)

## DNS 安全相关

> 本章节部分内容参考自： [OneAPM blog在新窗口打开](http://blog.oneapm.com/)

犯罪分子会抓住任何互联网服务或协议的漏洞发动攻击，这当然也包括域名系统（ DNS ）。他们会注册一次性域名用于垃圾邮件活动和僵尸网络管理，还会盗用域名进行钓鱼和恶意软件下载。他们会注入恶意查询代码以利用域名服务器的漏洞或扰乱域名解析过程。他们会注入伪造的响应污染解析器缓存或强化 DDOS 攻击。他们甚至将 DNS 用作数据渗漏或恶意软件更新的隐蔽通道。

你可能没办法了解每一个新的 DNS 漏洞攻击，但## 网络协议 - DNS 相关详解

> DNS的核心工作就是将域名翻译成计算机IP地址, 它是基于UDP协议实现的，本文将具体阐述DNS相关的概念，解析，调度原理（负载均衡和区域调度）等DNS相关的所有知识点。 @pdai

+   [DNS简介](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns%E7%AE%80%E4%BB%8B)
    +   [域名层级结构](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E5%9F%9F%E5%90%8D%E5%B1%82%E7%BA%A7%E7%BB%93%E6%9E%84)
    +   [域名服务器](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E5%9F%9F%E5%90%8D%E6%9C%8D%E5%8A%A1%E5%99%A8)
+   [DNS 解析流程](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns-%E8%A7%A3%E6%9E%90%E6%B5%81%E7%A8%8B)
    +   [为什么DNS通常基于UDP](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E4%B8%BA%E4%BB%80%E4%B9%88dns%E9%80%9A%E5%B8%B8%E5%9F%BA%E4%BA%8Eudp)
+   [DNS 查询](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns-%E6%9F%A5%E8%AF%A2)
    +   [dig 查询](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dig-%E6%9F%A5%E8%AF%A2)
    +   [host查询](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#host%E6%9F%A5%E8%AF%A2)
    +   [nslookup查询](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#nslookup%E6%9F%A5%E8%AF%A2)
    +   [whois查询](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#whois%E6%9F%A5%E8%AF%A2)
    +   [在线工具查询](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E5%9C%A8%E7%BA%BF%E5%B7%A5%E5%85%B7%E6%9F%A5%E8%AF%A2)
+   [DNS 调度原理](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns-%E8%B0%83%E5%BA%A6%E5%8E%9F%E7%90%86)
    +   [地理位置调度不准确](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E5%9C%B0%E7%90%86%E4%BD%8D%E7%BD%AE%E8%B0%83%E5%BA%A6%E4%B8%8D%E5%87%86%E7%A1%AE)
    +   [规则变更生效时间不确定](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E8%A7%84%E5%88%99%E5%8F%98%E6%9B%B4%E7%94%9F%E6%95%88%E6%97%B6%E9%97%B4%E4%B8%8D%E7%A1%AE%E5%AE%9A)
    +   [高可用](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E9%AB%98%E5%8F%AF%E7%94%A8)
+   [DNS 安全相关](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns-%E5%AE%89%E5%85%A8%E7%9B%B8%E5%85%B3)
    +   [什么是DNS劫持](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E4%BB%80%E4%B9%88%E6%98%AFdns%E5%8A%AB%E6%8C%81)
    +   [什么是DNS污染](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E4%BB%80%E4%B9%88%E6%98%AFdns%E6%B1%A1%E6%9F%93)
    +   [为什么要DNS流量监控](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81dns%E6%B5%81%E9%87%8F%E7%9B%91%E6%8E%A7)
    +   [DNS 流量监控](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns-%E6%B5%81%E9%87%8F%E7%9B%91%E6%8E%A7)
    +   [DNS 服务器监控](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#dns-%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%9B%91%E6%8E%A7)
+   [参考文章](https://pdai.tech/md/develop/protocol/dev-protocol-dns.html#%E5%8F%82%E8%80%83%E6%96%87%E7%AB%A0)

## DNS简介

域名系统并不像电话号码通讯录那么简单，通讯录主要是单个个体在使用，同一个名字出现在不同个体的通讯录里并不会出现问题，但域名是群体中所有人都在用的，必须要保持唯一性。为了达到唯一性的目的，因特网在命名的时候采用了层次结构的命名方法。每一个域名（本文只讨论英文域名）都是一个标号序列（labels），用字母（A-Z，a-z，大小写等价）、数字（0-9）和连接符（-）组成，标号序列总长度不能超过255个字符，它由点号分割成一个个的标号（label），每个标号应该在63个字符之内，每个标号都可以看成一个层次的域名。级别最低的域名写在左边，级别最高的域名写在右边。域名服务主要是基于UDP实现的，服务器的端口号为53。

### 域名层级结构

![](imgs/dev-network-dns-9.png)

> 注意：最开始的域名最后都是带了点号的，比如 `www.kernel.org.` ，最后面的点号表示根域名服务器，后来发现所有的网址都要加上最后的点，就简化了写法，干脆所有的都不加即`www.kernel.org`，但是你在网址后面加上点号也是可以正常解析的。

### 域名服务器

有域名结构还不行，还需要有一个东西去解析域名，手机通讯录是由通讯录软件解析的，域名需要由遍及全世界的域名服务器去解析，域名服务器实际上就是装有域名系统的主机。由高向低进行层次划分，可分为以下几大类：

+   **根域名服务器**：最高层次的域名服务器，也是最重要的域名服务器，本地域名服务器如果解析不了域名就会向根域名服务器求助。全球共有13个不同IP地址的根域名服务器，它们的名称用一个英文字母命名，从a一直到m。这些服务器由各种组织控制，并由 ICANN（互联网名称和数字地址分配公司）授权，由于每分钟都要解析的名称数量多得令人难以置信，所以实际上每个根服务器都有镜像服务器，每个根服务器与它的镜像服务器共享同一个 IP 地址，中国大陆地区内只有6组根服务器镜像（F，I（3台），J，L）。当你对某个根服务器发出请求时，请求会被路由到该根服务器离你最近的镜像服务器。所有的根域名服务器都知道所有的顶级域名服务器的域名和地址，如果向根服务器发出对 “pdai.tech” 的请求，则根服务器是不能在它的记录文件中找到与 “pdai.tech” 匹配的记录。但是它会找到 "tech" 的顶级域名记录，并把负责 "tech" 地址的顶级域名服务器的地址发回给请求者。
+   **顶级域名服务器**：负责管理在该顶级域名服务器下注册的二级域名。当根域名服务器告诉查询者顶级域名服务器地址时，查询者紧接着就会到顶级域名服务器进行查询。比如还是查询"pdai.tech"，根域名服务器已经告诉了查询者"tech"顶级域名服务器的地址，"tech"顶级域名服务器会找到 “pdai.tech”的域名服务器的记录，域名服务器检查其区域文件，并发现它有与 “pdai.tech” 相关联的区域文件。在此文件的内部，有该主机的记录。此记录说明此主机所在的 IP 地址，并向请求者返回最终答案。
+   **权限域名服务器**：负责一个区的域名解析工作
+   **本地域名服务器**：当一个主机发出DNS查询请求的时候，这个查询请求首先就是发给本地域名服务器的。

![](imgs/dev-network-dns-11.png)

## DNS 解析流程

> 网上找了个比较好的例子

.com.fi国际金融域名DNS解析的步骤一共分为9步，如果每次解析都要走完9个步骤，大家浏览网站的速度也不会那么快，现在之所以能保持这么快的访问速度，其实一般的解析都是跑完第4步就可以了。除非一个地区完全是第一次访问（在都没有缓存的情况下）才会走完9个步骤，这个情况很少。

+   1、本地客户机提出域名解析请求，查找本地HOST文件后将该请求发送给本地的域名服务器。
+   2、将请求发送给本地的域名服务器。
+   3、当本地的域名服务器收到请求后，就先查询本地的缓存。
+   4、如果有该纪录项，则本地的域名服务器就直接把查询的结果返回浏览器。
+   5、如果本地DNS缓存中没有该纪录，则本地域名服务器就直接把请求发给根域名服务器。
+   6、然后根域名服务器再返回给本地域名服务器一个所查询域（根的子域）的主域名服务器的地址。
+   7、本地服务器再向上一步返回的域名服务器发送请求，然后接受请求的服务器查询自己的缓存，如果没有该纪录，则返回相关的下级的域名服务器的地址。
+   8、重复第7步，直到找到正确的纪录。
+   9、本地域名服务器把返回的结果保存到缓存，以备下一次使用，同时还将结果返回给客户机。

![](imgs/dev-network-dns-12.png)

注意事项：

**递归查询**：在该模式下DNS服务器接收到客户机请求，必须使用一个准确的查询结果回复客户机。如果DNS服务器本地没有存储查询DNS信息，那么该服务器会询问其他服务器，并将返回的查询结果提交给客户机。

**迭代查询**：DNS所在服务器若没有可以响应的结果，会向客户机提供其他能够解析查询请求的DNS服务器地址，当客户机发送查询请求时，DNS服务器并不直接回复查询结果，而是告诉客户机另一台DNS服务器地址，客户机再向这台DNS服务器提交请求，依次循环直到返回查询的结果为止。

### 为什么DNS通常基于UDP

使用基于UDP的DNS协议只要一个请求、一个应答就好了

而使用基于TCP的DNS协议要三次握手、发送数据以及应答、四次挥手

明显基于TCP协议的DNS更浪费网络资源！

当然以上只是从数据包的数量以及占有网络资源的层面来进行的分析，那数据一致性层面呢？

DNS数据包不是那种大数据包，所以使用UDP不需要考虑分包，如果丢包那么就是全部丢包，如果收到了数据，那就是收到了全部数据！所以只需要考虑丢包的情况，那就算是丢包了，重新请求一次就好了。而且DNS的报文允许填入序号字段，对于请求报文和其对应的应答报文，这个字段是相同的，通过它可以区分DNS应答是对应的哪个请求

> DNS通常是基于UDP的，但当数据长度大于512字节的时候，为了保证传输质量，就会使用基于TCP的实现方式

## DNS 查询

### dig 查询

> 用`dig`可以查看整个过程，看下下面的返回就能理解的

+   dig www.sina.com

```bash
pdaiMbp:/ pdai$ dig www.sina.com

; <<>> DiG 9.10.6 <<>> www.sina.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15304
;; flags: qr rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;www.sina.com.			IN	A

;; ANSWER SECTION:
www.sina.com.		32	IN	CNAME	us.sina.com.cn.
us.sina.com.cn.		32	IN	CNAME	spool.grid.sinaedge.com.
spool.grid.sinaedge.com. 32	IN	A	115.238.190.240

;; Query time: 20 msec
;; SERVER: 192.168.3.1#53(192.168.3.1)
;; WHEN: Fri Jan 31 14:53:39 CST 2020
;; MSG SIZE  rcvd: 108
```

+   dig +trace www.sina.com // 分级查询

```bash
pdaiMbp:/ pdai$ dig +trace www.sina.com

; <<>> DiG 9.10.6 <<>> +trace www.sina.com
;; global options: +cmd
.			52722	IN	NS	f.root-servers.net.
.			52722	IN	NS	k.root-servers.net.
.			52722	IN	NS	m.root-servers.net.
.			52722	IN	NS	l.root-servers.net.
.			52722	IN	NS	d.root-servers.net.
.			52722	IN	NS	i.root-servers.net.
.			52722	IN	NS	c.root-servers.net.
.			52722	IN	NS	e.root-servers.net.
.			52722	IN	NS	a.root-servers.net.
.			52722	IN	NS	g.root-servers.net.
.			52722	IN	NS	b.root-servers.net.
.			52722	IN	NS	h.root-servers.net.
.			52722	IN	NS	j.root-servers.net.
;; Received 228 bytes from 192.168.3.1#53(192.168.3.1) in 3 ms

com.			172800	IN	NS	a.gtld-servers.net.
com.			172800	IN	NS	b.gtld-servers.net.
com.			172800	IN	NS	c.gtld-servers.net.
com.			172800	IN	NS	d.gtld-servers.net.
com.			172800	IN	NS	e.gtld-servers.net.
com.			172800	IN	NS	f.gtld-servers.net.
com.			172800	IN	NS	g.gtld-servers.net.
com.			172800	IN	NS	h.gtld-servers.net.
com.			172800	IN	NS	i.gtld-servers.net.
com.			172800	IN	NS	j.gtld-servers.net.
com.			172800	IN	NS	k.gtld-servers.net.
com.			172800	IN	NS	l.gtld-servers.net.
com.			172800	IN	NS	m.gtld-servers.net.
com.			86400	IN	DS	30909 8 2 E2D3C916F6DEEAC73294E8268FB5885044A833FC5459588F4A9184CF C41A5766
com.			86400	IN	RRSIG	DS 8 1 86400 20200213050000 20200131040000 33853 . zMeZpKg/LGzpVjlBUJRfkmk8tSvZW+L0UFHnzSn8agztJ8sMGU+knBLW 5LLoPoh6iG7exLV5wVIJZVh+0ISk3AG85VJXZ3HSTWcHZfjMOYI7JXpe pv/5JqT9Eai0ScEJAowDa1qctGOE/LHdNwr30VF8U0LoZL0iXVN3KQ4k iKnl0S0hB41KH+BHFcNpWqxKHRK2piMZRNe8+8Nu9I4GilfW/D90e69p SgG7puU3J3srarhccj0OS5WcLi6nsMf/2k0C6rQMe+WD7aOVZXoLts93 /thoNSWIprseKrYze2STnuG+T/VxzZRJ3fjoZARGHtDf3gTibHC2syXL xaXz5w==
;; Received 1172 bytes from 198.97.190.53#53(h.root-servers.net) in 54 ms

sina.com.		172800	IN	NS	ns1.sina.com.cn.
sina.com.		172800	IN	NS	ns2.sina.com.cn.
sina.com.		172800	IN	NS	ns3.sina.com.cn.
sina.com.		172800	IN	NS	ns1.sina.com.
sina.com.		172800	IN	NS	ns2.sina.com.
sina.com.		172800	IN	NS	ns4.sina.com.
sina.com.		172800	IN	NS	ns3.sina.com.
CK0POJMG874LJREF7EFN8430QVIT8BSM.com. 86400 IN NSEC3 1 1 0 - CK0Q1GIN43N1ARRC9OSM6QPQR81H5M9A  NS SOA RRSIG DNSKEY NSEC3PARAM
CK0POJMG874LJREF7EFN8430QVIT8BSM.com. 86400 IN RRSIG NSEC3 8 2 86400 20200207054811 20200131043811 56311 com. N15f7ia8A0pd2A5iWM/8t+T6gs8mQJaOWe/aj3bs4cWxpG7WmCaquZp7 6gfbfotFmss+DuBm9MAd6bwe2fm9m60FQgROWGOZwGRrvZqawy/5eDeV sLIJqhnwM0lT1PuDgNe2SFYsV506melwC4cEtR8M6gkX3nwYMCf6Frus anO+4Lufi229N5Y00N4x9vrlO3zsGBR1yg2xBki9Ni379A==
TGAG8VMC6NS5VVK68CIGRJ6Q414N2KB2.com. 86400 IN NSEC3 1 1 0 - TGAGRAEN3DVBS761O1PSQ1TU0407EVHO  NS DS RRSIG
TGAG8VMC6NS5VVK68CIGRJ6Q414N2KB2.com. 86400 IN RRSIG NSEC3 8 2 86400 20200206061700 20200130050700 56311 com. TMrk1/56Wa+isS5Y6OFQz9OZWMAAbt2TOEzaBp1Uuj9z1Eg4uio92+ff sWRB6vACYSBAJJLy4NPJfqxYpue39hvaDgFRYAZreDuCC0x+p9yi7yQ8 JaN2mS7W8Mbv0iEV0AUyzZGyhYq83DA58slNSGRhZfcvLYBAETURyH0X bJp+Hgq0bqXOqGyi/lAAv8/2mr+tiramb/pNst1MBBPaig==
;; Received 791 bytes from 192.48.79.30#53(j.gtld-servers.net) in 215 ms

www.sina.com.		60	IN	CNAME	us.sina.com.cn.
us.sina.com.cn.		60	IN	CNAME	spool.grid.sinaedge.com.
;; Received 103 bytes from 180.149.138.199#53(ns2.sina.com.cn) in 30 ms
```

域名与IP之间的对应关系，称为"记录"（record）。根据使用场景，"记录"可以分成不同的类型（type），前面已经看到了有A记录和NS记录。

常见的DNS记录类型如下。

+   **A**：地址记录（Address），返回域名指向的IP地址。
+   **NS**：域名服务器记录（Name Server），返回保存下一级域名信息的服务器地址。该记录只能设置为域名，不能设置为IP地址。
+   **MX**：邮件记录（Mail eXchange），返回接收电子邮件的服务器地址。
+   **CNAME**：规范名称记录（Canonical Name），返回另一个域名，即当前查询的域名是另一个域名的跳转，详见下文。
+   **PTR**：逆向查询记录（Pointer Record），只用于从IP地址查询域名，详见下文。

一般来说，为了服务的安全可靠，至少应该有两条NS记录，而A记录和MX记录也可以有多条，这样就提供了服务的冗余性，防止出现单点失败。

CNAME记录主要用于域名的内部跳转，为服务器配置提供灵活性，用户感知不到。

### host查询

```bash
pdaiMbp:/ pdai$ host www.sina.com
www.sina.com is an alias for us.sina.com.cn.
us.sina.com.cn is an alias for spool.grid.sinaedge.com.
spool.grid.sinaedge.com has address 115.238.190.240
spool.grid.sinaedge.com has IPv6 address 240e:f7:a000:221::75:71
```

### nslookup查询

```bash
pdaiMbp:/ pdai$ nslookup
> www.sina.com
Server:		192.168.3.1
Address:	192.168.3.1#53

Non-authoritative answer:
www.sina.com	canonical name = us.sina.com.cn.
us.sina.com.cn	canonical name = spool.grid.sinaedge.com.
Name:	spool.grid.sinaedge.com
Address: 115.238.190.240
```

### whois查询

```bash
pdaiMbp:/ pdai$ whois www.sina.com
% IANA WHOIS server
% for more information on IANA, visit http://www.iana.org
% This query returned 1 object

refer:        whois.verisign-grs.com

domain:       COM

organisation: VeriSign Global Registry Services
address:      12061 Bluemont Way
address:      Reston Virginia 20190
address:      United States

contact:      administrative
name:         Registry Customer Service
organisation: VeriSign Global Registry Services
address:      12061 Bluemont Way
address:      Reston Virginia 20190
address:      United States
phone:        +1 703 925-6999
fax-no:       +1 703 948 3978
e-mail:       info@verisign-grs.com

contact:      technical
name:         Registry Customer Service
organisation: VeriSign Global Registry Services
address:      12061 Bluemont Way
address:      Reston Virginia 20190
address:      United States
phone:        +1 703 925-6999
fax-no:       +1 703 948 3978
e-mail:       info@verisign-grs.com

nserver:      A.GTLD-SERVERS.NET 192.5.6.30 2001:503:a83e:0:0:0:2:30
nserver:      B.GTLD-SERVERS.NET 192.33.14.30 2001:503:231d:0:0:0:2:30
nserver:      C.GTLD-SERVERS.NET 192.26.92.30 2001:503:83eb:0:0:0:0:30
nserver:      D.GTLD-SERVERS.NET 192.31.80.30 2001:500:856e:0:0:0:0:30
nserver:      E.GTLD-SERVERS.NET 192.12.94.30 2001:502:1ca1:0:0:0:0:30
nserver:      F.GTLD-SERVERS.NET 192.35.51.30 2001:503:d414:0:0:0:0:30
nserver:      G.GTLD-SERVERS.NET 192.42.93.30 2001:503:eea3:0:0:0:0:30
nserver:      H.GTLD-SERVERS.NET 192.54.112.30 2001:502:8cc:0:0:0:0:30
nserver:      I.GTLD-SERVERS.NET 192.43.172.30 2001:503:39c1:0:0:0:0:30
nserver:      J.GTLD-SERVERS.NET 192.48.79.30 2001:502:7094:0:0:0:0:30
nserver:      K.GTLD-SERVERS.NET 192.52.178.30 2001:503:d2d:0:0:0:0:30
nserver:      L.GTLD-SERVERS.NET 192.41.162.30 2001:500:d937:0:0:0:0:30
nserver:      M.GTLD-SERVERS.NET 192.55.83.30 2001:501:b1f9:0:0:0:0:30
ds-rdata:     30909 8 2 E2D3C916F6DEEAC73294E8268FB5885044A833FC5459588F4A9184CFC41A5766

whois:        whois.verisign-grs.com

status:       ACTIVE
remarks:      Registration information: http://www.verisigninc.com

created:      1985-01-01
changed:      2017-10-05
source:       IANA

No match for domain "WWW.SINA.COM".
>>> Last update of whois database: 2020-01-31T07:03:29Z <<<
```

### 在线工具查询

[https://www.nslookuptool.com/chs/在新窗口打开](https://www.nslookuptool.com/chs/)

![](imgs/dev-network-dns-10.png)

## DNS 调度原理

> 本节转自：[【网易MC】DNS 调度原理解析在新窗口打开](https://segmentfault.com/a/1190000010787338)

现在，大部分应用和业务都采用域名作为服务的入口，因此用 DNS 来负载均衡和区域调度是非常普遍的做法，网易云也有着一套基于 DNS 的调度系统。某些用户在进行直播推流时用的并不是网易云的直播 SDK，而是一些第三方的推流软件，如obs，这样就不能使用我们的 GSLB 全局调度服务器来调度。对于这些用户，我们使用 DNS 调度的方式，对不同地域的请求返回不同解析结果，将请求调度到离用户最近的服务器节点，从而减少延迟访问。

![](imgs/dev-network-dns-2.jpeg.jpg)

咋一看，DNS 调度这么简单方便，那为什么不让所有的用户都走 DNS 调度呢？想知道原因？来，我们继续讲。

### 地理位置调度不准确

在 DNS 解析过程中，与权威服务器通信的只有 DNS 缓存服务器，所以权威服务器只能根据 DNS 缓存服务器的IP来进行调度。因此 DNS 调度有一个前提：假定用户使用的缓存DNS与用户本身在同个网络内，即至少在同一个 AS(自治域)内，在该前提下，DNS 的解析才是准确的。通常情况下，用户使用 ISP 提供的本地缓存(简称 local DNS)，local DNS 一般与用户在同个网络内，这时候 DNS 调度是有效的。

但近些年，不少互联网厂商推广基于 BGP Anycast 的公共 DNS (Public DNS)，而这些Anycaset IP 的节点一般是远少于各个ISP的节点，例如可能广州电信用户使用了某公共 DNS，但该公共 DNS 里用户最近的是上海电信节点，甚至更极端的如 Google DNS 8.8.8.8，在中国大陆没有节点(最近的是台湾)。而不幸的是国内有不少用户使用了 Google DNS，这其实降低了他们的网络访问体验。总的来说，**使用公共 DNS，实际上破坏了上文的前提，导致 DNS 区域调度失效，用户以为得到了更快更安全的 DNS 解析，但实际得到了错误的解析，增加了网络访问延迟**。

传统 DNS 协议的区域调度过程示例如下图，假定某业务以 foo.163.com 对外提供服务，在北京和东京各有一个节点，业务期望国内大陆的用户访问北京节点，而非大陆用户则访问东京节点。因为权威是根据 DNS 缓存来决定返回的结果，所以当用户使用不用的 DNS 缓存时，可能会解析到不同的结果。

![](imgs/dev-network-dns-3.jpeg.jpg)

2011 年，Google 为首的几家公司在提出了一个 DNS 的扩展方案 edns-client-subnet (以下简称 ECS)，该扩展方案的核心思想是通过在 DNS 请求报文里加入原始请求的 IP(即 client 的 IP)，使得权威能根据该信息返回正确的结果。目前，该方案仍处于草案阶段。该方案很好地解决了上述提到的 remote DNS 导致解析不准确的问题，但也带了一些问题：

+   至少需要 cache 和权威都支持，才能完成完整的 ECS 解析
+   ECS 给 cache 增加了很大的缓存压力，因为理论上可能需要为每个IP段分配空间去缓存解析结果

![](imgs/dev-network-dns-4.jpeg.jpg)

### 规则变更生效时间不确定

当缓存服务器向权威服务器查询得到记录之后，会将其缓存起来，在缓存有效期内，如果收到相同记录的查询，缓存服务器会直接返回给客户端，而不需要再次向权威查询，当有效期过后，缓存则是需要再次发起查询。这个缓存有效期即是 TTL。

虽然 DNS 的缓存机制在大多数情况下缩短了客户端的记录解析时间，但缓存也意味着生效同步的延迟。当权威服务器的记录变更时，需要等待一段时间才能让所有客户端能解析到新的结果，因为很可能缓存服务器还缓存着旧的记录。

我们将权威的记录变更到全网生效这个过程称为 propagation，它的时间是不确定的，理论上的最大值即是 TTL 的值，对于记录变更或删除，这个时间是记录原本的 TTL，对于记录新增则是域的 nTTL 值。

如果一个域名记录原本的 TTL 是 18000，可以认为，变更该记录理论上需要等待 5 个小时才能保证记录能生效到全网。假设该域名的业务方希望缩短切换的时间，正确的做法是，至少提前5个小时修改记录，仅改小 TTL，例如改为5分钟，等待该变更同步到全网之后，再进行修改指向的操作，确认无误再将 TTL 修改为原本的值。

虽然 DNS 协议标准里建议缓存服务器应该记住或者缩短 TTL 的值，但实际上，有一些DNS缓存会修改权威服务器的 TTL，将其变大，这在国内几大运营商中是很常见的。例如，某域记录的 TTL 值实际上设置为 60，但在运营商的 DNS 缓存上，却变成 600 或者更大的值，甚至还有一些 DNS 缓存是不遵循 TTL 机制。这些都会影响域名的实际生效时间。

### 高可用

为避免受 DNS 缓存的影响，需要保证 DNS 中 A 记录的 IP 节点高可用性。对此，网易云DNS 调度系统采用的方案是在同一区域的多台直播服务器节点之间做负载均衡，对外只暴露一个虚 IP，这样，即使某台服务器宕机，负载均衡能迅速感知到，排除故障节点，而对 DNS 而言，因为虚 IP 不变而不受影响。

![](imgs/dev-network-dns-5.jpeg.jpg)

## DNS 安全相关

> 本章节部分内容参考自： [OneAPM blog在新窗口打开](http://blog.oneapm.com/)

犯罪分子会抓住任何互联网服务或协议的漏洞发动攻击，这当然也包括域名系统（ DNS ）。他们会注册一次性域名用于垃圾邮件活动和僵尸网络管理，还会盗用域名进行钓鱼和恶意软件下载。他们会注入恶意查询代码以利用域名服务器的漏洞或扰乱域名解析过程。他们会注入伪造的响应污染解析器缓存或强化 DDOS 攻击。他们甚至将 DNS 用作数据渗漏或恶意软件更新的隐蔽通道。

你可能没办法了解每一个新的 DNS 漏洞攻击，但是可以使用防火墙、网络入侵监测系统或域名解析器报告可疑的 DNS 行为迹象，作为主动防范的措施。

先讲下最常用的手段：`DNS劫持`和`DNS污染`。

### 什么是DNS劫持

DNS劫持就是通过劫持了DNS服务器，通过某些手段取得某域名的解析记录控制权，进而修改此域名的解析结果，导致对该域名的访问由原IP地址转入到修改后的指定IP，其结果就是对特定的网址不能访问或访问的是假网址，从而实现窃取资料或者破坏原有正常服务的目的。DNS劫持通过篡改DNS服务器上的数据返回给用户一个错误的查询结果来实现的。

> DNS劫持症状：在某些地区的用户在成功连接宽带后，首次打开任何页面都指向ISP提供的“电信互联星空”、“网通黄页广告”等内容页面。还有就是曾经出现过用户访问Google域名的时候出现了百度的网站。这些都属于DNS劫持。

### 什么是DNS污染

DNS污染是一种让一般用户由于得到虚假目标主机IP而不能与其通信的方法，是一种DNS缓存投毒攻击（DNS cache poisoning）。其工作方式是：由于通常的DNS查询没有任何认证机制，而且DNS查询通常基于的UDP是无连接不可靠的协议，因此DNS的查询非常容易被篡改，通过对UDP端口53上的DNS查询进行入侵检测，一经发现与关键词相匹配的请求则立即伪装成目标域名的解析服务器（NS，Name Server）给查询者返回虚假结果。

而DNS污染则是发生在用户请求的第一步上，直接从协议上对用户的DNS请求进行干扰。

**DNS污染症状**：目前一些被禁止访问的网站很多就是通过DNS污染来实现的，例如YouTube、Facebook等网站。

**解决方法**:

+   对于DNS劫持，可以采用使用国外免费公用的DNS服务器解决。例如OpenDNS（208.67.222.222）或GoogleDNS（8.8.8.8）。
+   对于DNS污染，可以说，个人用户很难单单靠设置解决，通常可以使用VPN或者域名远程解析的方法解决，但这大多需要购买付费的VPN或SSH等，也可以通过修改Hosts的方法，手动设置域名正确的IP地址。

### 为什么要DNS流量监控

预示网络中正出现可疑或恶意代码的 DNS 组合查询或流量特征。例如：

+   1.来自伪造源地址的 DNS 查询、或未授权使用且无出口过滤地址的 DNS 查询，若同时观察到异常大的 DNS 查询量或使用 TCP 而非 UDP 进行 DNS 查询，这可能表明网络内存在被感染的主机，受到了 DDoS 攻击。
    
+   2.异常 DNS 查询可能是针对域名服务器或解析器（根据目标 IP 地址确定）的漏洞攻击的标志。与此同时，这些查询也可能表明网络中有不正常运行的设备。原因可能是恶意软件或未能成功清除恶意软件。
    
+   3.在很多情况下，DNS 查询要求解析的域名如果是已知的恶意域名，或具有域名生成算法( DGA )（与非法僵尸网络有关）常见特征的域名，或者向未授权使用的解析器发送的查询，都是证明网络中存在被感染主机的有力证据。
    
+   4.DNS 响应也能显露可疑或恶意数据在网络主机间传播的迹象。例如，DNS 响应的长度或组合特征可以暴露恶意或非法行为。例如，响应消息异常巨大（放大攻击），或响应消息的 Answer Section 或 Additional Section 非常可疑（缓存污染，隐蔽通道）。
    
+   5.针对自身域名组合的 DNS 响应，如果解析至不同于你发布在授权区域中的 IP 地址，或来自未授权区域主机的域名服务器的响应，或解析为名称错误( NXDOMAIN )的对区域主机名的肯定响应，均表明域名或注册账号可能被劫持或 DNS 响应被篡改。
    
+   6.来自可疑 IP 地址的 DNS 响应，例如来自分配给宽带接入网络 IP 段的地址、非标准端口上出现的 DNS 流量，异常大量的解析至短生存时间( TTL )域名的响应消息，或异常大量的包含“ name error ”( NXDOMAIN )的响应消息，往往是主机被僵尸网络控制、运行恶意软件或被感染的表现。
    

### DNS 流量监控

> 如何借助网络入侵检测系统、流量分析和日志数据在网络防火墙上应用这些机制以检测此类威胁?

#### 防火墙

我们从最常用的安全系统开始吧，那就是防火墙。所有的防火墙都允许自定义规则以防止 IP 地址欺骗。添加一条规则，拒绝接收来自指定范围段以外的 IP 地址的 DNS 查询，从而避免域名解析器被 DDOS 攻击用作开放的反射器。

接下来，启动 DNS 流量检测功能，监测是否存在可疑的字节模式或异常 DNS 流量，以阻止域名服务器软件漏洞攻击。具备本功能的常用防火墙的介绍资料在许多网站都可以找到（例如 Palo Alto、思科、沃奇卫士等）。Sonicwall 和 Palo Alto 还可以监测并拦截特定的 DNS 隧道流量。

#### 入侵检测系统

无论你使用 Snort、Suricata 还是 OSSEC，都可以制定规则，要求系统对未授权客户的 DNS 请求发送报告。你也可以制定规则来计数或报告 NXDomain 响应、包含较小 TTL 数值记录的响应、通过 TCP 发起的 DNS 查询、对非标准端口的 DNS 查询和可疑的大规模 DNS 响应等。DNS 查询或响应信息中的任何字段、任何数值基本上都“能检测”。唯一能限制你的，就是你的想象力和对 DNS 的熟悉程度。防火墙的 IDS （入侵检测系统）对大多数常见检测项目都提供了允许和拒绝两种配置规则。

#### 流量分析工具

Wireshark 和 Bro 的实际案例都表明，被动流量分析对识别恶意软件流量很有效果。捕获并过滤客户端与解析器之间的 DNS 数据，保存为 PCAP （网络封包）文件。创建脚本程序搜索这些网络封包，以寻找你正在调查的某种可疑行为。或使用 PacketQ （最初是 DNS2DB ）对网络封包直接进行 SQL 查询。

（记住：除了自己的本地解析器之外，禁止客户使用任何其他解析器或非标准端口。）

#### DNS 被动复制

该方法涉及对解析器使用传感器以创建数据库，使之包含通过给定解析器或解析器组进行的所有 DNS 交易（查询/响应）。在分析中包含 DNS 被动数据对识别恶意软件域名有着重要作用，尤其适用于恶意软件使用由算法生成的域名的情况。将 Suricata 用做 IDS （入侵检测系统）引擎的 Palo Alto 防火墙和安全管理系统，正是结合使用被动 DNS 与 IPS （入侵防御系统）以防御已知恶意域名的安全系统范例。

#### 解析器日志记录

本地解析器的日志文件是调查 DNS 流量的最后一项，也可能是最明显的数据来源。在开启日志记录的情况下，你可以使用 Splunk 加 getwatchlist 或是 OSSEC 之类的工具收集 DNS 服务器的日志，并搜索已知恶意域名。

尽管本文提到了不少资料链接、案例分析和实际例子，但也只是涉及了众多监控 DNS 流量方法中的九牛一毛，疏漏在所难免，要想全面快捷及时有效监控 DNS 流量，不妨试试 DNS 服务器监控。

### DNS 服务器监控

应用管理器可对域名系统（ DNS ）进行全面深入的可用性和性能监控，也可监控 DNS 监控器的个别属性，比如响应时间、记录类型、可用记录、搜索字段、搜索值、搜索值状态以及搜索时间等。

DNS 中被监控的一些关键组件：

| 指标 | 描述 |
| --- | --- |
| 响应时间 | 给出 DNS 监控器的响应时间，以毫秒表示 |
| 记录类型 | 显示记录类型连接到 DNS 服务器的耗时 |
| 可用记录 | 根据可用记录类型输出 True 或 False |
| 搜索字段 | 显示用于 DNS 服务器的字段类型 |
| 搜索值 | 显示在DNS 服务器中执行的搜索值 |
| 搜索值状态 | 根据输出信息显示搜索值状态：Success (成功)或 Failed (失败) |
| 搜索时间 | DNS 服务器中的搜索执行时间 |

`监控可用性`和`响应时间`等性能统计数据。这些数据可绘制成性能图表和报表，即时可用，还可以按照可用性和完善性对报表进行分组显示。

若 DNS 服务器或系统内任何特定属性出现问题，会根据配置好的阈值生成通知和警告，并根据配置自动执行相关操作。目前，国内外 DNS 监控工具主要有 New relic、appDynamic、OneAPM。

## 参考文章

+   [【网易MC】DNS 调度原理解析 在新窗口打开](https://segmentfault.com/a/1190000010787338)
+   [DNS 原理入门 在新窗口打开](http://www.ruanyifeng.com/blog/2016/06/dns.html)
+   [DNS原理及其解析过程【精彩剖析】在新窗口打开](http://blog.51cto.com/369369/812889)
+   [当我们谈网络时，我们谈些什么(2)--DNS的工作原理 在新窗口打开](https://segmentfault.com/a/1190000004127680)
+   [企业如何抵御利用DNS隧道的恶意软件? 在新窗口打开](https://segmentfault.com/a/1190000003876712)
+   [监控 DNS 流量，预防安全隐患五大招！在新窗口打开](https://segmentfault.com/a/1190000004332646)
+   [DNS协议详解及报文格式分析](https://pdai.tech/2017/06/18/dns-protocol-principle.html)是可以使用防火墙、网络入侵监测系统或域名解析器报告可疑的 DNS 行为迹象，作为主动防范的措施。

先讲下最常用的手段：`DNS劫持`和`DNS污染`。

### 什么是DNS劫持

DNS劫持就是通过劫持了DNS服务器，通过某些手段取得某域名的解析记录控制权，进而修改此域名的解析结果，导致对该域名的访问由原IP地址转入到修改后的指定IP，其结果就是对特定的网址不能访问或访问的是假网址，从而实现窃取资料或者破坏原有正常服务的目的。DNS劫持通过篡改DNS服务器上的数据返回给用户一个错误的查询结果来实现的。

> DNS劫持症状：在某些地区的用户在成功连接宽带后，首次打开任何页面都指向ISP提供的“电信互联星空”、“网通黄页广告”等内容页面。还有就是曾经出现过用户访问Google域名的时候出现了百度的网站。这些都属于DNS劫持。

### 什么是DNS污染

DNS污染是一种让一般用户由于得到虚假目标主机IP而不能与其通信的方法，是一种DNS缓存投毒攻击（DNS cache poisoning）。其工作方式是：由于通常的DNS查询没有任何认证机制，而且DNS查询通常基于的UDP是无连接不可靠的协议，因此DNS的查询非常容易被篡改，通过对UDP端口53上的DNS查询进行入侵检测，一经发现与关键词相匹配的请求则立即伪装成目标域名的解析服务器（NS，Name Server）给查询者返回虚假结果。

而DNS污染则是发生在用户请求的第一步上，直接从协议上对用户的DNS请求进行干扰。

**DNS污染症状**：目前一些被禁止访问的网站很多就是通过DNS污染来实现的，例如YouTube、Facebook等网站。

**解决方法**:

+   对于DNS劫持，可以采用使用国外免费公用的DNS服务器解决。例如OpenDNS（208.67.222.222）或GoogleDNS（8.8.8.8）。
+   对于DNS污染，可以说，个人用户很难单单靠设置解决，通常可以使用VPN或者域名远程解析的方法解决，但这大多需要购买付费的VPN或SSH等，也可以通过修改Hosts的方法，手动设置域名正确的IP地址。

### 为什么要DNS流量监控

预示网络中正出现可疑或恶意代码的 DNS 组合查询或流量特征。例如：

+   1.来自伪造源地址的 DNS 查询、或未授权使用且无出口过滤地址的 DNS 查询，若同时观察到异常大的 DNS 查询量或使用 TCP 而非 UDP 进行 DNS 查询，这可能表明网络内存在被感染的主机，受到了 DDoS 攻击。
    
+   2.异常 DNS 查询可能是针对域名服务器或解析器（根据目标 IP 地址确定）的漏洞攻击的标志。与此同时，这些查询也可能表明网络中有不正常运行的设备。原因可能是恶意软件或未能成功清除恶意软件。
    
+   3.在很多情况下，DNS 查询要求解析的域名如果是已知的恶意域名，或具有域名生成算法( DGA )（与非法僵尸网络有关）常见特征的域名，或者向未授权使用的解析器发送的查询，都是证明网络中存在被感染主机的有力证据。
    
+   4.DNS 响应也能显露可疑或恶意数据在网络主机间传播的迹象。例如，DNS 响应的长度或组合特征可以暴露恶意或非法行为。例如，响应消息异常巨大（放大攻击），或响应消息的 Answer Section 或 Additional Section 非常可疑（缓存污染，隐蔽通道）。
    
+   5.针对自身域名组合的 DNS 响应，如果解析至不同于你发布在授权区域中的 IP 地址，或来自未授权区域主机的域名服务器的响应，或解析为名称错误( NXDOMAIN )的对区域主机名的肯定响应，均表明域名或注册账号可能被劫持或 DNS 响应被篡改。
    
+   6.来自可疑 IP 地址的 DNS 响应，例如来自分配给宽带接入网络 IP 段的地址、非标准端口上出现的 DNS 流量，异常大量的解析至短生存时间( TTL )域名的响应消息，或异常大量的包含“ name error ”( NXDOMAIN )的响应消息，往往是主机被僵尸网络控制、运行恶意软件或被感染的表现。
    

### DNS 流量监控

> 如何借助网络入侵检测系统、流量分析和日志数据在网络防火墙上应用这些机制以检测此类威胁?

#### 防火墙

我们从最常用的安全系统开始吧，那就是防火墙。所有的防火墙都允许自定义规则以防止 IP 地址欺骗。添加一条规则，拒绝接收来自指定范围段以外的 IP 地址的 DNS 查询，从而避免域名解析器被 DDOS 攻击用作开放的反射器。

接下来，启动 DNS 流量检测功能，监测是否存在可疑的字节模式或异常 DNS 流量，以阻止域名服务器软件漏洞攻击。具备本功能的常用防火墙的介绍资料在许多网站都可以找到（例如 Palo Alto、思科、沃奇卫士等）。Sonicwall 和 Palo Alto 还可以监测并拦截特定的 DNS 隧道流量。

#### 入侵检测系统

无论你使用 Snort、Suricata 还是 OSSEC，都可以制定规则，要求系统对未授权客户的 DNS 请求发送报告。你也可以制定规则来计数或报告 NXDomain 响应、包含较小 TTL 数值记录的响应、通过 TCP 发起的 DNS 查询、对非标准端口的 DNS 查询和可疑的大规模 DNS 响应等。DNS 查询或响应信息中的任何字段、任何数值基本上都“能检测”。唯一能限制你的，就是你的想象力和对 DNS 的熟悉程度。防火墙的 IDS （入侵检测系统）对大多数常见检测项目都提供了允许和拒绝两种配置规则。

#### 流量分析工具

Wireshark 和 Bro 的实际案例都表明，被动流量分析对识别恶意软件流量很有效果。捕获并过滤客户端与解析器之间的 DNS 数据，保存为 PCAP （网络封包）文件。创建脚本程序搜索这些网络封包，以寻找你正在调查的某种可疑行为。或使用 PacketQ （最初是 DNS2DB ）对网络封包直接进行 SQL 查询。

（记住：除了自己的本地解析器之外，禁止客户使用任何其他解析器或非标准端口。）

#### DNS 被动复制

该方法涉及对解析器使用传感器以创建数据库，使之包含通过给定解析器或解析器组进行的所有 DNS 交易（查询/响应）。在分析中包含 DNS 被动数据对识别恶意软件域名有着重要作用，尤其适用于恶意软件使用由算法生成的域名的情况。将 Suricata 用做 IDS （入侵检测系统）引擎的 Palo Alto 防火墙和安全管理系统，正是结合使用被动 DNS 与 IPS （入侵防御系统）以防御已知恶意域名的安全系统范例。

#### 解析器日志记录

本地解析器的日志文件是调查 DNS 流量的最后一项，也可能是最明显的数据来源。在开启日志记录的情况下，你可以使用 Splunk 加 getwatchlist 或是 OSSEC 之类的工具收集 DNS 服务器的日志，并搜索已知恶意域名。

尽管本文提到了不少资料链接、案例分析和实际例子，但也只是涉及了众多监控 DNS 流量方法中的九牛一毛，疏漏在所难免，要想全面快捷及时有效监控 DNS 流量，不妨试试 DNS 服务器监控。

### DNS 服务器监控

应用管理器可对域名系统（ DNS ）进行全面深入的可用性和性能监控，也可监控 DNS 监控器的个别属性，比如响应时间、记录类型、可用记录、搜索字段、搜索值、搜索值状态以及搜索时间等。

DNS 中被监控的一些关键组件：

| 指标 | 描述 |
| --- | --- |
| 响应时间 | 给出 DNS 监控器的响应时间，以毫秒表示 |
| 记录类型 | 显示记录类型连接到 DNS 服务器的耗时 |
| 可用记录 | 根据可用记录类型输出 True 或 False |
| 搜索字段 | 显示用于 DNS 服务器的字段类型 |
| 搜索值 | 显示在DNS 服务器中执行的搜索值 |
| 搜索值状态 | 根据输出信息显示搜索值状态：Success (成功)或 Failed (失败) |
| 搜索时间 | DNS 服务器中的搜索执行时间 |

`监控可用性`和`响应时间`等性能统计数据。这些数据可绘制成性能图表和报表，即时可用，还可以按照可用性和完善性对报表进行分组显示。

若 DNS 服务器或系统内任何特定属性出现问题，会根据配置好的阈值生成通知和警告，并根据配置自动执行相关操作。目前，国内外 DNS 监控工具主要有 New relic、appDynamic、OneAPM。

## 参考文章

+   [【网易MC】DNS 调度原理解析 在新窗口打开](https://segmentfault.com/a/1190000010787338)
+   [DNS 原理入门 在新窗口打开](http://www.ruanyifeng.com/blog/2016/06/dns.html)
+   [DNS原理及其解析过程【精彩剖析】在新窗口打开](http://blog.51cto.com/369369/812889)
+   [当我们谈网络时，我们谈些什么(2)--DNS的工作原理 在新窗口打开](https://segmentfault.com/a/1190000004127680)
+   [企业如何抵御利用DNS隧道的恶意软件? 在新窗口打开](https://segmentfault.com/a/1190000003876712)
+   [监控 DNS 流量，预防安全隐患五大招！在新窗口打开](https://segmentfault.com/a/1190000004332646)
+   [DNS协议详解及报文格式分析](https://pdai.tech/2017/06/18/dns-protocol-principle.html)