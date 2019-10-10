# `Linux` 和 `MacOS` 设备智能分流方案

<div align=center><img width="700" src="img/punch.gif"/></div>



The 360 degree painless patriotic routing scheme has emerged in endlessly, but we have to face a very real problem: 

** You can't go anywhere with a router! **

To solve this problem, this tutorial was born. The goal is to teach you how to automatically cross the Great Wall 360 degrees without dead corners on different operating systems. Clap:

PS: At present, it can only be implemented on Linux and MacOS systems. Windows
Users please 滚！嘻嘻嘻 😁

## Characteristics of Patriotic Program in this tutorial
Give up the blacklist scheme. The number of websites on the wall is increasing dramatically every day. Limited life can not be spent in endless manual blacklisting and restarting devices.

Avenue to Jane, once and for all!
+ Establish a list of important websites in China and conduct DNS inquiries in China
+ Other websites query DNS from `shadowsocks'client to `shadowsocks' server
+ Domestic or Asian IP traffic flows through domestic channels
+ Other traffic is forwarded through `shadowsocks'server

## Knowledge is meaningless if it is not shared.
What is a saint? A saint is a person who gets and pays in a balanced way. Heaven and earth gave birth to me, and I respect heaven and earth; my parents raised me, and I also adopt parents; to acquire knowledge on the Internet, we should also share knowledge on the Internet. So it took many days to search for information, write tutorials, debug firmware, and before I knew it, a day passed.
It feels good to be free: `youtube', `hulu', `twitter', `facebook', `google'...

This document does not cover the principles and basic configurations of `shadowsocks'. If you don't even grasp these basic knowledge, please turn left and take a detour. Learn these basic knowledge and then come back to this tutorial.

Welcome to mention `Issues'to participate in maintenance projects.
<br/>
> There are two ways to achieve global intelligent shunting, one is through the firewall strategy, the other is through the policy routing table.

## Realization through Firewall Policy
There are many kinds of firewall tools. I have tried and successfully implemented two kinds of functions, one is `iptables', the other is `nftables'. ` iptables should be familiar to everyone. nftables may be unfamiliar to most people. If you want to know more, please refer to [the reason why Linux first introduced nftables, you might like nftables] (http://blog.jobbole.com/59624/).

** Unfortunately, this solution is not applicable to the MacOS system. If you have any good suggestions, please give me some help. **

### 1. Intelligent shunting through iptables
The idea of this scheme is to use `ipset'to load the IP list of chnroute and `iptables' to realize the global proxy with automatic diversion of domestic and foreign traffic.

+ [Linux 系统](./docs/iptables-linux.md)
+ MacOS 系统：暂无实现，与之类似的方案请参考 [一个基于 VirtualBox 和 openwrt 构建的项目, 旨在实现 macOS / Windows 平台的透明代理](https://github.com/icymind/VRouter)

### 2. Adopt nftables Realizing Intelligent Divergence

+ [Linux System](./docs/nftables-linux.md)
+ MacOS System: Not yet implemented

## Implementation by Policy Routing Table

The general idea of this scheme is to start a local `socks'proxy, then disguise `socks' proxy as `vpn'through tools, and then distribute it through policy routing.

There are two tools to disguise `socks'agents as `vpn'.
### 1. Intelligent shunting through badvpn
This paper mainly introduces `tun2socks', which is actually a part of `badvpn'.

` Tun2socks `implements a mechanism that allows you to encapsulate data in a completely transparent `socks'protocol without changing any application, forward it to a `socks' agent, which is then responsible for forwarding application data between the agent and the real server.

There are two ways to use proxy. One is to configure proxy explicitly by yourself. In this way, when data leaves your host, its target address is the proxy server.

The other is to act as a transparent proxy, that is, to redirect the original data to an application in the middle and forward it by the proxy agent.

` Tun2socks `completes the encapsulation of the `socks'protocol on the basis of the second one, and uses a powerful `tun' network card to implement the mechanism without having to configure complex `iptables'rules.

+ [Linux 系统](./docs/badvpn-linux.md)
+ MacOS 系统：暂时无法编译成功，如有人编译成功，望告知

### 2. 通过 gotun2socks 实现智能分流

` gotun2socks is actually the `go'language implementation of `badvpn', and is more intelligent. It will automatically add tuntap network card to you at startup and automatically delete the network card at stop, without us adding and deleting it manually. How about a little excitement? 

Have you fallen in love with go language ever since? 

+ [Linux 系统](./docs/gotun2socks-linux.md)
+ [MacOS 系统](./docs/gotun2socks-macos.md)

## Outside Chapter
Although all these fancy patriotic schemes can achieve global intelligence diversion, they are still too complex and daunting for most people. Most people don't have a strong need for global intelligent shunting, just let some special applications use proxies. Some applications allow you to choose to use proxies, but many applications do not provide this part of the configuration at all. Now, in order to let some software traffic that the original logic does not consider/do not use/cannot configure the agent go through the agent, the system calls can only be hijacked by hook.


Using [Proxifier] (https://www.proxifier.com/) to achieve this function, with the help of Proxifier, you can easily play traffic forwarding through simple configuration even if you don't understand any network principles. Compared with VPN (Virtual Private Network) global proxy, Proxifier's flexible configuration can also achieve some unexpected functions, such as monitoring the traffic of an application or shielding advertisements. Of course, how you use it ultimately depends entirely on your imagination.

In order to better use Proxifier, we use the following schematic diagram to understand the working principle of Proxifier:
1. Proxifier takes over all network request connections in the system after starting.
2. After taking over, network requests are processed by Proxifier configuration rules.
3. Direct (direct) direct access to the external network; Proxy (proxy) handed over the request to the proxy server for processing before connecting to the external network; Block (prohibition) intercepted outgoing requests.

<div align=center><img src="https://ws3.sinaimg.cn/large/006tNc79gy1fz0edczefvj30dp06xdg8.jpg"/></div>
It needs to be pointed out that Proxifier is charged, which is about tens of dollars. We'd better support the genuine version. I also provide a MacOS cracked version: [Proxifier_2.22.1_xclient.info.dmg] (https://www.lanzous.com/i2tv3je). The decompressed password is `xclient.info', and the key is in the decompressed text. The following usage tutorials are for MacOS users, similar to Windows platforms.

### 使用教程

接下来配置的三步顺序：

+ 代理服务器配置
+ 代理规则设置
+ 域名解析设置

① 打开软件点击 Proxies：

<div align=center><img width="700" src="https://ws1.sinaimg.cn/large/006tNc79gy1fz0f3d99hwj316a0u04d2.jpg"/></div>

+ 点击 “Add”
+ 输入本地 shadowshocks 的 ip（默认127.0.0.1）和端口（默认1080）
+ 选择 `SOCKS Versin 5`
+ OK

<div align=center><img width="700" src="https://ws1.sinaimg.cn/large/006tNc79gy1fz0faoblsej30u00vdjvs.jpg"/></div>

**接下来的两步配置至关重要，配置错误可能导致代理失败或者循环代理！**

② 配置第二步

+ 点击 Rules
+ 选中 localhost,点击 Edit
+ Target hosts 处添加 shadowshocks 代理服务器的 IP 地址（以 123.123.123.123 示例）
+ Action选择Direct(直连)
+ OK

<div align=center><img width="700" src="https://ws1.sinaimg.cn/large/006tNc79gy1fz0feaqxvcj30t60tuadn.jpg"/></div>

** Note: This configuration step allows packets sent to the proxy server to pass through to prevent looping proxy errors. **

The configuration is as follows:

<div align=center><img width="700" src="https://ws4.sinaimg.cn/large/006tNc79gy1fz0fhuwiqoj316u03a3zs.jpg"/></div>

(3) Configuration step 3
+ Click DNS
+ Select the second Resolve hostnames through proxy (domain name resolution via proxy server)
+ OK

<div align=center><img width="700" src="https://ws2.sinaimg.cn/large/006tNc79gy1fz0fnrzenij30vq0qggoz.jpg"/></div>

** If you have configured a non-polluting DNS, you can choose Detect DNS settings automatically and use the default DNS. **
+ OK

So far, the agent has been configured. Next, I will give some examples of specific usage scenarios. 

The most commonly used proxy tool in daily work is `git'. In order to make git use proxy compulsively, only one proxy rule needs to be created in Proxifier:
+ Click Rules
+ Click Add
+ Name field fills in Git
+ Fill in `git-remote-https'in the Applications field`
+ Action Selects Proxy SOCKS 5 127.0.0.1:1080

< div align = center > < img width = "700" SRC = "https://ws4.sinaimg.cn/large/006tNc79gy1fz0g2zxu4oj30t60tu41s.jpg"/> </div>

If you don't know what to write in the Applications field, I can teach you a way to find the process of using the agent in Git clone by following commands:

```bash
$ sudo ps -ef|grep git

  501  5623     1   0  2Dec18 ??         0:00.89 /Applications/Atom.app/Contents/Frameworks/Squirrel.framework/Resources/ShipIt com.github.atom.ShipIt /Users/yangcs/Library/Caches/com.github.atom.ShipIt/ShipItState.plist
  501 77481 92668   0  5:14PM ttys002    0:00.00 grep --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn git
  501 77184 62902   0  5:14PM ttys003    0:00.07 git clone https://github.com/kubernetes/kubernetes
  501 77185 77184   0  5:14PM ttys003    0:01.58 /usr/local/Cellar/git/2.18.0/libexec/git-core/git-remote-https origin https://github.com/kubernetes/kubernetes
  501 77189 77185   0  5:14PM ttys003    0:00.39 /usr/local/Cellar/git/2.18.0/libexec/git-core/git fetch-pack --stateless-rpc --stdin --lock-pack --thin --check-self-contained-and-connected --cloning https://github.com/kubernetes/kubernetes/
  501 77190 77189   0  5:14PM ttys003    0:01.52 /usr/local/Cellar/git/2.18.0/libexec/git-core/git index-pack --stdin -v --fix-thin --keep=fetch-pack 77189 on MacBookPro --check-self-contained-and-connected --pack_header=2,877904
```

很明显，`git-remote-https` 就是我们想找的进程，如果你还不放心，可以将 `git` 也加入 Applications 字段。

<div align=center><img width="700" src="https://ws1.sinaimg.cn/large/006tNc79gy1fz0gesufy3j30t60tugox.jpg"/></div>

现在如果你通过 `git clone` 来拉取仓库，就可以看到详细的连接统计信息：

<div align=center><img width="700" src="https://ws2.sinaimg.cn/large/006tNc79gy1fz0gjpd5pqj318b0u0ai6.jpg"/></div>

另外一个典型的使用场景就是 Docker。配置方法和 git 类似，我就不演示了，重点提醒一下 Applications 字段值是 `com.docker.vpnkit`。如果你不放心，可以使用通配符 `*docker*`。Target Hosts 字段填入 `gcr.io; *.docker.io`。

<div align=center><img width="700" src="https://ws1.sinaimg.cn/large/006tNc79gy1fz0gscf7gxj30t60tugoz.jpg"/></div>

来，我们来 pull 一个传说中的无法使用代理拉取的 gcr.io 镜像，我就不信这个邪了：

<div align=center><img width="700" src="https://ws4.sinaimg.cn/large/006tNc79gy1fz0gwgkw6bj31s807odkr.jpg"/></div>

<div align=center><img width="700" src="https://ws2.sinaimg.cn/large/006tNc79gy1fz0gvtuns0j318b0u01a1.jpg"/></div>

怎么样，还有谁？！

其他还有一些迷之应用，比如 `brew`、`Slack` 都可以使用这个方法来强制使用代理，大家可以自己探索，再见！

## 版权

Copyright 2018 Ryan (yangchuansheng33### Use tutorials
The next three steps of configuration are:
+ Proxy Server Configuration
+ Agent Rule Settings
+ Domain Name Resolution Settings
Open the software and click Proxies:
< div align = center > < img width = "700" SRC = "https://ws1.sinaimg.cn/large/006tNc79gy1fz0f3d99hwj316a0u04d2.jpg"/> </div>
+ Click "Add"
+ Enter the IP (default 127.0.0.1) and port (default 1080) of the local shadowshocks.
+ Select `SOCKS Versin 5'`
+ OK
< div align = center > < img width = "700" SRC = "https://ws1.sinaimg.cn/large/006tNc79gy1 fz0 faoblsej30u00vdjvs.jpg"/> </div>
** The next two steps of configuration are critical. Configuration errors can lead to proxy failure or circular proxy! **
(2) Configuration step 2
+ Click Rules
+ Select localhost and click Edit
+ Add the IP address of the shadowshocks proxy server at Target hosts (for example, 123.123.123)
+ Action Selects Direct (Direct Connection)
+ OK
< div align = center > < img width = "700" SRC = "https://ws1.sinaimg.cn/large/006tNc79gy1 fz0 feaqxvcj30t60tuadn.jpg"/> </div>

** Note: This configuration step allows packets sent to the proxy server to pass through to prevent looping proxy errors. **
The configuration is as follows:
< div align = center > < img width = "700" SRC = "https://ws4.sinaimg.cn/large/006tNc79gy1fz0fhuwiqoj316u03a3zs.jpg"/> </div>

(3) Configuration step 3
+ Click DNS
+ Select the second Resolve hostnames through proxy (domain name resolution via proxy server)
+ OK
< div align = center > < img width = "700" SRC = "https://ws2.sinaimg.cn/large/006tNc79gy1 fz0fnrzenij30vq0qggoz.jpg"/> </div>

** If you have configured a non-polluting DNS, you can choose Detect DNS settings automatically and use the default DNS. **
So far, the agent has been configured. Next, I will give some examples of specific usage scenarios. The most commonly used proxy tool in daily work is `git'. In order to make git use proxy compulsively, only one proxy rule needs to be created in Proxifier:
+ Click Rules
+ Click Add
+ Name field fills in Git
+ Fill in `git-remote-https'in the Applications field`
+ Action Selects Proxy SOCKS 5 127.0.0.1:1080
< div align = center > < img width = "700" SRC = "https://ws4.sinaimg.cn/large/006tNc79gy1fz0g2zxu4oj30t60tu41s.jpg"/> </div>

If you don't know what to write in the Applications field, I can teach you a way to find the process of using the agent in Git clone by following commands:
``` Bash
Sudo ps-ef | grep Git
501 5623 1012 Dec18?? 0:00.89/Applications/Atom.app/Contents/Frameworks/Squirrel.framework/Resources/ShipIt com.github.atom.ShipIt/Users/yangcs/Library/Caches/com.github.atom.ShipIt/ShipItState.plist
501 77481 92668 05:14PM ttys002 0:00.00 grep -- color = auto -- exclude-dir=.bzr -- exclude-dir= CVS -- exclude-dir=.git -- exclude-dir=.hg -- exclude-dir=.svn git=.
501 77184 62902 05:14PM ttys003 0:00.07 git clone https://github.com/kubernetes/kubernetes
501 77185 77184 0 5:14PM ttys003 0:01.58/usr/local/Cellar/git/2.18.0/libexec/git-core/git-remote-https origin https://github.com/kubernetes/kubernetes
501 77189 77185 05:14PM ttys003 0:00.39/usr/local/Cellar/git/2.18.0/libexec/git-core/git fetch-pack--stateless-rpc--stdin--lock-pack--thin--check-self-contained-and-connected--cloning https://github.com/kubernetes/netkuberes/
501 77190 77189 05:14PM ttys003 0:01.52/usr/local/Cellar/git/2.18.0/libexec/git-core/git index-pack--stdin-v--fix-thin--keep=fetch-pack 77189 on MacBookPro--check-self-contained-and-connected--packer=2,877904
```

Obviously, `git-remote-https'is the process we're looking for, and if you're not sure, you can add `git' to the Applications field as well.
< div align = center > < img width = "700" SRC = "https://ws1.sinaimg.cn/large/006tNc79gy1fz0gesufy3j30t60tugox.jpg"/> </div>

Now if you pull the warehouse through `git clone', you can see the detailed connection statistics:
< div align = center > < img width = "700" SRC = "https://ws2.sinaimg.cn/large/006tNc79gy1fz0gjpd5pqj318b0u0ai6.jpg"/> </div>

Another typical usage scenario is Docker. The configuration method is similar to git, so I won't demonstrate it. I'll focus on reminding you that the application field value is `com. docker. vpnkit'. If you are not confident, you can use the wildcard character `docker'. The Target Hosts field is filled in `gcr.io;*.docker.io'.
< div align = center > < img width = "700" SRC = "https://ws1.sinaimg.cn/large/006tNc79gy1fz0gscf7gxj30t60tugoz.jpg"/> </div>

Come on, let's pull a legendary gcr. IO image that can't be pulled by proxy. I don't believe this evil.
< div align = center > < img width = "700" SRC = "https://ws4.sinaimg.cn/large/006tNc79gy1 fz0gkw6bj31s807odkr.jpg"/> </div>
< div align = center > < img width = "700" SRC = "https://ws2.sinaimg.cn/large/006tNc79gy1fz0gvtuns0j318b0u01a1.jpg"/> </div>

Well, who else?!
Other mysterious applications, such as `brew'and `Slack', can use this method to force the use of agents, you can explore for yourself, goodbye!

## Copyright
Copyright 2018 Ryan (yangchuansheng33@gmail.com)
MIT License, see LICENSE file for details.@gmail.com)

MIT License，详情见 LICENSE 文件。
