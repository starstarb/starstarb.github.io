---
title: spring boot学习第二次
tags: spring
categories: spring
abbrlink: ca103a9f
date: 2019-07-27 16:41:09
---
Spring Boot系列
<!--more-->
### spring boot 集成 slf4j 和 logback
__slf4j和logback区别：__

概念：

    slf4j是java的一个日志门面，实现了日志框架一些通用的api;

    logback是具体的日志框架。它和log4j是同一个作者，他是为了解决log4j存在的问题而开发的新的日志框架。

    slf4j和logback可以简单的看作jdbc和其具体数据库的JDBC的jar包的关系。

推荐使用slf4j，而不是直接使用logback：

    slf4j的用法：

    log.info("Begin Start {}...{}", str1, str2);

    logback:


    log.info("Begin Start " + str1 +"..." + str2);

    如上，sl4j无论是写法和性能都比logback要强

spring boot 集成日志

POM.xml

    引入logback和slf4j依赖jar包

        <dependency>  
           <groupId>ch.qos.logback</groupId>  
           <artifactId>logback-classic</artifactId>  
        </dependency>
         <dependency>                                    
        	<groupId>org.slf4j</groupId>                
        	<artifactId>jcl-over-slf4j</artifactId>     
        </dependency>

logback-spring.xml
```
        <?xml version="1.0" encoding="UTF-8"?>
        <configuration>
        	
        	<!--定义日志文件的存储地址 勿在 LogBack的配置中使用相对路径 -->
        	<property name="LOG_HOME" value="/tmp/log" />
         
        	<!-- 控制台输出 -->
        	<appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        		<encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
        			<pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{30} - %msg%n</pattern>
        		</encoder>
        	</appender>
         
        	<!-- 按照每天生成日志文件 -->
        	<appender name="FILE"
        		class="ch.qos.logback.core.rolling.RollingFileAppender">
        		<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
        			<FileNamePattern>${LOG_HOME}/logs/smsismp.log.%d{yyyy-MM-dd}.log</FileNamePattern>
        			<!--日志文件保留天数 -->
        			<MaxHistory>30</MaxHistory>
        		</rollingPolicy>
        		<encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
        			<!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符 -->
        			<pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{30} - %msg%n</pattern>
        		</encoder>
        		<!--日志文件最大的大小 -->
        		<triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
        			<MaxFileSize>10MB</MaxFileSize>
        		</triggeringPolicy>
        	</appender>
        	 
        	<!-- 日志输出级别 -->
            <root level="INFO">
                <appender-ref ref="STDOUT" />
                <appender-ref ref="FILE" />
            </root>
         
        	<!-- 定义各个包的详细路径，继承root宝的值 -->
        	<logger name="com.hry.spring.log" level="INFO" />
        	<logger name="com.hry.spring" level="TRACE" />
         	
         	<!-- 此值由 application.properties的spring.profiles.active=dev指定-->
        	<springProfile name="dev">
        		<!--定义日志文件的存储地址 勿在 LogBack 的配置中使用相对路径 -->
        		<property name="LOG_HOME" value="/tmp/log" />
        		<logger name="org.springboot.sample" level="DEBUG" />
        	</springProfile>
         
        	<springProfile name="pro">
        		<!--定义日志文件的存储地址 勿在 LogBack 的配置中使用相对路径 -->
        		<property name="LOG_HOME" value="/home" />
        		<logger name="org.springboot.sample2" level="INFO" />
        	</springProfile>
        	
        </configuration>
```
    appender name="STDOUT": 日志打印到控制台

    appender name="FILE"： 日志按日打印到文件中，最多保留MaxHistory天，每个文件大水为MaxFileSize

    encoder：定义输出格式

        %d{HH:mm:ss.SSS}——日志输出时间
        %thread——输出日志的进程名字，这在Web应用以及异步任务处理中很有用
        %-5level——日志级别，并且使用5个字符靠左对齐
        %logger{36}——日志输出者的名字
        %msg——日志消息
        %n——平台的换行符

    <root level="INFO">： 定义根logger，通过appender-ref指定前方定义的appender

    <logger name="com.hry.spring.log" level="INFO" />：在继承root的logger上对com.hry.spring.log包日志作特殊处理

    <springProfile name="dev">： 定义profile的值，只有特定profile的情况下，此间定义的内容才启作用


application.properties
```
        server.port=8080
        spring.profiles.active=dev
```
    spring.profiles.active指定本次启动的active的值是什么。本次是dev，则logback-spring.xml里<springProfile name="dev">的内容启作用


用法：
```
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import org.springframework.boot.SpringApplication;
        import org.springframework.boot.autoconfigure.SpringBootApplication;
         
        @SpringBootApplication
        public class LogApplication {
        	private static final Logger log = LoggerFactory.getLogger(LogApplication.class);
        	public static void main(String[] args) {
        		String str1 = "string1";
        		String str2 = "string2";
        		log.info("Begin Start {}...{}", str1, str2);
        		SpringApplication.run(LogApplication.class, args);
        		log.info("Stop ...");
        	}
        }

```
启动服务：

    spring boot也可以通过启动命令启动指定的profile:

    java -jar log.jar --spring.profiles.active=dev

    如果不在启动命令中不指定active值，则使用application.properties里的值
