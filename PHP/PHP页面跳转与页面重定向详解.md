## PHP页面跳转与页面重定向详解

首先解释下，页面跳转与页面重定向的关系？

页面重定向一定会有页面跳转，页面跳转不一定会有页面重定向，也就是说页面重定向真包含于页面跳转，页面重定向是页面跳转的充分不必要条件。

总结下PHP下的几种页面跳转的方法

### 1、meta标签实现
只需在head里加上下面这一句就行了，在当前页面停留0秒后跳转到目标页面
```php
echo '<meta http-equiv="refresh" content="0;url=https://www.baidu.com">';
```

### 2、JavaScript实现
```php
echo '<script>window.location.href = 'https://www.baidu.com';</script>';
```

### 3、PHP页面重定向实现
```php
// 先返回301状态码后再重定向
header('HTTP/1.1 301 Moved Permanently');
header('Location: https://www.baidu.com');
```

```php
header("Location: http://www.feiyuseo.com", true, 301);
exit;
```