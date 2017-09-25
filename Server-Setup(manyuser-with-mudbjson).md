# ShadowsocksR 多用户版安装教程 #

以下命令均以root用户执行，或sudo方式执行

### 基本库安装 ###
centos： 
```
yum install python-setuptools && easy_install pip
yum install git
```
ubuntu/debian： 
```
apt-get install python-pip
apt-get install git
```

### 获取源代码 ###
~~~
git clone -b master https://github.com/wuming2018/test.git
mv ssr/ /usr/local/shadowsocksr
~~~
执行完毕后安装目录为/usr/local/shadowsocksr,其中根目录的是多用户版,即数据库版,个人用户请忽略这个,子目录中的是单用户版,即/usr/local/shadowsocksr/shadowsocks  

### 服务端配置 ###
进入根目录初始化配置(假设根目录在`/usr/local/shadowsocksr`，如果不是，命令需要适当调整)：
```
cd /usr/local/shadowsocksr
bash initcfg.sh
```

### 多用户配置文件  
手动编辑配置文件
```
vi mudb.json
```
#### 示例,下面有3个用户的配置文件
`1081` 端口:1081,加密:aes-256-cfb,协议:auth_sha1_v4_compatible,混淆:http_simple_compatible,密码:1234567  
`1082` 端口:1082,加密:aes-256-cfb,协议:auth_sha1_v4_compatible,混淆:tls1.2_ticket_auth_compatible,密码:1234567  
`1083` 端口:1083,加密:aes-256-cfb,协议:auth_sha1_v4_compatible,混淆:tls1.2_ticket_auth_compatible,密码:1234567  
```
[
    {
        "d": 0,
        "enable": 1,
        "forbidden_port": "",
        "port": 1081,
        "method": "aes-256-cfb",
        "passwd": "1234567",
        "protocol": "auth_sha1_v4_compatible",
        "protocol_param": "",
        "obfs": "http_simple_compatible",
        "speed_limit_per_con": 0,
        "speed_limit_per_user": 0,
        "transfer_enable": 900727656415232,
        "u": 0,
        "user": "1081"
    },
    {
        "d": 0,
        "enable": 1,
        "forbidden_port": "",
        "port": 1082,
        "method": "aes-256-cfb",
        "passwd": "1234567",
        "protocol": "auth_sha1_v4_compatible",
        "protocol_param": "",
        "obfs": "tls1.2_ticket_auth_compatible",
        "speed_limit_per_con": 0,
        "speed_limit_per_user": 0,
        "transfer_enable": 900727656415232,
        "u": 0,
        "user": "1082"
    },
    {
        "d": 0,
        "enable": 1,
        "forbidden_port": "",
        "port": 1083,
        "method": "aes-256-cfb",
        "passwd": "1234567",
        "protocol": "auth_sha1_v4_compatible",
        "protocol_param": "",
        "obfs": "tls1.2_ticket_auth_compatible",
        "speed_limit_per_con": 0,
        "speed_limit_per_user": 0,
        "transfer_enable": 900727656415232,
        "u": 0,
        "user": "1083"
    }
]        
```
可以按自己喜好自行修改/添加/删除  
为了方便,上面的示例已建立一个现成的mudb_json,可以在安装目录下执行这个命令使用这个现成的:  
```
// 此命令的意思:删除现存的mudb.json,删除成功后,复制mudb_json为mudb.json
rm mudb.json && cp mudb_json mudb.json
```
配置文件说明:
```
        "d": 1215596,	//下载字节数
        "enable": 1,		//开启这个用户
        "forbidden_port": "",	//禁止访问端口
        "port": 1083,	//端口
        "method": "aes-128-ctr",	//加密方式
        "passwd": "1234567",	//密码
        "protocol": "auth_sha1_v4_compatible",	//协议
        "protocol_param": "",	//端口同一时间能链接的客户端数量
        "obfs": "tls1.2_ticket_auth_compatible",	//混淆
        "speed_limit_per_con": 0,	//端口单用户限速
        "speed_limit_per_user": 0,	//端口总用户限速
        "transfer_enable": 900727656415232,	//流量限制(单位:字节数) (5g=5368709120) (100g=107374182400)
        "u": 147824,	//上传字节数
        "user": "1083"	//用户名
```
#### 参考资料 : <https://breakwa11.blogspot.com/2017/01/shadowsocksr-mu.html>  

或者通过使用脚本`mujson_mgr.py`添加端口及相应的加密、协议、混淆等配置，具体方法通过执行以下命令查看该脚本的说明及提示：  
`python mujson_mgr.py`  
示例:增加一个用户:001, 端口:1234, 密码:1234567, 加密方式:aes-256-cfb, 协议:auth_sha1_v4,混淆:http_simple  
```
python mujson_mgr.py -a -u "001" -p 1234 -k 1234567 -m aes-256-cfb -O auth_sha1_v4 -o http_simple
```
示例运行结果:
```
### add user info
    user : 001
    port : 1234
    method : aes-256-cfb
    passwd : 1234567
    protocol : auth_sha1_v4
    obfs : http_simple
    transfer_enable : 8388608.0  G Bytes
    u : 0
    d : 0
    ssr://xxx.xxx.xxx.xxx:1234:auth_sha1_v4:aes-256-cfb:http_simple:xxxxxxxxx
    ssr://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

### 服务端运行与停止 ###

后台运行（无log，ssh窗口关闭后也继续运行） 

`./run.sh`

后台运行（输出log，ssh窗口关闭后也继续运行） 

`./logrun.sh`

后台运行时查看运行情况 

`./tail.sh`

停止运行 

`./stop.sh`

注：通过脚本运行默认日志会保存在根目录的ssserver.log，可手动查看。

### 开机自启动
systemd脚本，适用于CentOS/RHEL7以上，Ubuntu 15以上，Debian8以上,安装目录为/usr/local/shadowsocksr  
```
cp /usr/local/shadowsocksr/shadowsocksr.service /etc/systemd/system/shadowsocksr.service
systemctl enable shadowsocksr.service && systemctl start shadowsocksr.service
```
低版本可以去参考 System startup script : <https://github.com/wuming2018/test/blob/master/System-startup-script.md>  

### 其它异常 ###
如果你的服务端python版本在2.6以下，那么必须更新python到2.6.x或2.7.x版本

<del>其它参见 https://github.com/breakwa11/shadowsocks-rss/wiki/ulimit</del>