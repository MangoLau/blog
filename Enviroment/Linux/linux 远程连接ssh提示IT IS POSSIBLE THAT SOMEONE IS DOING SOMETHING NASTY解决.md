ssh出现以下报错
```
$ ssh root@192.168.2.108
```

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
07:36:8e:d0:72:88:38:f7:21:10:c3:12:d6:35:ad:55.
Please contact your system administrator.
Add correct host key in /Users/watsy/.ssh/known_hosts to get rid of this message.
Offending RSA key in /Users/watsy/.ssh/known_hosts:1
RSA host key for 192.168.2.108 has changed and you have requested strict checking.
Host key verification failed.
```

提示以上错误
### 解决办法
```
rm -rf ~/.ssh/known_hosts
```

从新ssh连接，ok了。