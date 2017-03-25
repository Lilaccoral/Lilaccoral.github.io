---
title: Linux常用命令
layout: post
tags:
  - Linux
  - 学习
category: 
---

#### 系统基础相关

- 使用root用户的环境变量切换到root用户 su -
- 显示当前工作路径 pwd
- 显示当前系统默认语言及键盘布局 localectl
- 显示系统中能支持的所有语言 localectl list-locales
- 配置系统默认语言为中文 localectl set-locale LANG=zh_CN.gb2312
- 重启机器 reboot
- 关机 poweroff
- 退出当前的shell logout/exit

---

#### 命令帮助

- command --help
- man command
- info command
- 列出命令的简短使用信息(当使用whatis报错时，需要运行mandb命令生成索引文件) whatis command
---

#### 日常使用命令

- 显示或者更改日期 date
- 显示日历 cal
- 统计文本行数或字符数以及其他相关信息 wc
- 找出命令的绝对路径 which
- 列出最近使用过的number条命令(rehl下默认保存1000条) history number
- 默认显示文本前10行内容，如需要显示更多行可以加减number实现 head [+- number]
- 默认显示文本后10行内容, 如需显示更多可以加减number tail [+- number]
- 自上而下显示文本内容 cat
- 自下而上显示文本内容 tac
- 切换工作路径 cd
- 显示目录内容 ls
- 复制文件或目录，复制目录时，加上-r选项表示递归复制 cp
- 重命名/移动文件或者目录 mv
- 删除文件或目录，删除目录时，加上-r选项表示递归,加上-f选项表示强制删除并且不提醒 rm
- 创建目录,递归创建加上-p选项 mkdir
- 创建空文件，或者更新时间戳 touch
- 列出目录树 tree
- 文件校验 sha1sum sha224sum sha256sum sha384sum sha512sum
- 校验文件md5的值 md5sum
- 逐屏浏览文本内容 less

---

#### 用户, 组以及权限相关

- 打印用户身份信息 id
- 更改user用户的密码 passwd user
- 添加用户 useradd
- 更改已添加用户的相关信息(uid, gid以及groups) usermod
- 删除用户 userdel
- 添加组 groupadd
- 删除组 groupdel
- 更改用户权限和组以及id等 change
- 同时更改file文件的所属用户及属组为student chown student.student file
- 更改文件的所属组 chgrp
- 更改文件权限 chmod
- 掩码方式更改 umask

--- 

#### 网络配置相关
- 杂七杂八
```shell
网络配置相关的文件存放在
/etc/sysconfig/network-scripts/ifcfg-*
/etc/hosts        #静态IP到名称解析文件
/etc/hostname    #主机名称配置文件
接口命名规则
ethx    #以太网接口
wlanx    #无线网卡接口
pppxx    #PPPOE拨号接口
```

- VI编辑配置文件来配置网络
```shell
配置静态IPv4地址(vi纯手工编辑配置文件)
cat /etc/sysconfig/network-scripts/ifcfg-
DEVICE=        #此处填写物理网卡名称
BOOTPROTO=none        #地址分配类型{dhcp|none|static}
IPADDR=1.2.3.4        #IPv4地址
PREFIX=24        #Netmask
GATEWAY=1.2.3.254    #GW
DNS1=1.2.3.254
DNS2=1.2.3.253
ONBOOT=yes        #配置此接口是否在开机时启用
#systemctl restart network
配置动态IPv4地址(vi纯手工编辑配置文件)
cat /etc/sysconfig/network-scripts/ifcfg-
DEVICE=        #此处填写物理网卡名称
BOOTPROTO=dhcp        #地址分配类型{dhcp|none|static}
ONBOOT=yes        #配置此接口是否在开机时启用
#systemctl restart network
配置DNS客户端
#cat /etc/resolv.conf
search    redhat.com    #搜索域
nameserver    1.2.3.4
nameserver    4.3.2.1
配置静态IP到名称的解析列表，当内网中没有DNS服务器时，就可以编辑hosts文件实现IP地址到名称的解析
#cat /etc/hosts
10.1.1.1        server1 server1.example.com
10.1.1.2        server2 server2.example.com
更改主机名称
#cat /etc/hostname
server.example.com
```

---

#### 解压缩相关

- tar
   * c 创建
   * t 列出
   * x 解压
   * f 文件名称
   * C 解压到指定目录
   * z 采用gzip压缩
   * j 采用bzip2压缩
   * J 采用xz进行压缩
- 打包 tar cvf filename.tar /path
- 打包并压缩成gzip格式 tar czvf filename.tar.gz /path
- 解压到指定文件夹 tar xvf filename.tar /path
- 查看压缩包内容但不解压 tar tvf filename.tar

---

#### 软件包管理相关

- yum常用命令
```shell
yum install a b c d    #安装软件包a b c d    (加上-y选项，可以在安装软件包时，不弹出是否继续的提示)
yum remove a b c d    #卸载软件包a b c d
yum groups list    #查看已安装的软件组和可用的软件组
yum groups  install "Infiniband Support"    #安装软件组
yum groups remove "Infiniband Support"    #卸载软件组
yum info a b c    #查看软件包a b c d的相关信息，如大小，版本等...
yum update a b c d    #更新软件包a b c d
yum update    #整体更新所有可更新的软件包
yum provides 文件或目录        #查看文件由哪个rpm包提供的
yum search tree        #从仓库中搜索关键词为tree的包
yum history        #查看yum运行历史记录
```
- rpm 常用命令

```shell
rpm -qa        #查询本机安装的所有RPM包
rpm -qa --last    #按照时间先后顺序查询本机安装的所有RPM包
rpm -qf 文件或目录    #查看文件由哪个rpm包提供的
rpm -Va 包名称    #校验RPM包完整性，也可不填，不填，则代表校验所有RPM包
rpm -qd 包名称    #查看RPM包附带的文档有哪些
rpm -ql 包名称    #查看RPM包释放了哪些文件在哪个目录下
rpm -qc 包名称    #查看RPM包附带的配置文件有哪些
rpm -e 包名称    #卸载RPM包，多个包以空格隔开
rpm -e 包名称 --nodeps    #不检查RPM包之间的依赖关系，直接卸载RPM包
rpm -ivh 包名称    #安装一个或多个RPM包
rpm -Uvh 包名称    #升级一个或多个RPM包
```
---

#### 文件系统相关

- 设备文件命名规则
```shell
Linux下的设备文件命名规则
/dev/sda        #第一块串口硬盘
/dev/hda        #第一块并口硬盘
/dev/vda        #基于KVM下的virtio驱动的第一块虚拟化磁盘
/dev/xvda    #基于Xen虚拟化技术的虚拟磁盘
/dev/cdrom    #CD/DVD设备，该文件通常链接到/dev/sr0，也就是第一个CD/DVD设备，第二个光驱设备，则是/dev/sr1，以此类推
/dev/vgname/lvname    #逻辑卷磁盘
/dev/sda1        #第一块串口硬盘的第一个分区
/dev/hda1    #第一块并口硬盘的第一个分区
备注: 当Linux下的磁盘超过24个时，比如从/dev/sda>/dev/sdz，那么则多余的磁盘会继续以/dev/sdaa,/dev/sdab排列
df    #显示文件系统使用情况
du    #统计文件大小
mount    #挂载分区至某个目录，或者显示挂载情况  
```
---

#### 文件搜索

- 搜索前, 先执行updatedb建立索引数据库然后再执行 locate filename
- find搜索
```shell
find / -name ccie    #从/分区遍历所有子目录，然后根据文件名称查找
find / -type d -name ccie    #从/分区遍历所有子目录，然后只查找名为ccie的目录
find / -size 10M        #从/分区遍历所有子目录，然后查找大小为差不多10M的文件
find / -perm 0755    #从/分区遍历所有子目录，然后查找权限为0755的文件
find / -user student    #从/分区遍历所有子目录，然后查找student用户的文件
```

---

#### 服务与进程相关

-  在rehl7中使用systemctl来管理
```shell
systemctl    -t help    #列出所有的单元类型
systemctl --type "unit"    #查看指定单元类型的状况
systemctl --failed        #查看所有加载失败的单元信息
systemctl status cups.service    #查看cups服务单元状况
systemctl start cups.service    #启动cups服务单元
systemctl stop cups.service    #停止cups服务单元
systemctl restart cups.service    #重启cups服务单元
systemctl enable cups.service    #配置cups服务单元开机自动启动
systemctl disable cups.service    #配置cups服务单元开机不启动
systemctl reload cups.service    #重新加载cups服务单元的配置文件
systemctl is-active cups.service    #查看cups服务单元当前是否运行
systemctl is-enabled cups.service    #查看cups服务单元开机是否自动运行
systemctl mask NetworkManager.service        #彻底屏蔽NM服务单元
systemctl unmask NetworkManager.service    #取消屏蔽NM服务单元
```

