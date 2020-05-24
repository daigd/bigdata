#### no device found for connection报错问题
> 使用`ifconfig -a` 命令找出对应网关(比如eth3)的HWaddr地址;

> `sudo vi /etc/sysconfig/network-scripts/ifcfg-Auto_eth3`,将对应的HWADDR值修改正确；

> 重启网关服务:`sudo service network restart`。

#### 设置主机名与IP的映射
>  修改主机名:`sudo vi /etc/sysconfig/network`;

> 设置主机名与IP的映射:`sudo vi /etc/hosts` 。

> 更改虚拟机linux系统的ip地址: `sudo vi /etc/sysconfig/network-scripts/ifcfg-eth0 `(把`ifcfg`开头的几个文件都改一下)

在ContOS 7下：
```shell

[root@daigd ~]# ifconfig -a
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.197.128  netmask 255.255.255.0  broadcast 192.168.197.255
        inet6 fe80::3cef:3f45:cb27:334d  prefixlen 64  scopeid 0x20<link>
        ether 00:50:56:2c:79:d0  txqueuelen 1000  (Ethernet)
        RX packets 7579  bytes 8829614 (8.4 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1721  bytes 119786 (116.9 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 2  bytes 98 (98.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2  bytes 98 (98.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

[root@daigd ~]# vi /etc/sysconfig/network-scripts/ifcfg-ens33

# 编辑以下文件
BROWSER_ONLY="no"
BOOTPROTO="dhcp"  # dhcp 改成 static
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="3ee7f2ee-8b82-46f2-8006-d84c44dff93c"
DEVICE="ens33"
ONBOOT="yes" # 是否开机启用的意思
IPADDR=192.168.1.154 # 设置指定的IP地址，没有就加上这行，IP网段要在子网IP网段内,子网IP为：192.168.1.0
NETMASK=255.255.255.0 # 子网掩码
GATEWAY=192.168.1.2 # 要跟虚拟机设置的网关地址一样
DNS=8.8.8.8

 ```

> 更改完毕后,重启网络服务: `sudo service network restart` 。

#### `ping www.baidu.com` 提示 `ping: unknown host www.baidu.com` 
> 如果`ping 8.8.8.8` 能通,可知是dns服务器没配好,
执行`sudo vi /etc/resolv.conf`,添加` nameserver 8.8.8.8` 即可。

#### 重启网络服务时,提示`this device is not active` 解决方法:
停止networkmaager服务
> `sudo service NetworkManager stop`

禁止自启动
> `sudo chkconfig NetworkManager off`
  
重启网络服务
>`service network restart`

原因是:安装了networmanager服务，同时两个服务network和networmanager管理网络导致冲突。


#### 解决 ```dgd 不在 sudoers 文件中。此事将被报告。```的问题
```shell
# 1 查看权限
ls -l /etc/sudoers
# 2. 如果是只读模式，修改为可写权限
chmod u+w /etc/sudoers 

# 3.找到Allow root to run any commands anywhere这一行
## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
# 添加下面一行内容，dgd 是系统用户名
dgd ALL=(ALL) All

# 4.保存退出，并恢复/etc/sudoers的访问权限为440
chmod 440 /etc/sudoers

```