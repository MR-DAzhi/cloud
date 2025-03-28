# 科学上网：VPS 搭建 X-ui 面板，用 DNS 申请 SSL 证书

## 使用 X-ui 搭建代理服务，具有以下优点：

- 支持系统状态监控：如 CPU、内存、硬盘等状态
- 支持多用户多协议，网页可视化操作
- 支持流量统计
- 支持自定义 Xray 配置模板
- 支持HTTPS访问面板
- 支持面板自定义端口，账号与密码
- 快速生成分享连接或二维码
- 支持 CDN 套用
- 支持 Fallback 分流设置

### 与本期视频相关教程


域名注册教程：https://youtu.be/2uJQdWpM46k

Cloudflare 接管域名教程：https://youtu.be/1GtDTWybJNM

X-ui 搭建教程一：https://youtu.be/n5koU-pj094

### 准备工作

- 1、域名一个，托管到 Cloudflare （方法见上）

- 2、VPS 一台，例如：Debian/Ubuntu/CentOS

- 3、下载并安装 FinalShell SSH 工具

Windows 版本下载地址:
http://www.hostbuf.com/downloads/finalshell_install.exe

MacOS 版本下载地址:
http://www.hostbuf.com/downloads/finalshell_install.pkg

 

### 更新安装系统
下面环境的安装方式，大家根据自己的系统选择命令安装就好了。
### 1、Debian/Ubuntu 系统执行以下命令：
    apt update -y         
    apt install -y curl    
    apt install -y socat    
    
### 2、CentOS 系统执行以下命令：   

    yum update -y         
    yum install -y curl   
    yum install -y socat   
    
### 如果机器出口没有IPv4，可通过安装WARP来给机器添加IPv4出口
WARP好处
支持 chatGPT，解锁奈飞流媒体
避免 Google 验证码或是使用 Google 学术搜索
可调用 IPv4 接口，使青龙和V2P等项目能正常运行
由于可以双向转输数据，能做对方VPS的跳板和探针，替代 HE tunnelbroker
能让 IPv6 only VPS 上做的节点支持 Telegram
IPv6 建的节点能在只支持 IPv4 的 PassWall、ShadowSocksR Plus+ 上使用

### warp 运行脚本
```
wget -N https://gitlab.com/fscarmen/warp/-/raw/main/menu.sh && bash menu.sh [option] [lisence/url/token]
```
### 再次运行
```
warp [option] [lisence]
```
教程 https://github.com/fscarmen/warp-sh
### 安装 BBR 加速
 本脚本建议在Debian≥9或是CentOS≥8以上的系统中使用
 
    wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
 
### 一键X-ui面板
```
bash <(curl -Ls https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install.sh)
``` 
 
完X-ui 安装以后，我们可以输入 VPS IP:端口（如1.1.1.1:54321） 登录3X-ui 的管理面板（可以登录代表安装成功，登录不了请放行端口。)

###  放行端口指令
放行443端口:

    iptables -I INPUT -p tcp --dport 443 -j ACCEPT
  
放行54321端口:

    iptables -I INPUT -p tcp --dport 54321 -j ACCEPT

### CDN放行端口 IPV4

```# 放行 HTTP 和 HTTPS 端口
iptables -A INPUT -p tcp -m multiport --dports 80,443,2052,2053,2082,2083,2086,2087,2095,2096,8443 -j ACCEPT
```


### CDN放行端口ipv6


```# 放行 HTTP 和 HTTPS 端口
ip6tables -A INPUT -p tcp -m multiport --dports 80,443,2052,2053,2082,2083,2086,2087,2095,2096,8443 -j ACCEPT
```


### 申请 SSL 的证书

输入命令 <code> x-ui</code>  ，进入 X-ui 的命令菜单
选择 16，申请 SSL 的证书。（申请需要有 Cloudflare API ，可以 观看视频 获取 API）

申请的时候是申请的泛域名证书，所以，填写域名的时候，只填入 域 也就好了，例如<code>  xxx.com</code>  的格式。

申请成功以后，证书和密钥文件在 VPS 目录的<code> /root/cert </code>文件夹里面


### 安装证书到指定文件夹

```
~/.acme.sh/acme.sh --installcert -d baidu.xyz --key-file /root/cert/private.key --fullchain-file /root/cert/cert.crt
```
将/root/private.key和 /root/cert.crt 是把密钥和证书安装到 /root 目录，并改名为 private.key 和 cert.crt
###  x-ui 管理面板设置
添加证书和密钥路径，重启面板
通过域名访问x-ui 管理面板：https://域名:54321
 
### 添加科学上网节点
 这一步扩展性很强，大家可以根据自己的需求设置相关的节点规则。

### 关于x-ui节点电脑V2ray能连接上，扫描导入手机V2rayNG连接不了？
在 X-ui面板设置中，“面板证书公钥文件路径”，不要用带自己域名的证书，输入<code> /root/cert/fullchain.cer</code> （这是标准的CA证书集的文件之一）的路径。然后重启面板。在创建的节点时候，公钥文件路径同样填入 <code> /root/cert/fullchain.cer</code> 。
这样设置之后，解决了问题： io read/write on closed pipe

 ----------------
这种 Xray 可视化管理面板 的方式，也是支持伪装网站以及多网站并存的，包括支持宝塔面板的搭建方式。


测试VPS服务器的性能、路由、下载速度脚本：


1、显示vps的系统信息、网络信息、硬盘的读写速度、测试不同位置的下载和上传速度、

    curl -sL yabs.sh | bash   
   
   低宽带服务器应该考虑加入-r减少位置测试，或者-i禁用网络测试 

    curl -sL yabs.sh | bash -s -- -r
    

    curl -sL yabs.sh | bash -s -- i

   
2、三网回程路由和延迟测试

    wget -qO- git.io/besttrace | bash
   

3、三网测速，使用全国各地三大运营商的speedtest测速节点进行测速

    curl -Lso- https://git.io/superspeed.sh
    



