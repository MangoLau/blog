## 设置Vi/Vim的tab键为4个空格
### 1. 找到配置文件
- vi：/etc/virc
- vim：/etc/vimrc

### 2. 在该文件最后加上如下配置：
```
set tabstop=4
set softtabstop=4
set shiftwidth=4
set expandtab
```

- 其中 `tabstop` 表示一个 tab 显示出来是多少个空格的长度，默认 8。
- `softtabstop` 表示在编辑模式的时候按退格键的时候退回缩进的长度，当使用 `expandtab` 时特别有用。
- `shiftwidth` 表示每一级缩进的长度，一般设置成跟 `softtabstop` 一样。
- 当设置成 `expandtab` 时，缩进用空格来表示，`noexpandtab` 则是用制表符表示一个缩进。

(via)[https://www.jianshu.com/p/162c19cc9c11]