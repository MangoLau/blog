## 生成一个RSA密钥 
openssl genrsa -des3 -out server.key 1024

## 拷贝一个不需要输入密码的密钥文件
openssl rsa -in server.key -out server_nopass.key

## 生成一个证书请求
openssl req -new -key server.key -out server.csr

## 自己签发证书
openssl x509 -req -days 3650 -in server.csr -signkey server.key -out server.crt

## nginx配置
编辑nginx配置文件 nginx.conf，加https协议
```
server {
    server_name tfjybj.com;
    listen 443;
    ssl on;
    ssl_certificate /usr/local/nginx/conf/tfjybj.crt;
    ssl_certificate_key /usr/local/nginx/conf/tfjybj_nopass.key;
    # 若ssl_certificate_key使用tfjybj.key，则每次启动Nginx服务器都要求输入key的密码。
    （开始我不知道，纳闷为啥启动nginx、关闭nginx都要输入密码）
}
```
重启Nginx 
自己颁发的SSL证书能够实现加密传输功能，但浏览器并不信任，会给出安全提示