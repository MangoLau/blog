## Mac ssh使用pem文件登录远程服务器
登录远程服务器我们可以使用ssh命令，部分远程服务器访问需要授权，ssh命令支持使用pem文件进行授权访问。

## 命令如下：
```
ssh -i identity_file user@hostname
```

例如：
```
ssh -i key.pem root@192.168.1.111
```

如果执行后出现以下错误，表示pem文件的权限太大，需要设置为只有拥有者读写权限(600)。
```
Permissions 0644 for ‘key.pem’ are too open. 
It is required that your private key files are NOT accessible by others. 
This private key will be ignored. 
Load key “key.pem”: bad permissions 
Permission denied (publickey).
```

修改pem文件权限
```
sudo chmod 0600 key.pem
```
修改后错误提示消失，可正常登录。 

如果需要长期登录远程服务器，可以使用ssh-add把pem文件添加，下次直接登录。
```
ssh-add -K key.pem
ssh root@192.168.1.111
```