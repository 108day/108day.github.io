---
layout: post
title: springboot实战-第一课 初识SpringBoot
category: springboot
tags: [springboot]
---

# What SpringBoot？
 养成良好的学习习惯，对于概念，理解类的疑惑首先去官网看看，不懂英文的可以用百度，网易等翻译工具看，下面我们看[官网](https://spring.io/)对SpringBoot 的解释：
> Spring Boot is designed to get you up and running as quickly as possible,with minimal upfront configuration of Spring. Spring Boot takes an opinionated view of building production-ready applications.

`翻译过来大意就是：Spring Boot被设计为使用Spring的最小预配置来使您尽快启动和运行。Spring Boot对构建生产就绪的应用程序持己见。`

按照我对这句话的理解就是：springboot 就是把需要我们自己可以灵活配置的地方或者说可以扩展的地方（必要的地方），给你组合了一种配置，默认给你配置好了，为的就是可以快速启动和运行，减小使用难度。

springboot并不是什么新的技术，而是一种思想，一种理念。约定大于配置，默认大于配置，能不配置就不要配置，需要配置的地方我帮你配置好，就这样。
其实这些事我们自己用之前的SSH,SSM等框架也可以做，自己配置好一个项目，如果你有源码的话，还可以打成一个包，然后作为模板来用，以后开发的时候就可以直接拿过来用，这样提高开发速度，不用再创建，配置，构建等等，springboot就是帮我们做完这些事，而且别人团队技术牛X，大咖很多，代码质量高，设计思想先进，我们直接用人家的就行。

总感觉现在对于快速有种特别的追求，其他的项目也有这种思想，比如：
- Docker自动化快速构建部署环境（一劳永逸，不再担心多环境部署，加机器，一样的配置环境不一致等问题）
- mybatis generator 自动生成代码（只要数据库有表，就可以给你生成对应的三层代码）
- maven对jar包的管理（你不用再copy包到lib下）
- JPA（根据接口方法名称自动生成对应的SQL，实现对数据库的操作）你自己也可以写很多很多的常用，重载方法，所有可能用到的方法一网打尽，并且持续添加场景，最后打成一个jar包，一劳永逸。
……

这些都是为了节省一些体力活，如是而已。工欲善其事，必先利其器。

# 为什么要用SpringBoot?
- 快速启动，部署，开发
- 可以快速集成很多第三方的模块，灵活配置
- 和spring,spring mvc 一脉相承
- 最好的微服务落地框架，并且有服务治理 springcloud 架构配合，优势大于弱势
- 基于以上的优点，很多企业都在使用，大势所趋，不学不行

## 创建一个SpringBoot的项目
不废话了，开始上手,直接去官网创建一个demo先，地址：https://start.spring.io/   如下图所示：
![enter description here][1]

以上就是需要的maven依赖，在springboot 页面搜索，或者勾选，最后 点击 Generate Project ，会下载demo到本地，然后解压并导入到IDE中。

 导入之前 需要的环境 JDK1.8, Maven 3.x
> 可能出现的问题：
导入之后maven会自动去本地maven仓库去找配置所需的依赖，如果本地没有找到，就会远程去中央仓库去找，一般不修改远程仓库的话，是国外的，下载速度很慢，而导致项目依赖不到jar包，项目就会有报错信息。
所以我们要修改maven默认去找的远程仓库，改为国内的镜像库阿里的，在maven安装目录中，例如：apache-maven-3.1.1/conf/setting.xml 中 ，找到 mirrors ，添加如下:

```
<mirror>
     <id>alimaven</id>
     <mirrorOf>central</mirrorOf>
     <name>aliyun maven</name>
     <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
</mirror>
```

然后再配置eclipse的maven 配置：设置Global setting 和 User setting 一致
![enter description here][2]

然后就可以可以看到eclipse右下角在下载maven依赖包了
idea 同理，配置就不在这里讲解了。

### 创建Controller

项目导入到IDE 正常后 ，我们就可以开始我们的Coding旅程了，SpringBoot 说它部署，启动速度很快，我们就来看看到底有多快。

在com.example.demo下面 新建 包 controller ，再包中新建类 DemoController.java 代码如下：
```java
package com.example.demo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.HashMap;
import java.util.Map;
/**
 * 控制层
 * @author 108day
 *
 */
@RestController
public class DemoController {

    @GetMapping(value = "/helloworld")
    public String hello(Model model) {
        String name = "jiangbei";
        model.addAttribute("name", name);
        return "helloworld";
    }
    
```
### 测试DemoController

启动 springboot 
我们没有连接数据库，但是又依赖了mysql的jar包，所以要指定排除数据源连接的方式启动
    修改：DemoApplication.java  在启动类的注解后面加 (exclude={DataSourceAutoConfiguration.class,HibernateJpaAutoConfiguration.class}) 修改后代码如下：
    
```java
    @SpringBootApplication(exclude={DataSourceAutoConfiguration.class,HibernateJpaAutoConfiguration.class})
 ```
 
 解释：exclude 不包含的意思，DataSourceAutoConfiguration 自动配置数据源，自动配置JPA,所以这段注解就是告诉程序，不用自动去连接数据库。然后右键运行就行了
    
 直接在浏览器测试接口 ：http://localhost:8001/helloworld 结果如下：
 
> helloWorld

可以看到我访问的端口是 8001 ,这个可以改，我用的配置文件是 .yml结尾，所以在application.yml 中 配置 ：
```
server:
    tomcat:
        uri-encoding: UTF-8  #请求url 字符编码
        max-threads: 1000 #内置tomcat最大连接数
        min-spare-threads: 30
    port: 8001   # 端口
    servlet:
      context-path: /admin  #上下文访问路径 
```
### 集成JPA
 之前我们创建项目的时候jar包已经依赖了，这里就不说明了
### 新建entity 实体
 ```
 @Entity
public class User implements Serializable {

    private static final long serialVersionUID = 1L;
    @Id
    @GeneratedValue
    private Long id;
    @Column(nullable = false, unique = true)
    private String userName;
    @Column(nullable = false)
    private String passWord;
    @Column(nullable = false, unique = true)
    private String email;
    @Column(nullable = true, unique = true)
    private String nickName;
    @Column(nullable = false)
    private String regTime;
     /**省略getter settet方法、构造方法，记得加上，否则查询数据库时不会有数据，返回的都是空对象*/
```

### 新建dao层

 ```
 public interface UserRepository extends JpaRepository<User, Long> {

    User findByUserName(String userName);

    User findByUserNameOrEmail(String username, String email);
}

 ```
 
 因为JPA是根据方法可以自动生成相对应的SQL，所以不用实现接口，接口只需要集成JpaRespository 即可
 还有很多已经实现的方法，比如：findAll()， 可以直接反编译看源码，就知道他提供了什么默认接口了。
 源码:
 ![enter description here][3]
### 新建service层
接口：
```java
public interface IUserRepositoryService{

    /**
     * 服务一般都写在service层
     * findAll方法名随便起的，和JPA没有关系
     */
    public List<User> findAll();

}
```
实现：
```java
@Service("userRepositoryService")
public class UserRepositoryServiceImpl implements IUserRepositoryService {

    @Autowired
    private UserRepository userRepository;

    public List<User> findAll(){
        return userRepository.findAll();
    }
}
```
### application.yml 中配置JPA
 想要接口可以连接到数据库，前提是你要先创建一个数据库，我创建了一个demo 数据库
 配置如下：
 ```
 spring:
  datasource:
    url: jdbc:mysql://localhost:3306/demo?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=UTF-8&useSSL=false
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver # 数据库驱动

  jpa:
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQL5InnoDBDialect   # 决定生成的sql，和连接的数据库对应
        hbm2ddl:
          auto: update # 常用属性；作用：第一次启动访问后，会根据entity 注解配置创建表，后面再次启动时只对比数据库更新，不创建，也不删除原有数据
    show-sql: true  # 是否打印SQL
 ```
> 这里重点说下，Hibernate的自动生成数据库表结构的策略 auto: update 参数
-  create : 自动创建|更新|验证数据库表结构
- create-drop : 每次加载hibernate时根据model类生成表，但是sessionFactory一关闭,表就自动删除
- update：最常用的属性，第一次加载hibernate时根据model类会自动建立起表的结构（前提是先建立好数据库），以后加载hibernate时根据 model类自动更新表结构，即使表结构改变了但表中的行仍然存在不会删除以前的行。要注意的是当部署到服务器后，表结构是不会被马上建立起来的，是要等 应用第一次运行起来后才会。
- validate ：每次加载hibernate时，验证创建数据库表结构，只会和数据库中的表进行比较，不会创建新表，但是会插入新值。

接下来开始启动，因为我们已经连接了数据库，所以需要数据源，所以需要把之前启动类的注解还原。
也就是去掉之前添加的(exclude={DataSourceAutoConfiguration.class,HibernateJpaAutoConfiguration.class})  然后再启动就可以了。
因为没有数据，所以写个单元测试测试下:

```java
@Autowired
	private UserRepository userRepository;

	@Test
	public void test() throws Exception {
		Date date = new Date();
		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG);
		String formattedDate = dateFormat.format(date);
		userRepository.deleteAll();
		List<User> list = userRepository.findAll();
		if(list==null || list.size()==0) {
			System.out.println(String.format("====================>%s","保存"));
			userRepository.save(new User("沈万三", "123@126.com", "123", "123", formattedDate));
			userRepository.save(new User("唐明皇", "456@126.com", "456", "456", formattedDate));
			userRepository.save(new User("秦始皇", "789@126.com", "789", "789", formattedDate));
		}
		List<User> result = userRepository.findAll();
		for(User user:result){
			System.out.println(String.format("====================>%s,%s,%s",user.getUserName(),user.getEmail(),user.getNickName()));
		}
	}
```

测试结果截图：

![enter description here][4] 

### 集成Thymeleaf

Thymeleaf ：前端渲染引擎和JSP，Freemarker类似 项目生成之时已经引入了maven 配置，在此就不多说

新建一个方法：

 ```java
       /**
     * 查询所用用户
     * @return
     */
    @GetMapping
    public ModelAndView list(Model model) {
        model.addAttribute("userList",userRepository.findAll() );
        model.addAttribute("title", "用户管理");
        return new ModelAndView("list", "userModel", model);
    }
 ```
    
 在templates 下面 创建 list.html
 
``` html
<html xmlns="http://www.w3.org/1999/xhtml"
      xmlns:th="http://www.thymeleaf.org">

<head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <title>k</title>
</head>
<body>
<!--/*@thymesVar id="name" type="java.lang.String"*/-->

<ol>
	<li th:each="item: ${userModel.userList}"> 
		<span th:text="|Welcome to our application, ${item.id},${item.userName}!|" >
	</li>
</ol>
</body>
</html>
```

结果截图如下：
![enter description here][5]

最终目录如下：
![enter description here][6]

## 总结
- IDE的缓存问题。刚开始用ideal ，结果最后一直找不到 templates下的模板，看了很多的文章，有的说是配置问题，有的说是模板问题，最后我引用eclipse重新测试就可以了
- 配置application.yml 的问题。之前用的application.properties，然后就想知道这两种的区别，然后就换了.yml的文件，发现就是 把properties 中的"." 用冒号：换行，要注意缩进，然后把等号 变为 冒号 ： 加空格 ，再把值跟在后面。但是配置过程还是有些不一样，因为最后一个点，在把properties是驼峰设计，在yml中是 - 相连，都是小写。缩进不对，缺少空格，都不行，配置检查都不会过。两种配置文件一起使用，有限读取.yml配置文件，这个你测试代码时就知道。
- [配置文件不正确问题](https://stackoverflow.com/questions/38891866/when-spring-boot-startup-throw-out-the-method-names-must-be-tokens-exception) 看了好几篇文章，结果说的根本不对，不是因为tomcat缓冲区的问题，还是因为读取配置文件出错，而导致tomcat报错，解决的根本还是重新检查配置文件。从配置文件易用性来说，传统的.properties 文件容易配置，从阅读的方便直观角度来看.yml 配置文件比较易读性更好。可以根据自己的需要取舍。
- 返回对象是ModelAndView时，页面展示时的问题。具体看list.html的[Thymeleaf表达式 ](https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html#basic-configuration)

从整个项目的搭建过程中可以看出来，相比以前写一个SpringMvc的工程是简便了非常多，不过也还是遵循SpringMVC的开发模式。而且因为配置大量减少，且集中在一个配置文件中，这使得项目搭建更容易成功，报错之后更容易定位错误位置。

[Thymeleaf官网文档](https://www.thymeleaf.org/doc/tutorials/3.0/thymeleafspring.html#basic-configuration)

[本项目源代码地址](https://github.com/108day/springboot-examples)  项目名称：springboot-jpa-thymeleaf

> 本着开源分享的心态，对于参考过的文章、代码等都尽量在下面了，如果有版权问题，请联系我修改！

## 参考文章
- [springboot:web综合开发](http://www.ityouknow.com/springboot/2016/02/03/spring-boot-web.html)
- [springboot+jpa+thymeleaf增删改查示例](http://www.ityouknow.com/springboot/2017/09/23/spring-boot-jpa-thymeleaf-curd.html)
- [springboot+maven+thymeleaf配置实战demo](https://www.cnblogs.com/nicknailo/p/8947643.html)
- [报错 Invalid character found in method name. HTTP method names must be tokens](https://stackoverflow.com/questions/38891866/when-spring-boot-startup-throw-out-the-method-names-must-be-tokens-exception) ：不是缓存问题，缓存错误提示只是表象，根本原因是配置文件错误
-  [报错 Invalid character found in method name. HTTP method names must be tokens](https://www.cnblogs.com/breath-taking/articles/6070046.html) 同上，不是缓存问题，缓存错误提示只是表象，根本原因是配置文件错误
- [spring boot的 yml和properties的对比](https://www.cnblogs.com/dyh-air/articles/9090882.html)
- [错误Cannot find template location: classpath:/templates/ (please add some templates or check your Thymeleaf configuration)](https://stackoverflow.com/questions/41318005/how-to-locate-thymeleaf-template-from-spring-boot) 根本原因是ideal缓存，用eclipse重新打开下项目就好了，代码基本没有改动。


  [1]: /assets/images/2018/springboot/springboot-install-1.png "springboot-install-1.png"
  [2]: /assets/images/2018/springboot/springboot-install-2.png "springboot-install-2.png"
  [3]: /assets/images/2018/springboot/springboot-install-3.png "springboot-install-3.png"
  [4]: /assets/images/2018/springboot/springboot-install-4.png "springboot-install-4.png"
  [5]: /assets/images/2018/springboot/springboot-install-5.png "springboot-install-5.png"
  [6]: /assets/images/2018/springboot/springboot-install-6.png "springboot-install-6.png"
