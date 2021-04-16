## Linux 增加虚拟内存Swap（使用文件）
### 1、简介
如果你的服务器的总是报告内存不足，并且时常因为内存不足而引发服务被强制 kill 的话，在不增加物理内存的情况下，启用 swap 交换区作为虚拟内存是一个不错的选择。　　　　

为了测试一些功能我在阿里云购买了 1 核 1G 的 ECS 服务器几台（最便宜的了，再贵舍不得啊）,一台服务器就安装了 LANMP，redis,memcache,elk 等等耗内存较大的软件，内存各种不够用啊，这时候虚拟内存就派上用场了。

虚拟内存一般设置为物理内存的2倍即可，多了也是浪费硬盘。

### 2、新增 swap 分区
由于服务器已经安装了各种软件，懒得重新给硬盘分区，所以这里使用文件作为 swap 分区 ，下面操作需要在 root 用户下操作
```
[root@iZ28bmksx9mZ ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           990M        282M         77M         50M        631M        508M
Swap:            0B          0B          0B
```

使用 free -h 查看当前内存占用情况，可以看到物理内存所剩无几，下面的 swap 也是使用的文件作为虚拟内存使用的。


创建要作为 swap 分区的文件:增加 1GB 大小的交换分区，则命令写法如下，其中的 count 等于想要的块的数量（bs*count= 文件大小），如下面是 2G
```
# dd if=/dev/zero of=/root/swapfile2 bs=1M count=2048
```

```
[root@iZ28bmksx9mZ ~]# dd if=/dev/zero of=/root/swapfile bs=1M count=2048
记录了2048+0 的读入
记录了2048+0 的写出
2147483648字节(2.1 GB)已复制，24.6681 秒，87.1 MB/秒
[root@iZ28bmksx9mZ ~]# ll -h
总用量 2.1G
-rw-r--r-- 1 root root    0 11月  1 2019 chown
-rw-r--r-- 1 root root 2.0G 11月 12 22:20 swapfile
```

这里我使用的 of 为 /root/swapfile ，可以看到该文件是新创建的，这时候这个文件还不能直接使用为 swap 文件

修改文件权限，如不修改，在启用swap文件的时候会提示建议修改权限的信息（不影响使用，建议修改）
```
# chmod 0600 /root/swapfile
```

格式化为交换分区文件，建立 swap 的文件系统，/root/swapfile 需要与上面的 of 的值一致，这个目录可以自定义
```
# mkswap /root/swapfile
```
```
[root@iZ28bmksx9mZ ~]# mkswap /root/swapfile
正在设置交换空间版本 1，大小 = 2097148 KiB
无标签，UUID=4cbbabb5-01b3-4f73-8609-129b81708e91
```

启用swap文件:
```
# swapon /root/swapfile
```
```
[root@iZ28bmksx9mZ ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           990M        285M         73M         50M        632M        511M
Swap:            0B          0B          0B
[root@iZ28bmksx9mZ ~]# swapon /root/swapfile
[root@iZ28bmksx9mZ ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           990M        287M         71M         50M        632M        509M
Swap:          2.0G          0B        2.0G
```

可以看到未启用时虚拟内存为2G，启用后内存增加了2G

使系统开机时自启用
```
# vim /etc/fstab
```

在文件/etc/fstab中添加一行
```
/root/swapfile swap swap defaults 0 0
```

### 3、调整 swap 空间使用的优先级
如果内存够大，应当告诉 Linux 不必太多的使用 SWAP 分区， 可以通过修改 swappiness 的数值。

swappiness=0 的时候表示最大限度使用物理内存，然后才是 swap 空间，swappiness＝100 的时候表示积极的使用 swap 分区，并且把内存上的数据及时的搬运到 swap 空间里面。

各个操作系统的优先级可能都不一样，如果不调整，你会发现添加的虚拟内存几乎没有用到。

查看当前 swappiness 值：
```
# cat /proc/sys/vm/swappiness
```

修改 swappiness 值为 60（临时修改，重启后即还原为默认值）：
```
# sudo sysctl vm.swappiness=60
```
```
[root@iZ28bmksx9mZ ~]# cat /proc/sys/vm/swappiness
0
[root@iZ28bmksx9mZ ~]# sudo sysctl vm.swappiness=60
vm.swappiness = 60
[root@iZ28bmksx9mZ ~]# cat /proc/sys/vm/swappiness
60
```

永久修改swappiness默认值（重启生效）：
```
# vim /etc/sysctl.conf
```
找到 vm.swappiness，如果没有则需要手动添加一行 vm.swappiness = 60 保存即可。


参考博客：
https://www.cnblogs.com/chennl/p/10167088.html