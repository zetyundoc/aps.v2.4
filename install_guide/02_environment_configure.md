#  配置基础环境

在执行APS基础组件以及APS私有组件安装之前，需要先配置基础环境，以保证各组件可顺利完成安装。

## 配置列表

基础环境配置如下表所示： 

|序号|	配置项		|配置说明	|														
| :--- | :--- |:--- |
|1	 | 关闭SELinux	|该操作需要在所有主机执行。               |
|2	 | 基础环境检查	|该操作需要在所有主机执行。               |
|3	 |设置主机名	|该操作需要在所有主机执行。                     |
|4	 |设置信任关系	|该操作需要在所有主机执行。                            |
|5	 |创建安装用户	|需要在每台主机创建相同的安装用户名aps/密码123456。    |

说明：如无特殊说明，所有的操作步骤都是生产环境的投产步骤。

## 关闭SELinux

以下操作需要在每个节点上执行。

1. 登录服务器并切换到root用户。
    
  ```
  su - root
  ```
2. 编辑“/etc/selinux/config”文件，将SELINUX=enforcing改为SELINUX=disabled 。

  ```
  vi /etc/selinux/config
  
  SELINUX=disabled
  ```  
3. 检查Selinux。   

 ```
  getenforce
 ```

## 基础环境检查

1. 检查aps服务器各节点的kernel。

  ```
  #uname -rs
  内核版本应该是kernel-3.10.0.514.26.2.el7.x86_64.rpm，若不是则升级内核：
  #rpm -ivh kernel-3.10.0.514.26.2.el7.x86_64.rpm

  ```
  
2. 重启服务器。

  ```
  shutdown -r now      #重启服务器

  ```

2. 修改服务器网卡参数。（需要根据实际网卡名称调整或增加此参数）。

  ```
  # vim /etc/sysconfig/network-scripts/ifcfg-eth0 
  # PEERDNS="yes"
  PEERDNS="no"
  ONBOOT="yes"

  ```
  修改需执行如下命令重启网卡：
  ```
  systemctl restart network
  ```
  
3. 检查磁盘

  1. 检查每个节点是否具有单独1块数据盘，用于做docker存储，检查命令如下：
  
        ```
      sudo fdisk -l
      ```
  
  2. 检查nfs共享盘检查其中1个节点是否有第2块数据盘，用于做nfs共享盘，检查命令如下：

        ```
      sudo fdisk -l
      ```
  
  3. 检查repo磁盘，在第一个节点上，磁盘空间为2T，查看命令：
 
     ```
    sudo fdisk -l
    ```
  
## 设置主机名

主机名用于标识一台主机，请按照如下步骤配置主机名,根据集群实际节点数进行修改。

1. 用root用户执行如下命令：
  ```
  （1节点执行）
  hostnamectl set-hostname aps01.zetyun.com
  （2节点执行）
  hostnamectl set-hostname aps02.zetyun.com
  （3节点执行）
  hostnamectl set-hostname aps03.zetyun.com
  （4节点执行）
  hostnamectl set-hostname aps04.zetyun.com
  ```
2. 关闭当前控制台并重新登录，检查主机名是否修改成功。

## 配置信任关系

### 配置root用户信任关系
在复制CDH Client时，不同节点之间需要相互执行shell操作，因此需要配置各节点之间的信任关系。

本节示例以配置两个节点的ssh信任关系为例进行说明，在实际操作时需要为所有节点之间配置信任关系。

1. 以root用户登录节点IP为10.200.139.11的服务器, 执行**ssh-keygen -f ~/.ssh/id_rsa -N '' -t rsa -q -b 2048**命令，生成公私钥。

  ```
# ssh-keygen -f ~/.ssh/id_rsa -N '' -t rsa -q -b 2048
  ```
  
2. 执行ssh-copy-id命令，将本机公钥发送给APS所有的主机的root用户。

  ```
  # ssh-copy-id 第一个节点IP
  # ssh-copy-id 第二个节点IP
  # ssh-copy-id 第三个节点IP
  # ssh-copy-id 第四个节点IP

  # scp -r .ssh 第二个节点IP:~/
  # scp -r .ssh 第三个节点IP:~/
  # scp -r .ssh 第四个节点IP:~/

  ```
  
## 创建安装用户并添加sudo权限
使用root账户操作，在每台节点创建aps用户并添加sudo权限。

1. 使创建安装用户：

  ```
  groupadd aps -g 3000
  useradd -u 3000 -g aps -d /home/aps -s /bin/bash aps
  echo aps | passwd --stdin aps 
  ```
  
2. 为安装用户添加sudo权限：

  ```
  sed -i 'N;94a\aps ALL=NOPASSWD:ALL' /etc/sudoers
  
  ```


### 配置aps用户信任关系

在APS执行安装脚本时，不同节点之间需要相互执行shell操作，因此需要配置各节点之间的信任关系。

本节示例以配置两个节点的ssh信任关系为例进行说明，在实际操作时需要为所有节点之间配置信任关系。

1. 以aps用户登录节点IP为10.200.139.11的服务器, 执行**ssh-keygen -f ~/.ssh/id_rsa -N '' -t rsa -q -b 2048**命令，生成公私钥。

  ```
# ssh-keygen -f ~/.ssh/id_rsa -N '' -t rsa -q -b 2048
  ```
  
2. 执行ssh-copy-id命令，将本机公钥发送给APS所有的主机的aps用户，密码为aps。

  ```
  # su - aps
  # ssh-copy-id 第一个节点IP
  # ssh-copy-id 第二个节点IP
  # ssh-copy-id 第三个节点IP
  # ssh-copy-id 第四个节点IP

  # scp -r .ssh 第二个节点IP:~/
  # scp -r .ssh 第三个节点IP:~/
  # scp -r .ssh 第四个节点IP:~/

  ```
















