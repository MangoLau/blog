## cakephp 2 缓存写入问题
`
_cake_core_ cache was unable to write 'cake_dev_zh' to File cache 
`

### 解决方法
还请确保以下文件夹 
```
app/tmp/cache/models
app/tmp/cache/persistent
```
存在并且可写