## 算法定制
用户通过算法定制功能可以创建自定义的分析模块。此外，算法定制也提供了分析模块的版本控制等功能。

### 创建算法模块
单击页面左侧导航栏“算法定制”，系统跳转到“算法定制”界面。
![](/user_guide/fig/fig_33.png)

算法定制是指用户可以选择R、Python2.7、Python3.6等语言来编写轻量级应用分析模块，用户可根据需求选择语言。

1. 单击“新建”，弹出“新建模块”对话框，填写名称、描述，单击“确定”，系统跳转到算法编辑页面。
    ![](/user_guide/fig/fig_34.png)

<table>
   <tr>
      <td>配置项</td>
      <td colspan="2">说明</td>
   </tr>
   <tr>
      <td>名称</td>
      <td colspan="2">分析模块的名称。</td>
   </tr>
   <tr>
      <td>选择镜像</td>
      <td>模块运行的依赖环境</td>
      <td><p>minimal_python：仅包含python标准库。</p><p>zetdata_troy_python：除包含python标准库外，还引入了Pandas、Numpy、Sklearn等扩展库</p></td>
   </tr>
   <tr>
      <td>描述</td>
      <td colspan="2">对该分析模块的简要描述。</td>
   </tr>
</table>
   
2.填写完成后单击“确定”，系统跳转到算法编辑页面，支持用户自定义开发模块。

如下所示为一个Python语言算法定制的样例，在“文件管理”区域，“run.py”是由用户创建并编写的代码文件外，其它文件都是系统自动生成的文件。
![](/user_guide/fig/fig_35.png)

算法编辑页面主要分为四个区域：
<table>
   <tr>
      <td>区域划分</td>
      <td colspan="2">说明</td>
   </tr>
   <tr>
      <td>文件管理</td>
      <td>文件管理下的文件为算法程序文件，为系统自动生成的标准文件。文件列表中默认只有系统文件夹和文件，文件夹input和output，文件有spec.json、main.py（ main.R）、global.json、example、dependencies和readme.md，除readme.md外不可编辑</td>
     <td><li>Input下的文件存放测试时上传的文件，必须先在模块设置中定义“输入”然后再上传文件，Input下的文件个数不能超过定义的“输入”的个数。</li>
<li>Output文件夹下的文件是测试通过后输出的文件，默认为空；</li>
<li>spec.json文件：记录了算法的参数、输入、输出、私有、类型、版本、描述、名称和标签的信息，以及cmd信息；</li>
<li>main.py(main.R)用来记录runtime、CMD和entrypoint的信息；初始值为空，点击测试或者发布后才会有值；</li>
<li>Example文件：为静态文件，提供模块引用示例且内容不允许修改；</li>
<li>dependencies记录安装的程序包信息，初始信息为空；</li>
<li>readme.md为文件记录算法的说明，可以进行编辑内容，重命名或者删除。</li></td> 
   </tr>
   <tr>
      <td>模块设置</td>
      <td>可设置模块的参数、输入和输出，还支持上传和下载R/Ptyhon程序包。</td>
      <td>R在合法范围内可以输入任意值，但是R程序都按照字符串处理；Python支持三种参数类型：string、float、integer</td>
   </tr>
   <tr>
      <td>代码编辑区</td>
      <td colspan="2">分析模块的代码设计，支持代码高亮等。</td>
   </tr>
      <tr>
      <td>测试调试区</td>
      <td colspan="2">测试调试运行结果展示。</td>
   </tr>
</table>

3.参数设置。在模块设置区域填写模块的参数、输入和输出。
* **R语言传参模式及规则**
   
| 模块设置 | 输入格式 | 调用模式 |
| :--- | :--- | :--- |
| 参数 | 三个输入框依次为：参数名称、默认值、参数类型 | rt$Params$参数名称$Val |
| 输入 | 两个输入框依次为：文件引用名称、文件类型 | rt$Input$输入名称$Val |
| 输出 | 两个输入框依次为：文件引用名称、文件类型 | rt$Output$输出名称$Val |
   
下图中的“模块设置”，表示的含义如下所示：
   
- 该算法模块只有一个参数，其名称为param1，该参数的默认值为“age”，该参数的类型为“string”。
- 该算模块只有一个输入文件，在代码中其引用名称为input1，文件类型为“csv”；当该算法模块发布为分析模块后，在工作流中引用该模块时，该模块只有一个输入，且只能与输出类型为csv或any的模块相连。
- 该算法模块只有一个输出文件，其代码中其引用名称为input2，文件类型为“csv”；当该算法模块发布为分析模块后，在工作流中引用该模块时，该模块只有一个输出，且只能与输如类型为csv或any的模块相连。

 ![](/user_guide/fig/fig_36.png)

* **Python语言传参模式及规则**
   
| 模块设置 | 调用模式 |
| :--- | :--- |
| 参数 | Params.参数名称 |
| 输入 | Input.输入名称 |
| 输出 | Output.输出名称 |
  
下图中的“模块设置”，表示的含义如下所示：
   
 - 该算法模块只有三个参数，名称为param1的参数的默认值为“age”，参数类型为“string”；名称为param2的参数默认值为3.1415，参数类型为“float”；名称为param3的参数默认值为“3”，参数类型为integer。
 - 该算模块只有一个输入文件，在代码中其引用名称为input1，文件类型为“csv”；当该算法模 块发布为分析模块后，在工作流中引用该模块时，该模块只有一个输入，且只能与输出类型为csv或any的模块相连。
 - 该算法模块只有一个输出文件，其代码中其引用名称为input2，文件类型为“csv”；当该算法模块发布为分析模块后，在工作流中引用该模块时，该模块只有一个输出，且只能与输如类型为csv或any的模块相连。
    
 ![](/user_guide/fig/fig_37.png)
   
4.安装程序包。
  
系统支持上传和下载R/Python程序包。单击“包管理”，弹出 包管理对话框。例如我们本次的分析模块需要pandas程序包，通过搜索找到“pandas”，单击“安装”，系统会自动安装pandas程序包供用户调用。
  ![](/user_guide/fig/fig_38.png)
  
5.新建文件编辑代码。

将鼠标放在“文件管理”区域的根文件夹上，点击鼠标右键弹出对话框，选择“添加文件”，输入文件名称，以“run.py”为例。在代码编辑区写入模块代码，Ctrl+s保存文件。
![](/user_guide/fig/fig_39.png)

6.调试代码。

通过调试，可检测当前文件是否有语法错误、逻辑错误等；同时支持查看当前文件的运行结果。单击“请选择”下拉框的“添加”按钮，填写名称，选择需要调试的文件。如下所示的设置，表示调试时，将在容器中运行“run.py”脚本，从而检查其是否有语法错误。
![](/user_guide/fig/fig_40.png)

7.测试代码。

调试成功后可对文件进行测试。单击“测试”，弹出“发布测试”对话框。如下所示表示将“模块设置”中的输入“TrainDa”与input目录下的文件“train_dataset.csv”关联，作为调试的输入。若当前input目录下没有文件，则需要先上传文件。
![](/user_guide/fig/fig_41.png)

单击“测试”，系统会对该模块进行测试，在调试测试区查看运行结果。
![](/user_guide/fig/fig_42.png)

8.发布。

测试成功后发布模块。单击“发布”，弹出“发布”对话框，填写版本号。

9.版本控制。

分析模块的版本发布时可以自定义版本信息；系统默认从0.0版本起，之后自动加0.1。

10.版本号填写完成后单击“发布”，系统跳转到“算法定制”页面。
![](/user_guide/fig/fig_43.png)

| 创建状态 | 说明 |
| :--- | :--- |
| 编辑 | 模块处于编辑状态 |
| 发布中 | 模块处于发布状态 |
| 发布失败 | 模块发布失败 |

11.发布成功。

模块发布成功后，会自动添加到分析模块库中，此时算法定制界面不再呈现该记录。
![](/user_guide/fig/fig_44.png)

### 算法定制示例
在算法过程中，以文件作为其输入和输出。定制的算法模块发布后，在工作流中，从其上游模块中接收数据或向下游模块输出数据，这类数据是动态的，在工作流运行过程中流入或流出分析模块；此外，也可能用到静态的数据，例如在算法定制中可以引用保存在Zeppelin个人工作目录下的数据，也可以引用保存在HDFS上的数据，这类算法在发布后，在工作流运行时，依然从引用位置读取文件。这种静态文件的方式，可以用于依赖于大量数据的模型训练中，您可以更新训练数据集，但只要数据集的路径和文件名保持不变，则工作流在下次运行时将使用最新的数据集完成模型的训练。

说明：对于使用Zeppelin个人工作目录下的数据的算法，应设置为私人模块，否则若其他开发人员在工作流中引用了该模块，可能因为没有读取原开发者工作目录下数据的权限而导致整个工作流运行失败。

* 读取Zeppelin个人工作空间的Python代码示例
    - 场景说明：当前登录用户为“testuser”，在Zeppelin中其个人工作空间目录为“/mnt/nfsfile/userdata/testuser@MCIPT.COM/upload/”，如下示例给出了读写个人工作空间目录的代码样例。
    - 代码样例：
          #!/usr/bin/env python
          # -*- coding: utf-8 -*-
          def main(params, inputs, outputs):
          #如下代码向当前用户的工作空间目录的文件中写入内容
          f_w = open('/mnt/nfsfile/userdata/testuser@MCIPT.COM/upload/testfileA.txt','w+')
          f_w.write('Hello，this is a test.')
          f_w.close()
          #如下代码从当前用户的工作空间目录的文件中读取内容
          f_r = open('/mnt/nfsfile/userdata/testuser@MCIPT.COM/upload/csvfile.csv','r+')
          f = open('testfileB','w')
          f.write(f_r.read())
          f.close
          f_r.close()
       
     说明：读取的文件必须是在当前用户的工作空间下的，即路径“/mnt/nfsfile/userdata/username@MCIPT.COM/upload/”下的文件，其中username应使用实际的用户名替换。
  
* 读取HDFS的Python代码示例
    - 场景说明：在Python中，可以使用OS模块来执行操作系统命令，从而完成对HDFS上文件的读取操作。
    - 代码样例：
          import os
          import json
          def main(params, inputs, outputs):
          with open(inputs.input, 'r') as f:
               a = json.loads(f.read())
               url = a.get("URL")
               path = url[len(("/").join(url.split("/")[0:3])):]
               print path
             os.system("hdfs dsf -copyToLocal %s %s" %(path,outputs.output))

       说明：上述代码在执行时将HDFS中的文件拷贝到output中。


