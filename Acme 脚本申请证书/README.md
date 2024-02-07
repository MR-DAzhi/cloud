
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
## 验证 DNS 并申请证书
```
~/.acme.sh/acme.sh --issue --dns dns_cf -d 4240333.xyz -d *.4240333.xyz
mkdir /root/cert
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
## 将 /root/cert 目录以及其下所有的文件和子目录的权限设置为拥有者具有读、写、执行权限，相同组的用户具有读、执行权限，其他用户也具有读、执行权限。
```
chmod -R 755 /root/cert
```
