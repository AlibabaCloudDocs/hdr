# 步骤三：部署BCDR网关 {#concept_92482_zh .concept}

当容灾站点创建完成后，您需要在数据中心内部署一台关键业务型（BCDR） 网关。BCDR 网关用来聚合所有来自被容灾保护的服务器上的数据，并将其压缩加密后安全高效地上传到阿里云上。

部署 BCDR 网关，包含以下四个步骤：

1.  [创建并下载网关镜像](#)
2.  [部署网关镜像](#)
3.  [配置网关网络与时间](#)
4.  [激活网关](#)

**说明：** 

-   BCDR 网关需要打开公网出方向的 1883 和 443 端口，用于和云上控制台通信以及数据传输。
-   BCDR 网关通过 9090 和 9095 端口向被保护服务器提供服务，8080 端口用于通过浏览器访问来激活网关。
-   客户端程序通过 9080 端口接受 BCDR 网关的指令。

因此您需要合理设置防火墙，保证被保护服务器可以连通网关的 9090 和 9095 端口，并且被保护服务器的 9080 端口允许 BCDR 网关访问。

## 创建并下载网关镜像 {#download .section}

1.  登录[混合云容灾控制台](https://hdr.console.aliyun.com)。
2.  在关键业务型容灾页面，单击**容灾网关**页签。
3.  单击右上角的**创建网关**。
4.  在创建主站容灾网关页面，填写**名称**，部署环境选择**灾备一体机**、**VMware**或**Hyper-V**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553239288_zh-CN.jpg)

5.  单击**创建**。
6.  在创建主站容灾网关页面，单击**下载容灾网关镜像**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553233649_zh-CN.png)

    您也可以在容灾网关列表中，单击下载容灾网关镜像图标![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553233648_zh-CN.png)，如下图所示：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553233647_zh-CN.jpg)


## 部署网关镜像 {#mirror .section}

本节介绍如何在 [VMware](#) 以及 [Hyper-V](#) 中部署网关镜像。

-   在 VMware 中部署 BCDR 网关

    OVA 格式的网关镜像下载完成后，您就可以在 VMware vSphere 平台上部署 BCDR 网关了。BCDR VMWare 镜像可以在 VMWare 5.5 - 6.5 上部署。

    **注意：** 

    -   强烈建议将 BCDR 网关部署在已配置了高可用（HA）的 vSphere 集群上，确保 BCDR 网关的高可用性。
    -   OVA 仅支持通过 vCenter 的网页客户端（Web Client）来部署，通过 vCenter 的 C\# 客户端或者 Esxi UI 部署都会使网关无法工作。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553333720_zh-CN.jpg)

    在 VMware vSphere 平台上部署 BCDR 网关操作步骤如下：

    1.  用管理员身份登录 vSphere 网页客户端后，vSphere 集群上单击右键选择**部署OVF模板…**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553333721_zh-CN.png)

    2.  按照 OVF 部署向导，在**选择模板**页面，单击**浏览**选择下载好的网关镜像 OVA 文件。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553333722_zh-CN.jpg)

    3.  在**选择名称和位置**页面，输入**名称**，并选择镜像部署位置，通常只需要选择到集群级别，或者选择文件夹。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553433723_zh-CN.jpg)

    4.  在**选择资源**页面，选择 BCDR 网关部署的目标资源池。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553433724_zh-CN.png)

    5.  在**查看详细信息**页面，查看 BCDR 网关部署详细信息。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553433725_zh-CN.jpg)

    6.  在**选择存储**页面，选择部署网关的数据存储。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553533726_zh-CN.png)

        **说明：** 数据存储建议选择有 RAID 配置的共享存储，以确保网关的稳定性和高可用性。

    7.  在**自定义模板**页面，配置网关网络信息。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553533727_zh-CN.jpg)

        **说明：** 网关子网掩码要使用 CIDR 格式，比如 255.255.255.0 的子网掩码，您应当输入24。

        管理员用户设置主要用于后续登录 BCDR 虚拟机做内部网络等配置。

    8.  确认网关部署信息，单击**完成**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553533734_zh-CN.png)

    9.  待部署完成后，您可以在 vSphere 的虚机列表中看到已部署完成的虚拟机。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553533737_zh-CN.jpg)

    10. （可选，建议配置）增强虚机高可用性

        在配置了高可用（HA\) vSphere 集群上，如果 BCDR 网关所在的 Esxi 宕机，网关会在同集群其他 Esxi 服务器上运行起来，宕机时间取决于 vSphere 集群的 HA 配置。建议您提高网关虚机重启优先级，以尽量缩短虚机启动时间。

        对于 vSphere 6.5 以前的版本，您可以参考[vSphere文档](https://pubs.vmware.com/vsphere-50/topic/com.vmware.vsphere.avail.doc_50/GUID-F9C1EBCF-87C5-4FF2-8149-19D2ACF09890.html)进行配置。

        对于 vSphere 6.5 以上版本，您可以按照以下步骤增强虚机高可用性：

        1.  在 BCDR 网关所在集群的**配置**页面，单击**虚拟机替代项**，然后单击**添加**。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553633739_zh-CN.jpg)

        2.  在添加虚拟机替代项页面，单击图标![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553633740_zh-CN.jpg)选择虚拟机。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553633745_zh-CN.jpg)

        3.  在选择虚拟机的**筛选**页面，勾选本示例中的虚拟机**Aliyun-BCDR-Server-1.0.0**。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553633747_zh-CN.jpg)

        4.  **虚拟机重新启动优先级**一栏，选择**最高**。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553733748_zh-CN.jpg)

        完成以上步骤后，一旦 BCDR 网关虚机出现宕机，vSphere 将会尽快将网关在其他 Esxi 服务器上重启，让容灾复制继续进行。

-   在 Hyper-V 中部署 BCDR 网关

    **说明：** 建议将 BCDR 网关部署在已配置了高可用的 \(HA\) 的 Hyper-v 集群上，确保 BCDR 的高可用性。

    在 Hyper-V 中部署 BCDR 网关操作步骤如下：

    1.  解压上述步骤中[创建的 BCDR 网关镜像](#)。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553740759_zh-CN.png)

    2.  打开 Hyper-V 管理器，右键选择**导入虚拟机**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553740760_zh-CN.png)

    3.  在定位文件夹页签，选择第 1 步已解压的文件夹。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553740768_zh-CN.png)

    4.  在选择导入类型页面，勾选**还原虚拟机（使用现有的唯一 ID）**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553740770_zh-CN.png)

    5.  在选择目标页面，选择使用默认文件夹或创建新的文件夹。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553840783_zh-CN.png)

    6.  在选择存储文件夹页面，选择虚拟磁盘存放路径。

        **说明：** 请根据实际情况选择剩余空间容量较大的分区。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553840786_zh-CN.png)

    7.  在摘要页面，待导入向导完成后，单击**完成**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553840787_zh-CN.png)

    8.  右击虚拟机选择**设置**，单击**网络适配器**选择虚拟交换机。

        **说明：** BCDR 网关默认配置的虚拟交换机导入完成后会失效，请新建一个虚拟交换机或者使用可用的虚拟交换机。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553840790_zh-CN.png)

    9.  指定网络适配器后单击**确定**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553840792_zh-CN.png)

    10. 单击**虚拟交换机管理器**配置虚拟交换机。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553940794_zh-CN.png)

    11. 选择虚拟机配置时新建的虚拟交换机，选择需要共享网络的物理网卡，并勾选**允许管理操作系统共享此网络适配器**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553940795_zh-CN.png)


## 配置网关网络与时间 {#NTP .section}

**说明：** VMWare 平台上用 OVA 部署的网关在部署过程中已经完成了网络配置。如果您不需要对网络配置进行修改，请直接执行[激活网关](#)中的步骤。

网关虚机或混合云灾备一体机部署好之后，就可以做基本的网络配置了。BCDR 网关网络既要能够连接容灾保护的服务器，还要允许通过公网或者专线连接至阿里云。

对于虚拟化部署的环境，您可以直接在虚拟化平台控制台中进入虚机界面。如果是混合云灾备一体机，则可以直接从本地连接显示器登录服务器。

配置网关网络与时间步骤如下：

1.  使用默认的用户名root、密码Aliyun!23登录虚拟化平台控制台。
2.  在配置页面，单击**NetWork**选项，根据您网络的实际情况，配置 IP 地址、子网掩码、网关、DNS 服务器。

    **说明：** 其中，子网掩码网需使用CIDR格式 。比如，子网掩码为255.255.255.0，您应当输入24。

3.  单击**Date/Time**，将 NTP Server 设置为阿里云 NTP Server，即ntp.aliyun.com 。

## 激活网关 {#activate .section}

激活网关是为了让网关与阿里云正式建立连接，并且准备好接收容灾复制数据流。

网关网络与时间配置完成后，您可以按照如下步骤激活网关：

1.  在容灾网关列表中，单击操作类下的生成激活码图标![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553933796_zh-CN.jpg)。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553933797_zh-CN.jpg)

    **说明：** 激活码的有效时间为 15 分钟。

2.  登录关键业务型容灾网关注册页面，地址格式为：http://网关IP:8080。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64154/156478553933799_zh-CN.png)

    各参数配置如下：

    -   **Access Key**和**Access Key Secret**：使用混合云容灾服务权限的 AK 即可。
    -   **激活码**：填写步骤一中获取的激活码。
    -   **密码**：密码用于后续需要做容灾保护的服务器与网关建立安全连接，您可以按照自己的要求指定密码。
3.  单击**确认**。

BCDR 网关激活完成后，您需要在关键业务服务器上安装阿里云复制服务（Aliyun Replication Service, AReS），将服务器复制到阿里云。

