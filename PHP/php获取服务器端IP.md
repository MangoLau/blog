## php获取服务器端IP

服务器端IP相关的变量

a. $_SERVER["SERVER_NAME"]，需要使用函数gethostbyname()获得。这个变量无论在服务器端还是客户端均能正确显示。

b. $_SERVER["SERVER_ADDR"]，在服务器端测试：127.0.0.1（这个与httpd.conf中BindAddress的设置值相关）。在客户端测试结果正确。

```php

/**
* 获取服务器端IP地址
 * @return string
 */
function getServerIp() { 
    if (isset($_SERVER)) { 
        if($_SERVER['SERVER_ADDR']) {
            $server_ip = $_SERVER['SERVER_ADDR']; 
        } else { 
            $server_ip = $_SERVER['LOCAL_ADDR']; 
        } 
    } else { 
        $server_ip = getenv('SERVER_ADDR');
    } 
    return $server_ip; 
}

// 或者
function getServerIP(){    
    return gethostbyname($_SERVER["SERVER_NAME"]);    
}
```