#  安装基础组件

## 安装说明

APS的基础组件主要是指APS运行时的依赖组件，包括Repo仓库、CDH Client等。

## 安装CDH Client

APS需要访问CDH获取集群资源，因此需要将CDH集群中各组件的配置文件复制到APS集群的所有需要运行算法定制容器（与mesos-slave服务器安装相同）节点上（默认aps01、aps02、aps03节点）并进行相关环境变量的设置。

1. 从CDH集群复制配置文件到APS集群中的主机。

   CDH集群中任何一台具有完整配置文件的主机执行如下命令：

   说明:请使用实际IP地址替换如下命令中的“ip-host”。
  
   在拷贝客户端之前需要确认所有的配置文件和客户端是否完整，以下命令默认在aps01、aps02、aps03节点执行（拷贝过程中需要输入的cdh节点root密码向cdh集群管理员申请获取）。

   ```
   sudo su - root
   scp -r root@cdh-ip:/opt/cloudera/parcels  /opt/cloudera/
   scp -r root@cdh-ip:/etc/hadoop  /etc/
   scp -r root@cdh-ip:/etc/hive  /etc/
   scp -r root@cdh-ip:/etc/hbase /etc/ 
   scp -r root@cdh-ip:/etc/impala  /etc/ 
   scp -r root@cdh-ip:/etc/spark  /etc/
   scp -r root@cdh-ip:/etc/spark2 /etc/
   scp -r root@cdh-ip:/etc/zookeeper /etc/
   ```

2. 配置环境变量。

  1. 在aps01、aps02、aps03节点配置Hadoop环境变量，除模型发布节点（默认aps04）外添加环境变量。
   ```
   echo "export HADOOP_HOME=/opt/cloudera/parcels/CDH/lib/hadoop" >> /etc/profile  
   echo "export PATH=\$PATH:\$HADOOP_HOME/bin" >> /etc/profile
   source /etc/profile 
   ```
   
   2. 创建软连接。
   ```
   ln -s /opt/cloudera/parcels/CDH/bin/hdfs /usr/bin/hdfs
   ln -s /opt/cloudera/parcels/CDH/bin/hive /usr/bin/hive
   ln -s /opt/cloudera/parcels/CDH/bin/beeline /usr/bin/beeline
   ln -s /opt/cloudera/parcels/CDH/bin/hbase /usr/bin/hbase
   ln -s /opt/cloudera/parcels/CDH/bin/impala-shell /usr/bin/impala-shell
   ln -s /opt/cloudera/parcels/CDH/bin/pyspark /usr/bin/pyspark
   ln -s /opt/cloudera/parcels/CDH/bin/spark-shell /usr/bin/spark-shell
   ln -s /opt/cloudera/parcels/CDH/bin/spark-submit /usr/bin/spark-submit
   ln -s /opt/cloudera/parcels/CDH/bin/yarn /usr/bin/yarn
   ln -s /opt/cloudera/parcels/CDH/bin/zookeeper-client /usr/bin/zookeeper-client
   ln -s /opt/cloudera/parcels/SPARK2/bin/spark2-shell /usr/bin/spark2-shell
   ln -s /opt/cloudera/parcels/SPARK2/bin/spark-submit /usr/bin/spark2-submit
   ```