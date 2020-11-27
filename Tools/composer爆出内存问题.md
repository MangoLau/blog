## composer 爆出内存问题
使用下面的命令，去掉内存限制
```shell
COMPOSER_MEMORY_LIMIT=-1 composer update -vvv
```