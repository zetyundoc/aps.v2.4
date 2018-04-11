#  安装概览

##安装说明

根据安装规划只需要在aps02和aps03两台服务器上执行本节的步骤进行Zeppelin安装。但在系统运行时只需要启动一个Zeppelin服务即可，仅当启动的Zeppelin服务发生不可用时，才需要启动另一个Zeppelin服务。

##安装及配置Zeppelin

1. 安装Zeppelin组件（主备都需要执行）

 1. 基础配置(在aps02上执行)
    ```
   zeppelin的日志和配置目录修改 新建/mnt/nfsfile/log/zeppelin目录
   sudo su - root
   mkdir -p /mnt/nfsfile/log/zeppelin
   chmod 775 /mnt/nfsfile/log/zeppelin
   chmod 755 /mnt/nfsfile/zeppelin/conf
   chown -R zeppelin:zeppelin /mnt/nfsfile/zeppelin
   chmod 777 -R /mnt/nfsfile/log/
   ```
 
 2. 备份
 ```
sudo su - root
cp -r /mnt/nfsfile/zeppelin/conf  /mnt/nfsfile/zeppelin/confbak
 ```
 3. 修改配置文件（aps02，aps03主备）
 ```
su - zeppelin  密码 zeppelin vi /opt/zeppelin/bin/common.sh export ZEPPELIN_CONF_DIR="/mnt/nfsfile/zeppelin/conf" export ZEPPELIN_LOG_DIR="/mnt/nfsfile/log/zeppelin"
 ```
 4. 修改shrio.ini （aps02，aps03主备）
 
    文件路径
 
    ```
    /opt/zeppelin/conf
    ```
  
    使用AD，请使用 shrio.ini.AD 替换 shrio.ini
    
    ```  
    cp shrio.ini.AD shrio.ini
    ```
   
    不使用AD，请使用 shrio.ini.NOAD 替换 shrio.ini

    ```
    cp shrio.ini.NOAD shrio.ini
    ```
    
   5. 检查Zeppelin配置文件及启动服务
 
    * 检查aps02和aps03节点的/opt/zeppelin/ 都属于zeppelin用户除了bin下面的delete_user_util.sh 和 removeInterpreter.sh为root用户,若不是则

    ```
    chown -R zeppelin:zeppelin /opt/zeppelin/XXX
    ```
    
    * 检查/mnt/nfsfile/log/ 下面的zeppelin目录权限属于775 和 zeppelin用户,若不是则

    ```
    chown -R zeppelin:zeppelin /mnt/nfsfile/log/zeppelin
    ```
    * 检查/mnt/nfsfile/zeppelin/下的conf 和notebook 权限属于755 和 zeppelin,若不是则
    
    ```
    chown -R zeppelin:zeppelin /mnt/nfsfile/zeppelin/
    ```
  6. 进入aps02节点，用zeppelin用户启动命令

    ```
    #su - zeppelin
    #cd /opt/zeppelin/bin
    #./zeppelin-daemon.sh start
    ```
    
###配置Zeppelin的Interpreter

使用AD需要配置模拟用户，不使用AD不是用模拟用户

在浏览器中访问http://aps01，登录，“服务-用户中心-安全中心-获取”。

单击“服务-分析应用-自助分析”，右上角login，并使用管理员账号登录，点击用户名，在下拉菜单中选择“Interpreter”，配置各Interpreter如下所示。

 ![](/install_guide/fig/fig_04.png) 

任何Interpreter添加、修改都需要重启。

**参数说明**

  * edit：对Interpreter进行修改。
  * restart：Interpreter重启
  * remove： Interpreter删除
  * 所有的interpreter 修改为Globally shared
  
  
  1. 配置Shell
  
  ![](/install_guide/fig/fig_05.png)
 
    **Properties（示例）**

    |name	  |	 value|
    | :--- | :--- | :--- |                  
    |zeppelin.interpreter.localRepo  |   /opt/zeppelin/local-repo/2D81Y8APD|
    |zeppelin.interpreter.output.limit |	102400|
    |zeppelin.python |	python|
    |zeppelin.python.maxResult |   1000|

    **测试方法**
    
    ```
    %sh
    klist
    ```
        
  2. 配置Python
 
  **Properties（示例）**
  
  ![](/install_guide/fig/fig_06.png)
  
    |name	  |	 value|
    | :--- | :--- | :--- |                  
    |zeppelin.interpreter.localRepo(不添加)  |   opt/zeppelin/local-repo/2CYVZF7AQ|
    |zeppelin.interpreter.output.limit |	102400|
    |zeppelin.python |	python|
    |zeppelin.python.maxResult |   1000|

    **测试方法：**
    
    ```
    %python
    import os
    print “this is test”
    ```

  3. hive配置
  
  ![](/install_guide/fig/fig_07.png)
  
  **Properties**
  
    |name	  |	 value|
    | :--- | :--- | :--- |                  
    |common.max_count|  1000|
    |default.driver |	org.apache.hive.jdbc.HiveDriver|
    |default.password |	python|
    |default.url（根据实际cdh集群进行配置） | jdbc:hive2://cdh5:10000/default;principal=hive/cdh5.test.com@TEST.COM;tez.queue.name=root.user|
    |default.user|	hive|
    |zeppelin.interpreter.localRepo（不添加） |	python|
    |zeppelin.python.maxResult |  /opt/zeppelin/local-repo/2DAADHRKQ|
    |zeppelin.interpreter.output.limit|  102400|
    |zeppelin.jdbc.auth.type	 |KERBEROS|
    |zeppelin.jdbc.concurrent.max_connection |	10|
    |zeppelin.jdbc.concurrent.use|true|
    |zeppelin.jdbc.keytab.location|	|
    |zeppelin.jdbc.principal	 |	|

  **Dependencies**
  
    |artifact	  |	 exclude|
    | :--- | :--- | :--- |                  
    |/opt/cloudera/parcels/CDH/jars/hive-jdbc-1.1.0-cdh5.10.1.jar	|  |
    |/opt/cloudera/parcels/CDH/jars/hive-jdbc-1.1.0-cdh5.10.1-standalone.jar |  |
    |default.password | |
    |/opt/cloudera/parcels/CDH/jars/hadoop-common-2.6.0-cdh5.10.1.jar | |
    |/opt/cloudera/parcels/CDH/jars/hive-shims-0.23-1.1.0-cdh5.10.1.jar|	|
    |/opt/cloudera/parcels/CDH/jars/hadoop-auth-2.6.0-cdh5.10.1.jar |	 |
  

  **测试方法**
%hive
use aps;
create table t1(id int);
show tables;
select count(*) from test1;

4)	配置Impala
 
	Properties
name	value
common.max_count	1000
default.driver	org.apache.hive.jdbc.HiveDriver
default.password	
default.url	jdbc:hive2://cdh5:10000/default;principal=hive/cdh5.test.com@TEST.COM;tez.queue.name=root.user
default.user	impala
zeppelin.interpreter.localRepo	/opt/zeppelin/local-repo/2D8U7GYP8
zeppelin.interpreter.output.limit	102400
zeppelin.jdbc.auth.type	KERBEROS
zeppelin.jdbc.concurrent.max_connection	10
zeppelin.jdbc.concurrent.use	true
zeppelin.jdbc.keytab.location	
zeppelin.jdbc.principal	
	Dependencies（示例）
artifact	exclude
/opt/cloudera/parcels/CDH/jars/hive-jdbc-1.1.0-cdh5.10.1.jar	
/opt/cloudera/parcels/CDH/jars/hive-jdbc-1.1.0-cdh5.10.1-standalone.jar	
/opt/cloudera/parcels/CDH/jars/hadoop-common-2.6.0-cdh5.10.1.jar	
/opt/cloudera/parcels/CDH/jars/hive-shims-0.23-1.1.0-cdh5.10.1.jar	
/opt/cloudera/parcels/CDH/jars/hadoop-auth-2.6.0-cdh5.10.1.jar	
	测试方法
%impala
show tables;
select count(*) from test1;
	配置spark2
   
5)	Properties
name	value
SPARK_HOME	/opt/cloudera/parcels/SPARK2/lib/spark2
args	
master	yarn
spark.app.name	zeppelin
spark.cores.max	
spark.deployMode	client
spark.yarn.queue	root.yarn_default
zeppelin.R.cmd	R
zeppelin.R.image.width	100%
zeppelin.R.knitr	true
zeppelin.R.render.options	out.format = 'html', comment = NA, echo = FALSE, results = 'asis', message = F, warning = F
zeppelin.dep.additionalRemoteRepository	spark-packages,http://dl.bintray.com/spark-packages/maven,false;
zeppelin.dep.localrepo	local-repo
zeppelin.interpreter.localRepo	/opt/zeppelin/local-repo/2DB7UNPCG
zeppelin.interpreter.output.limit	102400
zeppelin.pyspark.python	/opt/cloudera/parcels/SPARK2/bin/spark2-submit
zeppelin.spark.concurrentSQL	false
zeppelin.spark.importImplicit	true
zeppelin.spark.maxResult	1000
zeppelin.spark.printREPLOutput	true
zeppelin.spark.sql.stacktrace	false
zeppelin.spark.useHiveContext	false

6)	配置Notebook settings：
通过点击“Notebook”- 进行配置。将白色的“sh %sh”点击选择变成蓝色选中，点击 则表示重启该Interpreter。
 

