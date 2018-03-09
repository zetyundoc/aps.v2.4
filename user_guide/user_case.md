# 数据应用示例

## 场景介绍
基于客户属性、交易行为、偏好等进行聚类，再基于聚类结果进行分析，提炼信息（例如高端金融男等）后，即可针对不同类别客户提供差异化营销。
本数据应用的主要目的是为了在众多信息中归纳总结对客户分群。

## 前提条件
承载基础数据的数据模块kmeansdemo1已经设置好，相关数据存在。
本文所提到的分析模块，系统已经集成。

## 具体步骤

1. 创建分析工作流。
    1. 登录系统后，单击“服务”>“分析应用”进入分析应用界面。
    2. 单击页面左侧导航栏“数据应用”，系统跳转到“数据应用”界面。
    ![](/user_guide/fig/fig_66.png)
    3. 单击页面右上角的“新建”按钮，弹出“新建数据应用”窗口。
        输入数据应用的名称“kmeans_test”和描述“聚类分析”，勾选“私有”按钮，单击“确定”。就可以在数据应用界面看到刚创建的kmeans_test数据应用。
        ![](/user_guide/fig/fig_67.png)
    4. 单击kmeans_test数据应用，进入kmeans_test基础信息展示页面。
    ![](/user_guide/fig/fig_68.png)
    5. 单击“打开工作流”按钮，进行数据分析工作。
    ![](/user_guide/fig/fig_69.png)
2. 添加数据模块。
    
    搜索kmeansdemo1数据模块后单击回车，将数据模块kmeansdemo1拖曳到画布中。
    ![](/user_guide/fig/fig_70.png)
3. 下载并加载数据。

    i. 搜索datasource_downloader分析模块后单击回车，拖曳分析模块datasource_downloader1到画布中实现下载数据到本地的功能。连接kmeansdemo1输出参数和datasource_downloader1的输入参数。
    ![](/user_guide/fig/fig_71.png)
    ii. 搜索load_csv分析模块后单击回车，拖曳分析模块load_csv1到画布中实现数据加载功能。连接datasource_downloader1输出参数和load_csv1的输入参数。
    ![](/user_guide/fig/fig_72.png)
        单击分析模块load_csv设置变量，例如设置select_by_col_name为 #表示选取所有列，单击“保存”。
    ![](/user_guide/fig/fig_73.png)
    
4. 添加聚类分析算法模块，展现聚类分析结果。

    i. 搜索kmeans分析模块后单击回车，拖曳分析模块kmeans1到画布中实现聚类分析算法。连接load_csv1输出参数和kmeans1的输入参数。
    ![](/user_guide/fig/fig_74.png)
    ii. 拖拽分析模块FormShow1、Visual1和FormShow1三个模块到画布中展示聚类分析结果。Kmeans1有四个输出参数，分别为分类类别展示、模型可视化、轮廓系数和簇个数评估、聚类畸变。
    iii. means1输出参数1和第一个FormShow1的输入参数；连接kmeans1输出参数2 、4和Visual1的输入参数1、2；连接kmeans1输出参数3和第二个FormShow1的输入参数。
    ![](/user_guide/fig/fig_75.png)
5. 保存并运行数据分析。
    i. 单击右上角“保存”按钮保存数据应用。
    ii. 单击右上角“运行”按钮，配置应用运行。例如设置任务名称为kmeans，描述为“run”单击“运行”按钮运行数据应用。
    ![](/user_guide/fig/fig_76.png)
    系统会显示运行状态。
    ![](/user_guide/fig/fig_77.png)
6. 查看运行结果。
      数据应用运行成功后，在任务列表里可查看到相应的数据应用运行结果。
      
      单击“任务列表”查看任务名称为kmeans的任务，单击名称进入kmeans运行结果。
      
     i. 在 “运行状态”页签找到第一个formshow1模块，鼠标滑过模块出现“+”时，单击“+”后，出现![](/user_guide/icon/link.png)图标。
     ![](/user_guide/fig/fig_78.png)
     单击![](/user_guide/icon/link.png)图标进入相应分析模块的输出界面。
     ![](/user_guide/fig/fig_79.png)
     单击结果中的![](/user_guide/icon/resultshow.png)图标，进入formshow输出页面。可以查看到分类类别展示的结果。
     ![](/user_guide/fig/fig_80.png)
     ii. 在“运行状态”页签找到Visual1模块，鼠标滑过模块出现“+”时，单击“+”后，出现![](/user_guide/icon/link.png)图标。
     ![](/user_guide/fig/fig_81.png)
     单击![](/user_guide/icon/link.png)图标进入相应分析模块的输出界面。
     ![](/user_guide/fig/fig_82.png)
     单击结果中的![](/user_guide/icon/resultshow.png)图标，进入Visual输出页面。可以查看到模型可视化和聚类畸变的结果。
     ![](/user_guide/fig/fig_83.png)
     ![](/user_guide/fig/fig_84.png)
     iii. 在“运行状态”页签找到第二个formshow模块，鼠标滑过模块出现“+”时，单击“+”后，出现![](/user_guide/icon/link.png)图标。
     ![](/user_guide/fig/fig_85.png)
     单击![](/user_guide/icon/link.png)图标进入相应分析模块的输出界面。
     ![](/user_guide/fig/fig_86.png)
     单击![](/user_guide/icon/link.png)图标进入相应分析模块的输出界面。
     单击结果中的![](/user_guide/icon/resultshow.png)图标，进入formshow输出页面。可以查看到轮廓系数和簇个数评估的结果。
     ![](/user_guide/fig/fig_87.png)


