# 普通 Linux 主机接入 {#concept_vvp_1jy_ngb .concept}

除了阿里云 ECS 和 Kubernetes 主机外，您可以通过手动安装探针的方式将任意厂商的 Linux 主机接入应用高可用服务 AHAS。

只要您的主机有公网连接，即可接入 AHAS 控制台，使用架构感知、故障测评、流控降级等功能。

## 前提条件 {#section_hpc_5yy_ngb .section}

已[开通 AHAS 服务](https://help.aliyun.com/document_detail/90323.html)。

## 选择地域 {#section_fdp_jky_ngb .section}

接入 Linux 主机前，在控制台选择地域：**公网**。

1.  登录 [AHAS 控制台](https://ahas.console.aliyun.com/)。

2.  在控制台左上角，选择**公网**。
3.  （可选）每个地域会有一个默认（Default）环境，您也可以添加自定义环境，如开发环境、测试环境等。
    1.  单击概览页面左上角的下拉列表，单击**添加环境**。

        ![](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/ahas/sc_env_selection.png) 

    2.  填写环境名称，单击**确定**。添加完成后，即可以在刚刚添加的环境中进行操作。

        如需切换，单击左上角下拉菜单，选择其他环境。


## 安装探针（地域：公网） {#section_dzl_4qh_dgb .section}

当您在地域列表中选择了**公网**，按照以下步骤安装应用高可用探针和 Java 探针。

1.  在 AHAS 控制台的**概览**页，单击**架构感知**模块下的**接入向导**。

    ![](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/ahas/sc_ecs_agent_installation_overview.png)

2.  在选择环境页签下，单击您要安装的环境。

    ![](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/ahas/pg_linux.png)

3.  在**安装应用高可用探针**页签下，建议您按照页面上的提示操作。

    **说明：** 在安装探针的过程中，需要使用 License。在控制台页面的命令行中，已经自动分配了您的 License。

    如果您按照本文中的步骤操作，请在控制台页面找到您的 License 并带入到以下命令行的 License 变量中。

    登陆主机，使用 root 用户执行以下命令：

    **说明：** license 替换为控制台命令行提示中的 license。

    ```
    wget -q https://ahasoss-cn-public.oss-cn-hangzhou.aliyuncs.com/agent/prod/aliyunahasctl.sh  -O /tmp/aliyunahasctl.sh && sh /tmp/aliyunahasctl.sh install -e prod -s public -k <license\> -r cn-public -n default
    
    ```

4.  单击**下一步**。
5.  如果需要同时使用流控降级功能，则需要安装 Java 探针；否则可跳过。

    通过在 Java 虚拟机中加载 Java 探针来识别 Java 应用，Java 应用名由传入的参数 AppName 来定义。

    根据 Java 虚拟机运行的环境，您可以从以下方式中选择一种方式来传入 Java 探针：

    -   **方式一：非容器化应用服务器**

        1.  确认文件 ahas-java-agent.jar 已经下载到主机的/opt/aliyunahas/agent目录下。通常，安装 AHAS 探针成功后，AHAS Java 探针也会随之下载到主机的 /opt/aliyunahas/agent目录下。
        2.  在应用 JVM 启动命令中，添加以下参数，将 AppName 替换为您的应用名称、license替换为控制台命令行提示中的 license。

            ```
            -Dproject.name=<AppName\> -Dahas.namespace=default -Dahas.license=<license\> -javaagent:/opt/aliyunahas/agent/ahas-java-agent.jar
            ```

        3.  启动 JVM。
        **说明：** 更多具体示例，请查看控制台引导页面提示页面提示。

    -   **方式二：Docker 容器**
        1.  初始化 ahas-java-agent 容器。

            ```
            sudo docker pull \
            registry.cn-hangzhou.aliyuncs.com/ahascr-public/ahas-java-agent:latest && \
            docker run \
            --rm \
            --detach \
            --privileged=true \
            -v ahas-javaagent:/var/lib/aliyunahas/agent:rw \
            registry.cn-hangzhou.aliyuncs.com/ahascr-public/ahas-java-agent:latest
            ```

        2.  将 ahas-java-agent 挂载到相应的 Java 应用容器上。在docker run命令后面添加以下启动参数。如已有JAVA\_OPTS，可将内容添加在后面。将AppName替换为您的应用名称、license 替换为控制台命令行提示中的 license。

            ```
            -v ahas-javaagent:/var/lib/aliyunahas/agent:rw \
            --env JAVA_OPTS="-Dproject.name=<AppName\> -Dahas.namespace=default -Dahas.license=<license\> -javaagent:/var/lib/aliyunahas/agent/ahas-java-agent.jar"
            ```

    -   **方式三：Docker Compose**
        1.  初始化 ahas-java-agent 容器。

            ```
            sudo docker pull \
            registry.cn-hangzhou.aliyuncs.com/ahascr-public/ahas-java-agent:latest && \
            docker run \
            --rm \
            --detach \
            --privileged=true \
            -v ahas-javaagent:/var/lib/aliyunahas/agent:rw \
            registry.cn-hangzhou.aliyuncs.com/ahascr-public/ahas-java-agent:latest
            ```

        2.  在 docker-compose.yaml 中添加以下配置项。如已有JAVA\_OPTS，可将内容添加在后面。将 AppName 替换为您的应用名称、license 替换为控制台命令行提示中的 license。

            ```
            environment: 
            - JAVA_OPTS="-Dproject.name=<AppName\> -Dahas.namespace=default -Dahas.license=<license\> -javaagent:/var/lib/aliyunahas/agent/ahas-java-agent.jar"
            volumes:
            - ahas-javaagent:/var/lib/aliyunahas/agent:rw
            ```

        3.  在 docker-compose.yaml 文件的最后面添加以下内容。

            ```
            volumes:
             ahas-javaagent:
              external: true
            ```

        4.  重启 Docker Compose。

            ```
            docker-compose up --build -d
            ```


## 后续操作 {#section_sxn_k2z_ngb .section}

接入成功后，您可以执行操作：

-   [查看系统架构](cn.zh-CN/架构感知/查看系统架构.md#)
-   [测评应用的高可用能力](https://help.aliyun.com/document_detail/90327.html)
-   [实时监控应用数据](../../../../../cn.zh-CN/流控降级/控制台指南/实时监控应用数据.md#)

