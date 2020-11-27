## BOM头是什么？解释BOM头和去掉方法

### 什么是bom头？

在utf-8编码文件中BOM在文件头部，占用三个字节，用来标示该文件属于utf-8编码，现在已经有很多软件识别bom头，但是还有些不能识别bom头，比如PHP就不能识别bom头，这也是用记事本编辑utf-8编码后执行就会出错的原因了。

### 去掉bom头的办法，简单的是下面两种：

#### 1、editplus去BOM头的方法

编辑器调整为UTF8编码格式后，保存的文件前面会多出一串隐藏的字符（也即是BOM），用于编辑器识别这个文件是否是以UTF8编码。

运行Editplus，点击工具，选择首选项，选中文件，UTF-8标识选择 总是删除签名，


然后对PHP文件编辑和保存后的PHP文件就是不带BOM的了。


#### 2、ultraedit去除bom头办法

打开文件后，另存为选项的编码格式里选择（utf-8 无bom头），确定就ok了

怎么样，去掉bom头很简单吧

再来一段议论utf8的BOM信息的
BOM是指php文件本身的存储方式为带BOM的UTF-8,普通页面的中文乱码方式一般不是由这个原因导致的。

header("Content-type: text/html; charset=utf-8");
这句话控制html输出页面的编码方式，

BOM只有在WINDOWS下采用“记事本”存储为UTF-8时才会有，这个可以用WINHEX把开始的2个字节删掉。
在dreamweaver里面编码设置里面可以设置是否带BOM,一般只要php输出的不是图片(GDI Stream），BOM都不会导致问题。
GDI Stream如果开头有了额外的 字符就会显示为 红叉。