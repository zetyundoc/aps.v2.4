# 安装Zeppelin

## 安装说明

根据安装规划只需要在aps02和aps03两台服务器上执行本节的步骤进行Zeppelin安装。但在系统运行时只需要启动一个Zeppelin服务即可，仅当启动的Zeppelin服务发生不可用时，才需要启动另一个Zeppelin服务。

## 安装及配置Zeppelin

### 安装Zeppelin组件（主备都需要执行）

请参照如下步骤安装Zeppelin组件，以下步骤在主备都需要执行。

1. 配置Zeppelin免密登录。

   ```
   su - zeppelin
   ssh-keygen -f ~/.ssh/id_rsa -N '' -t rsa -q -b 2048
   ssh-copy-id zeppelin@localhost
   
   ```

 
2. 备份配置文件。

    ```
    sudo su - root
    cp -r /mnt/nfsfile/zeppelin/conf  /mnt/nfsfile/zeppelin/confbak
    ```
    
 
    
3. 检查Zeppelin配置文件及启动服务。
 
   1. 检查aps02和aps03节点的/opt/zeppelin/下的文件属主，是否都属于zeppelin用户，若不是则可执行如下命令修改相应文件的属主：

      ```
      chown -R zeppelin:zeppelin /opt/zeppelin
 
      ```
   2. 检查/mnt/nfsfile/zeppelin/下的conf和notebook权限是否为755以及属主是否为zeppelin,若不是请参考如下命令进行修改：
    
      ```
      chown -R zeppelin:zeppelin /mnt/nfsfile/zeppelin/
      ```
4. 进入aps02节点，用zeppelin用户执行如下命令启动Zeppelin。

   ```
    #su - zeppelin
    #cd /opt/zeppelin/bin
    #./zeppelin-daemon.sh start
   ```
    
### 配置Zeppelin的Interpreter

#### 背景信息

APS支持AD或非AD两种鉴权模式：
* 使用AD：

  需要配置模拟用户，并获取keytab：在浏览器中访问http://aps01，登录，“用户中心-安全中心-获取”。

* 不使用AD（默认不使用AD）：

  不使用模拟用户。

    
#### 设置模拟用户

1. 登录Zeppelin。
   
   登录APS，并选择“分析应用”>“自助分析”>“Zeppelin”。
   
   Zeppelin默认登录账号为apsadmin，密码为Server2008! 
   
2. 设置Interpreter。

   如下图，在下拉菜单中选择“Interpreter”。
   
   ![](/install_guide/fig/fig_15.png) 

3. 修改sh为非模拟用户。

   如下图，搜索sh解释器，单击“edit”，并在option中设置**Per User**参数为**shared**：
   
   ![](/install_guide/fig/fig_17.png) 

4. 修改sh为模拟用户。

   如下图，搜索sh解释器，单击“edit”，并在option中设置**Per User**参数为**isolated**：
   
   ![](/install_guide/fig/fig_16.png) 


#### 新增interpreter

Zeppelin通过interpreter接入多种后端，如下以Hive为例给出配置过程，并给出其它interpreter的主要配置参数说明。

1. 登录Zeppelin，在下拉菜单中选择“Interpreter”，如下图所示。
   
   ![](/install_guide/fig/fig_18.png) 

2. 单击“Create”按钮，如下图所示：

   ![](/install_guide/fig/fig_19.png) 

3. 配置相关参数，如下图所示：

   ![](/install_guide/fig/fig_20.png)
   
4. 配置属性、依赖等参数，如下图所示：
 
   ![](/install_guide/fig/fig_21.png)

5. 单击“Save”，保存配置，如下图所示：

   ![](/install_guide/fig/fig_22.png)
  

** Hive Interpreter配置参数说明 **

| 参数名 | 取值 |
| :--- | :--- |
| **Interpreter Name**	　 |  - | 
| hive	　             |  - | 
| **Interpreter group**	　 |  - | 
| jdbc	　         | - | 
| **Option**	       |   Globally shared | 
| **Properties**	　 | - | 
| **name**	| **value** | 
| common.max_count	|  1000 | 
| default.driver	|  org.apache.hive.jdbc.HiveDriver | 
| default.password	| 　- | 
| default.url	   |  （根据cdh集群实际情况修改）jdbc:hive2://cdh2:10000/default;tez.queue.name=root.user | 
| default.user	   |    hive | 
| zeppelin.interpreter.localRepo	| /opt/zeppelin/local-repo/2DBS2PBU1 | 
| zeppelin.interpreter.output.limit | 	102400 | 
| zeppelin.jdbc.auth.type	　 |  - | 
| zeppelin.jdbc.concurrent.max_connection	| 10 | 
| zeppelin.jdbc.concurrent.use	TRUE  |  - | 
| zeppelin.jdbc.keytab.location	　|  - | 
| zeppelin.jdbc.principal	　|  - | 
| Dependencies（根据cdh集群实际情况修改）	 | 　  - | 
| **artifact**	|  **exclude**   | 
| /opt/cloudera/parcels/CDH/jars/hive-jdbc-1.1.0-cdh5.7.0.jar	　|  - | 
| /opt/cloudera/parcels/CDH/jars/hive-jdbc-1.1.0-cdh5.7.0-standalone.jar	| 　 - | 
| /opt/cloudera/parcels/CDH/jars/hadoop-common-2.6.0-cdh5.7.0.jar	　|  - | 
| /opt/cloudera/parcels/CDH/jars/hive-shims-0.23-1.1.0-cdh5.7.0.jar	　|  - | 
| /opt/cloudera/parcels/CDH/jars/hadoop-auth-2.6.0-cdh5.7.0.jar	　|   - | 

** Impala Interpreter配置参数说明 **

| 参数名 | 取值 |
| :--- | :--- |
| **Interpreter Name**	　 |  - | 
| impala	　             |  - | 
| **Interpreter group**	　 |  - | 
| jdbc	　         | - | 
| **Option**	       |   Globally shared | 
| **Properties**	　 | - | 
| **name**	| **value** | 
| common.max_count |	1000 |
| default.driver |	org.apache.hive.jdbc.HiveDriver |
| default.password |	- |
| default.url | （根据cdh集群实际情况修改）jdbc:hive2://cdh1:21050/default;auth=noSasl |
| default.user| impala |
| zeppelin.interpreter.localRepo| /opt/zeppelin/local-repo/2DBNFBYEA |
| zeppelin.interpreter.output.limit | 102400 |
| zeppelin.jdbc.auth.type |	- |
| zeppelin.jdbc.concurrent.max_connection	|	10 |
| zeppelin.jdbc.concurrent.use | TRUE |
| zeppelin.jdbc.keytab.location | - |
| zeppelin.jdbc.principal |	- |
| **Dependencies(根据cdh集群实际情况修改)** |	- |
| **artifact** |	**exclude** |
| /opt/cloudera/parcels/CDH/jars/hive-jdbc-1.1.0-cdh5.10.1.jar|	- |
| /opt/cloudera/parcels/CDH/jars/hive-jdbc-1.1.0-cdh5.10.1-standalone.jar	|	- |
| /opt/cloudera/parcels/CDH/jars/hadoop-common-2.6.0-cdh5.10.1.jar |	- |
| /opt/cloudera/parcels/CDH/jars/hive-shims-0.23-1.1.0-cdh5.10.1.jar	|	- |
| /opt/cloudera/parcels/CDH/jars/hadoop-auth-2.6.0-cdh5.10.1.jar	|	- |

** Spark Interpreter配置参数说明 **

| 参数名 | 取值 |
| :--- | :--- |
| **Interpreter Name**	　 |  - | 
| spark	　             |  - | 
| **Interpreter group**	　 |  - | 
| spark	　         | - | 
| **Option**	       |   Globally shared | 
| **Properties**	　 | - | 
| **name**	| **value** | 
| args	　| - | 
| master | 	local[*] | 
| spark.app.name	 | Zeppelin | 
| spark.cores.max	　 | - | 
| spark.executor.memory	　 | - | 
| zeppelin.R.cmd | 	R | 
| zeppelin.R.image.width | 	1 | 
| zeppelin.R.knitr | 	TRUE | 
| zeppelin.R.render.options	 | out.format = 'html', comment = NA, echo = FALSE, results = 'asis', message = F, warning = F | 
| zeppelin.dep.additionalRemoteRepository	 | spark-packages,http://dl.bintray.com/spark-packages/maven,false; | 
| zeppelin.dep.localrepo	 | local-repo | 
| zeppelin.interpreter.localRepo | 	/opt/zeppelin/local-repo/2DBRWKBPG | 
| zeppelin.interpreter.output.limit	 | 102400 | 
| zeppelin.pyspark.python	 | python | 
| zeppelin.spark.concurrentSQL	 | FALSE | 
| zeppelin.spark.importImplicit	 | TRUE | 
| zeppelin.spark.maxResult	 | 1000 | 
| zeppelin.spark.printREPLOutput	 | TRUE | 
| zeppelin.spark.sql.stacktrace	 | FALSE | 
| zeppelin.spark.useHiveContext	 | TRUE | 


** Spark2 Interpreter配置参数说明 **

| 参数名 | 取值 |
| :--- | :--- |
| **Interpreter Name**	　 |  - | 
| spark2	　             |  - | 
| **Interpreter group**	　 |  - | 
| spark	　         | - | 
| **Option**	       |   Globally shared | 
| **Properties**	　 | - | 
| **name**	| **value** | 
| SPARK_HOME	|	/opt/cloudera/parcels/SPARK2/lib/spark2 |
| args |	- |
| master	|	yarn |
| spark.app.name	|	Zeppelin|
| spark.cores.max | - |
| spark.executor.memory | - |
| zeppelin.R.cmd	|	R |
| zeppelin.R.image.width	|	1 |
| zeppelin.R.knitr|	TRUE |
| zeppelin.R.render.options |	out.format = 'html', comment = NA, echo = FALSE, results = 'asis', message = F, warning = F |
| zeppelin.dep.additionalRemoteRepository	| spark-packages,http://dl.bintray.com/spark-packages/maven,false; |
| zeppelin.dep.localrepo	|	local-repo |
| zeppelin.interpreter.localRepo	|	/opt/zeppelin/local-repo/2DBPECR5E |
| zeppelin.interpreter.output.limit	|	102400 |
| zeppelin.pyspark.python|	python |
| zeppelin.spark.concurrentSQL| FALSE |
| zeppelin.spark.importImplicit	| TRUE |
| zeppelin.spark.maxResult | 1000 |
| zeppelin.spark.printREPLOutput | TRUE |
| zeppelin.spark.sql.stacktrace	|FALSE |
| zeppelin.spark.useHiveContext	| TRUE |


** sh Interpreter配置参数说明 **

| 参数名 | 取值 |
| :--- | :--- |
| **Interpreter Name**	　 |  - | 
| sh	　             |  - | 
| **Interpreter group**	　 |  - | 
| sh	　         | - | 
| **Option**	       |   Globally shared | 
| **Properties**	　 | - | 
| **name**	| **value** | 
| shell.command.timeout.millisecs	| 60000 | 
| shell.working.directory.user.home	| FALSE | 
| zeppelin.interpreter.localRepo	| /opt/zeppelin/local-repo/2D9P9A8WW | 
| zeppelin.interpreter.output.limit	| 102400 | 
| zeppelin.shell.auth.type	　| - | 
| zeppelin.shell.keytab.location	| 　- | 
| zeppelin.shell.principal	　|  - | 


** Python Interpreter配置参数说明 **

| 参数名 | 取值 |
| :--- | :--- |
| **Interpreter Name**	　 |  - | 
| python	　             |  - | 
| **Interpreter group**	　 |  - | 
| python	　         | - | 
| **Option**	       |   Globally shared | 
| **Properties**	　 | - | 
| **name**	| **value** | 
| zeppelin.interpreter.localRepo	| /opt/zeppelin/local-repo/2DDEHGWYS | 
| zeppelin.interpreter.output.limit	| 102400 | 
| zeppelin.python	python | - | 
| zeppelin.python.maxResult	1000 | -| 



#### 测试已配置的Interpreter

创建notebook，并在Paragraph中测试已配置的Interpreter。

1. 在Zeppelin中选择“Notebook” > “Create new note” ，如下图所示：

   ![](/install_guide/fig/fig_23.png)

2. 在“Create new note” 对话框中输入相关参数，如下图所示：
 
   ![](/install_guide/fig/fig_24.png)      
   
3. 单击“Create Note”，完成创建。

4. 在新创建的Note中测试已配置的Interpreter。

   1. 测试Shell。
      
      ![](/install_guide/fig/fig_25.png)
      
   2. 测试Hive。
   
      ![](/install_guide/fig/fig_26.png)

   3. 测试Impala。
    
      ![](/install_guide/fig/fig_27.png)
      
   4. 测试Spark。
   
      ![](/install_guide/fig/fig_28.png)
      
   5. 测试Spark2。
      
      ![](/install_guide/fig/fig_29.png)
      
   6. 测试Python。
      
      ![](/install_guide/fig/fig_31.png)



#### 配置Notebook settings：

通过点击“Notebook”配置按钮进行配置。将白色的“sh %sh”点击选择变成蓝色选中，点击双箭头按钮则表示重启该Interpreter。

  ![](/install_guide/fig/fig_10.png)
 

