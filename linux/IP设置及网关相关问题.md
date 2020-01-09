#### no device found for connection报错问题
> 使用`ifconfig -a` 命令找出对应网关(比如eth3)的HWaddr地址;

> `sudo vi /etc/sysconfig/network-scripts/ifcfg-Auto_eth3`,将对应的HWADDR值修改正确；

> 重启网关服务:`sudo service network restart`。

> 设置主机名与IP的映射: `sudo vi /etc/hosts` 。

> 更改虚拟机linux系统的ip地址: `sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0 `(把`ifcfg`开头的几个文件都改一下),
> 更改完毕后,重启网络服务: `sudo service network restart` 。

`ping www.baidu.com` 提示 `ping: unknown host www.baidu.com` ,但是`ping 8.8.8.8` 能通,可知是dns服务器没配好,
执行`sudo vi /etc/resolv.conf`,添加` nameserver 8.8.8.8` 即可。

#### 重启网络服务时,提示`this device is not active` 解决方法:
停止networkmaager服务
> `sudo service NetworkManager stop`

禁止自启动
> `sudo chkconfig NetworkManager off`
  
重启网络服务
>`service network restart`

原因是:安装了networmanager服务，同时两个服务network和networmanager管理网络导致冲突。