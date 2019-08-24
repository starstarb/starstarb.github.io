---
title: Spring学习计划第一次
categories: Spring
tags: Spring
abbrlink: dd33a848
date: 2019-07-07 14:25:00
password: 753159
message: Welcome to my blog, enter password to read.  
abstract: Welcome to my blog, enter password to read.  
---
Spring学习
<!--more-->
#### Spring框架本身的四大原则：
  1. 使用POJO进行轻量级和最小侵入式开发。
  2. 通过依赖注入和基于接口编程实现松耦合
  3. 通过AOP和默认习惯进行声明式编程
  4. 使用AOP和模板（template）减少模板化代码。

  注：POJO（Plain Ordinary Java Object）简单的Java对象，实际就是普通JavaBeans，是为了避免和EJB混淆所创造的简称。
使用POJO名称是为了避免和EJB混淆起来, 而且简称比较直接. 其中有一些属性及其getter setter方法的类,没有业务逻辑，有时可以作为VO(value -object)或dto(Data Transform Object)来使用.当然,如果你有一个简单的运算属性也是可以的,但不允许有业务方法,也不能携带有connection之类的方法。

  注：企业级JavaBean（Enterprise JavaBean EJB）是一个用来构筑企业级应用的服务器端可被管理组件。EJB规范的目的在于为企业及应用开发人员实现后台业务提供一个标准方式，从而解决一些此前总是在作业过程中总是重复发生的问题。EJB最早于1997年由IBM提出，旋即被太阳微系统采用并形成标准（EJB 1.0 和EJB 1.1）。其特点包括网络服务支持和核心开发工具(SDK)。 在J2EE里，分别是会话Bean（Session Bean），实体Bean（Entity Bean）和消息驱动Bean（MessageDriven Bean）。EJB以一个标准方式自动处理了诸如数据持久化，事务集成，安全对策等不同应用的共有问题，使得软件开发人员可以专注于程序的特定需求而不再饱受那些非业务元素的困扰。

### 收集的Spring面试题
##### 什么是 Spring Framework？

1. Spring 是一个开源应用框架，旨在降低应用程序开发的复杂度。
2. 它是轻量级、松散耦合的。
3. 它具有分层体系结构，允许用户选择组件，同时还为 J2EE 应用程序开发提供了一个有凝聚力的框架。
4. 它可以集成其他框架，如 Structs、Hibernate、EJB 等，所以又称为框架的框架。
 
### 列举 Spring Framework 的优点。

1. 由于 Spring Frameworks 的分层架构，用户可以自由选择自己需要的组件。
2. Spring Framework 支持 POJO(Plain Old Java Object) 编程，从而具备持续集成和可测试性。
3. 由于依赖注入和控制反转，JDBC 得以简化。
4. 它是开源免费的。

###  Spring Framework 有哪些不同的功能？

1. 轻量级 - Spring 在代码量和透明度方面都很轻便。
2. IOC - 控制反转
3. AOP - 面向切面编程可以将应用业务逻辑和系统服务分离，以实现高内聚。
4. 容器 - Spring 负责创建和管理对象（Bean）的生命周期和配置。
5. MVC - 对 web 应用提供了高度可配置性，其他框架的集成也十分方便。
6. 事务管理 - 提供了用于事务管理的通用抽象层。Spring 的事务支持也可用于容器较少的环境。
7. JDBC 异常 - Spring 的 JDBC 抽象层提供了一个异常层次结构，简化了错误处理策略

### 什么是 Spring IOC 容器？

    Spring 框架的核心是 Spring 容器。容器创建对象，将它们装配在一起，配置它们并管理它们的完整生命周期。Spring 容器使用依赖注入来管理组成应用程序的组件。容器通过读取提供的配置元数据来接收对象进行实例化，配置和组装的指令。该元数据可以通过 XML，Java 注解或 Java 代码提供。

### 什么是依赖注入？

在依赖注入中，您不必创建对象，但必须描述如何创建它们。您不是直接在代码中将组件和服务连接在一起，而是描述配置文件中哪些组件需要哪些服务。由 IoC 容器将它们装配在一起。

### 可以通过多少种方式完成依赖注入？

通常，依赖注入可以通过三种方式完成，即：

1. 构造函数注入
2. setter 注入
3. 接口注入
在 Spring Framework 中，仅使用构造函数和 setter 注入。

### spring 中有多少种 IOC 容器？

BeanFactory - BeanFactory 就像一个包含 bean 集合的工厂类。它会在客户端要求时实例化 bean。
ApplicationContext - ApplicationContext 接口扩展了 BeanFactory 接口。它在 BeanFactory 基础上提供了一些额外的功能。

### 列举 IoC 的一些好处。

IoC 的一些好处是：

1. 它将最小化应用程序中的代码量。
2. 它将使您的应用程序易于测试，因为它不需要单元测试用例中的任何单例或 JNDI 查找机制。
3. 它以最小的影响和最少的侵入机制促进松耦合。
4. 它支持即时的实例化和延迟加载服务。

### Spring IoC 的实现机制。

Spring 中的 IoC 的实现原理就是工厂模式加反射机制。

示例：
```
<pre style="margin: 0px; padding: 0px; white-space: pre-wrap; overflow-wrap: break-word; color: rgb(0, 0, 0); font-size: 12px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: left; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-style: initial; text-decoration-color: initial;">
interface Fruit {
     public abstract void eat();
}
class Apple implements Fruit {
    public void eat(){
        System.out.println("Apple");
    }
}
class Orange implements Fruit {
    public void eat(){
        System.out.println("Orange");
    }
}
class Factory {
    public static Fruit getInstance(String ClassName) {
        Fruit f=null;
        try {
            f=(Fruit)Class.forName(ClassName).newInstance();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return f;
    }
}
class Client {
    public static void main(String[] a) {
        Fruit f=Factory.getInstance("io.github.dunwu.spring.Apple");
        if(f!=null){
            f.eat();
        }
    }
}</pre>

```


###  Spring Boot基础

Spring Boot核心功能：
1. 独立运行Spring项目  运行Spring Boot只需通过java -jar xx.jar运行即可
2. 内嵌Servlet容器 Spring Boot可选择内嵌Tomcat,Jetty ,Undertow等，无需以war包形式部署项目。
3. 提供starter 简化Maven配置，  Spring 提供了一系列的starter pom 来简化Maven的依赖加载，
   spring-boot-starter-web
4. 自动配置Spring Spring Boot 会根据在类路径中的jar包、类，为jar包里的类自动配置Bean，这样会极大的减少我们要使用的配置。（也可以自定义自动配置）
5. 准生产的应用监控  Spring Boot提供基于http、ssh、telnet对运行时的项目进行监控。
6. 无代码生成和xml配置
    
@SpringBootApplication是Spring Boot项目的核心注解，它是一个组合注解，主要目的是开启自动配置。
源码如下：
```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@Configuration //主要注解·
@EnableAutoConfiguration //
@ComponentScan //
public @interface SpringBootApplication{
  Class<?>[] exclude() default{};
  String [] excludeName() default{};
} 
```
其中，@EnableAutoConfiguration让Spring Boot根据类路径中的jar包依赖为当前项目进行自动配置。
例如：添加了spring-boot-starter-web依赖，会自动添加Tomcat和Spring MVC的依赖，那么Spring Boot会对Tomcat He Spring MVC 进行自动配置。

Spring Boot会自动扫描@SpringBootApplication所在类的同级包以及下级包里的Bean(若为JPA项目还可以扫描标注@Entity的实体类)。
建议入口类放置的位置在groupID+arctifactID组合的包名下。

修改banner :http://patorjk.com/software/taag


Spring Boot的全局配置文件application.properties或application.yml

Spring Boot不仅支持常规的properties配置文件，还支持yaml语言的配置文件。
* yaml是以数据为中心的语言，在配置数据的时候具有面向对象的特征。



修改默认端口号及默认访问路径 application.properties
server.port=9090
server.servlet.context-path = /helloboot


application.yml

server:
  port: 9090
  servlet:
    context-path: /helloboot

Spring Boot允许使用properties文件、yaml文件或者命令行参数作为外部配置。


* 常规属性配置，在Spring Boot 中，只需在application.properties定义属性，直接使用@Value注入即可。
```
book.author= javastar
book.name=Spring boot







@Value("${book.author}")
    private  String bookAuthor;
    @Value("${book.name}")
    private String bookName;
    @RequestMapping("/")
    String index()
    {
        return "book name is :" +bookName+"and book author is: "+bookAuthor;
    }
```


  Spring Boot为了解决@Value注入过多值的问题，提供了基于类型安全的配置方式，通过@ConfigurationProperties将
properties属性和一个Bean及其属性关联，从而实现类型安全的配置。

例子：
通过
 @Autowired
    private AuthorSettings authorSettings;
直接注入配置文件。
application.properties:
```
author.name=javastar
author.age=20
```
类型安全的Bean:
```
package club.javastar.xin.testspring.config;


import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties(prefix = "author")
public class AuthorSettings {
    private  String name;
    private  Long age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Long getAge() {
        return age;
    }

    public void setAge(Long age) {
        this.age = age;
    }
}


```
检验代码：
```
package club.javastar.xin.testspring;

import club.javastar.xin.testspring.config.AuthorSettings;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@SpringBootApplication
public class TestspringApplication {
    @Autowired
    private AuthorSettings authorSettings;
    @RequestMapping("/")
    public String index()
    {
        return "author name is "+authorSettings.getName()+"and author age is "+authorSettings.getAge();
    }
    public static void main(String[] args) {
        SpringApplication.run(TestspringApplication.class, args);

    }

}

```
### 日志配置
  Spring Boot 支持Java Util Logging、Log4J、Log4J2 和Logback作为日志框架。
  默认使用Logback作为日志框架：
 * 配置日志级别：
  logging.file=E:/mylog/log.log
 * 配置日志文件，格式为logging.level.包名=级别：
  logging.level.org.springframework.web=DEBUG

### Profile配置
    Profile是Spring用来针对不同的环境对不同的配置提供支持的，全局Profile配置使用application-{profile}.properties(如application-prod.properties)
    通过在application.properties中设置spring.profiles.active=prod来指定活动的Profile。
    
### Spring Boot运行原理

@SpringBootApplication注解的核心功能是由@EnableAutoConfiguration注解提供的
@EnableAutoConfiguration
```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@Import({EnableAutoConfigurationImportSelector.class,AutoConfigurationPackages.Registrar.class})
public @interface EnableAutoConfiguration
{
   Class<?> [] exclude() default{};
   String [] excludeName() default{};
}
```
关键功能：__@Import__
EnableAutoConfigurationImportSelector使用SpringFactoriesLoader.loadFactoryNames方法来扫描具有META-INF/spring.factories文件的jar包，而spring-boot-autoconfigure-x.x.jar里就有一个spring.factories文件，
此文件中声明了有哪些自动配置。

### 核心注解
    org.springframework.boot:spring-boot-autoconfig包里
     spring.factories中任意一个AutoConfiguration文件，一般都有如下的条件注解，
```
@ConditionalOnBean:当容器有指定的Bean的条件下
@ConditionalOnClass：当类路径下有指定的类的条件下
@ConditionalOnExpression：基于SpEL表达式作为判断条件
@ConditionalOnJava：基于JVM版本作为判断条件
@ConditionalOnJndi：在JNDI存在的条件下查找指定的位置
@ConditionalOnMissingBean：当容器里没有指定Bean的情况下
@ConditionalOnMissingClass：当类路径下没有指定的类的条件下
@ConditionalOnNotWebApplication：当前项目不是web项目的条件下
@ConditionalOnProperty：指定的属性是否有指定的值
@ConditionalOnResource：类路径是否有指定的值
@ConditionalOnSingleCandidate：当指定Bean在容器中只有一个，或者虽然有多个但是指定首选的Bean
@ConditionalOnWebApplication:当前项目时web项目的条件下
```
上述注解都是组合了@Conditional元注解，只是使用了不同的条件(Condition)


@ConditionalOnWebApplication注解
```
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package org.springframework.boot.autoconfigure.condition;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import org.springframework.context.annotation.Conditional;

@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Conditional({OnWebApplicationCondition.class})
public @interface ConditionalOnWebApplication {
    ConditionalOnWebApplication.Type type() default ConditionalOnWebApplication.Type.ANY;

    public static enum Type {
        ANY,
        SERVLET,
        REACTIVE;

        private Type() {
        }
    }
}



```
分析源码,此注解使用的是OnWebApplicationCondition,查找这个条件如何构造
```
//
// Source code recreated from a .class file by IntelliJ IDEA
// (powered by Fernflower decompiler)
//

package org.springframework.boot.autoconfigure.condition;

import java.util.Map;
import org.springframework.boot.autoconfigure.AutoConfigurationMetadata;
import org.springframework.boot.autoconfigure.condition.ConditionMessage.Builder;
import org.springframework.boot.autoconfigure.condition.ConditionalOnWebApplication.Type;
import org.springframework.boot.autoconfigure.condition.FilteringSpringBootCondition.ClassNameFilter;
import org.springframework.boot.web.reactive.context.ConfigurableReactiveWebEnvironment;
import org.springframework.boot.web.reactive.context.ReactiveWebApplicationContext;
import org.springframework.context.annotation.ConditionContext;
import org.springframework.core.annotation.Order;
import org.springframework.core.type.AnnotatedTypeMetadata;
import org.springframework.util.ClassUtils;
import org.springframework.util.ObjectUtils;
import org.springframework.web.context.ConfigurableWebEnvironment;
import org.springframework.web.context.WebApplicationContext;

@Order(-2147483628)
class OnWebApplicationCondition extends FilteringSpringBootCondition {
    private static final String SERVLET_WEB_APPLICATION_CLASS = "org.springframework.web.context.support.GenericWebApplicationContext";
    private static final String REACTIVE_WEB_APPLICATION_CLASS = "org.springframework.web.reactive.HandlerResult";

    OnWebApplicationCondition() {
    }

    protected ConditionOutcome[] getOutcomes(String[] autoConfigurationClasses, AutoConfigurationMetadata autoConfigurationMetadata) {
        ConditionOutcome[] outcomes = new ConditionOutcome[autoConfigurationClasses.length];

        for(int i = 0; i < outcomes.length; ++i) {
            String autoConfigurationClass = autoConfigurationClasses[i];
            if (autoConfigurationClass != null) {
                outcomes[i] = this.getOutcome(autoConfigurationMetadata.get(autoConfigurationClass, "ConditionalOnWebApplication"));
            }
        }

        return outcomes;
    }

    private ConditionOutcome getOutcome(String type) {
        if (type == null) {
            return null;
        } else {
            Builder message = ConditionMessage.forCondition(ConditionalOnWebApplication.class, new Object[0]);
            if (Type.SERVLET.name().equals(type) && !ClassNameFilter.isPresent("org.springframework.web.context.support.GenericWebApplicationContext", this.getBeanClassLoader())) {
                return ConditionOutcome.noMatch(message.didNotFind("servlet web application classes").atAll());
            } else if (Type.REACTIVE.name().equals(type) && !ClassNameFilter.isPresent("org.springframework.web.reactive.HandlerResult", this.getBeanClassLoader())) {
                return ConditionOutcome.noMatch(message.didNotFind("reactive web application classes").atAll());
            } else {
                return !ClassNameFilter.isPresent("org.springframework.web.context.support.GenericWebApplicationContext", this.getBeanClassLoader()) && !ClassUtils.isPresent("org.springframework.web.reactive.HandlerResult", this.getBeanClassLoader()) ? ConditionOutcome.noMatch(message.didNotFind("reactive or servlet web application classes").atAll()) : null;
            }
        }
    }

    public ConditionOutcome getMatchOutcome(ConditionContext context, AnnotatedTypeMetadata metadata) {
        boolean required = metadata.isAnnotated(ConditionalOnWebApplication.class.getName());
        ConditionOutcome outcome = this.isWebApplication(context, metadata, required);
        if (required && !outcome.isMatch()) {
            return ConditionOutcome.noMatch(outcome.getConditionMessage());
        } else {
            return !required && outcome.isMatch() ? ConditionOutcome.noMatch(outcome.getConditionMessage()) : ConditionOutcome.match(outcome.getConditionMessage());
        }
    }

    private ConditionOutcome isWebApplication(ConditionContext context, AnnotatedTypeMetadata metadata, boolean required) {
        switch(this.deduceType(metadata)) {
        case SERVLET:
            return this.isServletWebApplication(context);
        case REACTIVE:
            return this.isReactiveWebApplication(context);
        default:
            return this.isAnyWebApplication(context, required);
        }
    }

    private ConditionOutcome isAnyWebApplication(ConditionContext context, boolean required) {
        Builder message = ConditionMessage.forCondition(ConditionalOnWebApplication.class, new Object[]{required ? "(required)" : ""});
        ConditionOutcome servletOutcome = this.isServletWebApplication(context);
        if (servletOutcome.isMatch() && required) {
            return new ConditionOutcome(servletOutcome.isMatch(), message.because(servletOutcome.getMessage()));
        } else {
            ConditionOutcome reactiveOutcome = this.isReactiveWebApplication(context);
            return reactiveOutcome.isMatch() && required ? new ConditionOutcome(reactiveOutcome.isMatch(), message.because(reactiveOutcome.getMessage())) : new ConditionOutcome(servletOutcome.isMatch() || reactiveOutcome.isMatch(), message.because(servletOutcome.getMessage()).append("and").append(reactiveOutcome.getMessage()));
        }
    }

    private ConditionOutcome isServletWebApplication(ConditionContext context) {
        Builder message = ConditionMessage.forCondition("", new Object[0]);
        if (!ClassNameFilter.isPresent("org.springframework.web.context.support.GenericWebApplicationContext", context.getClassLoader())) {
            return ConditionOutcome.noMatch(message.didNotFind("servlet web application classes").atAll());
        } else {
            if (context.getBeanFactory() != null) {
                String[] scopes = context.getBeanFactory().getRegisteredScopeNames();
                if (ObjectUtils.containsElement(scopes, "session")) {
                    return ConditionOutcome.match(message.foundExactly("'session' scope"));
                }
            }

            if (context.getEnvironment() instanceof ConfigurableWebEnvironment) {
                return ConditionOutcome.match(message.foundExactly("ConfigurableWebEnvironment"));
            } else {
                return context.getResourceLoader() instanceof WebApplicationContext ? ConditionOutcome.match(message.foundExactly("WebApplicationContext")) : ConditionOutcome.noMatch(message.because("not a servlet web application"));
            }
        }
    }

    private ConditionOutcome isReactiveWebApplication(ConditionContext context) {
        Builder message = ConditionMessage.forCondition("", new Object[0]);
        if (!ClassNameFilter.isPresent("org.springframework.web.reactive.HandlerResult", context.getClassLoader())) {
            return ConditionOutcome.noMatch(message.didNotFind("reactive web application classes").atAll());
        } else if (context.getEnvironment() instanceof ConfigurableReactiveWebEnvironment) {
            return ConditionOutcome.match(message.foundExactly("ConfigurableReactiveWebEnvironment"));
        } else {
            return context.getResourceLoader() instanceof ReactiveWebApplicationContext ? ConditionOutcome.match(message.foundExactly("ReactiveWebApplicationContext")) : ConditionOutcome.noMatch(message.because("not a reactive web application"));
        }
    }

    private Type deduceType(AnnotatedTypeMetadata metadata) {
        Map<String, Object> attributes = metadata.getAnnotationAttributes(ConditionalOnWebApplication.class.getName());
        return attributes != null ? (Type)attributes.get("type") : Type.ANY;
    }
}

```
从Spring5.x看出，相对于书中Spring4.x源代码进行了改进，增加了 
isWebApplication 通过switch case语句分析转到下列函数分析，优化了程序
isAnyWebApplication
isServletWebApplication
isReactiveWebApplication等。

选择isServletWebApplication分析，
1. 先判断servlet web application classes是否在类路径中
2. 容器里是否有名为session的scope
3. 当前容器的Environment是否为ConfigurableWebEnvironment
4. 当前的ResourceLoader是否为WebApplicationContext(ResourceLoader是ApplicationContext的顶级接口之一)
5. 构造ConditionOutcome类的对象，通过ConditionOutcome.isMatch方法返回布尔值来确定条件。



