## linux查看磁盘空间占用情况
查看磁盘空间占用情况，可以使用**du**和**df**命令
1. df -h 命令查看磁盘空间
```c
[root@10-9-128-211 ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1        20G   15G  4.2G  78% /
tmpfs           1.9G     0  1.9G   0% /dev/shm

```
2. ==du -ah --max-depth=1 /== 查看根目录下各个文件占用情况
```c
[root@10-9-128-211 ~]# du -ah --max-depth=1 /var/log/
84K     /var/log/anaconda.xlog
152K    /var/log/wtmp
144K    /var/log/php-fpm
0       /var/log/tallylog
20M     /var/log/secure
4.0K    /var/log/httpd
4.0K    /var/log/ntpstats
4.0K    /var/log/mysqld.log.rpmsave
32K     /var/log/lastlog
4.0K    /var/log/ppp
4.0K    /var/log/ConsoleKit
4.0K    /var/log/zabbix
17M     /var/log/sa
4.0K    /var/log/messages
456K    /var/log/cron
8.0K    /var/log/redis
4.0K    /var/log/supervisor
44K     /var/log/anaconda.syslog
32K     /var/log/dmesg.old
0       /var/log/spooler
4.0K    /var/log/audit
32K     /var/log/dmesg
4.0K    /var/log/nginx
37M     /var/log/
```
最后一条记录是统计。

==max-depth==表示目录的深度。

3. 查看某个目录 ==du -bsh==命令查看var目录大小
```
[root@10-9-128-211 ~]# du -bsh /var/
2.2G    /var/
[root@10-9-128-211 ~]#
```

4. 进入find命令找到大于100M文件 ==find . -size +100M==
可以看到，小写的m不识别
```
[root@10-9-128-211 usr]# find . -size +100m
find: invalid -size type `m'
[root@10-9-128-211 usr]# find . -size +100M
./sbin/mysqld
./sbin/mysqld-debug
[root@10-9-128-211 usr]#
```

5. du常用的选项：

>     -h：以人类可读的方式显示
>     -a：显示目录占用的磁盘空间大小，还要显示其下目录和文件占用磁盘空间的大小
>     -s：显示目录占用的磁盘空间大小，不要显示其下子目录和文件占用的磁盘空间大小
>     -c：显示几个目录或文件占用的磁盘空间大小，还要统计它们的总和
>     --apparent-size：显示目录或文件自身的大小
>     -l：统计硬链接占用磁盘空间的大小
>     -L：统计符号链接所指向的文件占用的磁盘空间大小

==du -sh==：查看当前目录总共占的容量。而不单独列出各子项占用的容量

==du -lh --max-depth=1==：查看当前目录下一级子文件和子目录占用的磁盘容量