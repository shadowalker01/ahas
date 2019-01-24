# 查看容器组（Kubernetes） {#concept_ub4_s3z_ngb .concept}

为 Kubernetes 应用安装 AHAS 探针后，AHAS 能自动识别系统中的 Pod、Controller、Service 和它们与其他组件的依赖关系。

## 查看拓扑图 {#section_myn_2lz_ngb .section}

1.  登录 [AHAS 控制台](https://ahas.console.aliyun.com/)，从左侧导航栏选择**架构感知**。

2.  在架构感知页面，选择拓扑图左侧的**容器组**层，切换视图层次。拓扑图将显示当前环境中所有的容器组（默认显示 Pod 层架构信息）。

    在拓扑图上，可进行以下操作：

    -   **查看具体分层**：单击拓扑图左上角的 **Pod**、**Controller**和 **Service**，可查看具体分层视图。
    -   **筛选集群和命名空间**：在拓扑图上方的下拉列表中，可选择特定的 Kubernetes 集群或命名空间。
    -   **切换至表格视图**：单击拓扑图右上角的**拓扑图**开关图标，可切换至表格视图。
    ![](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/ahas/sc_container_group_topo.png)

3.  在拓扑图上，单击某个图标，可查看对应的 Pod、Controller 或 Service 的详情和具体拓扑图。示例如下图：

    ![](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/ahas/sc_container_group_detail.png)


## Pod 详情说明 {#section_g3x_tjg_4gb .section}

容器组中的 Pod 详情说明见下表（具体字段以实际接入的应用资源为准）。

|Pod 详情|说明|
|------|--|
|信息|命名空间（Namespace）|该 Pod 所在的命名空间|
|创建时间（Uptime）|该 Pod 创建的时间|
|重启次数（Restart）|重启次数|
|IP|IP 地址|
|容器数量（Container Count）|该 Pod 包含的容器个数|
|标签|Name|N/A|
|POD-TEMPLATE-HASH|N/A|
|容器|该 Pod 相关联的容器，单击容器名称可跳转至容器详情。|
|进程|该 Pod 相关联的进程，单击进程名称可跳转至进程详情。|

## Controller 详情说明 {#section_smg_lmg_4gb .section}

容器组中的 Controller 详情说明见下表（具体字段以实际接入的应用资源为准）。

|Controller 详情|说明|
|-------------|--|
|信息|命名空间（Namespace）|该 Controller 所在的命名空间|
|类型（Type）|N/A|
|创建时间（Uptime）|该 Controller 创建的时间|
|Replicas|N/A|
|ReadyReplicas|N/A|
|DesiredReplicas|N/A|
|UpdatedReplicas|N/A|
|ObservedGeneration|N/A|
|策略（Strategy）|N/A|
|容器数量（Container Count）|该 Controller 包含的容器个数|
|标签|TASK|N/A|
|K8S-APP|N/A|
|容器组|该 Controller 相关联的容器组，单击容器组名称可跳转至容器组详情。|
|容器|该 Controller 相关联的容器，单击容器名称可跳转至容器详情。|
|进程|该 Controller 相关联的进程，单击进程名称可跳转至进程详情。|
|入口连接|调用该 Controller 的所有组件及其端口；通过拓扑图中连接线的箭头方向可判断组件之间的调用关系。|
|出口连接|该 Controller 调用的所有组件及其端口；通过拓扑图中连接线的箭头方向可判断组件之间的调用关系。|

## Service 详情说明 {#section_j55_lng_4gb .section}

容器组中的 Service 详情说明见下表（具体字段以实际接入的应用资源为准）。

|Service 详情|说明|
|----------|--|
|信息|命名空间（Namespace）|该 Service 所在的命名空间|
|类型（Type）|N/A|
|创建时间（Uptime）|该 Service 创建的时间|
|IP|IP 地址|
|Ports|端口|
|Selector|N/A|
|容器数量（Container Count）|该 Service 包含的容器个数|
|标签|Name|N/A|
|容器组|该 Service 相关联的容器组，单击容器组名称可跳转至容器组详情。|
|容器|该 Service 相关联的容器，单击容器名称可跳转至容器详情。|

