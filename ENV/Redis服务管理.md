## Redis服务实例应当遵守以下规范：
- 实例的目录应当为 /srv/redis/{$端口}/
- 实例的配置文件、回写文件、pid文件、日志文件应当都保存于自已的目录中
- 实例应当使用systemctl统一管理，并开机自启动
- 实例应当尽量设置回写规则，避免纯内存的使用方式，除非数据完全可丢失
- Redis添加实例流程：
    
    到目标服务器的 /srv/redis 目录下创建新实例的目录：mkdir {$端口号}
- 创建配置文件：cp /etc/redis.conf /srv/redis/{$端口号}
- 修改配置文件，需要修改的配置项至少包括：port, pidfile, logfile, dir, save, maxmemory, maxmemory-policy, daemon(设置为1)，还应当按情况考虑需要不需要修改 bind 配置项的值
- 修改新实例目录和配置文件的权限：chown -R redis:redis /srv/redis/{$端口号}
- 启动新实例：systemctl start redis@{$端口号}
- 将新实例注册为服务：systemctl enable redis@{$端口号}
- 如果是测试环境需要对内网开放端口，还需要将新实例的端口添加到防火墙规则中：firewall-cmd --add-port=6383/tcp --permanent


## Redis多实例管理脚本：
使用systemctl管理多个redis实例需要使用redis@.service脚本，可复制192.168.0.212:/lib/systemd/system/redis@.service到目标服务器的相同位置。

文件内容
```shell
[Unit]
Description=Redis(Port %I)
After=network.target
Documentation=http://redis.io/documentation, man:redis-server(1)

[Service]
Type=forking
ExecStart=/usr/bin/redis-server /srv/redis/%I/redis.conf
PIDFile=/srv/redis/%I/redis.pid
TimeoutStopSec=0
Restart=always
User=redis
Group=redis

ExecStartPre=-/bin/run-parts --verbose /etc/redis/redis-server.pre-up.d
ExecStartPost=-/bin/run-parts --verbose /etc/redis/redis-server.post-up.d
ExecStop=-/bin/run-parts --verbose /etc/redis/redis-server.pre-down.d
ExecStop=/bin/kill -s TERM $MAINPID
ExecStopPost=-/bin/run-parts --verbose /etc/redis/redis-server.post-down.d

PrivateTmp=yes
PrivateDevices=yes
ProtectHome=yes
#ReadOnlyDirectories=/ #打开这一行开启会导致进程无法启动！
ReadWriteDirectories=-/srv/redis/%I
CapabilityBoundingSet=~CAP_SYS_PTRACE

# redis-server writes its own config file when in cluster mode so we allow
# writing there (NB. ProtectSystem=true over ProtectSystem=full)
ProtectSystem=true
#ReadWriteDirectories=-/etc/redis

[Install]
WantedBy=multi-user.target
#Alias=redis.service
```