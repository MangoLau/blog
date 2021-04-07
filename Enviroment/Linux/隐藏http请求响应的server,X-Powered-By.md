## 隐藏 HTTP 响应的 server,X-Powered-By
### 隐藏X-Powered-By
修改 `php.ini` 文件 设置 `expose_php = Off`



### nginx 隐藏 server
修改 `nginx.conf` 在 http 里面设置 
`server_tokens off;`