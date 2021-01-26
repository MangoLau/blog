## Centos7 重装 yum
因为不知道 Centos7 的 yum 依赖于 Python2.7.5，需要用到 Python3，然后无知的覆盖安装，导致 yum 不能使用，花了很多时间修复不成功，最后选择了重装 yum，现记录下重装步骤。

### rpm常用参数
```
-i: --install，安装包
-U: --update，更新包
-e: --erase，删除包
```

### 卸载 Python
```shell
rpm -qa| grep python| xargs rpm -ev --allmatches --nodeps
whereis python |xargs rm -frv
```
### 卸载 yum
```shell
rpm -qa|grep yum|xargs rpm -ev --allmatches --nodeps
whereis yum |xargs rm -frv
```
### 下载 rpm 包
注意需要根据自己的系统大版本号去下载对应的rpm包。仓库地址查看系统版本可以用下面的命令
```
cat /etc/redhat-release
```

下载包：
```
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/libxml2-python-2.9.1-6.el7.4.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/libxml2-python-2.9.1-6.el7.4.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/rpm-4.11.3-43.el7.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/rpm-build-4.11.3-43.el7.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/rpm-build-libs-4.11.3-43.el7.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/rpm-libs-4.11.3-43.el7.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/rpm-sign-4.11.3-43.el7.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/rpm-python-4.11.3-43.el7.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/python-2.7.5-88.el7.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/dbus-python-devel-1.1.1-9.el7.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/python-libs-2.7.5-88.el7.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/python-pycurl-7.19.0-19.el7.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/python-setuptools-0.9.8-7.el7.noarch.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/python-urlgrabber-3.10-10.el7.noarch.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/python-iniparse-0.4-9.el7.noarch.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/python-backports-1.0-8.el7.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/python-backports-ssl_match_hostname-3.5.0.1-1.el7.noarch.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/python-chardet-2.2.1-3.el7.noarch.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/python-ipaddress-1.0.16-2.el7.noarch.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/python-kitchen-1.1.1-5.el7.noarch.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/python-virtualenv-15.1.0-2.el7.noarch.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/yum-3.4.3-167.el7.centos.noarch.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/yum-metadata-parser-1.1.4-10.el7.x86_64.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/yum-utils-1.1.31-53.el7.noarch.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/yum-plugin-fastestmirror-1.1.31-53.el7.noarch.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/yum-plugin-protectbase-1.1.31-53.el7.noarch.rpm
wget https://vault.centos.org/7.8.2003/os/x86_64/Packages/yum-plugin-aliases-1.1.31-53.el7.noarch.rpm
```

### 安装更新 rpm 包
```
rpm -Uvh --force --nodeps --replacepkgs *.rpm
```

### 检查
输入 python 和 yum 命令进行测试，没有异常，安装完成。

### 安装 pip
```
yum install python-pip
pip install --upgrade pip
```