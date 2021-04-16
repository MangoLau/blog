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
        "server":"0.0.0.0",
        "server_port":8388,
        //"local_address":"127.0.0.1",
        //"local_port":1080,
        "password":"pkq@19921013",
        "timeout":300,
        "method":"aes-256-gcm",
        "fast_open":false
}
```
备注：
server填写你代理服务器的IP
server_port代理服务器设置的端口
password代理服务器密码

## 启动shadowsocks
```shell
ssserver -c /etc/shadowsocks.json -d start
```

### 配置自启动
`vim /etc/systemd/system/shadowsocks.service`

```
[Unit]
Description=Shadowsocks
[Service]
TimeoutStartSec=0
ExecStart=ssserver -c /etc/shadowsocks.json -d start
[Install]
WantedBy=multi-user.target
```

`systemctl start shadowsocks`

### 报错 不支持加密方式aes-256-gcm
解决方法:
`pip install https://github.com/shadowsocks/shadowsocks/archive/master.zip -U`

`systemctl start shadowsocks`


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
