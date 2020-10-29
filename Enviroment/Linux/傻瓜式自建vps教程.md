## 安装shadowsocks
```shell
yum install python-setuptools && easy_install pip
pip install shadowsocks
```
## 配置文件
```shell
vi /etc/shadowsocks.json
```
```
{
        "server":"149.28.232.182",
        "server_port":8388,
        "local_address":"127.0.0.1",
        "local_port":1080,
        "password":"pkq@19921013",
        "timeout":300,
        "method":"aes-256-cfb",
        "fast_open":false
}
```

## 启动shadowsocks
```shell
ssserver -c /etc/shadowsocks.json -d start
```

## Centos7关闭防火墙（查看防火墙状态是否是not running）
```shell
firewall-cmd --state    #查看防火墙状态
systemctl stop firewalld.service  #关闭防火墙
```

## 查看shadowsocks的日志
```shell
tail -f /var/log/shadowsocks.log（日志目录）
```

## 修改服务器时间（修改时区）
```shell
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtim
```
