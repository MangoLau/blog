### 1. 下载jdk并解压
```
tar -zxvf jdk-8u221-linux-x64.tar.gz
```
### 2. 把解压后的文件转移到/lib/jvm
```
mv jdk1.8.0_221 /lib/jvm
```
### 3. 配置环境变量
如果是对所有的用户都生效就修改vi /etc/profile 文件

如果只针对当前用户生效就修改 vi ~/.bahsrc 文件

```
vim /etc/profile
```
环境变量配置内容
```
#jdk
export JAVA_HOME=/lib/jvm
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JER_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```
### 4. 使环境变量生效
```
source /etc/profile
```
### 5. 验证
使用 java -version 和 javac -version 命令查看jdk版本及其相关信息，不会出现command not found错误，且显示的版本信息与前面安装的一致
```
java version "1.8.0_221"
Java(TM) SE Runtime Environment (build 1.8.0_221-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.221-b11, mixed mode)
```