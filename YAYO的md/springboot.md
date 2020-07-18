# 一.springboot入门

## 1.springboot简介

> 简化spring应用开发的一个框架；
>
> 整个spring技术栈的一个大整合；
>
> J2EE开发的一站式解决方案；
>
> 

## 2.微服务

微服务：架构风格（服务微化）

一个应用应该是一组小型服务，可以通过HTTP的方式进行互通；

每一个功能单元最终都是一个可独立替换的功能单元；

大型分布式应用：springboot快速构建一个应用，springcloud连接各个应用，springcloudDataFlow进行分布式之间的数据处理；



## 3.环境设置

jdk1.8

maven3.x

idea

springboot 1.5.9.RELEASE



### maven设置

maven--setting.xml -- 使用了阿里源

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">

	<pluginGroups>
	
	</pluginGroups>

	<proxies>
	
	</proxies>

	<servers>
	</servers>
	<mirrors>
		<mirror>
             <id>alimaven</id>
              <name>aliyun maven</name>
              <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
               <mirrorOf>central</mirrorOf>        
      </mirror>
	</mirrors>
	<profiles>
    	<profile>
      		<id>jdk-1.8</id>
      		<activation>
			<activeByDefault>true</activeByDefault>
       		<jdk>1.8</jdk>
     		</activation>
     		<properties>
				<maven.compiler.source>1.8</maven.compiler.source>
				<maven.compiler.target>1.8</maven.compiler.target>
				<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
	 		</properties>
    	</profile>
	</profiles>
</settings>
```



### idea设置

idea -- 设置maven

settings - maven

other settings - settings for new project - maven



## 4.springboot -- HelloWorld

做一个功能：浏览器发生hello请求，服务器接收请求并且处理，响应Hello World字符串；

### 1.springboot initializr创建一个springboot工程，选择jar包，选择模块web - spring web，导入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!-- 父项目（springboot的版本仲裁中心） ， 
		父项目的依赖spring-boot-dependencies，包含了各种子模块和限定了版本号
        这样我们导入依赖默认是不需要输入版本号的
        但是没有在spring-boot-dependencies中管理的依赖是需要写版本号的
    -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.9.RELEASE</version>
        <relativePath/>
        <!-- lookup parent from repository -->
    </parent>
    <groupId>com.yanyue</groupId>
    <artifactId>springbootdemo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springbootdemo</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <!--spring-boot-starter：springboot场景启动器；
        spring-boot-starter-web 帮我们导入了web应用正常运行所依赖的模块
        -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <!-- 这个插件，可以将应用打包成一个可执行的jar包 -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```



### 2.编写主程序类XXXApplication.java，启动springboot应用

```java
package com.yanyue.springbootdemo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

//@SpringBootApplication 来标注一个主程序类，说明这是一个springboot应用
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        // spring应用启动起来
        SpringApplication.run(DemoApplication.class, args);
    }
}

```



### 3.编写相关的Controller和Service -- HelloController

```java
package com.yanyue.springbootdemo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

//@RestController
//相当于@Controller+@ResponseBody两个注解的结合，返回json数据不需要在方法前面加@ResponseBody注解了，
// 但使用@RestController这个注解，就不能返回jsp,html页面，视图解析器无法解析jsp,html页面

//@Controller 用于标记在一个类上，使用它标记的类就是一个SpringMVC Controller 对象。分发处理器将会扫描使用了该注解的类的方法，并检测该方法是否使用了@RequestMapping 注解
@Controller
public class HelloController {

    //@ResponseBody的作用其实是将java对象转为json格式的数据
    @ResponseBody
    //@RequestMapping 来映射请求，也就是通过它来指定控制器可以处理哪些URL请求
    //@GetMapping get请求
    //@PostMapping  post请求
    @RequestMapping("/hello")
    public String hello() {
        return "hello world";
    }
}

```

### 4.简化部署

```xml
<!-- 这个插件，可以将应用打包成一个可执行的jar包 -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

lifecycle - package 进行打包，在target下xxx.jar

java  -jar  xxx.jar  运行jar包



## 5.HelloWorld探究--starter场景启动器

### 1.pom文件

```xml
<!-- 父项目（springboot的版本仲裁中心） ，
        父项目的依赖spring-boot-dependencies，包含了各种子模块和限定了版本号
        这样我们导入依赖默认是不需要输入版本号的
        但是没有在spring-boot-dependencies中管理的依赖是需要写版本号的
-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.9.RELEASE</version>
    <relativePath/>
</parent>

它的父项目
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>1.5.9.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
它来管理spring boot项目应用中的所有依赖版本号

```

springboot的版本仲裁中心；

以后我们导入依赖默认是不需要写版本号的；（没有在dependencies里面管理的依赖自然需要声明版本号的）

### 2.启动器starter

```xml
<!--
	spring-boot-starter：springboot场景启动器；
	spring-boot-starter-web 帮我们导入了web应用正常运行所依赖的模块
-->
<dependency>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

spring-boot-starter：springboot场景启动器；spring-boot-starter-web 帮我们导入了web应用正常运行所依赖的模块；

springboot将所有的功能场景都抽取出来，做成了一个starters（启动器），只需要在项目中引入这些starter相关场景的所有依赖都会导入进来；要用什么功能就导入什么功能的启动器；



## 6.HelloWorld细节--自动配置

































































