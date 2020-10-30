# ddns-scripts_aliyun
适用于 OpenWRT/LEDE 自带DDNS客户端的阿里云更新脚本

依赖: ddns-scripts wget openssl-util

建议安装: luci-app-ddns


安装后在DDNS服务提供商一栏多出一个 aliyun.com。

详情见恩山论坛介绍帖 [适用于OpenWRT/LEDE自带DDNS功能的阿里云脚本，完美嵌入](http://www.right.com.cn/forum/thread-267501-1-1.html)

**Note**

本脚本 fork 自 [https://github.com/sensec/ddns-scripts_aliyun]

原脚本实现了阿里云复杂的URL编码及签名验证逻辑，同时硬编码 wget 实现了完整的与阿里云通信的函数。然而后者是不必要的。此脚本并非独立运行，而是作为插件被调用，通用的通信函数 do_transfer 在 dynamic_dns_functions.sh 中已经实现，只要将构造好的URL作为参数传递给 do_transfer 进行调用就行了。

**一个坑**

do_transfer 同时支持 curl 和 wget 发送请求，并且 wget 的优先级高于 curl。
令人蛋疼的是 Openwrt 自己实现了一个迷你版的 wget，叫做 uclient-fetch，`/usr/bin/wget` 不过是一个指向 uclient-fetch 的软链接。然而这个迷你版的 wget 并不支持 wget 的大部分参数，导致更新脚本运行失败。解决办法：

    opkg update
    opkg install wget