#  安装APS

## 安装说明
APS的安装包括基础组件以及APS私有组件的安装。在安装过程中，只需要将APS安装包上传到APS集群的主节点，修改安装配置后执行安装脚本即可。安装脚本会根据配置将相关组件的安装程序包分发到对应的节点并完成各组件的安装和配置。

## 安装前提

在安装前，请确保APS中的主机满足如下条件：

1. 已完成各主机的基础环境配置，具体配置步骤请参考“配置基础环境”章节。
2. 已获取APS安装包。

## Repo配置

### 安装说明

只需要在APS集群的主节点APS01上安装Repo仓库即可。

安装Repo仓库使用的安装包列表如下所示：

* R镜像源：CRAN.tar.gz
* Python镜像源：pypi.tar.gz 

### 安装过程
```
mkdir -p /data/repo/
sudo mkfs -t ext4 [repo磁盘]  #[repo磁盘]格式化为ext4格式
mount [repo磁盘] /data/repo/
tar zxvf CRAN.tar.gz  -d /data/repo/
tar zxvf pypi.tar.gz  -d  /data/repo/
```

## Nfs配置

### 安装说明

只需要在APS集群的主节点APS03上安装配置nfs磁盘。

### 安装过程
```
mkdir -p /mnt/data
sudo mkfs -t ext4 [nfs磁盘]   #[nfs磁盘]格式化为ext4格式
mount [nfs磁盘] /mnt/data 
添加开机自动挂载磁盘
vi /etc/fatab
[nfs磁盘]  /mnt/data  ext4   defaults   0   0
```
## 安装过程

1.以aps用户登录APS集群主节点。

2.上传APS安装包。
  
  将安装包aps-deploy-*.tgz上传到APS集群主节点aps用户的家目录“/home/aps”下。

3.解压安装包。
  ```
  #su – aps
  $ cd
  $ tar -zxvf aps-deploy-*.tgz
  ```

4.修改配置文件“/home/aps/aps-deploy/conf/hosts”。

 该配置文件用于配置各组件安装服务器的IP地址，需要根据实际组网进行配置，配置样例如下所示： 
 
  ```
     # aps hosts
     [allips]
     aps01_ip
     aps02_ip
     aps03_ip
     aps04_ip
     [dnsmasq-master]
     aps01_ip
     [dnsmasq-backup]
     aps02_ip
     [nfs-server]
     aps03_ip
     [nfs-client]
     aps01_ip
     aps02_ip
     [docker-registry]
    aps01_ip  docker_disk_file=/dev/sda    #指定docker存储盘
    [docker-service]
    aps02_ip  docker_disk_file=/dev/sda     #指定docker存储盘
    aps03_ip  docker_disk_file=/dev/sda     #指定docker存储盘
    aps04_ip docker_disk_file=/dev/vdb      #指定docker存储盘
    [zookeeper]
    aps01_ip
    aps02_ip
    aps03_ip
    [mesos-master]
    aps01_ip
    aps02_ip
    aps03_ip
    [mesos-slave]
    aps01_ip
    aps02_ip
    aps03_ip
    [mesos-marathon]
    aps04_ip
    [rabbitmq-master]
    aps02_ip
    [rabbitmq-slave]
    aps03_ip
    [keepalived-master]
    aps02_ip vrrp_interface: eth0 [keepalived-backup]
    [postgresql-master]
    aps02_ip
    [postgresql-slave]
    aps03_ip
    [consul-master]
    aps01_ip
    aps02_ip
    aps03_ip
    [consul-slave]
    aps04_ip
    [git-master]
    aps01_ip
    [git-backup]
    aps02_ip
    [aps-push]
    aps01_ip
    [aps-pull]
    aps01_ip
    aps02_ip
    aps03_ip
    aps04_ip
    [zeppelin]
     aps02_ip
    aps03_ip
    [apollo]
    aps02_ip
    [nagios-server]
    aps01_ip
    [nagios-client]
    aps01_ip
    aps02_ip
    aps03_ip
    aps04_ip
   
  ```







