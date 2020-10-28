## ssh-keygen制作免密登录
A机器免密登录B机器，A机器IP（192.168.1.210），B机器IP（192.168.1.211），正确姿势：
1. 在A机器上生成密码：
```
ssh-keygen -t rsa
```

2. 把A机器生成的公钥id_rsa.pub文件发送到B机器上：
```
ssh-copy-id -i /root/.ssh/id_rsa.pub root@192.168.1.111
```
系统自动在B机器root/.ssh目录中生成authorized_keys文件

3. 在A机器上测试SSH登录B机器免密：
```
ssh 192.168.1.111
```