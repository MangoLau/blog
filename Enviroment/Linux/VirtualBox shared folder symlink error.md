由于VDI硬盘在删除文件后，不能自动缩小尺寸，写新的文件也不会占用已删除的空间，造成VDI体积越来越大。后来，就想将文件放到share folder。若干次发现，在解压缩时会出现Cannot create a symlink to ...... Read only file system. 的提示，造成解压缩失败。

转载两篇关于如何在shared folder 创建symlink的文章，下面是两个非常重要的地方，一定要加以注意。

1. 以管理员身份运行CMD

2. 以管理员身份启动虚拟机

### 第一篇：
**解決VirtualBox的共用資料夾無法建立軟連結**

我的電腦我習慣上桌面還是用Windows，但是開Server下指令還是Linux系統方便多了，所以開發時需要執行東西我會用VirtualBox開Ubuntu來進行。
VirtualBox有個方便的功能稱為共用資料夾(shared folder)，因此我習慣將在Windows的workspace利用共用資料夾分享到Ubuntu (VM)裡面，然後由Ubuntu來開server。
但是最近開始玩Node.js發現，VirtualBox共用資料夾安裝是無法增加軟連結(Symbolic link)。因為安裝npm package的時候經常需要增加軟連結的權限，因此安裝的最後就會出現錯誤訊息！
```
npm ERR! Error: EROFS, symlink '../express/bin/express'

npm ERR! If you need help, you may report this log at:

npm ERR!

npm ERR! or email it to:

npm ERR!     

npm ERR! System Linux 2.6.38-13-generic

npm ERR! command "/PATH-To-NODE/bin/node" "/PATH-To-NODE/bin/npm" "install" "express"

npm ERR! cwd /media/sf_SHARED_FOLDER/test

npm ERR! node -v v0.8.15

npm ERR! npm -v 1.1.66

npm ERR! path ../express/bin/express

```

而在console執行建立軟連結的指令也會錯誤
```
$ ln -sf /SOME_PATH/SOME_FILE /media/sf_SHARED_FOLDER/A_SYMBOLIC_LINK
ln: creating symbolic link `/media/sf_SHARED_FOLDER/A_SYMBOLIC_LINK': Read-only file system
```

google之後看到這篇文章發現原來python virtualenv也有類似的問題。總之在HOST OS為Windows 7、Client OS (VM)為 Ubuntu Linux的時候，共用資料夾就會發生軟連結無法寫入的情況！

修正方法如下，請在HOST (Windows) 開啟cmd (需要管理員權限開啟)：
```
c:\Oracle\VirtualBox\VBoxManage setextradata YOURVMNAME VBoxInternal2/SharedFoldersEnableSymlinksCreate/YOURSHAREFOLDERNAME 1
```
上面的參數請視情況修改：
```
c:\Oracle\VirtualBox\ ： 這是VirtualBox的預設資料夾，如果安裝在不同資料夾，請自行修改。
YOURVMNAME ： 請修改為你要開啟的VM名稱。
YOURSHAREFOLDERNAME ：修改為VM設定中共用資料夾的名稱
```
demo:
```
E:\Oracle\VirtualBox\VBoxManage setextradata lnmp_1 VBoxInternal2/SharedFoldersEnableSymlinksCreate/wwwroot 1
```

確認是不是設定成功了，可以用這個指令查看：
```
c:\Oracle\VirtualBox\VBoxManage getextradata YOURVMNAME enumerate
```
設定完畢後就可以順利使用了！
如果設定後出現這樣的錯誤：
```
ln: failed to create symbolic link `LINKNAME': Protocol error
```
表示你的VirtualBox不是用管理員權限打開。

### 第二篇：
http://ahtik.com/blog/2012/08/16/fixing-your-virtualbox-shared-folder-symlink-error/