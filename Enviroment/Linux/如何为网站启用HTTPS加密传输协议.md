## 前言
当今时代对上网的安全性要求比以前更高，chrome和firefox也都大力支持网站使用HTTPS，苹果也从2017年开始在iOS 10系统中强制app使用HTTPS来传输数据，微信小程序也是要求必须使用HTTPS请求，由此可见HTTPS势在必行。

本文主要介绍一下什么是HTTPS，以及如何使用Let’s Encrypt免费证书为网站启用HTTPS加密传输协议。
## HTTPS简介
HTTP协议被用于在Web浏览器和网站服务器之间传递信息。HTTP协议以明文方式发送内容，不提供任何方式的数据加密，如果攻击者截取了Web浏览器和网站服务器之间的传输报文，就可以直接读懂其中的信息，因此HTTP协议不适合传输一些敏感信息，比如信用卡号、密码等。

为了解决HTTP协议的这一缺陷，需要使用另一种协议：安全套接字层超文本传输协议HTTPS（全称：Hyper Text Transfer Protocol over Secure Socket Layer） 。为了数据传输的安全，HTTPS在HTTP的基础上加入了SSL协议，SSL依靠证书来验证服务器的身份，并为浏览器和服务器之间的通信加密。
### http与https的几点区别：
- 1、https协议需要到CA申请证书，一般是收费的。
- 2、http协议运行在TCP之上，所有传输的内容都是明文，https运行在SSL/TLS之上，SSL/TLS运行在TCP之上，所有传输的内容都经过加密的。
- 3、http与https是两种不同的链接方式，端口也不一样，http使用80端口，https使用443端口。
- 4、http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全。
## HTTPS解决的问题
### 信任主机的问题
采用https的服务器必须从CA （Certificate Authority）申请一个用于证明服务器用途类型的证书。该证书只有用于对应的服务器的时候，客户端才信任此主机。
### 通讯过程中的数据的泄密和被篡改
由于http协议下传输的内容都是明文，在数据传输的整个链路中任意一个节点（路由器、WIFI热点、网络运营网等等）都可以拿到传输的数据从而进行窜改（比如强行插入广告）。
## HTTPS SSL证书的选择
SSL证书，用于加密HTTP协议，也就是HTTPS。随着淘宝、百度等网站纷纷实现全站Https加密访问，搜索引擎对于Https更加友好，加上互联网上越来越多的人重视隐私安全，站长们给网站添加SSL证书似乎成为了一种趋势。

如果是首次考虑为网站部署HTTPS，估计在选择证书上会有些头疼，查阅一些资料后发现这里要考虑的因素确实有很多，比如是否支持多域名、泛域名、保额、证书的价格还有浏览器上的小图标样式区别等等。
目前主注的SSL证书主要分为DV SSL、OV SSL、EV SSL、还有自签名证书。
### DV SSL（ Domain Validation SSL）
DV SSL证书是只验证网站域名所有权的简易型（Class 1级）SSL证书，可10分钟快速颁发，能起到加密传输的作用，但无法向用户证明网站的真实身份。 目前市面上的免费证书都是这个类型的，只是提供了对数据的加密，但是对提供证书的个人和机构的身份不做验证。
### OV SSL （Organization Validation SSL）
OV SSL,提供加密功能,对申请者做严格的身份审核验证,提供可信身份证明。 和DV SSL的区别在于，OV SSL 提供了对个人或者机构的审核，能确认对方的身份，安全性更高。 所以这部分的证书申请是收费的。
### EV SSL （ Extended Validation SSL Certificate）
最安全、最严格EV SSL证书遵循全球统一的严格身份验证标准，是目前业界安全级别最高的顶级 (Class 4级)SSL证书。 金融证券、银行、第三方支付、网上商城等，重点强调网站安全、企业可信形象的网站，涉及交易支付、客户隐私信息和账号密码的传输。 这部分的验证要求最高，申请费用也是最贵的。

以上证书在浏览器上都会显示红色小锁头的图标，例如：
0?wx_fmt=png
也可以通过浏览器来查看某个网站的证书信息来得知这个网站使用的是什么机构提供的证书。

本文重点讲解免费DV证书的选择，一般个人网站或创业公司初期的网站都可以选用此类证书。

![image](https://ss.csdn.net/p?http://mmbiz.qpic.cn/mmbiz_png/0SRQ1fZzqkzic7Kic5jwSYgs62cOTeAmhWiaa8jBUCNRqvgib2yofEgFHHDmpK6cXKbFE8Ql6tzZ9bvj6qE4jHF9Kg/0?wx_fmt=png)

也可以通过浏览器来查看某个网站的证书信息来得知这个网站使用的是什么机构提供的证书。

本文重点讲解免费DV证书的选择，一般个人网站或创业公司初期的网站都可以选用此类证书。

![image](https://ss.csdn.net/p?http://mmbiz.qpic.cn/mmbiz_png/0SRQ1fZzqkzic7Kic5jwSYgs62cOTeAmhW31DVv4cTHVlnAnOtdbDPVKIicUfAody5ESkzIAbAP7Zpu9OC7Ne4R7Q/0?wx_fmt=png)
- 1、Let’s Encrypt是国外一个公共的免费SSL项目，由 Linux 基金会托管，它的来头不小，由Mozilla、思科、Akamai、IdenTrust和EFF等组织发起，目的就是向网站自动签发和管理免费证书，以便加速互联网由HTTP过渡到HTTPS。
- 2、Let’s Encrypt安装部署简单、方便，目前也已经有现成的安装脚本，可以很快的完成证书的申请及发放。
- 3、目前Let’s Encrypt免费证书的有效期只有90天，由于Let’s Encrypt的证书属于自动签发的，所以我们也可以自己写脚本来实现定期自动更新Let’s Encrypt证书，达到一劳永逸的效果。

![image](https://ss.csdn.net/p?http://mmbiz.qpic.cn/mmbiz_png/0SRQ1fZzqkzic7Kic5jwSYgs62cOTeAmhW8zicCVicqKmbgiaUTyHzJHbBTGx2nWIic9v0Gx4GJMK71dX3mln5RT2fXw/0?wx_fmt=png)
- 1、StartSSL是StartCom公司旗下的SSL证书，应该算是免费SSL证书中的“鼻祖”，最早提供完全免费的SSL证书并且被各大浏览器所支持的恐怕就只有StartSSL证书了。任何个人都可以从StartSSL中申请到免费一年的SSL证书。
- 2、如果你有看新闻，也许已经知道了“Mozilla正式提议将停止信任 WoSign 和 StartCom 签发的新证书”，对于StartSSL请观察事态发展后再谨慎使用。
## Let’s encrypt证书安装
![image](https://ss.csdn.net/p?http://mmbiz.qpic.cn/mmbiz_png/0SRQ1fZzqkzic7Kic5jwSYgs62cOTeAmhWuDceudAdsxXDInBw3Dn0Duniau5qic1LnfMYtqMxTLHNynzYTzypqnwg/0?wx_fmt=png)
利用脚本快速获取Let’s Encrypt SSL证书，官方推荐的自动化证书颁发和安装脚本是Certbot，所以这里也是采用这个脚本进行安装（执行脚本需要root权限 ）。
- 获取脚本：
```
$ wget https://dl.eff.org/certbot-auto
$ chmod a+x certbot-auto
$ mv certbot-auto /usr/local/sbin/
```
- 获取证书：
```
$ cd /usr/local/sbin
$ ./certbot-auto certonly
```
获取证书过程中Let’s encrypt需要验证用户对域名的所有权，根据提示操作即可。

![image](https://ss.csdn.net/p?http://mmbiz.qpic.cn/mmbiz_png/0SRQ1fZzqkzic7Kic5jwSYgs62cOTeAmhW69l8cSV3gCfT21O6aBet4xDs93kyuib7tIc9E8J7Z5uKV8NHr5wImjg/0?wx_fmt=png)

选择第一个使用webroot方式进行身份验证

![image](https://ss.csdn.net/p?http://mmbiz.qpic.cn/mmbiz_png/0SRQ1fZzqkzic7Kic5jwSYgs62cOTeAmhWjrg4H8YJ5cicKoB7xUfbtt0lO4VYkyakWdn6oWCrvr1INejJcPaXBdQ/0?wx_fmt=png)

输入你的邮箱

![image](https://ss.csdn.net/p?http://mmbiz.qpic.cn/mmbiz_png/0SRQ1fZzqkzic7Kic5jwSYgs62cOTeAmhWcNG8kJCib3vfxr6KDheDwnOwgiajLDrzibppIFfIE6MnDdmlBjIibzBmTg/0?wx_fmt=png)

Agree

![image](https://ss.csdn.net/p?http://mmbiz.qpic.cn/mmbiz_png/0SRQ1fZzqkzic7Kic5jwSYgs62cOTeAmhWU5cbxak6tRV3uvUn4a8GeKB8y6b3yrxlWMC0FGiavUpH39QLZuMgrUA/0?wx_fmt=png)

输入你的域名（以空格分割）domain.com www.domain.com

![image](https://ss.csdn.net/p?http://mmbiz.qpic.cn/mmbiz_png/0SRQ1fZzqkzic7Kic5jwSYgs62cOTeAmhWE0zAVBnJcRL5QeiaEKjZCl5bicop8jeYhyLucMI1htfeQ0pZKicj1qXDw/0?wx_fmt=png)

输入并选择自己的站点根目录，也可以自己指定一个目录，要确保能够访问到，获取证书过程中Let’s encrypt会在站点目录下随机生成一个文件，并在外部访问，以验证你是否对这个域名有所有权。

- 自定义目录，如下Nginx配置：
```
location /.well-known/acme-challenge/ {
    root /var/www/challenges/;
}
```
验证过程及生成证书过程输出：

![image](https://ss.csdn.net/p?http://mmbiz.qpic.cn/mmbiz_png/0SRQ1fZzqkzic7Kic5jwSYgs62cOTeAmhWlv07RWD2hZhhMT4BQXcxEb38SfCKYKpMcJF8QxxPYI9ibxicz0eBrnkg/0?wx_fmt=png)

以上图形安装步骤过程也可以统一使用一条命令指定参数来实现，直接在命令行中指定webroot目录以及需要获取证书的域名：
```
$ ./certbot-auto certonly --webroot -w /var/www/challenges -d example.com -d www.example.com
```
最终输出如下内容，表示证书已经获取成功：

![image](https://ss.csdn.net/p?http://mmbiz.qpic.cn/mmbiz_png/0SRQ1fZzqkzic7Kic5jwSYgs62cOTeAmhWDcIuRHibCJUDpiaD1Ef7fJdrSflhN4OZyjfDLliczPAn7JlxaibJoEbiajw/0?wx_fmt=png)

查看一下生成的证书等文件：

![image](https://ss.csdn.net/p?http://mmbiz.qpic.cn/mmbiz_png/0SRQ1fZzqkzic7Kic5jwSYgs62cOTeAmhWMZ2ibYqlc95SAoNgXWeL1ISxibp4S6ZbIofHOZicv79UHIyEWcvGzt9KQ/0?wx_fmt=png)

如果网站是多台服务器集群，也不需要再重新执行以上步骤，直接拷贝这个证书目录/etc/letsencrypt到集群中另外的服务器即可，注意这里面有4个软链接需要重建。

配置网站使用HTTPS访问，在Nginx中需要同时绑定两个Server分别监听80（HTTP）及443（HTTPS）端口。
Nginx关键配置清单：
```
server {
    listen  80;
    server_name  yourdomain.com www.yourdomain.com;
    ##80端口接收的请求直接重定向到HTTPS端口
    return 301 https://www.yourdomain.com$request_uri;
    ##....
}
server {
    listen 443 default ssl;
    ssl on;
    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    server_name  yourdomain.com www.yourdomain.com;
    ##...
}
```
配置完成后重载入Nginx配置即可生效，再次通过浏览器访问你的网站就可以看到绿色小锁头的图标了。
## 部分页面&请求配置HTTPS
如果是只想部分页面使用HTTPS的话，则需要在两个server{}分别进行设置，监听80端口的server通过location来识别，如果是需要加HTTPS的请求路径，把这个请求rewrite到https（443），监听443端口的server通过location来识别，如果是不需要加HTTPS的请求时，把这个请求rewrite到http（80）。

配置部分HTTPS略为麻烦一些，因为还要考虑到页面上加载的一些资源，加HTTPS的页面中引用的资源也要是HTTPS的，不然这个HTTPS就没有什么意义（其中引用的js脚本还是有被篡改的风险）。

页面上如果引用了非https的资源，浏览器上的小图标就变成了不是绿色的，这时候需要自己逐步排查。

如果是做微信的小程序开发或者APP开发时必需要用HTTPS请求，而对应网站又不想使用HTTPS时，使用此方案即可。

**假如想实现登录页使用HTTPS，其它页面仍然是HTTP，可参考如下，Nginx部分配置清单：**
```
server {
    listen  80;
    ##其它请求转到后端服务器处理
    location / {
        proxy_pass http://server_list;
        proxy_set_header X-Real-IP $remote_addr;
    }
    ##如果是登录页面请求，重定向到https
    location ^~ /login{
        rewrite ^ https://$server_addr$request_uri? permanent;
    }
    ##...
 }
server {
    listen 443 ssl;
    ssl on;
    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;
    ##其它请求重定向到http端口进行处理
    location / {
        rewrite ^ http://$server_addr$request_uri? permanent;
    }
    ##如果是登录页面请求，转到后端服务器处理
    location ^~ /login{
        proxy_pass http://server_list;
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-Ip $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
    ##...
}
```
## Let’s Encrypt 自动更新证书
Let’s Encrypt 证书的有效期为90天，建议是每60天更新证书，以免有误差影响正常服务，证书自动更新可以通过运行certbot-auto来进行。
为所有已安装证书的域名更新，运行如下命令：
```
$ ./certbot-auto renew
```
命令执行过程会检查证书的到期日期，如果证书还未到期会提示你的证书尚未到期，输出如下:
```
Saving debug log to /var/log/letsencrypt/letsencrypt.log
-------------------------------------------------------------------------------
Processing /etc/letsencrypt/renewal/yourdomain.com.conf
-------------------------------------------------------------------------------
Cert not yet due for renewal
The following certs are not due for renewal yet:
  /etc/letsencrypt/live/yourdomain.com/fullchain.pem (skipped)
No renewals were attempted.
```
注意：如果你创建了多个域名的证书，这里也只显示基本域名的信息，但证书更新会对证书包含的所有域名有效。
为确保证书永不过期，需要增加一个cron的定期执行任务，由于更新证书脚本首先会检查证书的到期日期，并且仅当证书距离少于30天时才会执行更新，因此可以安全的创建每周甚至每天运行的cron任务。
编辑crontab来创建一个定时调度任务（需要root权限），运行：
```
$ sudo crontab -e
```
添下以下代码：
```
30 3 * * 1 /usr/local/sbin/certbot-auto renew >> /var/log/letsencrypt-renew.log
40 3 * * 1 /usr/local/nginx/sbin/nginx -s reload
```
以上代码为创建两个定时调度任务，先是在每周一的上午3:30执行certbot-auto renew命令来更新证书，然后是在每周一上午3:40时重新加载Nginx，以使用更新的证书，命令生成的log将通过管道输出到/var/log/letsencrypt-renew.log日志文件。
至此，你的网站就已经开使用免费的Let’s Encrypt TLS/SSL证书来安全地提供HTTPS内容。
## 总结
整个过程配置完毕后发现HTTPS没有想像中那么难（中大型网站例外，要考虑的因素要多一些），配置HTTPS这部分工作一般是由运维部门的同事来进行，与开发同学的关系不是很大，了解即可。