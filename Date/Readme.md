# 设置时区为北京时间
## 一般国外的VPS的镜像都是默认的国外时区，使用起来不是很方便。可以把它修改成北京时间，就会方便很多。代码

```sudo rm -rf /etc/localtime

sudo ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
