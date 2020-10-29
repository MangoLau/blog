Redis keys命令支持模式匹配，但是del命令不支持模式匹配，有时候需要根据一定的模式来模糊删除key，这时只能结合shell命令来完成了。 具体命令是： 
```shell
redis-cli KEYS "pattern" | xargs redis-cli DEL 
```
其中pattern是keys命令支持的模式，这样就可以模糊删除key了。
注意注意这是shell命令，不是redis的命令！！

**example**
```shell
/usr/bin/redis-cli -h 192.168.0.210 -p 6377 keys 'event*' | xargs /usr/bin/redis-cli -h 192.168.0.210 -p 6377 del
```
