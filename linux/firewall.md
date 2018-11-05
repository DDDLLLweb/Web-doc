CentOS7中firewall防火墙详解和配置，.xml服务配置详解
修改防火墙配置文件之前，需要对之前防火墙做好备份

重启防火墙后，需要确认防火墙状态和防火墙规则是否加载，若重启失败或规则加载失败，则所有请求都会被防火墙

1.
firewall-cmd --state        
#查看firewall的状态
firewall-cmd --list-all 
#查看防火墙规则（只显示/etc/firewalld/zones/public.xml中防火墙策略）
firewall-cmd --list-all-zones 
#查看所有的防火墙策略（即显示/etc/firewalld/zones/下的所有策略）
firewall-cmd --reload
#重新加载配置文件
 
2、关闭firewall：
systemctl stop firewalld.service #停止firewall
systemctl disable firewalld.service #禁止firewall开机启动
firewall-cmd --state #查看默认防火墙状态（关闭后显示notrunning，开启后显示running）
 
1、firewalld的基本使用
启动： systemctl start firewalld
查看状态： systemctl status firewalld 
停止： systemctl disable firewalld
禁用： systemctl stop firewalld
 
2.systemctl是CentOS7的服务管理工具中主要的工具，它融合之前service和chkconfig的功能于一体。
启动一个服务：systemctl start firewalld.service
关闭一个服务：systemctl stop firewalld.service
重启一个服务：systemctl restart firewalld.service
显示一个服务的状态：systemctl status firewalld.service
在开机时启用一个服务：systemctl enable firewalld.service
在开机时禁用一个服务：systemctl disable firewalld.service
查看服务是否开机启动：systemctl is-enabled firewalld.service
查看已启动的服务列表：systemctl list-unit-files|grep enabled
查看启动失败的服务列表：systemctl --failed

3.配置firewalld-cmd

查看版本： firewall-cmd --version
查看帮助： firewall-cmd --help
显示状态： firewall-cmd --state
查看所有打开的端口： firewall-cmd --zone=public --list-ports
更新防火墙规则： firewall-cmd --reload
查看区域信息:  firewall-cmd --get-active-zones
查看指定接口所属区域： firewall-cmd --get-zone-of-interface=eth0
拒绝所有包：firewall-cmd --panic-on
取消拒绝状态： firewall-cmd --panic-off
查看是否拒绝： firewall-cmd --query-panic
 
那怎么开启一个端口呢
添加
firewall-cmd --zone=public --add-port=15672/tcp --permanent    （--permanent永久生效，没有此参数重启后失效）
重新载入
firewall-cmd --reload
查看
firewall-cmd --zone=public --query-port=6379/tcp
删除
firewall-cmd --zone=public --remove-port=15672/tcp --permanent

