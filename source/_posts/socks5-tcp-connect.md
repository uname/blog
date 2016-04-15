title: SOCKS5代理TCP CONNECT实验
date: 2016-04-15 14:37:41
tags:
---
######背景知识######
[RFC1928](https://www.ietf.org/rfc/rfc1928.txt)
[WiKi](https://zh.wikipedia.org/wiki/SOCKS)

######实验目的######
了解使用Socks代理进行TCP连接的过程。

######实验环境######
ubuntu

######实验步骤######
1. 安装sSocks(Socks5 Sevrer)
wget http://pilotfiber.dl.sourceforge.net/project/ssocks/ssocks-0.0.14.tar.gz
tar zxvf ssocks-0.0.14.tar.gz
cd ssocks-0.0.14
./configure
make

2. 安装nmap（我们用该软件包中的ncat创建一个TCP的echo server，作为要用代理连接的目标服务）
sudo apt-get install nmap
安装完成后使用命令:
ncat -l 8082 --keep-open --exec "/bin/cat"
测试echo server是否OK
<img src="/images/socks5-tcp-connect/socks5-1.png" width="80%" />

3. 启动sSocks server（无认证模式）和echo server
客户机连接到服务器，先发送一个版本标识/方法选择报文:
<img src="/images/socks5-tcp-connect/socks5-2.png" />
VER: X'05' SOCKS协议版本号
NMETHODS: METHODS的个数，我们使用X'01'
METHOD:
当前被定义的的值有：
　　>> X'00' 无验证需求
　　>> X'01' 通用安全服务应用程序接口(GSSAPI)
　　>> X'02' 用户名/密码(USERNAME/PASSWORD)
　　>> X'03' 至 X'7F' IANA 分配(IANA ASSIGNED) 
　　>> X'80' 至 X'FE' 私人方法保留(RESERVED FOR PRIVATE METHODS)
　　>> X'FF' 无可接受方法(NO ACCEPTABLE METHODS)
这里我们使用X'00'
我们发送十六进制数据: 05 01 00
Socks5服务器应答报文：
<img src="/images/socks5-tcp-connect/socks5-3.png" />
因为我们使用了无认证模式，因此正确情况下服务器应答：05 00
<img src="/images/socks5-tcp-connect/socks5-4.png" width="80%" />

4. 客户机发送代理需求报文
报文格式：
<img src="/images/socks5-tcp-connect/socks5-5.png" />
其中：
VER protocol version：X'05'
CMD
　CONNECT X'01'
　BIND X'02'
　UDP ASSOCIATE X'03'
RSV RESERVED
ATYP address type of following address
　IP V4 address: X'01'
　DOMAINNAME: X'03'
　IP V6 address: X'04'
DST.ADDR desired destination address
DST.PORT desired destination port in network octet order

本次实验，我们测试TCP CONNECT命令，请求代理服务器连接到我们使用ncat启动的TCO echo server。
发送十六进制数据：05 01 00 01 73 9f 9d 7e 1f 92
正常情况下，我们会收到服务器的应答报文，格式如下：
<img src="/images/socks5-tcp-connect/socks5-6.png" />
其中：
VER protocol version: X'05'
REP Reply field:
　　X'00' succeeded
　　X'01' general SOCKS server failure
　　X'02' connection not allowed by ruleset
　　X'03' Network unreachable
　　X'04' Host unreachable
　　X'05' Connection refused
　　X'06' TTL expired
　　X'07' Command not supported
　　X'08' Address type not supported
　　X'09' to X'FF' unassigned
RSV RESERVED
ATYP address type of following address
　　IP V4 address: X'01'
　　DOMAINNAME: X'03'
　　IP V6 address: X'04'
BND.ADDR server bound address
BND.PORT server bound port in network octet order
标志RESERVED(RSV)的地方必须设置为X'00'。

代理完成后，就可以发送一些数据测试下，是否能收到echo server的响应了：
<img src="/images/socks5-tcp-connect/socks5-7.png" />
