## 使用dd命令增加swap(虚拟内存)大小
服务器内存溢出，看了看虚拟内存不够用，遇到了swap分区不够的情况。问了群里的大牛，说了两种方法。一、lvm，二、dd。这里使用dd解决的。
```
# dd if=/dev/zero of=/data/swapfile bs=1M count=4096

记录了4096+0 的读入
记录了4096+0 的写出
4294967296字节(4.3 GB)已复制，38.4264 秒，112 MB/秒

# mkswap /data/swapfile

mkswap: /data/swapfile: warning: don't erase bootbits sectors
        on whole disk. Use -f to force.
Setting up swapspace version 1, size = 4194300 KiB
no label, UUID=365d973a-b1ae-4e04-9af7-f8032e22d33e

# swapon /data/swapfile
# echo "/data/swapfile swap swap defaults 0 0" >> /etc/fstab
```
### 1、创建Swap文件
```
# dd if=/dev/zero of=/data/swapfile bs=1M count=4096 
```
将/dev/zero内容写入/data/swapfile，读写块大小1024bytes ，块个数4096。 
/dev/zero是个未使用的文件模版，可以用它来创建“干净”的文件。后两个参数可以控制文件大小。
### 2、把这个文件变成swap文件
```
# mkswap /data/swapfile
```
### 3、激活使用这个swap文件
```
# swapon /data/swapfile 
```
查看状态：
```
# swapon -s 
Filename Type Size Used Priority 
/dev/dm-1 partition 2031608 0 -1 
/data/swapfile file 4194296 0 -2
```
### 4、设置开机启用 
```
# vi /etc/fstab --添加如下内容
/data/swapfile swap swap defaults 0 0
```
### 删除多余的swap空间 
#### 1、使用Swapoff命令收回Swap空间。
```
# swapoff swapfile
```
#### 2、编辑/etc/fstab文件，去掉此Swap文件的实体。
#### 3、从文件系统中回收此文件。
```
# rm swapfile
```