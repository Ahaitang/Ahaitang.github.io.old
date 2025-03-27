+++
title = '虚拟机安装——CentOS'
slug = 'Virtual machine installation'
date = 2025-03-17T16:53:22+08:00
weight=5
author=2025-03-17T16:53:22+08:00
categories = [
    "BigData",
    "VMware",
    "CentOS",
    "虚拟机安装"
]
image = "https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313104844434.png"
description = "Virtual machine installation"
+++

# 虚拟机

## 安装虚拟机

1. **打开虚拟机软件**
   ![image-20240313104844434](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313104844434.png)
2. **新建虚拟机**
   - 点击新建虚拟机后，选择自定义(高级)
     ![image-20240313105024957](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313105024957.png)
   - 点击下一步，知道出现下面页面，选择**稍后安装操作系统**
     ![image-20240313105433384](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313105433384.png)
   - 然后点击下一步，直到出现如图所示，然后**修改名称和存储位置**
     ![image-20240313105706614](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313105706614.png)      
      - 分配处理器内核总数   
        ![image-20240313105903312](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313105903312.png) 
      - 为虚拟分配内存，根据电脑来修改自己的分配
        ![image-20240313110033065](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313110033065.png)  
      - 点击下一步，直到为磁盘分配大小   
        ![image-20240313110205690](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313110205690.png)  
      - 一直下一步，点击完成即可  
        ![image-20240313110241122](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313110241122.png)
   
3. **安装centos系统**
   - 配置镜像文件
     ![image-20240313110739433](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313110739433.png)
   - 开启虚拟机，出现下面界面以后，回车等待加载
     ![image-20240313111648851](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313111648851.png)
   - 出现下面页面，选择语言
     ![image-20240313112206274](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313112206274.png)
   - 在下面页面配置**时间，软件安装，安装位置，网络和主机名**

     ![image-20240313112714885](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313112714885.png) ![image-20240313112758969](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313112758969.png)
     - 点击时间，进行修改
       ![image-20240313113048122](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313113048122.png)
     - 点击软件安装
       ![image-20240313113208215](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313113208215.png)
     - 点击安装位置
       ![image-20240313113428400](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313113428400.png)
     - 点击网络和主机名
       ![image-20240313113540628](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313113540628.png)
   - 开始安装，并设置 root 用户和普通用户
     ![image-20240313113748748](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313113748748.png)
   - 完成后重启，进入以下界面
     ![image-20240313132845952](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313132845952.png)
     ![image-20240313132956998](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313132956998.png)
## 配置网络环境	

1. **配置虚拟机网络环境**
   - 右键单机桌面，打开终端
     ![image-20240313134946309](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240313134946309.png)
   - 在终端四输入 # vim /etc/sysconfig/network-scripts/ifcfg-ens33 进入编辑页面
     - 修改个人信息，具体信息如下图显示
       ![image-20240320102913264](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240320102143189.png)

   - 在终端输入  # systemctl restart network 重启网络服务
     ![image-20240320102259462](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240320102259462.png)
   - 重启成功后 输入在终端输入 `#ifconfig` 查看图片中标记的地方是否与自己修改的一致，若没有修改成功，则再次重复上面配置**虚拟机网络环境**的操做
     ![image-20240320102913264](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240320102913264.png)
   - 修改用户名
     ![image-20240320103151754](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240320103151754.png)

2. **配置windos网络环境**
   - 在自己的 windos 电脑中进入 C:/Windos/System32/drivers/etc 找到hosts文件进修配置修改。
     ![image-20240320103952135](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240320103952135.png)
## 测试连接虚拟机

1. Xsehll  Xftp安装
   - 安装地址https://www.xshell.com/zh/free-for-home-school
   - 具体安装步骤省略
2. 通过 Xshell 连接虚拟机
   - ![image-20240322205406949](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240322205406949.png)
   - 成功连接后会出现以下界面
     - ![image-20240322205630551](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240322205630551.png)
   - 测试 Xftp 是否可以使用
     ![image-20240322205916968](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240322205916968.png)
## 基本环境配置
- **在终端输入** `ping www.baidu.com` **出现下面界面即网络正常**
![image-20240327133437723](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327133437723.png)
- **安装 epel-release**
    ```shell
      yum install -y epel-release
    ```
    > 注：Extra Packages for Enterprise Linux 是为“红帽系”的操作系统提供额外的软件包，适用于 RHEL、CentOS 和 Scientific Linux。相当于是一个软件仓库，大多数 rpm 包在官方 repository 中是找不到的）
    ![image-20240327133924571](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327133924571.png)

- **关闭防火墙，并停止开机自启动**
  `systemctl stop firewalld `
  `systemctl disable firewalld.service`
   ![image-20240327135621113](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327135621113.png)
- 给 普通用户 赋予 **root** 权限，后期方便普通用户使用 **root** 权限
    ``` shell
    vim /etc/sudoers
    ```
    ![image-20240327140603109](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327140603109.png)
- **在 /opt 目录下创建文件夹，并修改所属主和所属组**

  1. 在/opt 目录下创建 module、software 文件夹
 
    ```shell
    mkdir /opt/module
    mkdir /opt/software
    ```
    ![image-20240327141534735](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327141534735.png)

  2. 修改 module、software 文件夹的所有者和所属组均为 haitang 用户，并查看 module、software 文件夹的所有者和所属组
    ```shell
    chown atguigu:atguigu /opt/module
    chown atguigu:atguigu /opt/software  
    ```
    ![image-20240327142115123](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327142115123.png)
- **卸载电脑自带 JDK**
    ```shell
    rpm -qa | grep -i java | xargs -n1 rpm -e --nodeps
    ```
    > ➢ rpm -qa：查询所安装的所有 rpm 软件包  
    > ➢ grep -i：忽略大小写  
    > ➢ xargs -n1：表示每次只传递一个参数  
    > ➢ rpm -e –nodeps：强制卸载软件

- **重启虚拟机**
    ```shell
    reboot
    ```
​**虚拟机安装结束**
