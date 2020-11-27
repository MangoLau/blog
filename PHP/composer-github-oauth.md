## 设置github oauth token
首先去生成token
https://github.com/settings/tokens
选择 Personal access tokens
scope：repo全选

```shell
composer config --global --auth github-oauth.github.com <token>
```
查看全局配置是否生效
```shell
composer config -l -g
```