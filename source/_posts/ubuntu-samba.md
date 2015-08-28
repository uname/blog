title: 在ubuntu中配置samba
date: 2015-08-28 23:56:05
tags:
---

在虚拟机中安装了一个Ubuntu，用samba访问资源是非常方便的，之前在我的树莓派2中也安装了samba。这里备注下。

1. 安装
```bash
sudo apt-get install samba
```
安装完成后samba的配置目录在/etc/samba下。
![](/images/ubuntu-samba/1.png)
2. 添加用户
添加当前用户，需root权限。我的用户名是apache，所以我会这么写：
```bash
sudo smbpasswd -a apache
```
设置密码，完成添加。
![](/images/ubuntu-samba/2.png)
3. 为添加的用户书写配置
编辑/etc/samba/smb.conf，我在自己的虚拟机上使用，只需在smb.conf的末尾添加如下配置即可：
```bash
[apache]
   comment = Apache's Home
   path = /home/apache
   browseable = yes
   read only = no
   guest ok = no
   create mask = 0600
```
4. 重启samba，完成配置
```bash
sudo /etc/init.d/samba restart
```
![](/images/ubuntu-samba/3.png)


现在Windows下敲击Win+R，输入\\ubuntu-ip\apache
可以愉快的从windows资源管理器访问ubuntu下apache用户主目录下的文件了：）

