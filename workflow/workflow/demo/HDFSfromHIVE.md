
HDFS导入HIVE
----------------

### 步骤（1）新建工作流

此处为了方便阅读和表明工作流的专门用途，填写为“test_hdfs_to_hive”。工作流名称可以自定义，中英文均可。

![](media/bd8c04abb6b88a7fcaa34aaab68f2e60.png)

### 步骤（2）新建任务

首先，点击如图左上角的添加任务按钮，其次，任务类型选择“HDFS导入HIVE“，最后点击”确定“。

![](media/d43a7731a82918066c6600dac02a0838.png)

### 步骤（3）填写任务基本信息

需要填写的基本信息主要有：

1.  任务名称：用于唯一标识此次任务的名称，中英文均可。本次填写：hdfs_to_hive。

2.  责任人：指可以修改和操作该任务的用户，一个任务可以有多个责任人。默认是任务的创建者。

3.  告警方式：提供两种告警方式，分别是邮件和短信。（邮件和短信设置都可在用户基本信息中添加和修改）。

4.  告警接收人：指应该收到告警的用户，通常设置为责任人和运维人员，如果不添加的话，默认为责任人。

5.  告警类型：分为超时告警和失败告警两种。

6.  任务对应实例失败（终止）会触发失败告警。

7.  超时告警需要设置“预计任务耗时“，任务实例执行时长超过此时间，就会触发超时告警。

8.  预计任务耗时：任务预计执行时间。

![](media/9a6c86682eb7601671ef894b7c6fd0a7.png)

### 步骤（4）填写任务调度设置

此处主要填写的内容有:

1.  周期类型：周期类型支持分钟，小时，天，周，月四种周期类型。非周期即为一次性任务，任务整个生命周期只有一个实例。此处选择为“小时”。

2.  起始数据时间：指任务开始时间，如果不选择，默认为当天。

3.  自身依赖：指任务实例之间的依赖关系，选项有：是，否，并行。此处选择：否。

4.  是：当前实例依赖前一个周期的实例的状态，即前一个周期实例运行成功之后，才会开始运行当前实例）。

5.  否：当前实例和前一个周期实例没有依赖关系，如果一个任务同时存在多个实例，系统随机选取一个实例运行，也就是说同一时间只有一个实例是运行状态。

6.  并行：前一个周期实例和后一个周期实例之间没有依赖关系，如果一个任务同时存在多个实例，多个实例会同时运行。

7.  调度时间：与周期类型有关，系统会显示周期类型的下一子集供用户选择。若不填写，默认为零，小时任务会从整点开始，分钟任务从0秒开始，天任务从0点开始，月任务从每月1号开始，周任务从每周一开始。

![](media/df245a571d338fa637cdfe0ba76a7942.png)

### 步骤（5）填写任务参数设置

#### 1．配置源服务器

本示例为“HDFS导出Hive”，即从HDFS中导出数据，则源服务器类型为HDFS，参数配置如下：

![](media/2de5e4a3d661787e0c9e5b3d9558501f.png)

1.  服务器类型：此处为源服务器，选择hdfs。

2.  服务器标识：应命名唯一标识，即可唯一识别此服务器的名称，此处命名hdfs_Cluster。

3.  namenode主机地址：系统默认填写hdfsCluster。

4.  HDFS集群访问的并发用户数目：可同时访问此集群的用户数目，系统默认为10。

最后点击连接测试，显示“连接测试成功，配置有效！”，则表明，配置成功。如下图所示。

![](media/c26fdaaceaead32b9fad585e59127ac3.png)

#### 2．配置目标服务器

本示例为“HDFS导出Hive”，即从HDFS中导出数据至HIVE，则目标服务器类型为HIVE，参数配置如下：

![](media/bf59a99a9e4101fd4a1cf869bc80914e.png)

1.  服务器类型：此处为目标服务器，选择hive。

2.  服务器标识：应命名唯一标识，即可唯一识别此服务器的名称，此处命名hive_server。

3.  连接地址：为Hive服务的主节点地址，一般情况下，系统默认填写。

4.  端口：hive服务所用的端口号，一般默认填写为10000。

5.  服务器的并发度：系统默认为10，用户可自行配置。

最后点击连接测试，显示“连接测试成功，配置有效！”，则表明，配置成功。如下图所示。

![](media/3539ccef180926f0246cb02a76477a2d.png)

若出现“温馨提示”对话框，则会提示连接错误，需要重新检查参数之后再进行测试。

![](media/f2e2ddd6e570cca40fd3163658295728.png)

#### 3．填写任务参数

此次任务为“HDFS导入HIVE”，具体参数配置如下：

![](media/e570c48af6fa0c9146acbc7a1969b4ec.png)

1.  源目录：为HDFS为需要导入到hive的hdfs文件所在目录，此处为：“/project/testproject/tbds_test/hdfs_to_hive_test/”

2.  源文件名：为HDFS为需要导入到hive的hdfs文件，此处为：“hdfs2hive”。

3.  源文件字符集：默认GBK,可在创建Hive表时，进行设置。

4.  字段映射关系：hive表列名,以英文逗号分隔,表示列的内容顺序,需要和数据库中的列字段保持一致。决定从临时表往目的表里写的字段顺序。此处为：“name,sex,id”

5.  DB名称：待写入数据的hive数据库名称，此处创建为：“hdfs_to_hive_test”，具体创建过程在附录中说明。

6.  目标表名：待写入数据的hive表名，此处为：“hive_test”

### 步骤（6）任务审批

点击确定之后，系统会将任务状态设为“新建”，然后对画布中的任务点击右键，在弹出列表中点击“运行”

![](media/9be25b5a2108b614182e88b95cf1b961.png)

接着会弹出任务审批对话框，此处可选择审批通过后自动运行，或者审批之后手动开启，具体表现为是否勾选下图中红色部分。

![](media/9ffc9c5576fb36776f3febe5db23f539.png)

系统会向此项目的管理员发送审批邮件，由管理员来确定任务是否通过审批，若管理员通过此次任务运行，可以看到任务状态会变成下图所示。

![](media/e500b887bb1a9d749d8dd52f8359f8aa.png)

### 步骤（7）**查看任务状态**

再右键点击任务，选择“查看运行状态”，即可看到此次任务的运行状态

![](media/70e7a69199c01792023d114c2f83ae21.png)

至此，此次Demo完毕。