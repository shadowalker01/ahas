# 通过 SDK 接入 {#concept_gz5_d3c_kgb .concept}

您可以通过 SDK 接入的方式，将应用接入 AHAS 控制台，使用流控降级服务。目前 AHAS 提供 Java SDK。

## 操作引导 {#section_mqn_zjd_ggb .section}

您可以在 AHAS 控制台查看安装流控降级 SDK 的相关步骤。

1.  登录 [AHAS 控制台](https://ahas.console.aliyun.com/)。在左侧导航栏，单击**流控降级**。

2.  在流控降级页面右上角，单击**添加应用**，选择**SDK 接入**页签，根据页面引导，安装 SDK。

![](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/ahas/sc_sdk.png)


## 操作步骤 {#section_tlk_gxx_fgb .section}

1.  为应用添加依赖，可通过两种方式：
    -   （推荐方式）添加 Maven 依赖：在 pom.xml 文件中引入如下依赖，将 Version Number 替换为当前的版本号：

        ```
        <dependency>
          <groupId>com.alibaba.csp</groupId>
          <artifactId>ahas-sentinel-client</artifactId>
          <version>${Version Number}</version>
        </dependency>
        ```

    -   添加 JAR 包依赖：
        1.  根据控制台 SDK 接入页面提供的下载地址，下载压缩包。
        2.  解压压缩包，将压缩包目录下所有的 JAR 包拷贝到 classpath 下，引入即可。
2.  根据不同的应用类型，在应用中配置对应的代码。
    -   Dubbo 应用：无需配置。
    -   传统 Web 应用：在 web.xml 中引入 CommonFilter，并将其配置为第一个 Filter，例如：

        ```
        <filter>
          <filter-name>CommonFilter</filter-name>
          <filter-class>com.taobao.csp.sentinel.web.CommonFilter</filter-class>
        </filter>
        <filter-mapping>
          <filter-name>CommonFilter</filter-name>
          <url-pattern>*.htm</url-pattern>
        </filter-mapping>
        <filter-mapping>
          <filter-name>CommonFilter</filter-name>
          <url-pattern>*.html</url-pattern>
        </filter-mapping>
        <filter-mapping>
          <filter-name>CommonFilter</filter-name>
          <url-pattern>*.do</url-pattern>
        </filter-mapping>
        ```

    -   SpringBoot 应用：在工程中加入如下类：

        ```
        @Configuration public class WebConfig extends WebMvcConfigurerAdapter {
          @Bean public FilterRegistrationBean sentinelFilterRegistration() {
            logger.info("sentinelFilterRegistration(), add CommonFilter");
            FilterRegistrationBean registration = new FilterRegistrationBean(); 
            registration.setFilter(new CommonFilter());
            registration.addUrlPatterns("*.json","*.hml","*.html","/demo/*");
            registration.addInitParameter("paramName", "paramValue"); registration.setName("sentinelFilter");
            registration.setOrder(1); 
            return registration;
          }}
        ```

    -   手动埋点接入：使用如下代码块包住您的业务逻辑：

        ```
        Entry entry = null;
        try { 
          entry = SphU.entry("HelloWorld"); // BIZ logic being protected System.out.println("hello world");
        } catch (BlockException e) {
          // handle block logic
        } finally {
          // make sure that the exit() logic is called≠– 
          if (entry != null) {
            entry.exit();
          }
        }
        ```

3.  配置应用的启动参数，添加 -D 参数。将 APPName 替换为您的应用名称，将 xxx 替换为您的实际 License（通过控制台操作引导页面查看相应信息）。

    ```
    -Dproject.name=AppName -Dahas.license=xxx
    ```

    接入成功的应用将以卡片方式显示在流控降级页面。


## 后续操作 {#section_ckr_l2j_kgb .section}

接入了 AHAS 的流控降级服务后，您可以进一步了解：

-    [实时监控应用数据](..md) 
-    [流控规则](..md) 
-    [降级规则](..md) 
-    [系统规则](..md) 

## 常见问题 {#section_umj_m2j_kgb .section}

如果您已完成流控降级服务接入，流控降级页面仍查看不到您的应用，可参考常见问题：[流控降级页面找不到应用](..md)。

