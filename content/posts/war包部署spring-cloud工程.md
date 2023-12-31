---
title: war包部署spring cloud 工程
date: 2019-09-07
updated: 2019-09-07
toc: true
tags:
- spring boot
- war包部署

---

最近接到一个任务：将spring cloud架构的系统部署到国产中间件：apusic(金蝶)中去。

## 思路

由于之前完全没有接触过这个中间件，第一个想到的就是不用springboot 自带的tomcat，将服务器打成war包部署到外部tomcat中去。部署成功后再将他搬到apusic(当时想的是毕竟GC，操作应该大同小异)。

于是就开始了下面的操作:

> 项目是gradle构建

### 更改`build.gradle`将Springboot 服务打包成war

```gradle
apply plugin: 'java'
apply plugin: 'org.springframework.boot'
apply plugin: 'eclipse'
apply plugin: 'net.saliman.properties'
apply plugin: 'io.spring.dependency-management'
apply plugin: 'war' # 引入war插件

dependencies {
    ...
    # 由于用外部tomcat启动 去掉继承的tomcat
    providedRuntime 'org.springframework.boot:spring-boot-starter-tomcat'
    ...
}
```

### 新增一个启动配置类继承SpringBootServletInitializer 接口，重写configure方法

```java
/**
 * @Author: hesong
 * @Describe:
 */
public class MCStartApplication extends SpringBootServletInitializer {
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(ManagementCenterApplication.class);
    }
}
```
