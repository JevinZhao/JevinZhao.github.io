---
title: Linux开发环境配置
date: 2021-06-10 08:17:54
tags: 项目
categories: Linux
---
## Linux操作系统安装

### 安装VMware虚拟机

前往官网[下载](https://my.vmware.com/cn/web/vmware/downloads/info/slug/desktop_end_user_computing/vmware_workstation_pro/15_0)（破解文件百度）

点击安装，一路next即可

### 虚拟机中安装CentOS7系统

打开虚拟机，点击新建虚拟机

[![2RRUij.png](https://z3.ax1x.com/2021/06/10/2RRUij.png)](https://imgtu.com/i/2RRUij)

新建虚拟机向导，选择“典型”

[![2RRDyV.png](https://z3.ax1x.com/2021/06/10/2RRDyV.png)](https://imgtu.com/i/2RRDyV)

选择要安装的CentOS7的镜像文件

[![2RR2FJ.png](https://z3.ax1x.com/2021/06/10/2RR2FJ.png)](https://imgtu.com/i/2RR2FJ)

填写虚拟机名称以及安装位置

[![2RR5y6.png](https://z3.ax1x.com/2021/06/10/2RR5y6.png)](https://imgtu.com/i/2RR5y6)

设置磁盘容量，以及是否拆分磁盘

[![2RRHTe.png](https://z3.ax1x.com/2021/06/10/2RRHTe.png)](https://imgtu.com/i/2RRHTe)

自定义硬件设置

[![2RRLYd.png](https://z3.ax1x.com/2021/06/10/2RRLYd.png)](https://imgtu.com/i/2RRLYd)

可以自定义系统内存，处理器个数，以及网络适配器（要设置成NAT）

[![2RRvlt.png](https://z3.ax1x.com/2021/06/10/2RRvlt.png)](https://imgtu.com/i/2RRvlt)

完成

[![2RW90S.png](https://z3.ax1x.com/2021/06/10/2RW90S.png)](https://imgtu.com/i/2RW90S)

启动虚拟机，安装CentOS 7 系统

[![2RWikQ.png](https://z3.ax1x.com/2021/06/10/2RWikQ.png)](https://imgtu.com/i/2RWikQ)

进入语言设置界面，选择中文

[![2RWZ60.png](https://z3.ax1x.com/2021/06/10/2RWZ60.png)](https://imgtu.com/i/2RWZ60)

继续，进入安装信息摘要界面，可以选择安装源（如果之前没选择镜像文件，这里可以选择），软件选择（按需选择，可以选择“最小安装”，或者“基本网页服务器”）

[![2RWJ6x.png](https://z3.ax1x.com/2021/06/10/2RWJ6x.png)](https://imgtu.com/i/2RWJ6x)

点击安装位置（默认是自动分区）

[![2RWNnK.png](https://z3.ax1x.com/2021/06/10/2RWNnK.png)](https://imgtu.com/i/2RWNnK)

选择“我要配置分区”，点击“完成”

[![2RWBhd.png](https://z3.ax1x.com/2021/06/10/2RWBhd.png)](https://imgtu.com/i/2RWBhd)

进入手动分区配置界面，选择标准分区，再选择“+”添加新的挂载点

[![2RWs1I.png](https://z3.ax1x.com/2021/06/10/2RWs1I.png)](https://imgtu.com/i/2RWs1I)

这里我们一般添加3个挂载点

/boot  :用于系统启动时的配置文件，大小分配200M

[![2RW6jP.png](https://z3.ax1x.com/2021/06/10/2RW6jP.png)](https://imgtu.com/i/2RW6jP)

swap  ：交换区大小，一般为内存的2倍

[![2RW2B8.png](https://z3.ax1x.com/2021/06/10/2RW2B8.png)](https://imgtu.com/i/2RW2B8)

/  :其他区，大小为剩余硬盘容量

[![2RWbuV.png](https://z3.ax1x.com/2021/06/10/2RWbuV.png)](https://imgtu.com/i/2RWbuV)

完成分区

[![2RWLHU.png](https://z3.ax1x.com/2021/06/10/2RWLHU.png)](https://imgtu.com/i/2RWLHU)

接收更改

[![2RWv4J.png](https://z3.ax1x.com/2021/06/10/2RWv4J.png)](https://imgtu.com/i/2RWv4J)

点击“网络和主机名”，进入网络配置与主机名设置，（记住IP地址，子网掩码，默认路由）

[![2RfS3R.png](https://z3.ax1x.com/2021/06/10/2RfS3R.png)](https://imgtu.com/i/2RfS3R)

开始安装

[![2RfAED.png](https://z3.ax1x.com/2021/06/10/2RfAED.png)](https://imgtu.com/i/2RfAED)

设置管理员密码，太短要点2下“完成”

[![2RfV4H.png](https://z3.ax1x.com/2021/06/10/2RfV4H.png)](https://imgtu.com/i/2RfV4H)

等待安装即可

[![2Rfuvt.png](https://z3.ax1x.com/2021/06/10/2Rfuvt.png)](https://imgtu.com/i/2Rfuvt)
重启

[![2RfraF.png](https://z3.ax1x.com/2021/06/10/2RfraF.png)](https://imgtu.com/i/2RfraF)

登录

[![2RfWKx.png](https://z3.ax1x.com/2021/06/10/2RfWKx.png)](https://imgtu.com/i/2RfWKx)

系统安装完成

[![2RfHGd.png](https://z3.ax1x.com/2021/06/10/2RfHGd.png)](https://imgtu.com/i/2RfHGd)

### CentOS7系统网络设置

#### 操作步骤

1. 查看当前虚拟机网关（记住这个网关，后面使用）      [![2RhSIg.png](https://z3.ax1x.com/2021/06/10/2RhSIg.png)](https://imgtu.com/i/2RhSIg)                                                     

2. 进入目录命令：cd /etc/sysconfig/network-scripts/

   [![2RhELV.png](https://z3.ax1x.com/2021/06/10/2RhELV.png)](https://imgtu.com/i/2RhELV)

3. 编辑网卡配置文件命令：vim ifcfg-ens33

   [![2RhMW9.png](https://z3.ax1x.com/2021/06/10/2RhMW9.png)](https://imgtu.com/i/2RhMW9)  

4. 配置静态IP，增加修改如下信息：

   修改的内容：

   ```
   BOOTPROTO=static      
   ```

   增加的内容：在笔记中ctrl+c，在客户机中按shift+insert，你的IP地址要修改的

   IPADDR：ip地址

   GATEWAY：网关

   NETMASK：子网掩码

   DNS：域名解析服务器

   ```
   IPADDR=192.168.248.99
   GATEWAY=192.168.248.2
   NETMASK=255.255.255.0
   DNS1=8.8.8.8
   DNS2=114.114.114.114
   ```

   

5. 重启网卡服务

   [![2Rh3sx.png](https://z3.ax1x.com/2021/06/10/2Rh3sx.png)](https://imgtu.com/i/2Rh3sx) 

 

#### 执行结果

1. 查看ip

   [![2RhJeK.png](https://z3.ax1x.com/2021/06/10/2RhJeK.png)](https://imgtu.com/i/2RhJeK) 

2. Ping外网，如下信息说明可以连接外网

   [![2RhUFe.png](https://z3.ax1x.com/2021/06/10/2RhUFe.png)](https://imgtu.com/i/2RhUFe) 

3. 安装客户端软件

   [下载地址](https://mobaxterm.mobatek.net/download.html)，无脑安装即可

   新建会话

   [![2RhaJH.png](https://z3.ax1x.com/2021/06/10/2RhaJH.png)](https://imgtu.com/i/2RhaJH)

   连接成功

   [![2Rhyef.png](https://z3.ax1x.com/2021/06/10/2Rhyef.png)](https://imgtu.com/i/2Rhyef)

   可以在客户端软件敲命令啦

