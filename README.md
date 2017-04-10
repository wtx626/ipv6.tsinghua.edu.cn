## IPv6

http://ipv6.tsinghua.edu.cn/ 主页
---
layout: default 
title: IPv6 ISATAP 配置说明
active_nav: isatap
permalink: /isatap/
---

##  什么是ISATAP隧道?

ISATAP全名是 Intra-Site Automatic Tunnel Addressing Protocol，是一种IPv6隧道技术，使用户可以在IPv4网络上访问IPv6资源。具体技术原理参见（draft-ietf-ngtrans-ISATAP-23.txt）。

## 清华大学ISATAP隧道信息

- 清华大学 ISATAP隧道路由器的IPv4地址是：`isatap.tsinghua.edu.cn`
- 用户设置 ISATAP隧道的接入点为：`isatap.tsinghua.edu.cn`
- 清华大学 ISATAP 隧道IPv6地址前缀为：`2402:f000:1:1501::/64`

## 清华大学ISATAP隧道配置方法

### Windows 环境（Windows 7及以上系统适用）

以管理员身份运行cmd命令，进入命令行模式，输入如下命令

```
netsh int ipv6 isatap set router isatap.tsinghua.edu.cn
netsh int ipv6 isatap set state enable
```

以上两条命令分别为设定ISATAP路由器和启用ISATAP隧道。

此后，通过 `ipconfig` 应该可以看到一个 `2402:f000:1:1501:` 为前缀的v6地址，hostid为`200:5efe:x.x.x.x`， 其中`x.x.x.x` 为您的真实的IPv4地址，即可访问IPv6资源。

以下操作为非必须。如果按照上述提示操作以后仍无法正常访问IPv6站点，可以尝试：

+ 右键点击桌面“计算机”图标，选择“管理”，展开“服务和应用程序”，选择“服务”，确认“IP Helper”服务已开启；
+ 确认Teredo隧道已经关闭（管理员模式在命令行运行`netsh int teredo set state disable`）；
+ 确认原生IPv6已经关闭（`Internet 协议版本 6 (TCP/IPv6)`前的对勾取消，位置在`控制面板`→`网络和Internet`→`网络和共享中心`→`更改适配器设置`→双击`本地连接`→`属性`）；
+ 尝试重启系统。

### Linux 环境

Linux内核版本在 2.2.0 以后通常支持IPv6，请查看是否存在 `/proc/net/if_inet6` 文件，以确定您的系统是否支持IPv6，如果该文件不存在，可尝试如下命令加载IPv6模块：

```
sudo modprobe ipv6
```

成功加载后就可以配置IPv6了：

编辑 `/usr/local/bin/thu6tunnel.sh`，并加入以下内容

```bash
#!/bin/bash
REMOTE_IP6="2402:f000:1:1501:200:5efe"
REMOTE_IP4="166.111.21.1"

IFACE4=`ip route show|grep default|sed -e 's/^default.*dev \([^ ]\+\).*$/\1/'`
IP4=`ip addr show dev $IFACE4 | grep -m 1 'inet\ ' | sed -e 's/^.*inet \([^ \\]\+\)\/.*$/\1/'`

sudo ip tunnel del sit1  # 删除已经创建的设备，若没有则忽略
sudo ip tunnel add sit1 mode sit remote $REMOTE_IP4 local $IP4
sudo ip link set dev sit1 up
sudo ip -6 addr add $REMOTE_IP6:$IP4/64 dev sit1
sudo ip -6 route add default via $REMOTE_IP6:$REMOTE_IP4 dev sit1
```

更改权限

```
sudo chmod +x /usr/local/bin/thu6tunnel.sh
```

之后执行 `thu6tunnel.sh` 即可。

也可以单独执行以下命令:

```bash
REMOTE_IP6="2402:f000:1:1501:200:5efe"
REMOTE_IP4="166.111.21.1"
IP4="你的IPv4地址"  # 前三行不能有空格
sudo ip tunnel del sit1  # 删除已经创建的设备，若没有则忽略
sudo ip tunnel add sit1 mode sit remote $REMOTE_IP4 local $IP4
sudo ip link set dev sit1 up
sudo ip -6 addr add $REMOTE_IP6:$IP4/64 dev sit1
sudo ip -6 route add default via $REMOTE_IP6:$REMOTE_IP4 dev sit1
```

关闭IPv6

```
sudo ip link set sit1 down
sudo ip tunnel del sit1
```

### Mac OS X环境

编写脚本 `/usr/local/bin/thu6tunnel.sh`，加入以下内容

```bash
#!/bin/sh 
#清除IPV6路由表 
route delete -inet6 default  
ifconfig gif0 destroy
EN0_IP=`ifconfig en0 | grep inet | grep -v inet6 | awk '{print $2}'` 
EN1_IP=`ifconfig en1 | grep inet | grep -v inet6 | awk '{print $2}'`  
if [ -n “$EN0_IP” ]; then 
    LOCAL_IP=$EN0_IP 
else 
    LOCAL_IP=$EN1_IP 
fi  
if [ -n "$LOCAL_IP" ]; then 
    ifconfig gif0 create
    ifconfig gif0 tunnel $LOCAL_IP 166.111.21.1 
    ipconfig set gif0 MANUAL-V6 2402:f000:1:1501:200:5efe:$LOCAL_IP 64
    route add -inet6 ::/0 -interface gif0
fi
```

设置权限

```
sudo chmod +x /usr/local/bin/thu6tunnel.sh
```

用root权限运行脚本

```
sudo /usr/local/bin/thu6tunnel.sh
```

或者，打开终端，单独输入以下命令:

```shell
IP4="我的IPv4地址"  # 这里不能有空格
sudo ifconfig gif0 create
sudo ifconfig gif0 tunnel $IP4 166.111.21.1
sudo ipconfig set gif0 MANUAL-V6 2402:f000:1:1501:200:5efe:$IP4 64
sudo route add -inet6 ::/0 -interface gif0
```

这样 ISATAP就配置好了！ 

关闭IPv6

```
sudo ifconfig gif0 destroy
```

**注意，OS X 中 safari 对于 ISATAP 的 IPv6 接入不友好，仍然会打开 IPv4 地址。**
请通过

```
ping6 ipv6.tsinghua.edu.cn
```

验证接入。
