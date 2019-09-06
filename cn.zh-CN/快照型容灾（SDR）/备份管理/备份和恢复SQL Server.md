# 备份和恢复SQL Server {#concept_pnz_thd_2gb .concept}

在混合云灾备一体机中，您可以根据策略对Windows服务器中的SQL Server进行备份和恢复。

## 备份SQL Server {#section_rg4_h3d_2gb .section}

要对Windows服务器中的SQL Server进行备份保护，您需要执行以下步骤：

1.  添加服务器到保护列表
    1.  在物理机的主菜单中单击**备份**。
    2.  单击![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64165/154520953334315_zh-CN.png)添加保护服务器。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64165/154520953434314_zh-CN.jpg)

    3.  在保护服务器页面，类型选择**Windows服务器**，并使用管理员组内账户来做备份。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953434520_zh-CN.png)

    4.  单击**提交**。

        提交后，您可以在任务监控概览页的**状态**一栏查看所有任务的当前状态。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64165/154520953434378_zh-CN.png)

        等待1~2分钟后，服务器添加任务完成。单击左边备份列表中添加的服务器，此时右边菜单栏出现的**MS-SQL**选项，即用来备份SQL Server。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953434525_zh-CN.png)

2.  创建合理的备份策略

    服务器被添加到备份列表后，您就可以对服务器中的SQL Server数据库进行全量\(full\)和差异\(differential\)备份。在进行SQL Server数据库备份之前，您需要创建合理的备份策略。

    **说明：** SQL Server不能进行增量\(incremental\)备份，只能进行差异\(differential\)备份，因此创建策略时不能使用带增量备份的策略。

    SQL Server的备份策略需要包含以下几项，示例如下表所示：

    |备份策略|示例|
    |:---|:-|
    |全量备份的时间和频率|每周五22:00做一次全量备份。|
    |差异\(differential\)备份的时间和频率|周日到周四每天22:00做一次增量备份。|
    |备份是否上传阿里云|备份上传阿里云。|
    |本地备份和云上备份的保留时间|本地备份保留30天时间，阿里云上备份保留60天。|

    结合上述示例，执行如下步骤创建合理的备份策略：

    1.  在物理机的主菜单中单击**备份策略**。
    2.  单击**创建**。
    3.  在创建策略页面添加全量\(full\)备份策略，并**开启上云复制**，指定本地备份和云上备份保留时间。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953434527_zh-CN.png)

        在同一个页面，按照添加全量备份策略的步骤添加差异\(differential\)备份策略。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953434529_zh-CN.png)

3.  创建备份计划

    以上述创建好的备份策略为例，您可以基于该策略对被保护服务器的SQL Server数据库创建备份计划。

    1.  在备份页面，选择要指定备份计划的SQL Server所在的Windows服务器。
    2.  在MS-SQL页面，选择要备份的数据库实例。
    3.  右上角的下拉框选择**添加**，单击计划图标![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64167/154520953434460_zh-CN.jpg)创建备份计划。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953434530_zh-CN.png)

    4.  在创建计划页面，填写**任务名**，选择备份策略**sqlserver-policy**，并指定备份计划。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953434531_zh-CN.png)

        **说明：** 您可以选择**立即运行**、**运行时间**或**计划**，并指定具体的时间进行自动或手动备份。

        指定备份计划后，所选服务器就会按照备份策略指定的时间和方式，对SQL Server数据库实例进行定时备份。备份数据也会按照策略中所指定的上云设置存储到云端。您可以在所有计划菜单中查看服务器相关的所有计划。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953434534_zh-CN.png)

        -   如果您想在该备份计划之外的某个时间手动触发一次SQL Server的全量或差异备份。

1.  勾选需要全量或差异备份的计划，单击右上角的立即运行图标![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64167/154520953434469_zh-CN.jpg)。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953434535_zh-CN.png)

2.  在立即运行页面的**备份类型**中选择**Full**或**Differential**触发全量或差异备份。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953434536_zh-CN.png)

        -   如果您想要删除基于备份计划的自动备份，在**所有计划**中勾选要删除的计划，单击删除图标![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64167/154520953534476_zh-CN.jpg)删除备份计划。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953534539_zh-CN.png)

            **说明：** 删除备份计划并不会删除备份，这些备份将会在备份过期时自动删除。


## 恢复SQL Server {#section_cnq_c5x_dgb .section}

您可以在本地或者云上恢复已备份的SQL Server数据库实例。

-   本地恢复SQL Server

    本地恢复已备份的SQL Server数据库实例，您需要执行以下步骤：

    1.  将SQL Server数据库实例要恢复到的目标服务器添加到保护列表

        有关添加服务器到保护列表的详细说明，请参考[添加服务器到保护列表](#)。

        **说明：** 目标服务器必须为安装了SQL Server的Windows服务器。

    2.  选择要恢复的SQL Server数据库实例和备份时间点
        1.  单击主菜单中的**恢复**。
        2.  选择要恢复的服务器，在**MSSQL恢复**页面，根据您希望恢复到的时间点选择一个备份，单击**恢复**。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953534541_zh-CN.png)

        3.  在MS-SQL恢复向导页面中指定**目标服务器**，单击**下一步**。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953534542_zh-CN.png)

        4.  指定要恢复的SQL Server数据库实例，并为数据库实例指定目标路径。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953534543_zh-CN.png)

        5.  指定**任务名**。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953534544_zh-CN.png)

        6.  单击**提交**。

            提交后，您可以在监控页面查看已选择的SQL Server数据库实例的恢复进度。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64168/154520953534550_zh-CN.png)

            恢复完成后，可以在目标服务器中指定目标路径下查看恢复出的SQL Server数据库实例的.mdf文件、 .ldf文件和.ndf文件。在目标服务器中用Windows或SysAdmin登录SQL Server，添加恢复出的数据库实例后即可进行数据库操作。

-   云上恢复SQL Server

    云上恢复方法及配置与本地恢复相同，只是所有操作在云上网关进行。

    **说明：** 添加的目标服务器为云上ECS。恢复完成后，可以在ECS控制台查看恢复出的SQL Server数据库实例。


