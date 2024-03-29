# 搭建 Hysteria 2
## 安装 Hysteria 2 服务器端
## 更新 VPS 系统，安装所需组件 （根据系统，自行选择命令）
```
apt update -y              #Debian 命令
apt install curl sudo -y   #Debian 命令
```
```
yum update -y              #CentOS 命令
yum install curl sudo -y   #CentOS 命令
```
## Hysteria 官方的一键安装脚本
## 安装或升级到最新版本 Hysteria 2:


```
bash <(curl -fsSL https://get.hy2.sh/)
``` 
## 移除 Hysteria 2:
```
bash <(curl -fsSL https://get.hy2.sh/) --remove
```
## 无域名，生成自签证书
命令会在 VPS 目录 /etc/hysteria 中生成 server.crt 及 server.key 文件，并变更相关权限
```
openssl req -x509 -nodes -newkey ec:<(openssl ecparam -name prime256v1) -keyout /etc/hysteria/server.key -out /etc/hysteria/server.crt -subj "/CN=bing.com" -days 36500 && sudo chown hysteria /etc/hysteria/server.key && sudo chown hysteria /etc/hysteria/server.crt
```
## 服务端配置文件

服务端的配置要和客户端状态一致，也就是说，服务端配置的是无域名的配置，客户端也必须选择无域名的配置参数，必须一致，反之一样，服务端配置的是有域名的，那稍后客户端也必须选择有域名的参数
好在配置更改起来很简单，有域名和无域名更换起来，也就几分钟的事情。

推荐密码设置 ：<a href="https://1password.com/zh-cn/password-generator/">点击访问</a> 生成随机高强度密码

```

listen: :443
 
# 以下 acme 和 tls 字段，二选一
# 有域名部署的选择 acme ，无域名的选择 tls
# 选择 acme，必须注释掉 tls，反之一样
 
acme:
  domains:
    - cn2.bozai.us        # 域名
  email: your@email.com   # 邮箱，格式正确即可
 
#tls:
#  cert: /etc/hysteria/server.crt
#  key: /etc/hysteria/server.key
 
auth:
  type: password
  password: 88888888   # 请及时更改密码
 
masquerade:
  type: proxy
  proxy:
    url: https://bing.com # 伪装网站
    rewriteHost: true

```

## 服务相关命令
```
systemctl start hysteria-server.service    # 启动 hysteria 服务
systemctl enable hysteria-server.service   # 设置 hysteria 服务 开机自启
systemctl restart hysteria-server.service  # 重启 hysteria 服务
systemctl stop hysteria-server.service     # 停止 hysteria 服务
systemctl status hysteria-server.service   # 查看 hysteria 服务 状态
```
## 检查服务是否正常运行

如下图红框显示，即为服务正常！若有域名，并且多次CA证书下发失败，请尝试不用域名搭建


![image](https://github.com/MR-DAzhi/cloud/blob/main/Hysteria%202/1-2.png)

至此，服务端设置完毕。

## 客户端配置
Windows 推荐使用 V2rayN
当然 sing-box 正在研发之中，具体施工进程 <a href="https://github.com/2dust/v2rayN/releases/latest">点击查看网页访问</a>

V2rayN 科学上网工具下载：<a href="https://github.com/2dust/v2rayN/releases/latest">点击访问</a> 推荐使用带 Core 的版本 v2rayN-With-Core.zip

解压 V2rayN，找到文件目录中的 \bin\hysteria2 ，替换 hysteria-windows-amd64.exe 为 最新文件，<a href="https://github.com/apernet/hysteria/releases">点击下载</a>
## 客户端配置文件
请看清楚配置文件中的注释，修改 ip , auth（VPS 服务端 上面配置的密码） ， bandwidth ， sni ， insecure 等参数
```
server: ip:443
auth: ****
 
#bandwidth:
#  up: 20 mbps
#  down: 100 mbps
  
tls:
  sni: cn2.bozai.us  # 若无域名，请改为 bing.com
  insecure: false    # 若无域名，需要改参数为 true
 
socks5:
  listen: 127.0.0.1:1080
http:
  listen: 127.0.0.1:8080
```
配置文件导入


![image](https://github.com/MR-DAzhi/cloud/blob/main/Hysteria%202/2-2.png)

## OpenWRT  推荐使用 PassWall2
添加协议，如下图所示：

下图配置为有域名的配置参数。（请自行评估自己的上传和下载带宽，数值过大会引起流量浪费）

若是无域名的配置，请在 域名 栏目，填写 伪装网站 bing.com，并勾选 允许不安全连接 即可

认证密码，为 VPS 服务端 上面配置的密码

![image](https://github.com/MR-DAzhi/cloud/blob/main/Hysteria%202/3-1.png)
## Android / IOS / MacOS 推荐使用 sing-box
sing-box 图形化客户端下载：<a href="https://sing-box.sagernet.org/zh/clients/">点击下载</a>

## sing-box 配置文件
根据自己的需要，自行更改
```
{
  "dns": {
    "servers": [
      {
        "tag": "cf",
        "address": "https://1.1.1.1/dns-query"
      },
      {
        "tag": "local",
        "address": "223.5.5.5",
        "detour": "direct"
      },
      {
        "tag": "block",
        "address": "rcode://success"
      }
    ],
    "rules": [
      {
        "geosite": "category-ads-all",
        "server": "block",
        "disable_cache": true
      },
      {
        "outbound": "any",
        "server": "local"
      },
      {
        "geosite": "cn",
        "server": "local"
      }
    ],
    "strategy": "ipv4_only"
  },
  "inbounds": [
    {
      "type": "tun",
      "inet4_address": "172.19.0.1/30",
      "auto_route": true,
      "strict_route": false,
      "sniff": true
    }
  ],
  "outbounds": [
    {
      "type": "hysteria2",
      "tag": "proxy",
      "server": "ip",             // VPS ip
      "server_port": 443,
      "up_mbps": 50,              //上传速率，实际填写，过大会导致流量浪费
      "down_mbps": 200,           //下载速率，实际填写，过大会导致流量浪费
      "password": "**********",   //hysteria2 服务密码
      "tls": {
        "enabled": true,
        "server_name": "cn2.bozai.us",    //若域名搭建，请填写域名，若IP搭建，请填写 bing.com
        "insecure": false                 //若域名搭建，请填写 false，若IP搭建，请填写 true
      }
    },
    {
      "type": "direct",
      "tag": "direct"
    },
    {
      "type": "block",
      "tag": "block"
    },
    {
      "type": "dns",
      "tag": "dns-out"
    }
  ],
  "route": {
    "rules": [
      {
        "protocol": "dns",
        "outbound": "dns-out"
      },
      {
        "geosite": "cn",
        "geoip": [
          "private",
          "cn"
        ],
        "outbound": "direct"
      },
      {
        "geosite": "category-ads-all",
        "outbound": "block"
      }
    ],
    "auto_detect_interface": true
  }
}
```

