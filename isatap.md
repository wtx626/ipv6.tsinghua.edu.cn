---
layout: default 
title: IPv6 ISATAP 配置说明
active_nav: isatap
permalink: /isatap/
---

## IPv6 ISATAP配置說明

###  什么是ISATAP隧道?

ISATAP全名是 Intra-Site Automatic Tunnel Addressing Protocol，是一种IPv6隧道技术，使用户可以在IPv4网络上访问IPv6资源。具体技术原理参见（draft-ietf-ngtrans-ISATAP-23.txt）。

### 清华大学ISATAP隧道信息

- 清华大学 ISATAP隧道路由器的IPv4地址是：`isatap.tsinghua.edu.cn`
- 用户设置 ISATAP隧道的接入点为：`isatap.tsinghua.edu.cn`
- 清华大学 ISATAP 隧道IPv6地址前缀为：`2402:f000:1:1501::/64`

### 清华大学ISATAP隧道配置方法

#### Windows环境（以win7为例）

以管理员身份运行cmd命令，进入命令行模式C:\>

```
输入 netsh
输入 int ipv6 isatap（说明：进入isatap配置模式）
输入 set router isatap.tsinghua.edu.cn
输入 set state enable（说明：激活isatap隧道）
输入 exit（说明：退出netsh）
```

右键点击桌面“计算机”图标，选择“管理”，展开“服务和应用程序”，选择“服务”，确认“ip helper”服务已开启。

此后，通过 `ipconfig` 应该可以看到一个 `2402:f000:1:1501:` 为前缀的v6地址，hostid为`0:5efe:x.x.x.x`， 其中`x.x.x.x` 为您的真实的IPv4地址，即可访问IPv6资源。

#### Linux 环境

Linux内核版本在2.2.0以后通常支持IPv6，请查看是否存在/proc/net/if_inet6文件，以确定您的系统是否支持IPv6，如果该文件不存在，可尝试如下命令加载IPv6模块：

```
# modprobe ipv6
```

成功加载后就可以配置IPv6了：

```
# ifconfig  eth0  inet6 add IPV6ADDR  (IPV6ADDR为要临时设备的IPv6地址)
# route -A inet6  add default  gw  IPV6GATEWAY dev ethX  (为网络设备ethX添加IPv6网关IPV6GATEWAY地址)
# ping6 ipv6.tsinghua.edu.cn
```

如果Fedora9换成了2.6.25kernel都是支持ISATAP方式的ipv6tunnel接入的。配置方法如下：


 1.首先要保证 kernel 支持ipv6

 2.接着编辑 `/etc/sysconfig/network` ，增加下面这行

```
IPV6_DEFAULTGW=youripv6gateway
```

 3.然后再编辑`/etc/sysconfig/network-scripts/ifcfg-sit1`, 内容如下：

```
DEVICE=sit1
ONBOOT=yes
IPV6INIT=yes
IPV6TUNNELIPV4=yourisataptunnelIP
IPV6TUNNELIPV4LOCAL=yourlocalipv4ip
IPV6ADDR=youripv6address
```

 4.最后是ifupsit1

需注意，ifup-sit不会创建对应的sit1设备，先得手动创建以后才有效的。

这样 ISATAP就配置好了！

#### Mac OSX环境

1. 下载ISATAP client for Mac OS X
地址：http://www.momose.org/macosx/isatap.html
2. 解压ISATAP client
% cd /usr/local
% sudo tar xfz ~/Downloads/macosx-isatap-*.tar.gz
3. 更改权限
% sudo chown -R root:wheel /usr/local/isatap
% sudo chmod -R 644 /usr/local/isatap/isatap.kext
4. 配置ISATAP

配置ist0和得到IPv4地址（你需要制定现在使用的网卡，比如en0）
注：config-ist.sh有一行需要更改以适应清华ipv6，将第50行改为：

```
${ifconfig} ist0 inet6 2001:da8:200:900e:0:5efe:${v4addr} prefixlen 64
```

然后再执行：

```
% sudo ./config-ist.sh en0
```

指定ISATAP router

```
% sudo ./ifconfig ist0 isataprtr 59.66.4.50
% sudo ./rtsold.sh &
```

设置路由表

```
% sudo route delete -inet6 default
注：在执行上面命令之前可以用netstat -r查看ipv6路由表上是否有default这一项，没有则不用执行上面命令
% sudo route add -inet6 default -interface ist0
```

启动IPv6

```
% sudo ifconfig ist0 up
```

关闭IPv6

```
% sudo ifconfig ist0 down
```

这样 ISATAP就配置好了！ 
