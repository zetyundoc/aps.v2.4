#  验证APS基本功能
##安装检查
1. 以root用户检查DNS是否正常。

   查看当前DNS服务器地址是否为APS01：
 ```
 # cat /etc/resolv.conf
 ```

 如果不是则执行如下命令：
 ```
 echo "nameserver [dns_ip]" >> /etc/resolv.conf
 ```
 
 执行如下命令增加Registry备份：

 ```
 vim /mnt/nfsfile/backup/script/aps_bak.sh
 #sh $CURR/docker_registry_backup.sh
 将26行的docker_registry_backup.sh的“#”取消掉
 ```
2. 同步NTP时间。

 ```
 所有APS节点修改
 crontab –e
 */2 * * * * /usr/sbin/ntpdate 192.168.20.132 
 将上面定时任务删除
 登录CM01节点将/etc/ntp.conf文件拷贝到APS所有节点的/etc目录下
 Aps节点操作执行：
 #systemctl restart nptd
 #systemctl enable ntpd
 ```
 
3. 启动Consul服务。
```
su - aps -c "/home/aps/consul/consul-slave-start.sh"
```

##验证APS基础功能

使用APS管理员账号登录APS并创建普通操作员账号，《DataCanvas APS 2.4 管理员手册》和《DataCanvas APS 2.4用户手册》。

APS基础功能包括用户管理、集群管理、分析应用管理，请参考《DataCanvas APS 2.4管理员手册》和《DataCanvas APS 2.4用户手册》并按照如下步骤检查APS基本功能是否正常：

1. 在浏览器地址栏中输入" http://&lt;APS01_IP&gt;:8080/nagios "输入账号密码进行登录，通过监控可查看到目前系统服务运行是否正常。

 ```
 登录用户：nagiosadmin 密码：nagios
 ``` 

 ![](/install_guide/fig/fig_11.png)

2. 在浏览器地址栏中输入" http://&lt;APS_IP> "并按回车键，出现如下所示等登录界面，则说明APS安装成功。

 ```
 登录用户：apsadmin@MCIPT.COM 密码：spdb@1234
 ```

 ![](/install_guide/fig/fig_12.png)
   
3. 使用管理员账号登录APS并修改普通操作员的权限，检查修改后是否生效。

4. 使用普通操作员登录APS，在“服务>集群管理”页面检查是否可以创建并连接到已存在的CDH集群。

5. 使用普通操作员登录APS，依次创建数据模块、算法模块、分析模块和数据应用，检查是否可以操作成功。

6. 登录Zeppelin，验证权限是否正常。

 1. 以管理员身份登录APS并创建普通用户demo，并为该用户赋予访问探索交互的权限。
 
 2. 以demo用户登录APS，选择“服务 > 分析应用 > 交互探索”，检查是否可以登录zeppelin。
 
 3. 在Zeppelin中，新建笔记本，并创建一个使用Shell的段，然后使用kilst命令检查是否可以正常查看登录信息，系统显示类似如下信息则说明keytab服务正常。
 
 ![](/install_guide/fig/fig_13.png)
 
7. Python、R镜像服务验证。

 通过“算法定制”启动R、python的五个镜像minimal_r、minimal_python、minimal_python3、anaconda_python、anaconda_python3，然后通过mesos-master页面看到对应启动的“Host”主机查看/etc/krb5.conf是否与安装配置的相同。
 
 ![](/install_guide/fig/fig_14.png)

   ``` 
   #su – aps
   $docker exec –it <docker images ID> bash
   $cat /etc/krb5.conf
   ```
 
8. 查看Repo。

   Apsrepo自动化安装完成后，在浏览器中输入如下地址，检查是否可以仓库是否可以正常访问：

   * R仓库查看地址：" http://aps01:180 "
   * Python仓库查看地址：" http://aps01:18088 "

   说明:如果本地hosts文件中没有配置主机名aps01与IP地址的映射关系，可以使用实际IP地址替换上述地址中的主机名aps01。

###验证Zeppline与CDH集群的连通

Zeppelin通过解释器将CDH集群中的任何语言/数据处理后端插入到Zeppelin中。在Zeppelin中，默认提供的解释器有Apache Spark、Python、Markdown、Shell、HDFS等。可以通过创建notebook并在notebook中使用相关解释器验证Zeppelin是否与CDH集群连通，验证步骤请参考《DataCanvas APS 2.4 用户手册》中“交互探索”章节的应用示例。


