# 通过开源 Sentinel SDK接入 {#concept_gsp_yw3_kgb .concept}

通过开源组件 Sentinel，您可以便捷地接入 AHAS 控制台流控降级服务。仅通过替换安装包中的 JAR 包即可实现 Sentinel 和 AHAS 流控降级的灵活切换。

具体操作，参考开源版本的[新手指南](https://github.com/alibaba/Sentinel/wiki/%E6%96%B0%E6%89%8B%E6%8C%87%E5%8D%97#%E5%85%AC%E7%BD%91-demo) 来连接 AHAS 流控降级控制台。

**说明：** 若您之前接入了开源 Sentinel 控制台，您可以将 pom 包中的 `sentinel-transport-simple-http`模块替换为 `ahas-sentinel-client` 模块，接入 AHAS 流控降级控制台。

**注意：** 若在本机或非阿里云 VPC 网络运行，请注意在控制台左上角选择 Region 为**公网**。

