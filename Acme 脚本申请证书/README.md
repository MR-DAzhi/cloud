
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

安装 Acme 脚本之后，请先执行下面的命令（下面的邮箱为你的邮箱）
```

~/.acme.sh/acme.sh --register-account -m xxxx@xxxx.com
```
## 设置 Cloudflare API 令牌
```
export CF_Key="2095b201c3b6e2"
export CF_Email="dazhixiansheng777@gmail.com"
```
## 若是DNS验证申请不下来，请尝试使用下面的 Web 验证的方式申请证
```
1.    ~/.acme.sh/acme.sh --issue --dns dns_cf -d bozai3.xyz -d *.bozai3.xyz
2.    mkdir /root/cert
3.    ~/.acme.sh/acme.sh --installcert -d bozai3.xyz --key-file /root/cert/private.key --fullchain-file /root/cert/cert.crt
4.    ~/.acme.sh/acme.sh --upgrade --auto-upgrade
5.    chmod -R 755 /root/cert

```

## 安装证书到指定文件夹

```
~/.acme.sh/acme.sh --installcert -d 4240333.xyz --key-file /root/cert/private.key --fullchain-file /root/cert/cert.crt
```
将/root/private.key和 /root/cert.crt 是把密钥和证书安装到 /root 目录，并改名为 private.key 和 cert.crt

## 更新 Acme 脚本
```
~/.acme.sh/acme.sh --upgrade

```

