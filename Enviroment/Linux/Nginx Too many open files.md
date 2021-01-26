## Nginx Too many open files
```
2020/12/02 18:41:20 [alert] 330477#330477: *730525 socket() failed (24: Too many open files) while connecting to upstream, client: 127.0.0.1, server: , request: "POST /api/member/teacher_create_lessons/student_visit_zoom_link.json HTTP/1.0", upstream: "fastcgi://unix:/run/php/php7.4-fpm.sock:", host: "127.0.0.1:81"
```

出现这个错误可能是由于系统的 ulimit 限制和 nginx 自身的配置有关系，先来了解下概念。

- 什么是 ulimit?
ulimit 命令用来限制系统用户对 shell 资源的访问。如果不懂什么意思，下面一段内容可以帮助你理解：

假设有这样一种情况，当一台 Linux 主机上同时登陆了 10 个人，在系统资源无限制的情况下，这 10 个用户同时打开了 500 个文档，而假设每个文档的大小有 10M，这时系统的内存资源就会受到巨大的挑战。
ulimit 用于限制 shell 启动进程所占用的资源，支持以下各种类型的限制：所创建的内核文件的大小、进程数据块的大小、Shell 进程创建文件的大小、内存锁住的大小、
常驻内存集的大小、打开文件描述符的数量、分配堆栈的最大大小、CPU 时间、单个用户的最大线程数、Shell 进程所能使用的最大虚拟内存。同时，它支持硬资源和软资源的限制。
简单来说，ulimit 描述符可以对用户打开的文件数量进行限制（不止限制打开文件数量），让单个用户不至于打开较多的文件，导致系统奔溃或者资源不足的情况。

注意：修改 nginx 的 max open files 有个前提，就是你已经修改好了系统的 max open files.
修改系统中的 /etc/security/limits.conf文件中
```
 sudo vim /etc/security/limits.conf #在最后添加如下两行
* soft nofile 65535
* hard nofile 65535
```
这是网上搜了很多教程，但是到此还是无法解决问题
 
上面修改了系统的 max open files，并不代表nginx的
先查看 nginx 的 ulimits：
```
grep 'open files' /proc/$( cat /var/run/nginx.pid )/limits
Max open files            1024                1024                files
```

发现nginx 的 max open files 并没有变化
 
修改 nginx.service
```
sudo vim /lib/systemd/system/nginx.service # （仅适用于 ubuntu）
```
添加如下：
```
[Service]
LimitNOFILE=65535
```

重启服务：
```
sudo systemctl daemon-reload
```

修改 nginx.conf，
添加：以下两点
```
worker_rlimit_nofile 65535; # (has to be smaller or equal to LimitNOFILE set above)
events {
    worker_connections  65535;
}
```
重启 nginx:
```
sudo systemctl restart nginx
```

然后再去查看nginx的 max open files，看看是不是我们设置的65535
```
grep 'open files' /proc/$( cat /var/run/nginx.pid )/limits
```
到此就修改nginx的max open files结束。
 
但是如果你已经把nginx 的max open files值改的很大了，但是还是会出现Too many open files 的错误，那说明你后端服务器处理请求的速度太慢了，或是 并发太高了。

[转发](https://www.cnblogs.com/c9999/p/11249386.html)