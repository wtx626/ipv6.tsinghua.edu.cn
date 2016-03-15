---
layout: default 
title: 首页
active_nav: home
permalink: /
---

## 什么是 IPv6

IPv6指互联网协议（IP）第6版。目前大家上网主要使用互联网协议第四版，即IPv4。
在全球互联网高度发展的今天，IPv4 地址资源已经枯竭，互联网正在经历从IPv4网络向IPv6网络的过渡。
IPv4地址是类似 `A.B.C.D` 的格式，共32位，用 `.` 分成四段，用10进制表示；
而IPv6地址类似`X:X:X:X:X:X:X:X`的格式，它是128位的，用`:`分成8段，用16进制表示。
[RFC2373](http://www.ietf.org/rfc/rfc2373.txt) 中详细定义了IPv6地址，按照定义，一个完整的IPv6地址的表示法：`xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx:xxxx`。


## IPv4地址和IPv6地址的区别 
  
<table border="1" cellspacing="0" cellpadding="0">
	<tr>
		<td ><p align="left"><strong>IPv4</strong><strong>地址</strong><strong> </strong> </p></td>
		<td ><p align="left"><strong>IPv6</strong><strong>地址</strong><strong> </strong> </p></td>
	</tr>
	<tr>
		<td ><p align="left">组播地址（224.0.0.0/4） </p></td>
		<td ><p align="left">IPv6组播地址（FF00::/8） </p></td>
	</tr>
	<tr>
		<td ><p align="left">广播地址 </p></td>
		<td ><p align="left">无，只有任播（ anycast）地址 </p></td>
	</tr>
	<tr>
		<td ><p align="left">未指定地址为 0.0.0 .0 </p></td>
		<td ><p align="left">未指定地址为 :: </p></td>
	</tr>
	<tr>
		<td ><p align="left">回路地址为 127.0.0.1 </p></td>
		<td ><p align="left">回路地址为 ::1 </p></td>
	</tr>
	<tr>
		<td ><p align="left">公用 IP地址 </p></td>
		<td ><p align="left">可汇聚全球单播地址 </p></td>
	</tr>
	<tr>
		<td ><p align="left">私有地址（10.0.0.0/8、172.16.0.0/12、192.168.0.0/16） </p></td>
		<td ><p align="left">本地站点地址（ FEC0::/48） </p></td>
	</tr>
	<tr>
		<td ><p align="left">Microsoft自动专用IP寻址自动配置的地址（169.254.0.0/16） </p></td>
		<td ><p align="left">本地链路地址（ FE80::/64） </p></td>
	</tr>
	<tr>
		<td ><p align="left">表达方式：点分间隔十进制 </p></td>
		<td ><p align="left">表达方式：冒号间隔十六进制式 </p></td>
	</tr>
	<tr>
		<td ><p align="left">子网掩码表示：以点阵十进制表示法或前缀长度表示法（CIDR） </p></td>
		<td ><p align="left">子网掩码表示：仅使用前缀长度表示法（CIDR） </p></td>
	</tr>
</table>


## 清华哪些区域有IPv6网络服务？ 

清华大学校园网主干网、学生宿舍网、教师宿舍网已经全部支持 IPv6，各院系自建的局域网也基本都支持IPv6。如所在局域网不支持IPv6，也可以通过清华大学的ISATAP隧道服务访问IPv6资源。

## 如何使用IPv6?

Win7以上的操作系统和MAC系统都自带安装了IPv6协议栈，而且默认都是自动获取IPv6地址，不需要任何设置。 

## 为什么访问IPv6还会产生IPv4流量？ 

我们访问的IPv6网站往往都是IPv4和IPv6双栈运行的。大多数用户使用IPv6的时候跑IPv4流量都是用μTorrent下载IPv6分享站点的资源，然后在设置的时候没有通过启用IPfilter规则来屏蔽IPv4流量，并且没有断开IPv4的认证。因此，如果想访问纯IPv6资源，希望避免IPv4的流量，可以直接断开IPv4的认证或者索性禁用IPv4的协议，如果是用μTorrent下载东西，建议启用μTorrent的IPfilter规则屏蔽IPv4流量，主流的BT站点都有相关教程。

## 有哪些常用的IPv6资源？ 

 - 清华大学 TUNA 镜像站:  <https://mirrors.tuna.tsinghua.edu.cn/>
 - CNGI高校驻地网: <http://www.6rank.edu.cn>
 - 北京邮电大学(北邮人BT): <http://bt.byr.cn>
 - 北京交通大学(晨光BT): <http://ipv6.cgbt.cn>
 - 东北大学(六维空间): <http://bt.neu6.edu.cn>
 - 上海大学(乐乎论坛): <http://bt.shu6.edu.cn/index.php>


## 如何确认本机获取方式为自动获得IP地址？ 

控制面板→网络和Internet→打开“网络和共享中心”→更改适配器设置→双击“本地连接”→属性→单击“Internet协议版本6（TCP/IPv6）”→确认IP地址和DNS服务器地址都是自动获取→确定→关闭→关闭。

![](/static/img/image1.jpg)
![](/static/img/image2.jpg)

## 如何查看本机IPv6地址获取情况？

直接在开始菜单里所有程序中选择命令提示符cmd.exe或者再下方搜索框里输入cmd，然后回车，进入命令提示符窗口。

![](/static/img/image3.jpg)

输入`ipconfig /all`命令，可以查看本机是否获取到正确的IPv6地址（`2402:f000`开头），由于我校IPv6的DNS服务器搭建在双栈链路之上，无需专门指定IPv6的DNS服务器参数，沿用IPv4的DNS服务器设置即可，通常为自动获取。


## 如何报告IPv6问题？ 

请联系校园网用户服务部门，并提供如下信息，以便于定位问题。

1. 上网位置、上网帐号、联系方式，本机IPv6地址，命令提示符下分别输入`ipconfig /all`的结果，输入`arp -a`的结果。
2. 故障时间，故障现象，系统版本，网卡型号。周围同环境下其他人能否使用ipv6，把个人电脑换到其他端口上，重新获取ipv6地址后，现象是否重现。
3. `ping -n 30 ipv6.tsinghua.edu.cn`、`tracert www.edu.cn` 或其他IPv6网站的结果。

