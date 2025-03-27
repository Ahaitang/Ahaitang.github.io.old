+++
title = 'Hadoop安装'
date = 2025-03-17T17:07:36+08:00
weight=5
draft = true
author=2025-03-17T17:07:36+08:00

+++
# Hadoop 环境配置
## 修改网络配置
   - 修改hosts
    ![image-20240320103357773](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240320103357773.png)
## 虚拟机安装 JDK
- **用** **XShell** **传输工具将** **JDK** **导入到** **opt** **目录下面的** **software** **文件夹下面**
  ![image-20240327144635195](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327144635195.png)
- **解压 JDK 到 /opt/module 目录下**
    ```shell
    [haitng@hadoop111 software]$ tar -zxvf jdk-8u212-linux-x64.tar.gz -C /opt/module/
    ```
    ![image-20240327144952870](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327144952870.png)
- **配置 JDK 环境变量**
  1. 新建 /etc/profile.d/my_env.sh 文件， 并添加环境变量
    ```shell
    [haitng@hadoop111 software]$ sudo vim /etc/profile.d/my_env.sh

    # 文件内输入以下内容
    #JAVA_HOME
    export JAVA_HOME=/opt/module/jdk1.8.0_212
    export PATH=$PATH:$JAVA_HOME/bin
    ```
    ![image-20240327152154145](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327152154145.png)

  2. source  一下 /etc/profile  文件，让新的环境变量 PATH 生效
    ```shell
    [haitng@hadoop111 software]$ source /etc/profile
    [haitng@hadoop111 software]$ java -version
    ```
    ![image-20240327150111362](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327150111362.png)		
## 克隆虚拟机(配置集群)
**克隆两台虚拟机（以hadoop112 为例， hadoop113 同理）**
- **修改 hadoop112 的网络 IP**
    ```
    [haitng@hadoop112 ~]$ sudo vim /etc/sysconfig/network-scripts/ifcfg-ens33
    ```
    ![image-20240327162005401](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327162005401.png)
- **重启网络服务，并测试是否成功**
    ```shell
    # 重启网络服务
    [haitng@hadoop112 ~]$ systemctl restart network

    # 查看网络 IP
    [haitng@hadoop112 ~]$ ifconfig

    # 测试网络
    [haitng@hadoop112 ~]$ ping www.baidu.com
    ```
    ![image-20240327162534662](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327162534662.png)
- **修改主机名**
    ```shell
    [haitng@hadoop112 ~]$ vim /etc/hostname
    ```
    ![image-20240327162825606](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327162825606.png)
- **hadoop 113同上述操做**

### 配置免密登录
- **免密登录原理**
    ![image-20240328193132641](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240328193132641.png)
- **生成公钥和私钥**
    ```shell
    # 回到家目录
    [haitng@hadoop111 ~]$ cd ~
    # 进入 .ssh 文件
    [haitng@hadoop111 ~]$ cd .ssh
    # 输入下面命令, 并回车三次, 然后生成文件 id_rsa（私钥）、id_rsa.pub（公钥）
    [haitng@hadoop111 .ssh]$ ssh-keygen -t rsa
    ```
    ![image-20240328182021984](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240328182021984.png)


- **将公钥拷贝到要免密登录的目标机器上**
    ```shell
    [haitng@hadoop111 .ssh]$ ssh-copy-id haitang111
    [haitng@hadoop111 .ssh]$ ssh-copy-id haitang112
    [haitng@hadoop111 .ssh]$ ssh-copy-id haitang113
    ```
    - 需要在 hadoop112 上采用 haitang 账号配置一下无密登录到 hadoop111、hadoop112、hadoop113 服务器上;
    - 还需要在 hadoop113 上采用 haitang 账号配置一下无密登录到 hadoop111、hadoop112、hadoop113 服务器上;
    - 还需要在 hadoop111 上采用 root 账号，配置一下无密登录到 hadoop111、hadoop112、hadoop113 服务器上。
    ![image-20240328182616509](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240328182616509.png)
## 编写常用脚本
- **编写集群分发脚本 xsync**
    ```shell
    # 返回家目录
    [haitng@hadoop111 ~]$ cd ~
    # 查看当前环境变量
    [haitng@hadoop111 ~]$ echo $PATH 

    # 在家目录创建 bin 文件夹
    [haitng@hadoop111 ~]$ mkdir bin
    [haitng@hadoop111 ~]$ cd bin

    # 创建集群分发脚本 xsync
    [haitng@hadoop111 bin]$ vim xsync

    #!/bin/bash
    #1. 判断参数个数
    if [ $# -lt 1 ]
    then
    echo Not Enough Arguement!
    exit;
    fi

    #2. 遍历集群所有机器
    for host in hadoop111 hadoop112 hadoop113
    do
    echo ==================== $host ====================
    #3. 遍历所有目录，挨个发送
    for file in $@
    do
        #4. 判断文件是否存在
        if [ -e $file ]
            then
                #5. 获取父目录
                pdir=$(cd -P $(dirname $file); pwd)
                #6. 获取当前文件的名称
                fname=$(basename $file)
                ssh $host "mkdir -p $pdir"
                rsync -av $pdir/$fname $host:$pdir
            else
                echo $file does not exists!
        fi
    done
    done

    # 修改脚本 xsync 具有执行权限
    [haitng@hadoop111 bin]$ chmod +x xsync

    # 将脚本复制到/bin 中，以便全局调用
    [haitng@hadoop111 bin]$ sudo cp xsync /bin/
    ```

- **Hadoop 集群启停脚本（包含 HDFS，Yarn，Historyserver）：myhadoop.sh**
    ```shell
    # 返回家目录下的bin目录
    [haitng@hadoop111 ~]$ cd ~
    [haitng@hadoop111 ~]$ cd bin

    # 创建集群启停脚本 myhadoop.sh
    [haitng@hadoop111 bin]$ vim myhadoop.sh

    # 脚本内容如下
    #!/bin/bash
    if [ $# -lt 1 ]
    then
    echo "No Args Input..."
    exit ;
    fi
    case $1 in
    "start")
    echo " =================== 启动 hadoop 集群 ==================="
    echo " --------------- 启动 hdfs ---------------"
    ssh hadoop111 "/opt/module/hadoop-3.1.3/sbin/start-dfs.sh"
    echo " --------------- 启动 yarn ---------------"
    ssh hadoop112 "/opt/module/hadoop-3.1.3/sbin/start-yarn.sh"
    echo " --------------- 启动 historyserver ---------------"
    ssh hadoop111 "/opt/module/hadoop-3.1.3/bin/mapred --daemon start historyserver"
    ;;
    "stop")
    echo " =================== 关闭 hadoop 集群 ==================="
    echo " --------------- 关闭 historyserver ---------------"
    ssh hadoop111 "/opt/module/hadoop-3.1.3/bin/mapred --daemon stop historyserver"
    echo " --------------- 关闭 yarn ---------------"
    ssh hadoop112 "/opt/module/hadoop-3.1.3/sbin/stop-yarn.sh"
    echo " --------------- 关闭 hdfs ---------------"
    ssh hadoop111 "/opt/module/hadoop-3.1.3/sbin/stop-dfs.sh"
    ;;
    *)
    echo "Input Args Error..."
    ;;
    esac

    # 修改脚本 myhadoop.sh 具有执行权限
    [haitng@hadoop111 bin]$ chmod +x myhadoop.sh
    ```
  
- **查看三台服务器 Java 进程脚本：jpsall**
    ```shell
    # 返回家目录下的bin目录
    [haitng@hadoop111 ~]$ cd ~
    [haitng@hadoop111 ~]$ cd bin

    # 创建 java 进程脚本：jpsall
    [haitng@hadoop111 bin]$ vim jpsall

    #!/bin/bash
    for host in hadoop111 hadoop112 hadoop113
    do
    echo =============== $host ===============
    ssh $host jps 
    done

    # 修改脚本 jpsall 具有执行权限
    [haitng@hadoop111 bin]$ chmod +x jpsall
    ```
## 虚拟机安装 Hadoop
- **用 XShell 文件传输工具将 hadoop-3.1.3.tar.gz 导入到 opt 目录下面的 software 文件夹下面**
    ![image-20240327151713731](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327151713731.png)
- **解压安装文件到/opt/module 下面**
    ```shell
    tar -zxvf hadoop-3.1.3.tar.gz -C /opt/module/
    ```
    ![image-20240327151958123](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327151958123.png)

- **配置 Hadoop 环境变量**
  1. 新建 /etc/profile.d/my_env.sh 文件， 并添加环境变量
    ```shell
    sudo vim /etc/profile.d/my_env.sh

    #HADOOP_HOME
    export HADOOP_HOME=/opt/module/hadoop-3.1.3
    export PATH=$PATH:$HADOOP_HOME/bin
    export PATH=$PATH:$HADOOP_HOME/sbin
    ```
    ![image-20240327152437060](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327152437060.png)  
  2. source  一下 /etc/profile  文件，让新的环境变量 PATH 生效
    ```shell
    source /etc/profile
    hadoop version
    ```
    ![image-20240327152553565](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/image-20240327152553565.png)

- **配置集群文件**
  `此处同时配置了历史服务器与历史服务器`
  1. **核心配置文件(core-site.xml)**
    ```shell
    vim core-site.xml
    
    <?xml version="1.0" encoding="UTF-8"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    <!--
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at
    
        http://www.apache.org/licenses/LICENSE-2.0
    
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License. See accompanying LICENSE file.
    -->
    
    <!-- Put site-specific property overrides in this file. -->
    
    <configuration>
        <!--指定NameNode的地址 -->
        <property>
            <name>fs.defaultFS</name>
            <value>hdfs://hadoop111:8020</value>
        </property>
    
        <!-- 指定hadoop数据的存储目录 -->
        <property>
            <name>hadoop.tmp.dir</name>
            <value>/opt/module/hadoop-3.1.3/data</value>
        </property>
    
        <!-- 配置HDFS网页登录使用的静态用户为haitang -->
        <property>
            <name>hadoop.http.staticuser.user</name>
            <value>haitang</value>
        </property>
    </configuration>             
    ```

  2. **HDFS 配置文件(hdfs-site.xml)**
    ```shell
    vim hdfs-site.xml

    <?xml version="1.0" encoding="UTF-8"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    <!--
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License. See accompanying LICENSE file.
    -->

    <!-- Put site-specific property overrides in this file. -->

    <configuration>
    <!-- nn web端访问地址-->
    <property>
        <name>dfs.namenode.http-address</name>
        <value>hadoop111:9870</value>
    </property>
    <!-- 2nn web端访问地址-->
    <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>hadoop113:9868</value>
    </property>
    </configuration>
    ```

    3. **YARN 配置文件(yarn-site.xml)**
    ```shell
    vim yarn-site.xml




    <?xml version="1.0"?>
    <!--
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS
    IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License. See accompanying LICENSE file.
    -->
    <configuration>

    <!-- Site specific YARN configuration properties -->
    <!-- 指定MR走shuffle -->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>

    <!-- 指定ResourceManager的地址-->
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>hadoop112</value>
    </property>

    <!-- 环境变量的继承 -->
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
        <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CO
    NF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAP
    RED_HOME</value>
    </property>
    <!-- 开启日志聚集功能 -->
    <property>
        <name>yarn.log-aggregation-enable</name>
        <value>true</value>
    </property>
    <!-- 设置日志聚集服务器地址 -->
    <property>
        <name>yarn.log.server.url</name>
        <value>http://hadoop111:19888/jobhistory/logs</value>
    </property>
    <!-- 设置日志保留时间为7天 -->
    <property>
        <name>yarn.log-aggregation.retain-seconds</name>
        <value>604800</value>
    </property>
    </configuration>
    ```

     4. **MapReduce 配置文件(mapred-site.xml)**
 
    ```shell
    vim mapred-site.xml


    <?xml version="1.0"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    <!--
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License. See accompanying LICENSE file.
    -->

    <!-- Put site-specific property overrides in this file. -->

    <configuration>
    <!-- 指定MapReduce程序运行在Yarn上 -->
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <!-- 历史服务器端地址 -->
    <property>
        <name>mapreduce.jobhistory.address</name>
        <value>hadoop111:10020</value>
    </property>
    <!-- 历史服务器web端地址 -->
    <property>
        <name>mapreduce.jobhistory.webapp.address</name>
        <value>hadoop111:19888</value>
    </property>
    </configuration>
    ```
  
  5. **配置workers**
    ```shell
    vim workers

    hadoop111
    hadoop112
    hadoop113
    ```


### 启动集群
- **如果集群是第一次启动，需要在 hadoop111 节点格式化 NameNode**
  `注意：格式化 NameNode，会产生新的集群 id，导致 NameNode 和 DataNode 的集群 id 不一致，集群找不到已往数据。如果集群在运行过程中报错，需要重新格式化 NameNode 的话，一定要先停止 namenode 和 datanode 进程，并且要删除所有机器的 data 和 logs 目录，然后再进行格式化。`
    ```shell
    hdfs namenode -format
    ```
- **各个模块分开启动****/****停止（配置** **ssh** **是前提）
- **启动/停止 HDFS**
    ```shell
    sbin/start-dfs.sh / sbin/stop-dfs.sh
    ```

- **在配置了 ResourceManager 的节点（hadoop112）启动/停止 YARN**
    ```shell
    sbin/start-yarn.sh / sbin/stop-yarn.sh
    ```

- **在 hadoop111 启动/停止历史服务器**
    ```shell
    mapred --daemon start historyserver / mapred --daemon stop historyserver
    ```

- **各个服务组件逐一启动/停止**
  - **分别启动/停止 HDFS 组件**
    ```shell
    hdfs --daemon start/stop namenode/datanode/secondarynamenode
    ```

  - **启动/停止 YARN**
    ```shell
    yarn --daemon start/stop resourcemanager/nodemanager
    ```