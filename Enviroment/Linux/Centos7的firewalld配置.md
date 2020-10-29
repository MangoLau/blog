Centos6升级到Centos7以后，默认的防火墙发生了变化。从iptables变为了firewalld。下面我就说下firewalld的简单用法，如果不对之处还望指正！
首先，附上红帽官方的使用文档:
https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Security_Guide/sec-Using_Firewalls.html
```
# 查看版本
[root@osboxes java]# firewall-cmd --version
0.3.9
# 查看状态
[root@osboxes java]# systemctl status firewalld.service 
OR
[root@osboxes java]# firewall-cmd --state
running
# 获取启用的zone
[root@osboxes java]# firewall-cmd --get-active-zones
public
  interfaces: eno16777984
  ```
  ## 查看指定区域中开放的端口和服务
  ```
  [root@osboxes java]# firewall-cmd --zone=public --list-all
public (default, active)
  interfaces: eno16777984
  sources:
  services: dhcpv6-client mdns ssh
  ports:
  masquerade: no
  forward-ports:
  icmp-blocks:
  rich rules: 
  ```
  
  ## 查看系统中可用的服务
  ```
  # 列出已配置好可用的服务, 位于 /usr/lib/firewalld/services/ 下
[root@osboxes java]# firewall-cmd --get-services
amanda-client bacula bacula-client dhcp dhcpv6 dhcpv6-client dns ftp high-availability http https imaps ipp ipp-client ipsec kerberos kpasswd ldap ldaps libvirt libvirt-tls mdns mountd ms-wbt mysql nfs ntp openvpn pmcd pmproxy pmwebapi pmwebapis pop3s postgresql proxy-dhcp radius rpc-bind samba samba-client smtp ssh telnet tftp tftp-client transmission-client vnc-server wbem-https
# 强制列出包含用户设置在/etc/firewalld/services/, 但尚未loaded的服务
[root@osboxes java]# firewall-cmd --get-services --permanent
amanda-client bacula bacula-client dhcp dhcpv6 dhcpv6-client dns ftp high-availability http https imaps ipp ipp-client ipsec kerberos kpasswd ldap ldaps libvirt libvirt-tls mdns mountd ms-wbt mysql nfs ntp openvpn pmcd pmproxy pmwebapi pmwebapis pop3s postgresql proxy-dhcp radius rpc-bind samba samba-client smtp ssh telnet tftp tftp-client transmission-client vnc-server wbem-https
```

## 添加端口
```
# 不要忘记 --permanent 
[root@osboxes java]# firewall-cmd --zone=public --add-port=8080/tcp --permanent
# OR 添加一个地址段
[root@osboxes java]# firewall-cmd --zone=public --add-port=5060-5061/udp --permanent
success
# 需要reload后才启用, 热加载
[root@osboxes java]# firewall-cmd --reload
# OR 冷加载
[root@osboxes java]# firewall-cmd --complete-reload
success
# 能看到新端口已经添加
[root@osboxes java]# firewall-cmd --zone=public --list-all
public (default, active)
  interfaces: eno16777984
  sources:
  services: dhcpv6-client mdns ssh
  ports: 8080/tcp
  masquerade: no
  forward-ports:
  icmp-blocks:
  rich rules: 
# 删除一个端口
firewall-cmd --permanent --zone=public --remove-port=8080/tcp
firewall-cmd --permanent --zone=public --remove-port=8080/udp
```
