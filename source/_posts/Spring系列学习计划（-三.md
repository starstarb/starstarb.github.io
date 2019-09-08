---
title: Spring系列学习计划(三)
tags: Spring
categories: Spring
abbrlink: 388be574
date: 2019-07-28 10:22:27
password: 753159
message: Welcome to my blog, enter password to read.  
abstract: Welcome to my blog, enter password to read.  
---
Spring系列博客学习：参考自江南一点雨松哥的博客,仅用作自我学习,感谢松哥
<!--more-->
## Spring Boot配置application.properties

在 Spring Boot 中，配置文件有两种不同的格式，一个是 properties ，另一个是 yaml 。
虽然 properties 文件比较常见，但是相对于 properties 而言，yaml 更加简洁明了，而且使用的场景也更多，很多开源项目都是使用 yaml 进行配置（例如 Hexo）。除了简洁，yaml 还有另外一个特点，就是 yaml 中的数据是有序的，properties 中的数据是无序的，在一些需要路径匹配的配置中，顺序就显得尤为重要（例如我们在 Spring Cloud Zuul 中的配置），此时我们一般采用 yaml。

首先，当我们创建一个 Spring Boot 工程时，默认 resources 目录下就有一个 application.properties 文件，可以在 application.properties 文件中进行项目配置，但是这个文件并非唯一的配置文件，在 Spring Boot 中，一共有 4 个地方可以存放 application.properties 文件。

* 当前项目根目录下的 config 目录下
* 当前项目的根目录下
* resources 目录下的 config 目录下
* resources 目录下

按如上顺序，四个配置文件的优先级依次降低。如下：
![](https://github.com/starstarb/clouding/raw/master/js/640.png)

这四个位置是默认位置，即 Spring Boot 启动，默认会从这四个位置按顺序去查找相关属性并加载。但是，这也不是绝对的，我们也可以在项目启动时自定义配置文件位置。

#### 普通的属性注入
由于 Spring Boot 源自 Spring ，所以 Spring 中存在的属性注入，在 Spring Boot 中一样也存在。由于 Spring Boot 中，默认会自动加载 application.properties 文件，所以简单的属性注入可以直接在这个配置文件中写。

例如，现在定义一个 Book 类：
```
public class Book {
    private Long id;
    private String name;
    private String author;
    //省略 getter/setter
}
```
然后，在 application.properties 文件中定义属性：
```
book.name=三国演义
book.author=罗贯中
book.id=1
```
按照传统的方式（Spring中的方式），可以直接通过 @Value 注解将这些属性注入到 Book 对象中：
```
@Component
public class Book {
    @Value("${book.id}")
    private Long id;
    @Value("${book.name}")
    private String name;
    @Value("${book.author}")
    private String author;
    //省略getter/setter
}
```
##### 注意

Book 对象本身也要交给 Spring 容器去管理，如果 Book 没有交给 Spring 容器，那么 Book 中的属性也无法从 Spring 容器中获取到值。

配置完成后，在 Controller 或者单元测试中注入 Book 对象，启动项目，就可以看到属性已经注入到对象中了。
一般来说，我们在 application.properties 文件中主要存放系统配置，这种自定义配置不建议放在该文件中，可以自定义 properties 文件来存在自定义配置。

例如在 resources 目录下，自定义 book.properties 文件，内容如下：
```
book.name=三国演义
book.author=罗贯中
book.id=1
```
此时，项目启动并不会自动的加载该配置文件，如果是在 XML 配置中，可以通过如下方式引用该 properties 文件：
```
<context:property-placeholder location="classpath:book.properties"/>
```
如果是在 Java 配置中，可以通过 @PropertySource 来引入配置：
```
@Component
@PropertySource("classpath:book.properties")
public class Book {
    @Value("${book.id}")
    private Long id;
    @Value("${book.name}")
    private String name;
    @Value("${book.author}")
    private String author;
    //getter/setter
}
```
这样，当项目启动时，就会自动加载 book.properties 文件。

这只是 Spring 中属性注入的一个简单用法，和 Spring Boot 没有任何关系。

#### 类型安全的属性注入

Spring Boot 引入了类型安全的属性注入，如果采用 Spring 中的配置方式，当配置的属性非常多的时候，工作量就很大了，而且容易出错。

使用类型安全的属性注入，可以有效的解决这个问题。
```
@Component
@PropertySource("classpath:book.properties")
@ConfigurationProperties(prefix = "book")
public class Book {
    private Long id;
    private String name;
    private String author;
    //省略getter/setter
}
```
这里，主要是引入 @ConfigurationProperties(prefix = "book") 注解，并且配置了属性的前缀，此时会自动将 Spring 容器中对应的数据注入到对象对应的属性中，就不用通过 @Value 注解挨个注入了，减少工作量并且避免出错。

## Spring Boot中的yaml配置简介 
首先application.yaml在Spring Boot中可以写在四个不同的位置，分别是如下位置：

    项目根目录下的config目录中

    项目根目录下

    classpath下的config目录中

    classpath目录下

四个位置中的application.yaml文件的优先级按照上面列出的顺序依次降低。即如果有同一个属性在四个文件中都出现了，以优先级高的为准。

那么application.yaml是不是必须叫application.yaml这个名字呢？当然不是必须的。开发者可以自己定义yaml名字，自己定义的话，需要在项目启动时指定配置文件的名字，像下面这样：

![](https://github.com/starstarb/clouding/raw/master/js/yml.png)
当然这是在IntelliJ IDEA中直接配置的，如果项目已经打成jar包了，则在项目启动时加入如下参数：

    java -jar myproject.jar --spring.config.name=app

这样配置之后，在项目启动时，就会按照上面所说的四个位置按顺序去查找一个名为app.yaml的文件。当然这四个位置也不是一成不变的，也可以自己定义，有两种方式，一个是使用 spring.config.location属性，另一个则是使用 spring.config.additional-location这个属性，在第一个属性中，表示自己重新定义配置文件的位置，项目启动时就按照定义的位置去查找配置文件，这种定义方式会覆盖掉默认的四个位置，也可以使用第二种方式，第二种方式则表示在四个位置的基础上，再添加几个位置，新添加的位置的优先级大于原本的位置。

配置方式如下：
![](https://github.com/starstarb/clouding/raw/master/js/yml1.png)
这里要注意，配置文件位置时，值一定要以/结尾。
数组注入

yaml也支持数组注入，例如

    my:

      servers:

        - dev.example.com

        - another.example.com

这段数据可以绑定到一个带Bean的数组中：

    @ConfigurationProperties(prefix="my")

    @Component

    public class Config {


        private List<String> servers = new ArrayList<String>();


        public List<String> getServers() {

            return this.servers;

        }

    }

项目启动后，配置中的数组会自动存储到servers集合中。当然，yaml不仅可以存储这种简单数据，也可以在集合中存储对象。例如下面这种：

    redis:

      redisConfigs:

        - host: 192.168.66.128

          port: 6379

        - host: 192.168.66.129

          port: 6380

这个可以被注入到如下类中：

    @Component

    @ConfigurationProperties(prefix = "redis")

    public class RedisCluster {

        private List<SingleRedisConfig> redisConfigs;

        //省略getter/setter

    }

__优缺点__

不同于properties文件的无序，yaml配置是有序的，这一点在有些配置中是非常有用的，例如在Spring Cloud Zuul的配置中，当我们配置代理规则时，顺序就显得尤为重要了。当然yaml配置也不是万能的，例如，yaml配置目前不支持@PropertySource注解。

## Spring Boot + Vue前后端分离开发，前端网络请求封装与配置

前端网络访问，主流方案就是 Ajax，Vue 也不例外，在 Vue2.0 之前，网络访问较多的采用 vue-resources，Vue2.0 之后，官方不再建议使用 vue-resources ，这个项目本身也停止维护，目前建议使用的方案是 axios。今天松哥就带大家来看看 axios 的使用。
#### axios 引入

axios 使用步骤很简单，首先在前端项目中，引入 axios：

	npm install axios -S

装好之后，按理说可以直接使用了，但是，一般在生产环境中，我们都需要对网络请求进行封装。

因为网络请求可能会出错，这些错误有的是代码错误导致的，也有的是业务错误，不管是哪一种错误，都需要开发者去处理，而我们不可能在每一次发送请求时都去枚举各种错误情况。

因此我们需要对前端请求进行封装，封装完成后，将前端错误统一处理，这样，开发者只需要在每一次发送请求的地方处理请求成功的情况即可。
#### 请求封装

在 axios 中，我们可以使用 axios 自带的拦截器来实现对错误的统一处理。

在 axios 中，有请求拦截器，也有响应拦截器。

请求拦截器中可以统一添加公共的请求参数，例如单点登录中前端统一添加 token 参数。

响应拦截器则可以实现对错误的统一处理。

另外一个需要注意的地方则是错误的展示需要使用一种通用的方式，而不可以和页面绑定（例如，登录失败，在用户名/密码输入框后面展示错误信息，不支持这种错误显示方式），这里推荐使用 ElementUI 中的 Massage 来展示错误信息，这是一个页面无关的组件。

封装后的 axios 如下：
```
import axios from 'axios'
import {Message} from 'element-ui'
axios.interceptors.request.use(config => {
  return config;
}, err => {
  Message.error({message: '请求超时!'});
})
axios.interceptors.response.use(data => {
  if (data.status && data.status == 200 && data.data.status == 500) {
    Message.error({message: data.data.msg});
    return;
  }
  if (data.data.msg) {
    Message.success({message: data.data.msg});
  }
  return data.data;
}, err => {
  if (err.response.status == 504 || err.response.status == 404) {
    Message.error({message: '服务器被吃了⊙﹏⊙∥'});
  } else if (err.response.status == 403) {
    Message.error({message: '权限不足,请联系管理员!'});
  } else if (err.response.status == 401) {
    Message.error({message: err.response.data.msg});
  } else {
    if (err.response.data.msg) {
      Message.error({message: err.response.data.msg});
    }else{
      Message.error({message: '未知错误!'});
    }
  }
})
```
代码解释：

* 首先导入 axios 和 Massage 组件
* 接下来定义一个请求拦截器
* 最后定义一个响应拦截器，这个拦截器有两个参数，第一个参数 data 表示服务端处理成功的响应，第二个 err 表示服务端处理失败的响应。对照着 jQuery 中的 Ajax ，第一个相当于 success 回调，第二个相当于 error 回调。
* 响应的 data 表示服务端返回的数据，数据格式是 {data:{status:200,msg"",obj:{}},status:200} 其中，data 中的对象就是服务端返回的具体的 JSON ，外面的 status 表示 HTTP 响应码，里边的 status 是自定义的 RespBean 中返回的数据
* 首先判断 HTTP 响应码为 200 ，并且服务端返回的 status 为 500 ，表示业务逻辑错误，此时直接通过 Message 将错误信息展示出来，然后 return 即可。
* 如果服务端返回的字段中包含 msg ，则将 msg 显示出来，这个 msg 一般是成功的提示。
* 最后返回 data.data ，即将服务端返回的数据 return ，这个数据最终会来到请求调用的地方。
* 当 HTTP 响应码大于等于 400 时，进入 err 中。

### 方法封装

请求封装完成后，还需要对方法进行封装，方便调用：
```
let base = '';
export const postRequest = (url, params) => {
  return axios({
    method: 'post',
    url: `${base}${url}`,
    data: params,
    headers: {
      'Content-Type': 'application/json'
    }
  });
}
export const putRequest = (url, params) => {
  return axios({
    method: 'put',
    url: `${base}${url}`,
    data: params,
    headers: {
      'Content-Type': 'application/json'
    }
  });
}
export const deleteRequest = (url) => {
  return axios({
    method: 'delete',
    url: `${base}${url}`
  });
}
export const getRequest = (url) => {
  return axios({
    method: 'get',
    url: `${base}${url}`
  });
}
```
由于在前后端分离项目中，大多数情况下，后端接口都采用 RESTful 风格来设计，所以前端主要封装 GET\POST\PUT\DELETE 方法，然后所有的请求参数都是用 JSON。

这里一开始定义了一个 base 变量，这是请求的前缀，方便后期维护（如果需要统一修改请求前缀）

#### 制作 Vue 插件

封装好的方法已经可以直接使用了，但是比较麻烦，每次使用时，都需要在相关的 vue 文件中引入方法，像下面这样：

import {postRequest} from "../utils/api";

但是这种操作方式太麻烦，所以我们可以考虑将方法进一步封装成 Vue 的插件，这样在每一个 vue 文件中，不需要引入方法就能够直接调用方法了。

参考 Vue 官方文档 https://cn.vuejs.org/v2/guide/plugins.html
如下：
![](https://github.com/starstarb/clouding/raw/master/js/vuec.png)

官方给出了 5 种插件制作方式，我们这里采用第 4 种方案。 具体操作就是在 main.js 中引入所有的封装好的方法，然后挂载到 Vue.prototype 上即可，如下：

import {postRequest} from "./utils/api";
import {putRequest} from "./utils/api";
import {deleteRequest} from "./utils/api";
import {getRequest} from "./utils/api";
Vue.prototype.getRequest = getRequest;
Vue.prototype.deleteRequest = deleteRequest;
Vue.prototype.putRequest = putRequest;
Vue.prototype.postRequest = postRequest;

封装完成后，以后在 vue 文件中，直接通过 this 就可以获取到网络请求方法的引用了，如下：

this.postRequest("/doLogin", this.user).then(msg=>{
  if (msg) {
    //登录成功，页面跳转
  }
})

注意 ，then 中的 msg 就是响应拦截器中返回的 msg ，这个 msg 如果没有值，表示请求失败（失败已经在拦截器中进行处理了），如果有值，表示请求成功！
配置请求转发

在前后端分离中，前端和后端在不同的端口或者地址上运行，如果前端直接向后端发送请求，这个请求是跨域的。

但是在项目部署时，前端打包编译后拷贝到 Java 项目中，和 Java 项目一起运行，此时不存在跨域问题。

所以这里我们的解决思路不是解决跨域问题，而是通过配置 NodeJS 的请求转发，来实现网络请求顺利发送。

请求转发在 vue 项目的 config/index.js 文件中配置：
![](https://github.com/starstarb/clouding/raw/master/js/vue1.png)

添加了请求转发配置之后，一定要重启前端项目才会生效。

此时启动前端项目，就可以顺利发送网络请求了。


## Spring Boot的静态资源要放到哪里
SSM 中的配置

要讲 Spring Boot 中的问题，我们得先回到 SSM 环境搭建中，一般来说，我们可以通过 `<mvc:resources/>0`节点来配置不拦截静态资源，如下：

    <mvc:resources mapping="/js/**" location="/js/"/>
    <mvc:resources mapping="/css/**" location="/css/"/>
    <mvc:resources mapping="/html/**" location="/html/"/>

由于这是一种Ant风格的路径匹配符， /** 表示可以匹配任意层级的路径，因此上面的代码也可以像下面这样简写：

    <mvc:resources mapping="/**" location="/"/>

这种配置是在 XML 中的配置，大家知道，SpringMVC 的配置除了在XML中配置，也可以在 Java 代码中配置，如果在Java代码中配置的话，我们只需要自定义一个类，继承自WebMvcConfigurationSupport即可：

    @Configuration

    @ComponentScan(basePackages = "org.sang.javassm")

    public class SpringMVCConfig extends WebMvcConfigurationSupport {

        @Override

        protected void addResourceHandlers(ResourceHandlerRegistry registry) {

            registry.addResourceHandler("/**").addResourceLocations("/");

        }

    }

重写 WebMvcConfigurationSupport 类中的addResourceHandlers方法，在该方法中配置静态资源位置即可，这里的含义和上面 xml 配置的含义一致，因此无需多说。
这是我们传统的解决方案，在Spring Boot 中，其实配置方式和这个一脉相承，只是有一些自动化的配置了。
Spring Boot 中的配置

在 Spring Boot 中，如果我们是从 https://start.spring.io 这个网站上创建的项目，或者使用 IntelliJ IDEA 中的 Spring Boot 初始化工具创建的项目，默认都会存在 resources/static 目录，很多小伙伴也知道静态资源只要放到这个目录下，就可以直接访问，除了这里还有没有其他可以放静态资源的位置呢？为什么放在这里就能直接访问了呢？这就是本文要讨论的问题了。
整体规划

首先，在 Spring Boot 中，默认情况下，一共有5个位置可以放静态资源，五个路径分别是如下5个：

    classpath:/META-INF/resources/

    classpath:/resources/

    classpath:/static/

    classpath:/public/

    /

前四个目录好理解，分别对应了resources目录下不同的目录，第5个 / 是啥意思呢？我们知道，在 Spring Boot 项目中，默认是没有 webapp 这个目录的，当然我们也可以自己添加（例如在需要使用JSP的时候），这里第5个 / 其实就是表示 webapp 目录中的静态资源也不被拦截。如果同一个文件分别出现在五个目录下，那么优先级也是按照上面列出的顺序。

不过，虽然有5个存储目录，除了第5个用的比较少之外，其他四个，系统默认创建了 classpath:/static/， 正常情况下，我们只需要将我们的静态资源放到这个目录下即可，也不需要额外去创建其他静态资源目录，例如我在 classpath:/static/ 目录下放了一张名为1.png 的图片，那么我的访问路径是：

    http://localhost:8080/1.png  

这里大家注意，请求地址中并不需要 static，如果加上了static反而多此一举会报404错误。很多人会觉得奇怪，为什么不需要添加 static呢？资源明明放在 static 目录下。其实这个效果很好实现，例如在SSM配置中，我们的静态资源拦截配置如果是下面这样：

    <mvc:resources mapping="/**" location="/static/"/>

如果我们是这样配置的话，请求地址如果是 http://localhost:8080/1.png 实际上系统会去 /static/1.png 目录下查找相关的文件。

#### 自定义配置

当然，这个是系统默认配置，如果我们并不想将资源放在系统默认的这五个位置上，也可以自定义静态资源位置和映射，自定义的方式也有两种，可以通过 application.properties 来定义，也可以在 Java 代码中来定义，下面分别来看。
application.properties

在配置文件中定义的方式比较简单，如下：

    spring.resources.static-locations=classpath:/

    spring.mvc.static-path-pattern=/**

第一行配置表示定义资源位置，第二行配置表示定义请求 URL 规则。以上文的配置为例，如果我们这样定义了，表示可以将静态资源放在 resources目录下的任意地方，我们访问的时候当然也需要写完整的路径，例如在resources/static目录下有一张名为1.png 的图片，那么访问路径就是 http://localhost:8080/static/1.png ,注意此时的static不能省略。

**Java 代码定义**

当然，在Spring Boot中我们也可以通过 Java代码来自定义，方式和 Java 配置的 SSM 比较类似，如下：

    @Configuration

    public class WebMVCConfig implements WebMvcConfigurer {

        @Override

        public void addResourceHandlers(ResourceHandlerRegistry registry) {

            registry.addResourceHandler("/**").addResourceLocations("classpath:/aaa/");

        }

    }

## SpringMVC 中 @ControllerAdvice 注解的三种使用场景！
@ControllerAdvice ，很多初学者可能都没有听说过这个注解，实际上，这是一个非常有用的注解，顾名思义，这是一个增强的 Controller。使用这个 Controller ，可以实现三个方面的功能：

**全局异常处理**

**全局数据绑定**

**全局数据预处理**s

灵活使用这三个功能，可以帮助我们简化很多工作，需要注意的是，这是 SpringMVC 提供的功能，在 Spring Boot 中可以直接使用，下面分别来看。
#### 全局异常处理

使用 @ControllerAdvice 实现全局异常处理，只需要定义类，添加该注解即可定义方式如下：

    @ControllerAdvice

    public class MyGlobalExceptionHandler {

        @ExceptionHandler(Exception.class)

        public ModelAndView customException(Exception e) {

            ModelAndView mv = new ModelAndView();

            mv.addObject("message", e.getMessage());

            mv.setViewName("myerror");

            return mv;

        }

    }

在该类中，可以定义多个方法，不同的方法处理不同的异常，例如专门处理空指针的方法、专门处理数组越界的方法...，也可以直接向上面代码一样，在一个方法中处理所有的异常信息。

@ExceptionHandler 注解用来指明异常的处理类型，即如果这里指定为 NullpointerException，则数组越界异常就不会进到这个方法中来。
#### 全局数据绑定

全局数据绑定功能可以用来做一些初始化的数据操作，我们可以将一些公共的数据定义在添加了 @ControllerAdvice 注解的类中，这样，在每一个 Controller 的接口中，就都能够访问导致这些数据。

使用步骤，首先定义全局数据，如下：

    @ControllerAdvice

    public class MyGlobalExceptionHandler {

        @ModelAttribute(name = "md")

        public Map<String,Object> mydata() {

            HashMap<String, Object> map = new HashMap<>();

            map.put("age", 99);

            map.put("gender", "男");

            return map;

        }

    }

使用 @ModelAttribute 注解标记该方法的返回数据是一个全局数据，默认情况下，这个全局数据的 key 就是返回的变量名，value 就是方法返回值，当然开发者可以通过 @ModelAttribute 注解的 name 属性去重新指定 key。

定义完成后，在任何一个Controller 的接口中，都可以获取到这里定义的数据：

    @RestController

    public class HelloController {

        @GetMapping("/hello")

        public String hello(Model model) {

            Map<String, Object> map = model.asMap();

            System.out.println(map);

            int i = 1 / 0;

            return "hello controller advice";

        }

    }

#### 全局数据预处理

考虑我有两个实体类，Book 和 Author，分别定义如下：

    public class Book {

        private String name;

        private Long price;

        //getter/setter

    }

    public class Author {

        private String name;

        private Integer age;

        //getter/setter

    }

此时，如果我定义一个数据添加接口，如下：

    @PostMapping("/book")

    public void addBook(Book book, Author author) {

        System.out.println(book);

        System.out.println(author);

    }

这个时候，添加操作就会有问题，因为两个实体类都有一个 name 属性，从前端传递时 ，无法区分。此时，通过 @ControllerAdvice 的全局数据预处理可以解决这个问题

**解决步骤如下:**

1. 给接口中的变量取别名

    @PostMapping("/book")

    public void addBook(@ModelAttribute("b") Book book, @ModelAttribute("a") Author author) {

        System.out.println(book);

        System.out.println(author);

    }

2. 进行请求数据预处理
在 @ControllerAdvice 标记的类中添加如下代码:

    @InitBinder("b")

    public void b(WebDataBinder binder) {

        binder.setFieldDefaultPrefix("b.");

    }

    @InitBinder("a")

    public void a(WebDataBinder binder) {

        binder.setFieldDefaultPrefix("a.");

    }

@InitBinder("b") 注解表示该方法用来处理和Book和相关的参数,在方法中,给参数添加一个 b 前缀,即请求参数要有b前缀.

3. 发送请求

请求发送时,通过给不同对象的参数添加不同的前缀,可以实现参数的区分.

## Spring Boot通过CORS解决跨域问题

**同源策略**

很多人对跨域有一种误解，以为这是前端的事，和后端没关系，其实不是这样的，说到跨域，就不得不说说浏览器的同源策略。
同源策略是由Netscape提出的一个著名的安全策略，它是浏览器最核心也最基本的安全功能，现在所有支持JavaScript的浏览器都会使用这个策略。所谓同源是指协议、域名以及端口要相同。同源策略是基于安全方面的考虑提出来的，这个策略本身没问题，但是我们在实际开发中，由于各种原因又经常有跨域的需求，传统的跨域方案是JSONP，JSONP虽然能解决跨域但是有一个很大的局限性，那就是只支持GET请求，不支持其他类型的请求，而今天我们说的CORS（跨域源资源共享）（CORS，Cross-origin resource sharing）是一个W3C标准，它是一份浏览器技术的规范，提供了Web服务从不同网域传来沙盒脚本的方法，以避开浏览器的同源策略，这是JSONP模式的现代版。
在Spring框架中，对于CORS也提供了相应的解决方案，今天我们就来看看SpringBoot中如何实现CORS。
**实践**

接下来我们就来看看Spring Boot中如何实现这个东西。

首先创建两个普通的SpringBoot项目，这个就不用我多说，第一个命名为provider提供服务，第二个命名为consumer消费服务，第一个配置端口为8080，第二个配置配置为8081，然后在provider上提供两个hello接口，一个get，一个post，如下：

    @RestController

    public class HelloController {

        @GetMapping("/hello")

        public String hello() {

            return "hello";

        }

        @PostMapping("/hello")

        public String hello2() {

            return "post hello";

        }

    }

在consumer的resources/static目录下创建一个html文件，发送一个简单的ajax请求，如下：

    <div id="app"></div>

    <input type="button" onclick="btnClick()" value="get_button">

    <input type="button" onclick="btnClick2()" value="post_button">

    <script>

        function btnClick() {

            $.get('http://localhost:8080/hello', function (msg) {

                $("#app").html(msg);

            });

        }


        function btnClick2() {

            $.post('http://localhost:8080/hello', function (msg) {

                $("#app").html(msg);

            });

        }

    </script>

然后分别启动两个项目，发送请求按钮，观察浏览器控制台如下：

    Access to XMLHttpRequest at 'http://localhost:8080/hello' from origin 'http://localhost:8081' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.

可以看到，由于同源策略的限制，请求无法发送成功。

使用CORS可以在前端代码不做任何修改的情况下，实现跨域，那么接下来看看在provider中如何配置。首先可以通过@CrossOrigin注解配置某一个方法接受某一个域的请求，如下：

    @RestController

    public class HelloController {

        @CrossOrigin(value = "http://localhost:8081")

        @GetMapping("/hello")

        public String hello() {

            return "hello";

        }


        @CrossOrigin(value = "http://localhost:8081")

        @PostMapping("/hello")

        public String hello2() {

            return "post hello";

        }

    }

这个注解表示这两个接口接受来自http://localhost:8081地址的请求，配置完成后，重启provider，再次发送请求，浏览器控制台就不会报错了，consumer也能拿到数据了。

此时观察浏览器请求网络控制台，可以看到响应头中多了如下信息：
![](https://github.com/starstarb/clouding/raw/master/js/cors.png)
这个表示服务端愿意接收来自http://localhost:8081的请求，拿到这个信息后，浏览器就不会再去限制本次请求的跨域了。

provider上，每一个方法上都去加注解未免太麻烦了，在Spring Boot中，还可以通过全局配置一次性解决这个问题，全局配置只需要在配置类中重写addCorsMappings方法即可，如下：

    @Configuration

    public class WebMvcConfig implements WebMvcConfigurer {

        @Override

        public void addCorsMappings(CorsRegistry registry) {

            registry.addMapping("/**")

            .allowedOrigins("http://localhost:8081")

            .allowedMethods("*")

            .allowedHeaders("*");

        }

    }

`/**`表示本应用的所有方法都会去处理跨域请求，allowedMethods表示允许通过的请求数，allowedHeaders则表示允许的请求头。经过这样的配置之后，就不必在每个方法上单独配置跨域了。
存在的问题

了解了整个CORS的工作过程之后，我们通过Ajax发送跨域请求，虽然用户体验提高了，但是也有潜在的威胁存在，常见的就是CSRF（Cross-site request forgery）跨站请求伪造。跨站请求伪造也被称为one-click attack 或者 session riding，通常缩写为CSRF或者XSRF，是一种挟制用户在当前已登录的Web应用程序上执行非本意的操作的攻击方法，举个例子：

    假如一家银行用以运行转账操作的URL地址如下： http://icbc.com/aa?bb=cc，那么，一个恶意攻击者可以在另一个网站上放置如下代码： <imgsrc="http://icbc.com/aa?bb=cc">，如果用户访问了恶意站点，而她之前刚访问过银行不久，登录信息尚未过期，那么她就会遭受损失。

基于此，浏览器在实际操作中，会对请求进行分类，分为简单请求，预先请求，带凭证的请求等，预先请求会首先发送一个options探测请求，和浏览器进行协商是否接受请求。默认情况下跨域请求是不需要凭证的，但是服务端可以配置要求客户端提供凭证，这样就可以有效避免csrf攻击。

## Spring Data Jpa
**Jpa**
1. JPA是什么

    Java Persistence API：用于对象持久化的 API

    Java EE 5.0 平台标准的 ORM 规范，使得应用程序以统一的方式访问持久层
    ![](https://github.com/starstarb/clouding/raw/master/js/jpa.png)
2. JPA和Hibernate的关系

    JPA 是 Hibernate 的一个抽象（就像JDBC和JDBC驱动的关系）；

    JPA 是规范：JPA 本质上就是一种 ORM 规范，不是ORM 框架，这是因为 JPA 并未提供 ORM 实现，它只是制订了一些规范，提供了一些编程的 API 接口，但具体实现则由 ORM 厂商提供实现；

    Hibernate 是实现：Hibernate 除了作为 ORM 框架之外，它也是一种 JPA 实现

    从功能上来说， JPA 是 Hibernate 功能的一个子集

3. JPA的供应商

JPA 的目标之一是制定一个可以由很多供应商实现的 API，Hibernate 3.2+、TopLink 10.1+ 以及 OpenJPA 都提供了 JPA 的实现，Jpa 供应商有很多，常见的有如下四种：

    Hibernate
    JPA 的始作俑者就是 Hibernate 的作者，Hibernate 从 3.2 开始兼容 JPA。

    OpenJPA
    OpenJPA 是 Apache 组织提供的开源项目。

    TopLink
    TopLink 以前需要收费，如今开源了。

    EclipseLink
4. JPA的优势

    标准化: 提供相同的 API，这保证了基于JPA 开发的企业应用能够经过少量的修改就能够在不同的 JPA 框架下运行。

    简单易用，集成方便: JPA 的主要目标之一就是提供更加简单的编程模型，在 JPA 框架下创建实体和创建 Java 类一样简单，只需要使用 javax.persistence.Entity 进行注解；JPA 的框架和接口也都非常简单。

    可媲美JDBC的查询能力: JPA的查询语言是面向对象的，JPA定义了独特的JPQL，而且能够支持批量更新和修改、JOIN、GROUP BY、HAVING 等通常只有 SQL 才能够提供的高级查询特性，甚至还能够支持子查询。

    支持面向对象的高级特性: JPA 中能够支持面向对象的高级特性，如类之间的继承、多态和类之间的复杂关系，最大限度的使用面向对象的模型

5. JPA包含的技术

    ORM 映射元数据：JPA 支持 XML 和 JDK 5.0 注解两种元数据的形式，元数据描述对象和表之间的映射关系，框架据此将实体对象持久化到数据库表中。

    JPA 的 API：用来操作实体对象，执行CRUD操作，框架在后台完成所有的事情，开发者从繁琐的 JDBC 和 SQL 代码中解脱出来。

    查询语言（JPQL）：这是持久化操作中很重要的一个方面，通过面向对象而非面向数据库的查询语言查询数据，避免程序和具体的 SQL 紧密耦合。

### Spring Data

Spring Data 是 Spring 的一个子项目。用于简化数据库访问，支持NoSQL 和 关系数据存储。其主要目标是使数据库的访问变得方便快捷。Spring Data 具有如下特点：

    SpringData 项目支持 NoSQL 存储：
    MongoDB （文档数据库）
    Neo4j（图形数据库）
    Redis（键/值存储）
    Hbase（列族数据库）

    SpringData 项目所支持的关系数据存储技术：
    JDBC
    JPA

    Spring Data Jpa 致力于减少数据访问层 (DAO) 的开发量. 开发者唯一要做的，就是声明持久层的接口，其他都交给 Spring Data JPA 来帮你完成！

    框架怎么可能代替开发者实现业务逻辑呢？比如：当有一个 UserDao.findUserById() 这样一个方法声明，大致应该能判断出这是根据给定条件的 ID 查询出满足条件的 User 对象。Spring Data JPA 做的便是规范方法的名字，根据符合规范的名字来确定方法需要实现什么样的逻辑。




**Spring Data结合Jpa的使用方法**

整体步骤如下：

1. 使用 IntelliJ IDEA 创建项目，创建时选择 JavaEE Persistence ，如下：
![](https://github.com/starstarb/clouding/raw/master/js/jpa1.png)

2. 建成功后，添加依赖jar，由于 Jpa 只是一个规范，因此我们说用Jpa实际上必然是用Jpa的某一种实现，那么是哪一种实现呢？当然就是Hibernate了，所以添加的jar，实际上来自 Hibernate，如下：

![](https://github.com/starstarb/clouding/raw/master/js/jpa2.jpg)

3. 添加实体类

接下来在项目中添加实体类，如下：

    @Entity(name = "t_book")

    public class Book {

        private Long id;

        private String name;

        private String author;


        @Id

        @GeneratedValue(strategy = GenerationType.IDENTITY)

        public Long getId() {

            return id;

        }

        // 省略其他getter/setter

    }

首先@Entity注解表示这是一个实体类，那么在项目启动时会自动针对该类生成一张表，默认的表名为类名，@Entity注解的name属性表示自定义生成的表名。@Id注解表示这个字段是一个id，@GeneratedValue注解表示主键的自增长策略，对于类中的其他属性，默认都会根据属性名在表中生成相应的字段，字段名和属性名相同，如果开发者想要对字段进行定制，可以使用@Column注解，去配置字段的名称，长度，是否为空等等。

4. 创建 persistence.xml 文件

JPA 规范要求在类路径的 META-INF 目录下放置persistence.xml，文件的名称是固定的

    <?xml version="1.0" encoding="UTF-8"?>

    <persistence xmlns="http://java.sun.com/xml/ns/persistence" version="2.0">

        <persistence-unit name="NewPersistenceUnit" transaction-type="RESOURCE_LOCAL">

            <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>

            <class>org.sang.Book</class>

            <properties>

                <property name="hibernate.connection.url"

                          value="jdbc:mysql:///jpa01?useUnicode=true&amp;characterEncoding=UTF-8"/>

                <property name="hibernate.connection.driver_class" value="com.mysql.jdbc.Driver"/>

                <property name="hibernate.connection.username" value="root"/>

                <property name="hibernate.connection.password" value="123"/>

                <property name="hibernate.archive.autodetection" value="class"/>

                <property name="hibernate.show_sql" value="true"/>

                <property name="hibernate.format_sql" value="true"/>

                <property name="hibernate.hbm2ddl.auto" value="update"/>

            </properties>

        </persistence-unit>

    </persistence>

> 注意：

    persistence-unit 的name 属性用于定义持久化单元的名字, 必填。

    transaction-type：指定 JPA 的事务处理策略。RESOURCE_LOCAL：默认值，数据库级别的事务，只能针对一种数据库，不支持分布式事务。如果需要支持分布式事务，使用JTA：transaction-type="JTA"

    class节点表示显式的列出实体类

    properties中的配置分为两部分：数据库连接信息以及Hibernate信息

5. 执行持久化操作
```
    EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory("NewPersistenceUnit");

    EntityManager manager = entityManagerFactory.createEntityManager();

    EntityTransaction transaction = manager.getTransaction();

    transaction.begin();

    Book book = new Book();

    book.setAuthor("罗贯中");

    book.setName("三国演义");

    manager.persist(book);

    transaction.commit();

    manager.close();

    entityManagerFactory.close();
```
这里首先根据配置文件创建出来一个 EntityManagerFactory ，然后再根据 EntityManagerFactory 的实例创建出来一个 EntityManager ，然后再开启事务，调用 EntityManager 中的 persist 方法执行一次持久化操作，最后提交事务，执行完这些操作后，数据库中旧多出来一个 t_book 表，并且表中有一条数据。

__关于 JPQL__

    JPQL语言，即 Java Persistence Query Language 的简称。JPQL 是一种和 SQL 非常类似的中间性和对象化查询语言，它最终会被编译成针对不同底层数据库的 SQL 查询，从而屏蔽不同数据库的差异。JPQL语言的语句可以是 select 语句、update 语句或delete语句，它们都通过 Query 接口封装执行。

    Query接口封装了执行数据库查询的相关方法。调用 EntityManager 的 createQuery、create NamedQuery 及 createNativeQuery 方法可以获得查询对象，进而可调用 Query 接口的相关方法来执行查询操作。

Query接口的主要方法如下：

    int executeUpdate(); | 用于执行update或delete语句。

    List getResultList(); | 用于执行select语句并返回结果集实体列表。

    Object getSingleResult(); | 用于执行只返回单个结果实体的select语句。

    Query setFirstResult(int startPosition); | 用于设置从哪个实体记录开始返回查询结果。

    Query setMaxResults(int maxResult); | 用于设置返回结果实体的最大数。与setFirstResult结合使用可实现分页查询。

    Query setFlushMode(FlushModeType flushMode); | 设置查询对象的Flush模式。参数可以取2个枚举值：FlushModeType.AUTO 为自动更新数据库记录，FlushMode Type.COMMIT 为直到提交事务时才更新数据库记录。

    setHint(String hintName, Object value); | 设置与查询对象相关的特定供应商参数或提示信息。参数名及其取值需要参考特定 JPA 实现库提供商的文档。如果第二个参数无效将抛出IllegalArgumentException异常。

    setParameter(int position, Object value); | 为查询语句的指定位置参数赋值。Position 指定参数序号，value 为赋给参数的值。

    setParameter(int position, Date d, TemporalType type); | 为查询语句的指定位置参数赋 Date 值。Position 指定参数序号，value 为赋给参数的值，temporalType 取 TemporalType 的枚举常量，包括 DATE、TIME 及 TIMESTAMP 三个，，用于将 Java 的 Date 型值临时转换为数据库支持的日期时间类型（java.sql.Date、java.sql.Time及java.sql.Timestamp）。

    setParameter(int position, Calendar c, TemporalType type); | 为查询语句的指定位置参数赋 Calenda r值。position 指定参数序号，value 为赋给参数的值，temporalType 的含义及取舍同前。

    setParameter(String name, Object value); | 为查询语句的指定名称参数赋值。

    setParameter(String name, Date d, TemporalType type); | 为查询语句的指定名称参数赋 Date 值,用法同前。

    setParameter(String name, Calendar c, TemporalType type); | 为查询语句的指定名称参数设置Calendar值。name为参数名，其它同前。该方法调用时如果参数位置或参数名不正确，或者所赋的参数值类型不匹配，将抛出 IllegalArgumentException 异常。

__JPQL 举例__

和在 SQL 中一样，JPQL 中的 select 语句用于执行查询。其语法可表示为：
```
select_clause form_clause[where_clause][groupby_clause][having_clause][orderby_clause]
```
其中：

    from 子句是查询语句的必选子句。

    select 用来指定查询返回的结果实体或实体的某些属性。

    from 子句声明查询源实体类，并指定标识符变量（相当于SQL表的别名）。

    如果不希望返回重复实体，可使用关键字 distinct 修饰。select、from 都是 JPQL 的关键字，通常全大写或全小写，建议不要大小写混用。

在 JPQL 中，查询所有实体的 JPQL 查询语句很简单，如下：
selectofromOrdero或selectofromOrderaso
这里关键字 as 可以省去，标识符变量的命名规范与 Java 标识符相同，且区分大小写,调用 EntityManager 的 createQuery() 方法可创建查询对象，接着调用 Query 接口的 getResultList() 方法就可获得查询结果集，如下：

    Query query = entityManager.createQuery( "select o from Order o"); 

    List orders = query.getResultList();

    Iterator iterator = orders.iterator();

    while(iterator.hasNext() ) {

      // 处理Order

    }

其他方法的与此类似，这里不再赘述。


#### Spring Data Jpa配置

__基本环境搭建__

首先创建一个普通的Maven工程，并添加如下依赖：

    <dependencies>

        <dependency>

            <groupId>org.springframework</groupId>

            <artifactId>spring-orm</artifactId>

            <version>5.0.2.RELEASE</version>

        </dependency>

        <dependency>

            <groupId>org.springframework</groupId>

            <artifactId>spring-oxm</artifactId>

            <version>5.0.2.RELEASE</version>

        </dependency>

        <dependency>

            <groupId>org.springframework</groupId>

            <artifactId>spring-aop</artifactId>

            <version>5.0.2.RELEASE</version>

        </dependency>

        <dependency>

            <groupId>org.springframework</groupId>

            <artifactId>spring-aspects</artifactId>

            <version>5.0.2.RELEASE</version>

        </dependency>

        <dependency>

            <groupId>mysql</groupId>

            <artifactId>mysql-connector-java</artifactId>

            <version>5.1.27</version>

        </dependency>

        <dependency>

            <groupId>org.springframework</groupId>

            <artifactId>spring-context-support</artifactId>

            <version>5.0.2.RELEASE</version>

        </dependency>

        <dependency>

            <groupId>org.springframework</groupId>

            <artifactId>spring-expression</artifactId>

            <version>5.0.2.RELEASE</version>

        </dependency>

        <dependency>

            <groupId>org.hibernate</groupId>

            <artifactId>hibernate-core</artifactId>

            <version>5.2.12.Final</version>

        </dependency>

        <dependency>

            <groupId>org.hibernate</groupId>

            <artifactId>hibernate-jpamodelgen</artifactId>

            <version>5.2.12.Final</version>

        </dependency>

        <dependency>

            <groupId>com.alibaba</groupId>

            <artifactId>druid</artifactId>

            <version>1.0.29</version>

        </dependency>

        <dependency>

            <groupId>org.springframework.data</groupId>

            <artifactId>spring-data-jpa</artifactId>

            <version>1.11.3.RELEASE</version>

        </dependency>

    </dependencies>

这里除了 Jpa 的依赖之外，就是Spring Data Jpa 的依赖了。

接下来创建一个 User 实体类，创建方式参考 Jpa中实体类的创建方式，这里不再赘述。

接下来在resources目录下创建一个applicationContext.xml文件，并配置Spring和Jpa，如下：

    <context:property-placeholder location="classpath:db.properties"/>

    <context:component-scan base-package="org.sang"/>

    <bean class="com.alibaba.druid.pool.DruidDataSource" id="dataSource">

        <property name="driverClassName" value="${db.driver}"/>

        <property name="url" value="${db.url}"/>

        <property name="username" value="${db.username}"/>

        <property name="password" value="${db.password}"/>

    </bean>

    <bean class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean" id="entityManagerFactory">

        <property name="dataSource" ref="dataSource"/>

        <property name="jpaVendorAdapter">

            <bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter"/>

        </property>

        <property name="packagesToScan" value="org.sang.model"/>

        <property name="jpaProperties">

            <props>

                <prop key="hibernate.show_sql">true</prop>

                <prop key="hibernate.format_sql">true</prop>

                <prop key="hibernate.hbm2ddl.auto">update</prop>

                <prop key="hibernate.dialect">org.hibernate.dialect.MySQL57Dialect</prop>

            </props>

        </property>

    </bean>

    <bean class="org.springframework.orm.jpa.JpaTransactionManager" id="transactionManager">

        <property name="entityManagerFactory" ref="entityManagerFactory"/>

    </bean>

    <tx:annotation-driven transaction-manager="transactionManager"/>

    <!-- 配置jpa -->

    <jpa:repositories base-package="org.sang.dao"

                      entity-manager-factory-ref="entityManagerFactory"/>

这里和 Jpa 相关的配置主要是三个，一个是entityManagerFactory，一个是Jpa的事务，还有一个是配置dao的位置，配置完成后，就可以在 org.sang.dao 包下创建相应的 Repository 了，如下：

    public interface UserDao extends Repository<User, Long> {

        User getUserById(Long id);

    }

getUserById表示根据id去查询User对象，只要我们的方法名称符合类似的规范，就不需要写SQL，具体的规范一会来说。好了，接下来，创建 Service 和 Controller 来调用这个方法，如下：

    @Service

    @Transactional

    public class UserService {

        @Resource

        UserDao userDao;


        public User getUserById(Long id) {

            return userDao.getUserById(id);

        }

    }

    public void test1() {

        ClassPathXmlApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml");

        UserService userService = ctx.getBean(UserService.class);

        User user = userService.getUserById(1L);

        System.out.println(user);

    }

这样，就可以查询到id为1的用户了。
###### Repository

上文我们自定义的 UserDao 实现了 Repository 接口，这个 Repository 接口是什么来头呢？

首先来看 Repository 的一个继承关系图：
![](https://github.com/starstarb/clouding/raw/master/js/jpa3.jpg)
可以看到，实现类不少。那么到底如何理解 Repository 呢？

    Repository 接口是 Spring Data 的一个核心接口，它不提供任何方法，开发者需要在自己定义的接口中声明需要的方法 publicinterfaceRepository<T,IDextendsSerializable>{}

    若我们定义的接口继承了 Repository, 则该接口会被 IOC 容器识别为一个 Repository Bean，进而纳入到 IOC 容器中，进而可以在该接口中定义满足一定规范的方法。

    Spring Data可以让我们只定义接口，只要遵循 Spring Data 的规范，就无需写实现类。

    与继承 Repository 等价的一种方式，就是在持久层接口上使用 @RepositoryDefinition 注解，并为其指定 domainClass 和 idClass 属性。像下面这样：

    @RepositoryDefinition(domainClass = User.class, idClass = Long.class)

    public interface UserDao

    {

        User findById(Long id);

        List<User> findAll();

    }

基础的 Repository 提供了最基本的数据访问功能，其几个子接口则扩展了一些功能，它的几个常用的实现类如下：

    CrudRepository： 继承 Repository，实现了一组 CRUD 相关的方法

    PagingAndSortingRepository： 继承 CrudRepository，实现了一组分页排序相关的方法

    JpaRepository： 继承 PagingAndSortingRepository，实现一组 JPA 规范相关的方法

    自定义的 XxxxRepository 需要继承 JpaRepository，这样的 XxxxRepository 接口就具备了通用的数据访问控制层的能力。

    JpaSpecificationExecutor： 不属于Repository体系，实现一组 JPA Criteria 查询相关的方法

### 方法定义规范
1. 简单条件查询

    按照 Spring Data 的规范，查询方法以 find | read | get 开头

    涉及条件查询时，条件的属性用条件关键字连接，要注意的是：条件属性以首字母大写

例如：定义一个 Entity 实体类：

    class User｛ 

       private String firstName; 

       private String lastName; 

    ｝ 

使用And条件连接时，条件的属性名称与个数要与参数的位置与个数一一对应，如下：

    findByLastNameAndFirstName(String lastName,String firstName);

    支持属性的级联查询. 若当前类有符合条件的属性, 则优先使用, 而不使用级联属性. 若需要使用级联属性, 则属性之间使用 _ 进行连接.

查询举例：
1.按照id查询

    User getUserById(Long id);

    User getById(Long id);

2.查询所有年龄小于90岁的人

    List<User> findByAgeLessThan(Long age);

3.查询所有姓赵的人

    List<User> findByUsernameStartingWith(String u);

4.查询所有姓赵的、并且id大于50的人

    List<User> findByUsernameStartingWithAndIdGreaterThan(String name, Long id);

5.查询所有姓名中包含"上"字的人

    List<User> findByUsernameContaining(String name);

6.查询所有姓赵的或者年龄大于90岁的

    List<User> findByUsernameStartingWithOrAgeGreaterThan(String name, Long age);

7.查询所有角色为1的用户

    List<User> findByRole_Id(Long id);

2. 支持的关键字

支持的查询关键字如下图：
![](https://github.com/starstarb/clouding/raw/master/js/jpa4.png)

3. 查询方法流程解析

为什么写上方法名，JPA就知道你想干嘛了呢？假如创建如下的查询： findByUserDepUuid()，框架在解析该方法时，首先剔除 findBy，然后对剩下的属性进行解析，假设查询实体为Doc：

    先判断 userDepUuid （根据 POJO 规范，首字母变为小写）是否为查询实体的一个属性，如果是，则表示根据该属性进行查询；如果没有该属性，继续第二步；

    从右往左截取第一个大写字母开头的字符串(此处为Uuid)，然后检查剩下的字符串是否为查询实体的一个属性，如果是，则表示根据该属性进行查询；如果没有该属性，则重复第二步，继续从右往左截取；最后假设 user 为查询实体的一个属性；

    接着处理剩下部分（DepUuid），先判断 user 所对应的类型是否有depUuid属性，如果有，则表示该方法最终是根据 “ Doc.user.depUuid” 的取值进行查询；否则继续按照步骤 2 的规则从右往左截取，最终表示根据 “Doc.user.dep.uuid” 的值进行查询。

    可能会存在一种特殊情况，比如 Doc包含一个 user 的属性，也有一个 userDep 属性，此时会存在混淆。可以明确在属性之间加上 "" 以显式表达意图，比如 "findByUserDepUuid()" 或者 "findByUserDep_uuid()"

    还有一些特殊的参数：例如分页或排序的参数：

    Page<UserModel> findByName(String name, Pageable pageable);  

    List<UserModel> findByName(String name, Sort sort);

__@Query注解__

有的时候，这里提供的查询关键字并不能满足我们的查询需求，这个时候就可以使用 @Query 关键字，来自定义查询 SQL，例如查询Id最大的User：

    @Query("select u from t_user u where id=(select max(id) from t_user)")

    User getMaxIdUser();

如果查询有参数的话，参数有两种不同的传递方式，

1.利用下标索引传参，索引参数如下所示，索引值从1开始，查询中 ”?X” 个数需要与方法定义的参数个数相一致，并且顺序也要一致：

    @Query("select u from t_user u where id>?1 and username like ?2")

    List<User> selectUserByParam(Long id, String name);

2.命名参数（推荐）：这种方式可以定义好参数名，赋值时采用@Param("参数名")，而不用管顺序：

    @Query("select u from t_user u where id>:id and username like :name")

    List<User> selectUserByParam2(@Param("name") String name, @Param("id") Long id);

查询时候，也可以是使用原生的SQL查询，如下：

    @Query(value = "select * from t_user",nativeQuery = true)

    List<User> selectAll();

__@Modifying注解__

涉及到数据修改操作，可以使用 @Modifying 注解，@Query 与 @Modifying 这两个 annotation一起声明，可定义个性化更新操作，例如涉及某些字段更新时最为常用，示例如下：

    @Modifying

    @Query("update t_user set age=:age where id>:id")

    int updateUserById(@Param("age") Long age, @Param("id") Long id);

> 注意：

    可以通过自定义的 JPQL 完成 UPDATE 和 DELETE 操作. 注意: JPQL 不支持使用 INSERT

    方法的返回值应该是 int，表示更新语句所影响的行数

    在调用的地方必须加事务，没有事务不能正常执行

    默认情况下, Spring Data 的每个方法上有事务, 但都是一个只读事务. 他们不能完成修改操作

说到这里，再来顺便说说Spring Data 中的事务问题：

    Spring Data 提供了默认的事务处理方式，即所有的查询均声明为只读事务。

    对于自定义的方法，如需改变 Spring Data 提供的事务默认方式，可以在方法上添加 @Transactional 注解。

    进行多个 Repository 操作时，也应该使它们在同一个事务中处理，按照分层架构的思想，这部分属于业务逻辑层，因此，需要在Service 层实现对多个 Repository 的调用，并在相应的方法上声明事务。



## Spring Boot中的Starter,自动化配置

