准备工作
1、域名一个 （注册隐私域名，点击这里）

2、托管域名到 Cloudflare （不会点击这里）

3、后面 ACME 脚本申请证书，需要安装以后才可以进行使用，禁止直接使用 ACME 目录下面的证书文件。若是不会，请看相关的博客说明，或是仔细看 视频教程。

申请 SSL 证书
FreeSSL 申请证书
这种方式申请证书，比较简单，而且还可以申请到为期一年的 “亚洲诚信” 的证书，但是，目前此网站申请证书需要进行手机验证，所以，若是有疑虑的小伙伴们还是去尝试下面其他的方法。

相关的申请教程，请观看本期的 视频教程。

Acme 脚本申请证书
Acme 脚本申请证书，是我们用到的最常见的一种证书的申请方式，它有很多的申请方法，大家只需要找到一种适合自己的也就好了。

不管用下面的何种方式申请，都需要安装 Acme，有一部分的申请场景需要用到相关的插件，所以我们需要提前安装。

下面环境的安装方式，大家根据自己的系统选择命令安装就好了

# Debian命令
```apt update -y
apt update -y 
apt install -y curl 
apt install -y socat
```
# Ubuntu 命令
```
yum update -y   
yum install -y curl   
yum install -y socat   

```

# 安装 Acme 脚本
```
curl https://get.acme.sh | sh
```
## 重要申明

2021 年 6 月 17 日更新：

从 acme.sh v 3.0.0 开始，acme.sh 使用 Zerossl 作为默认 ca，您必须先注册帐户（一次），然后才能颁发新证书。

具体操作步骤如下
1、安装 Acme 脚本之后，请先执行下面的命令（下面的邮箱为你的邮箱）
~/.acme.sh/acme.sh --register-account -m xxxx@xxxx.com

2、其他的命令暂时没有变动

## 80 端口空闲的验证申请


如果你还没有运行任何 web 服务, 80 端口是空闲的, 那么 Acme.sh 还能假装自己是一个 WebServer, 临时监听在 80 端口, 完成验证
```
~/.acme.sh/acme.sh  --issue -d mydomain.com   --standalon
```
## Nginx 的方式验证申请
这种方式需要你的服务器上面已经部署了 Nginx 环境，并且保证你申请的域名已经在 Nginx 进行了 conf 部署。（被申请的域名可以正常被打开）
```
~/.acme.sh/acme.sh --issue  -d mydomain.com   --ngin
```
## http 的方式验证申请
这种方式需要你的服务器上面已经部署了网站环境。（被申请的域名可以正常被打开）

原理：Acme 自动在你的网站根目录下放置一个文件, （这个文件可以被互联网访问）来验证你的域名所有权,完成验证. 然后就可以生成证书了.

实例代码：（后面的路径请更改为你的 网站根目录 绝对路径 ）

