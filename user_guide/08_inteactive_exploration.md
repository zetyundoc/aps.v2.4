## 交互探索
APS支持的数据处理流程包括商业理解、数据理解、数据准备、建模、评估和部署上线这几个环节，而交互探索及Zeppelin则主要用于数据理解，即用于探索数据特征、进行简单的特征统计、检验数据的质量包括数据的完整性和正确性等。在APS中，一般而言，先由数据科学家使用Zeppelin对部分样本数据进行探索分析并输出报告，然后在此基础上算法工程师、数据应用工程师等开展相关的算法定制、数据应用设计的工作。
### 概念与特性
Zeppelin是一个基于Web的可实现交互式数据分析的笔记本，它支持数据可视化、多语言后端以及笔记本的协作分享。
Zeppelin具有如下特点：
* **多用途笔记本**：
在Zeppelin中个，通过笔记本可以实现数据摄入、数据发现、数据分析、数据可视化与协作等操作。
* **多语言后端**：
Zeppelin通过解释器将任何语言/数据处理后端插入到Zeppelin中。在Zeppelin中，默认提供的解释器有Apache Spark、Python、Markdown、Shell、HDFS、Impala等。
* **Apache Spark集成**：
Zeppelin提供内置的Apache Spark集成。使用者不需要为其构建单独的模块、插件或库。
* **数据可视化**
Zeppelin中包含了一些基本图表。可视化不限于Spark SQL查询，任何语言后端的任何输出都可以被识别和可视化。
* **共享协作**
笔记本网址可以在协作者之间共享，笔记本所有者做的最新更改也会在笔记本网址上被实时同步

### 界面资源介绍

#### 首页
进入服务>交互探索，单击右上角的“login”，输入账号和密码，登录Zeppelin，首页如下图所示：
![](/user_guide/fig/fig_51.png)
* 页面左侧列出所有现有的笔记本。

    说明：每个用户Zeppelin的登录首页仅显示自己拥有的以及别人共享的笔记本。
* 单击“Import note”可以从本地磁盘或从远程位置导入您的笔记本。
* 单击“Create note”可以创建一个新的笔记本。

#### 笔记本

Zeppelin笔记本由段落组成，即笔记本可以看作是一个段落容器。
![](/user_guide/fig/fig_52.png)

#### 段落
每个段落由两部分组成，Code section用于编辑源代码，Result section用户显示代码运行结果。
![](/user_guide/fig/fig_53.png)
段落右上角按钮命令如下所示：
* 执行段落代码
* 隐藏/显示Code Section
* 隐藏/显示Result Section
* 配置段落

#### 笔记本工具栏
在笔记本顶部是一个包含显示命令以及配置、安全性和显示选项的工具栏。
![](/user_guide/fig/fig_88.png)

工具栏按钮的作用如下所示：
* 按顺序执行所有段落
* 隐藏/显示Code Section
* 隐藏/显示Result Section
* 清除所有段落的Result Section
* 克隆当前笔记本
* 将当前笔记本导出为JSON文件
* 切换到所有者模式

### Zeppelin解释器
Zeppelin解释器是一款插件，可让Zeppelin用户使用特定的语言/数据处理后端。例如，要在Zeppelin中使用Scala代码，您需要%spark解释器。
单击笔记本工具栏右侧的齿轮图标，可以查看Zeppelin已绑定的解释器，如下图所示：
![](/user_guide/fig/fig_54.png)

### Zeppelin基本操作

#### 获取Keytab
在APS中，通过解释器的形式将语言和数据处理后端插入到Zeppelin中，而这些后端一般都部署在CDH集群中。出于安全考虑，Zeppelin在访问CDH资源时必须进行认证，在APS中，认证的过程通过Keytab来实现。即用户在通过Zeppelin操作CDH资源前，需要先确保其Keytab是有效的。
1. 单击“用户中心”>“安全中心”，进入Keytab管理页面。
2. 单击“重置”，重新获取Keytab。
    系统显示类似如下信息时，说明当前用户获取的keytab是有效的。
    ![](/user_guide/fig/fig_55.png)
    
    说明：当系统显示Keytab过期或失效时必须执行本步骤中的操作，否则会导致操作CDH集群资源失败。
    
#### 查看用户环境
Zeppelin为其每个用户都创建了一个后端操作系统用户，用户名为“AD_<Zeppelin用户名>”，工作目录为/home/用户名，文件上传目录为“/home/用户名/data/upload”。
1. 选择“服务”>“分析应用”>“交互探索”，系统显示Zeppelin登录页面。
2. 输入用户名和密码登录Zeppelin。
3. 在Zeppelin登录首页单击“Create new note”，在对话框中输入note名称“User environment”并单击“创建”。
    Zeppelin完成创建后会显示该note的编辑页面。
4. 在第一个段落中使用shell解释器查看用户环境，如下图所示。
    ![](/user_guide/fig/fig_56.png)

#### 了解用户Python环境
Zeppelin为每个用户提供了默认的Python执行环境，当默认Python环境的包不满足用户的需求时，用户可以使用pip从远程仓库中安装需要的外部包。

1. 选择“服务”>“分析应用”>“交互探索”，系统显示Zeppelin登录页面。
2. 输入用户名和密码登录Zeppelin。
3. 在Zeppelin登录首页单击“Create new note”，在对话框中输入note名称“Python environment”并单击“创建”。
    Zeppelin完成创建后会显示该note的编辑页面。
4. 在第一个段落中使用shell解释器查看Python环境，如下图所示。
    ![](/user_guide/fig/fig_57.png)
    上图中显示了查看Python版本、Python执行环境以及已安装包的命令以及执行结果。
    
    当“pip list”的结果中缺少用户所需的Python包时，可以通过如下命令进行安装：
    
        pip install scikit-learn -i http://192.168.1.87:18088/simple --trusted-host 192.168.1.87
    
    上述命令表示从仓库http://192.168.1.87:18088/simple中安装scikit-learn包，请根据实际情况修改所需的包名以及仓库地址。


### R语言统计分析示例
#### 场景说明
本节使用Zeppelin的R解释器实现一个文本文件“u.user”包含的人员的职业统计分析。在该文本文件中共包含943条记录，每条记录分别由“序号”、“年龄”、“性别”、“职业”和“邮编”组成，不同字段之间使用“|”进行分割，部分样例数据如下所示：
             
    1|24|M|technician|85711
    2|53|F|other|94043
    3|23|M|writer|32067
    4|24|M|technician|43537
    5|33|F|other|15213

#### 上传并查看文件
Zeppelin为其每个用户都创建了一个后端用户，用户名为“AD_<Zeppelin用户名>”，工作目录为/home/用户名，文件上传目录为“/home/用户名/data/upload”。

1. 选择“服务”>“分析应用”>“交互探索”，系统显示Zeppelin登录页面。
2. 输入用户名和密码登录Zeppelin。
3. 在Zeppelin登录首页单击“Create new note”，在对话框中输入note名称“Base R in Apache Zepplin”并单击“创建”。
    Zeppelin完成创建后会显示该note的编辑页面。
4. 单击页面右上角的上传文件，将本地文件u.user上传到Zeppelin服务器。
    说明：在Zeppelin中，每个用户都有其独立的工作环境，每个用户上传的文件保存在其家目录下的“data/upload”目录下。
5. 在第一个段落中使用shell解释器查看文件是否成功上传。
![](/user_guide/fig/fig_58.png)

#### 编辑并运行代码
1. 新建一个段落，并输入如下代码：
     
       %spark
       val user_data = sc.textFile("data/upload/u.user")
       user_data.first()
       val user_fields = user_data.map(line => line.split("\\|"))
       val num_users = user_fields.map(fields => fields(0)).count()
       val num_genders = user_fields.map(fields => fields(2)).distinct().count()
       val num_occupations = user_fields.map(fields => fields(3)).distinct().count()
       val num_zipcodes = user_fields.map(fields => fields(4)).distinct().count()
       val count_by_occupation = user_fields.map(fields => (fields(3),1)).reduceByKey(_ + _).collect().toList.sortBy(_._2).map(line => line._1+"\t"+line._2).toList.mkString("\n")
       println("%table occupation\tsize\n" + count_by_occupation)

2. 单击段落右上角![](/user_guide/icon/run.png)运行代码，显示结果如下所示：
![](/user_guide/fig/fig_59.png)

    代码成功运行后，默认以饼图的形式展示结果，通过单击Result Section上的操作按钮，可以将结果以不同的形式展示，如下为列表样式：
![](/user_guide/fig/fig_60.png)

### Hive建表示例
####场景说明
本节使用Zeppelin的sh解释器和hive解释器将一个本地文件上传到HDFS，在Hive中创建数据表并将数据文件加载到Hive表中，从而可以通过Hive SQL进行数据探查，避免开发繁琐的MapReduce程序。本节示例使用的文本文件样例如下所示，每条记录由“股票名”、“股票类型”、“交易时间”、“价格”等字段组成，不同字段之间由“,”分割。
          
          sh600000,JRHY,2013-12-26 15:00:07,9.09,0.01,0,0,1
          sh600000,JRHY,2013-12-26 15:00:02,9.08,-0.01,40,36320,-1
          sh600000,JRHY,2013-12-26 14:59:57,9.09,0.0,718,652734,1

#### 上传并查看文件
Zeppelin为其每个用户都创建了一个后端用户，用户名为“AD_<Zeppelin用户名>”，工作目录为/home/用户名，文件上传目录为“/home/用户名/data/upload”。

1. 选择“服务”>“分析应用”>“交互探索”，系统显示Zeppelin登录页面。
2. 输入用户名和密码登录Zeppelin。
3. 在Zeppelin登录首页单击“Create new note”，在对话框中输入note名称“File_import_to_Hive”并单击“创建”。
    Zeppelin完成创建后会显示该note的编辑页面。
4. 单击页面右上角的上传文件，将本地文件stock_data.csv上传到Zeppelin服务器。
    说明：在Zeppelin中，每个用户都有其独立的工作环境，每个用户上传的文件保存在其家目录下的“data/upload”目录下。
5. 在第一个段落中使用shell解释器查看文件是否成功上传。
![](/user_guide/fig/fig_61.png)

#### 编辑并运行代码
1. 新建一个段落，并输入如下代码，将文本文件上传到HDFS的/tmp目录下：
    
         %sh
         hdfs dfs -put data/upload/stock_data.csv /tmp
2. 新建一个段落，并输入如下代码，在Hive中创建数据库与表结构：

        %hive
        CREATE DATABASE  zet;
        %hive
        create table zet.stock_everydate_detail4
        (
        stock_name string,
        stock_type string,
        trans_time string,
        price double,
        up_or_down_price double,
        trans_count bigint,
        trans_nmoney bigint,
        trans_type string
        )
        PARTITIONED BY(dt String) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',';
3. 新建一个段落，并输入如下代码，将HDFS中的数据文件加载到已创建的Hive表中，并进行查看：

        %hive
        LOAD DATA INPATH '/tmp/stock_data.csv' INTO TABLE             zet.stock_everydate_detail4  PARTITION (dt='201312');
        use zet;

        select * from zet.stock_everydate_detail4 limit 10;

4. 单击段落右上角的![](/user_guide/icon/run.png)运行代码，显示结果如下所示：

![](/user_guide/fig/fig_62.png)

    代码成功运行后，以表格的形式展示表的前10行数据。也可以在hive解释器中执行其它Hive SQL对数据进行高效的查询和探查。














