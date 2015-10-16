[PAC](http://www.rpsofts.com/vvv)
=======
本项目主要介绍如何利用国外VPS搭建多协议代理服务。

感谢http://bbs.itzmx.com/thread-8815-1-1.html  及 http://www.rpsofts.com/vvv  本文基于itzmx修改。

# PAC地址
主：https://rplog.qiniudn.com/o_1a1bdhi212lroks75o19m3aha.pac
备：https://rplog.qiniudn.com/o_1a1bbteeq4q34427ev2ndfra.pac
# 使用方法
先介绍一下设置各个系统代理的方法。
## Windows
Internet选项 -> 连接 选项卡 -> 局域网设置(如果是电脑拨号上网, 这里点'设置') -> 
使用自动配置脚本 -> 填入PAC地址 -> 确定

![](http://rplog.qiniudn.com/o_1a1begcb49dn1f151fih19se1jsla.jpg)
## Mac OS X
系统设置 -> 网络 -> 高级 -> 代理 -> 自动代理配置 -> URL中填入PAC地址 -> 好

## IOS (IPhone/IPad)
打开设置, 选择Wi-Fi
选择当前使用的热点
![](http://i3.tietuku.com/2204fd1ef2747bd4.png)
拖到最下面的代理设置，选择"自动"，填写PAC地址 
![](http://i3.tietuku.com/a5a70098da4ea3f0.png)
## Android
安卓5.0以上版本 使用方法同苹果，早期版本需要使用第三方软件

自行搭建代理服务器
==============
在 25 端口搭建 http/https 代理。


Ubuntu（需要一行一行复制安装）:
-------
	apt-get -y install squid
	curl http://www.rpsofts.com/vvv/squid/ubuntu-squid.conf  > /etc/squid3/squid.conf
	mkdir -p /var/cache/squid
	chmod -R 777 /var/cache/squid
	service squid3 stop
	squid3 -z
	service squid3 restart


CentOS 6.7 x64（推荐用此系统）:
-------
	setenforce 0
	ulimit -n 1048576
	echo "* soft nofile 1048576" >> /etc/security/limits.conf
	echo "* hard nofile 1048576" >> /etc/security/limits.conf
	echo "alias net-pf-10 off" >> /etc/modprobe.d/dist.conf
	echo "alias ipv6 off" >> /etc/modprobe.d/dist.conf
	killall sendmail
	/etc/init.d/postfix stop
	chkconfig --level 2345 postfix off
	yum -y install squid
	wget -O /etc/squid/squid.conf http://www.rpsofts.com/vvv/squid/centos-squid.conf
	mkdir -p /var/cache/squid
	chmod -R 777 /var/cache/squid
	squid -z
	service squid restart
	chkconfig --level 2345 squid on


装完后记得reboot重启下服务器确保生效。

然后使用[ PAC](https://rplog.qiniudn.com/o_1a1bdhi212lroks75o19m3aha.pac " PAC")右键另存为 PAC 文件后修改其中的IP地址为你的服务器IP即可。

注意服务器DNS修改成8.8.8.8（配置文件目前强制指定了DNS，可以无需修改）
