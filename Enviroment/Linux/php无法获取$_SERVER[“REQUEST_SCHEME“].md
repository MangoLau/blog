## php无法获取$_SERVER["REQUEST_SCHEME"]

### 问题描述
PHP 打印 $_SERVER 时发现没有 REQUEST_SCHEME 的值，搜索资料后发现时 Nginx 没有配置。

### 解决方法：
在nginx配置中添加
```
location ~ \.php(.*)$ {
    ...
    fastcgi_param REQUEST_SCHEME $scheme;
}
```

配置好后重启 Nginx。