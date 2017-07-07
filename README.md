# Squid-Polipo-Shadowsocks
##使用Shadowsocks+Squid+Popilo搭建代理服务器，
##其中Shadowsocks完成服务器主机的翻墙，但是 其只支持Socks5，Popilo将其转化为HTTP协议，Squid实现代理功能

###1、Shadowsocks搭建：
```
sudo apt-get install python-pip   #安装pip
sudo pip install shadowsocks 
```
新建并修改config文件  ~/ss/*.conf
```
{
	"server":"remote-shadowsocks-server-ip-addr",
	"server_port":443,
	"local_address":"127.0.0.1",
	"local_port":1080,
	"password":"your-passwd",
	"timeout":300,
	"method":"chacha20-ietf",
	"fast_open":false,
	"workers":1
}
```

运行```sslocal -c ~/ss/*.conf```
后台运行命令 ：```nohup sslocal -c ~/ss/*.conf > /dev/null 2>&1 & ```

###2、安装polipo

```sudo apt-get install polipo```

配置polipo:  /etc/polipo/config,:

```
# This file only needs to list configuration variables that deviate
# from the default values.  See /usr/share/doc/polipo/examples/config.sample
# and "polipo -v" for variables you can tweak and further information.
logSyslog = true
logFile = /var/log/polipo/polipo.log
proxyAddress = "0.0.0.0"
proxyPort = 8123 #将要使用的转换成http的端口
socksParentProxy = "0.0.0.0:1080"  #这里的端口为上级socks5代理的端口
socksProxyType = socks5
```

开启polipo服务
```sudo service polipo restart```
###3、squid3
安装： sudo apt-get install squid3
配置：/etc/squid3/squid.conf
cache_peer 127.0.0.1 parent 8123 0 no-query no-digest round-robin weight=1 name=shadowsocks // important
http_access allow all
never_direct allow all   //which wasted my time 
重启： `service squid3 restart`
