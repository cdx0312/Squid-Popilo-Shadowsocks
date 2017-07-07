# Squid-Popilo-Shadowsocks
##使用Shadowsocks+Squid+Popilo搭建代理服务器，
##其中Shadowsocks完成服务器主机的翻墙，但是 其只支持Socks5，Popilo将其转化为HTTP协议，Squid实现代理功能

###1、Shadowsocks搭建：
```
sudo apt-get install python-pip   #安装pip
sudo pip install shadowsocks 
```
新建并修改config文件
运行```   ```
后台运行命令 ：``` ```

###2、安装polipo
```sudo apt-get install polipo```
配置polipo:  /etc/polipo/config,:
``` ```
开启polipo服务
###3、squid3
安装： 
配置：/etc/squid3/squid.conf
重启： `service squid3 restart`
