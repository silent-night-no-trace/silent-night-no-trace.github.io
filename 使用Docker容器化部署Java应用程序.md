## 引言
随着微服务架构的普及和云计算的快速发展，Docker作为一种轻量级的容器化技术，越来越受到开发者的青睐。Docker可以帮助我们将应用程序及其所有依赖打包到一个标准化的单元中，从而实现跨环境的一致性。在这篇博客中，我们将探讨如何使用Docker容器化Java应用程序，并提供一个简单的示例。

## 为什么选择Docker？
环境一致性：Docker可以确保在开发、测试和生产环境中运行相同的应用程序，消除“在我的机器上可以运行”的问题。
资源隔离：Docker容器提供了进程隔离，使得不同应用程序可以在同一台机器上安全地运行，而不会相互干扰。
快速部署：Docker镜像可以快速构建和部署，极大地提高了开发和运维的效率。
易于扩展：Docker容器可以轻松地进行水平扩展，满足高并发的需求。

## Docker 在 Java 生态中的作用
持续集成和持续部署（CI/CD）：
Docker 容器使得在 CI/CD 流程中快速、一致地构建和部署应用程序成为可能。开发者可以确保应用程序在不同环境间无差异地运行。

微服务架构：
Docker 与微服务架构天然契合。每个微服务都可以打包在自己的容器中，独立部署、扩展和管理。

环境隔离：
开发者可以使用 Docker 来隔离开发环境、测试环境和生产环境，确保环境一致性，避免“在我机器上可以运行”的问题。

资源利用：
Docker 容器共享主机操作系统内核，启动速度快，资源利用率高，相比传统虚拟机，更加轻量级。

可移植性：
Docker 容器可以在任何支持 Docker 的环境中运行，无论是本地机器、私有云、公有云还是混合云。
## 准备工作
在开始之前，请确保你已经安装了以下工具：

Java JDK：确保你已经安装了Java开发工具包（JDK）。
Maven：用于构建Java项目的工具。
Docker：确保Docker已安装并正在运行。
## 创建一个简单的Java应用程序
我们将创建一个简单的Spring Boot应用程序作为示例。
通过idea 可以快速使用springboot脚手架 快速创建springboot应用
### 新建项目
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/bd971b25c7474b73a21ce861529108d1.png)
创建一个maven项目 选择 jdk版本

### 选择需要添加Spring Boot依赖
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ca7ec87dc533418287a2eec7afc68666.png)
这里我们只选择一个 Spring Web组件 需要注意的是 spring3.x最低jdk版本要求为17



### 创建简单的REST控制器：
在src/main/java/com/example/demo目录下创建一个HelloController.java文件：

```java
package com.example.demo;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
    
    @GetMapping("/hello")
    public String hello() {
        return "Hello, Docker!";
    }
}
```

### 构建项目
在项目根目录下运行以下命令以构建Maven项目：

```bash
mvn clean package -DskipTests=true
```
项目构建将会生成一个可执行的jar文件

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/e5516bf605ba47e9a2d18e23723ccea5.png)


### 创建Dockerfile
在项目根目录下创建一个名为Dockerfile的文件，内容如下：

```Dockerfile
# 使用官方的Java镜像作为基础镜像
FROM openjdk:11-jre-slim

### 设置工作目录
WORKDIR /app

# 将项目的jar包复制到容器中
COPY target/demo-0.0.1-SNAPSHOT.jar app.jar

# 运行应用程序
ENTRYPOINT ["java", "-jar", "app.jar"]

```



### 构建和运行Docker镜像

#### 构建Docker镜像：
使用以下命令构建Docker镜像：

```bash
docker build -t demo-docker-example .
```

可以观察控制台输出 查看构建步骤以及日志
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/073edad9e5aa45c8a3cb91a4ff0d2ff5.png)

#### 运行Docker容器：
使用以下命令运行Docker容器：

```bash
docker run -p 8080:8080 demo-docker-example
```

可以观察控制台的启动日志
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/907e398f03804b6ea350272a0c80c6ee.png)
### 测试应用程序
在浏览器中访问 http://localhost:8080/hello，你应该能看到输出：

Hello, Docker!

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/22b511230e694bf0859fbf1d9eb42127.png)

## 总结
通过本篇博客，我们学习了如何使用Docker容器化一个简单的Java应用程序。Docker不仅简化了应用的部署过程，还提高了开发和运维的效率。希望这篇博客能帮助你更好地理解Docker在Java开发中的应用。

---
good day！！！
