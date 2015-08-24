title: nodetcpproxy
date: 2015-08-19 15:20:35
tags:
---
###nodetcpproxy 是在nodejs环境中运行的简易TCP转发服务器。

#####需求背景：
笔者在开发一款C/S架构的项目，C为Android移动客户端，S为Linux上C实现的服务器。
没有带公网IP的云主机，只有一台安装了VMware的笔记本。VMware中安装了Ubuntu15.04。
笔记本通过WiFi上网，因为XX原因，VMware的网络模式需要设置为NAT模式，因此Ubuntu中ifconfig得到的IP地址可能为192.168.45.133，笔记本VMware虚拟网卡地址为192.168.45.1，WiFi地址可能为10.65.195.129。
此时与笔记本在同一WiFi网络中的手机需要访问运行在笔记本虚拟机ubuntu中的服务器。
显然，手机端的服务器IP不能直接写192.168.45.133。因此笔者需要在笔记本Windows环境中运行一个TCP转发服务器。

-----

虽然TCP转发服务器有hin多，但是笔记最近了解了nodejs相关的技术，觉得其事件驱动的网络模型hin好，代码简洁，性能不俗。于是参考了官方的API，折腾出了一个nodetcpproxy.js的东东，满足开发调试需求。
>代码已经在Github开源https://github.com/uname/nodetcpproxy

#####测试：
首先要有nodejs环境，没有的话去[这里](https://nodejs.org/)下载。
首先根据实际情况设置本地和远程服务器IP/PORT，nodetcpproxy没有提供命令行参数，直接编辑位于源码开头的变量完成设置。
为验证工具是否可用，笔者为虚拟机中的sshd服务器设置个本地的代理。笔者的设置是这样的：
```javascript
var LOCAL_ADDR  = "0.0.0.0";
var LOCAL_PORT  = 8080;
var REMOTE_ADDR = "192.168.45.133";
var REMOTE_PORT = 22;
```
（往下翻看完整代码）

测试效果：
![](/images/nodetcpproxy/snapshort.png)

#####代码：

```javascript
var net = require("net");
 
var LOG_DATA    = true;
var HEX_MODE    = true;

var LOCAL_ADDR  = "0.0.0.0";
var LOCAL_PORT  = 8080;
var REMOTE_ADDR = "192.168.45.133";
var REMOTE_PORT = 22;
 
var server = net.createServer();
server.listen(LOCAL_PORT, LOCAL_ADDR);

data2print = function(data)
{
    if(!HEX_MODE) {
        return data.toString();
    }
    
    var hexStr = data.toString("hex")
    var outStr = "";
    for(var i in hexStr) {
        if(i % 2 == 0) {
            outStr += " "
        }
        if(i % 32 == 0) {
            outStr += "\n>>  ";
        }
        outStr = outStr + hexStr[i];
    }
    
    return outStr
}

logData = function(data)
{
	if(LOG_DATA) {
		console.log(data)
	}
}

server.on("connection", function(client)
{
    console.log("Client connected  " + client.remoteAddress + ":" + client.remotePort);
    var proxySocket = new net.Socket();
    proxySocket.connect(REMOTE_PORT, REMOTE_ADDR, function() {
        logData("Connected to remote -> " + REMOTE_ADDR + ":" + REMOTE_PORT);
    });
    
    proxySocket.on("close", function(data) {
        console.log("Remote closed");
        client.destroy();
    });
    
    proxySocket.on("data", function(data) {
        logData("Remote -> Proxy -> Client:\n" + data2print(data));
        client.write(data);
    });
    
    client.on("close", function(data) {
        console.log("Client closed");
        proxySocket.destroy();
    });

	client.on("error", function() {
		console.log("**Client error");
		proxySocket.destroy();
	});
    
    client.on("data", function(data) {
       logData("Client -> Proxy -> Remote:\n" + data2print(data));
       proxySocket.write(data);
    });
    
});
 
console.log("TCP proxy server started on " + LOCAL_ADDR + ":" + LOCAL_PORT);

```