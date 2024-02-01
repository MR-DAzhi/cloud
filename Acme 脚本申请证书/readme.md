
# Acme 脚本申请证书


## 安装环境

```
apt update -y           #Debian/Ubuntu 命令
apt install -y curl     #Debian/Ubuntu 命令
apt install -y socat    #Debian/Ubuntu 命令
```
## 安装 Acme 脚本

```
curl https://get.acme.sh | sh
```

## 设置 Cloudflare API 令牌
```
export CF_Key="2095b201c3b6e2"
export CF_Email="dazhixiansheng777@gmail.com"
```
## 验证 DNS 并申请证书
```
~/.acme.sh/acme.sh --issue --dns dns_cf -d 4240333.xyz -d *.4240333.xyz
mkdir /root/cert
~/.acme.sh/acme.sh --installcert -d 4240333.xyz --key-file /root/cert/private.key --fullchain-file /root/cert/cert.crt
~/.acme.sh/acme.sh --upgrade --auto-upgrade
chmod -R 755 /root/cert
```
