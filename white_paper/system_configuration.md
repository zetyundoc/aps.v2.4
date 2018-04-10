# 系统配置

## 大数据服务器以及网络推荐配置

大数据服务器一般需要三台以上主机作为主节点，以及数台（具体数量按照实际数据量增减）数据节点。为保证高效IO，需要对不同的服务（数据量较大的服务）配置不同的硬盘。推荐配置如下：
操作系统兼容性：大数据服务器的操作系统对Linux内核版本2.6以上版本全面支持。例如发行版CentOS6.5、SUSE11SP2以上版本。
<table>
   <tr>
      <td colspan="5">系统硬件推荐配置表</td>
   </tr>
   <tr>
      <td>每天日志量</td>
      <td>500 GB</td>
      <td>1 TB</td>
      <td>3 TB</td>
      <td>10 TB</td>
   </tr>
   <tr>
      <td>日志管理主机数</td>
      <td>4</td>
      <td>8</td>
      <td>22</td>
      <td>72</td>
   </tr>
   <tr>
      <td>日志分析主机数（*仅需要开启日志分析功能时需要）</td>
      <td>2</td>
      <td>4</td>
      <td>11</td>
      <td>36</td>
   </tr>
   <tr>
      <td>总主机数</td>
      <td>6</td>
      <td>12</td>
      <td>33</td>
      <td>108</td>
   </tr>
   <tr>
      <td>单机硬盘容量</td>
      <td>33T</td>
      <td>33T</td>
      <td>35T</td>
      <td>36T</td>
   </tr>
   <tr>
      <td>硬盘数（每块1T）</td>
      <td>33</td>
      <td>33</td>
      <td>35</td>
      <td>36</td>
   </tr>
   <tr>
      <td>CPU</td>
      <td>40核以上</td>
      <td>40核以上</td>
      <td>40核以上</td>
      <td>40核以上</td>
   </tr>
   <tr>
      <td>内存（GB）</td>
      <td>256</td>
      <td>256</td>
      <td>256</td>
      <td>256</td>
   </tr>
   <tr>
      <td>网卡</td>
      <td>千兆网卡（推荐万兆网卡）</td>
      <td>万兆网卡 （推荐）</td>
      <td>万兆网卡 （推荐）</td>
      <td>万兆网卡 （推荐）</td>
   </tr>
</table>

## 应用服务器推荐配置
应用服务器操作系统支持Linux内核版本2.6以上，以及Windows Server 2008 R2以上版本，应用服务器需安装1.7以上版本JAVA环境。

<table>
   <tr>
      <td>类别</td>
      <td>推荐配置</td>
   </tr>
   <tr>
      <td>操作系统</td>
      <td>CentOS7或SUSE11SP4</td>
   </tr>
   <tr>
      <td>CPU</td>
      <td>2核以上Intel处理器</td>
   </tr>
   <tr>
      <td>内存</td>
      <td>16GB</td>
   </tr>
   <tr>
      <td>硬盘</td>
      <td>1TB</td>
   </tr>
   <tr>
      <td>网卡</td>
      <td>千兆网卡</td>
   </tr>
</table>

## 采集端系统兼容性

APS采集端需运行Java 1.7+以上版本。

## Web浏览器兼容性

APS仅全面支持Chrome浏览器。 

## POC验证环境推荐配置

POC验证环境，APS至少需要三台主机，一台应用服务器主机即可，基本配置与大数据服务器相当。平台配置如下：

<table>
   <tr>
      <td>类别</td>
      <td>推荐配置</td>
   </tr>
   <tr>
      <td>操作系统</td>
      <td>CentOS7或SUSE11SP4</td>
   </tr>
   <tr>
      <td>CPU</td>
      <td>2路4核Intel处理器</td>
   </tr>
   <tr>
      <td>内存</td>
      <td>16GB</td>
   </tr>
   <tr>
      <td>硬盘</td>
      <td>1TB</td>
   </tr>
   <tr>
      <td>网卡</td>
      <td>千兆网卡</td>
   </tr>
</table>