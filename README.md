# ShadowsocksR 服务端安装教程 #
### 说明 ###
此教程为单用户版，适合个人用户。如果你是站长，请查看多用户版教程：  
[数据库多用户教程](https://github.com/wuming2018/test/blob/master/Server-Setup(manyuser-with-mysql).md)  
[json版多用户教程](https://github.com/wuming2018/test/blob/master/Server-Setup(manyuser-with-mudbjson).md)（仅一台服务器适用）

基本库安装 
-----
以下命令均以root用户执行，或sudo方式执行

centos：  

    yum install git

ubuntu/debian：  
 
    apt-get install git

#### 获取源代码
```
git clone -b master https://github.com/wuming2018/test.git
mv ssr/ /usr/local/shadowsocksr
```

执行完毕后安装目录为/usr/local/shadowsocksr，其中根目录的是多用户版（即数据库版，个人用户请忽略这个），子目录中的是单用户版(即/usr/local/shadowsocksr/shadowsocks)。

服务端配置
-----
初始化配置：
```
cd /usr/local/shadowsocksr
bash initcfg.sh
```

以下步骤要进入子目录：
```
cd /usr/local/shadowsocksr/shadowsocks
```

#### 快速运行 ####
```
python server.py -p 443 -k password -m aes-256-cfb -O auth_sha1_v4 -o http_simple

// 说明：-p 端口 -k 密码  -m 加密方式 -O 协议插件 -o 混淆插件
```
如果要后台运行：
```
python server.py -p 443 -k password -m aes-256-cfb -O auth_sha1_v4 -o http_simple -d start
```
如果要停止/重启：
```
python server.py -d stop/restart
```
查看日志：
```
tail -f /var/log/shadowsocksr.log
```

用 -h 查看所有参数

#### 设置客户端代理：
默认地址：127.0.0.1   默认端口: 1080 
注：python版客户端只支持socks代理。
----------------------------------

### 自启动 ###
[System startup script](https://github.com/wuming2018/test/blob/master/System-startup-script.md)

客户端
------
注：以下客户端中有：
windows客户端和python版客户端，ShadowsocksX-NG（macOS客户端之一）， 
Android客户端，Shadowrocket（iOS客户端之一，iTunes售价$3美元/￥18人民币）， 
可以使用SSR特性，
 
其他原版客户端只能以兼容的方式连接SSR服务器（SSR可兼容SS客户端）。

* [Windows] / [OS X] / [ShadowsocksX-NG]
* [Linux python] / [Linux Qt]
* [Android] / [iOS] / [Shadowrocket]
* [OpenWRT]

OSX上可使用GoAgentX的SSR插件。在你本地的 PC 或手机上使用图形客户端。具体使用参见它们的使用说明。

也可以直接使用 [Python] 版客户端（命令行）。

### 其它加密支持 ###
安装[libsodium]即可支持 salsa20, chacha20, chacha20-ietf 加密（暂不支持[AEAD]）

### 其它异常 ###
如果你的服务端python版本在2.6以下，那么必须更新python到2.6.x或2.7.x版本

[AEAD]              https://github.com/onelogin/aead  
[Debian sid]        https://packages.debian.org/unstable/python/shadowsocks  
[iOS]               https://github.com/shadowsocks/shadowsocks-iOS/wiki/Help  
[Linux Qt]          https://github.com/librehat/shadowsocks-qt5  
[OpenWRT]           https://github.com/shadowsocks/openwrt-shadowsocks  
[OS X]              https://github.com/shadowsocks/shadowsocks-iOS/wiki/Shadowsocks-for-OSX-Help  
[ShadowsocksX-NG]   https://github.com/yichengchen/ShadowsocksX-R  
[Shadowrocket]      https://itunes.apple.com/us/app/shadowrocket/id932747118  


<del>其它参见 https://github.com/breakwa11/shadowsocks-rss/wiki/ulimit</del>  
<del>[Windows]:           https://github.com/shadowsocksr/shadowsocksr-csharp</del>  
<del>[Android]:           https://github.com/shadowsocksr/shadowsocksr-android</del>  
<del>[Python]:            https://github.com/breakwa11/shadowsocks-rss/wiki/Python-client-setup-(Mult-language)</del>  
<del>[libsodium]:         https://github.com/breakwa11/shadowsocks-rss/wiki/libsodium</del>  
<del>[Linux python]:      https://github.com/shadowsocksr/shadowsocksr</del>  

