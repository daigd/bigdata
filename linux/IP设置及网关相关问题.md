#### no device found for connection报错问题
> 使用`ifconfig -a` 命令找出对应网关(比如eth3)的HWaddr地址;

> `sudo vi /etc/sysconfig/network-scripts/ifcfg-Auto_eth3`,将对应的HWADDR值修改正确；

> 重启网关服务:`sudo service network restart`。