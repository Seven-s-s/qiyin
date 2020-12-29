---
title: SpringCloud
date: 2020/12/28 16:47
categories:
	- [计算机, 框架]
tags:
	- SpringCloud
---

# 微服务技术栈

| 微服务条目                               | 技术落地                                                     |
| ---------------------------------------- | ------------------------------------------------------------ |
| 服务开发                                 | SpringBoot,Spring,SpringMVC                                  |
| 服务配置与管理                           | Netfix公司的Archaius、阿里的Diamond等                        |
| 服务注册与发现                           | Eureka、Consul、Zookeeper等                                  |
| 服务调用                                 | Rest、RPC、gRPC                                              |
| 服务熔断器                               | Hystrix、Envoy等                                             |
| 负载均衡                                 | Ribbon、Nginx等                                              |
| 服务接口调用（客户端调用服务的简化接口） | Fegin等                                                      |
| 消息队列                                 | Kafka、RabbitMQ、ActiveMQ等                                  |
| 服务配置中心管理                         | SpringCloudConfig、Chef等                                    |
| 服务路由（API网关）                      | Zuul等                                                       |
| 服务监控                                 | Zabbix、Nagios、Metrics、Specatator等                        |
| 全链路追踪                               | Zipkin、Brave、Dapper等                                      |
| 服务部署                                 | Docker、OpenStack、Kubernetes等                              |
| 数据流操作开发包                         | SpringCloud Stream（封装与Redis,Rabbit,Kafka等发送接收消息） |
| 事件消息总线                             | SpringCloud Bus                                              |

# 什么是SpringCloud

> SpringCloud，基于SpringBoot提供了一套微服务解决方案，包括服务注册与发现，配置中心，全链路监控，服务网关，负载均衡，熔断器等组件，除了基于NetFix的开源组件做高度抽象封装之外，还有一些选型中立的开源组件。
>
> SpringCloud利用了SpringBoot的开发便利性，巧妙地简化了分布式系统基础设施的开发，SpringCloud为开发人员提供了快速构建分布式系统的一些工具，**包括配置管理，服务发现，断路器，路由，微代理，事件总览，全局锁，决策竞选，分布式会话等等**，他们都可以用SpringBoot的开发风格做到一键启动和部署。
>
> SpringCloud是分布式微服务架构下的一站式解决方案，是各个微服务架构落地技术的集合体，俗称微服务全家桶。

# SpringCloud和SpringBoot的关系

> * SpringBoot专注于快速方便的开发单个个体微服务。
> * SpringCloud是关注全局的微服务协调整理治理框架，他将SpringBoot开发的一个个单体微服务整合并管理起来，为各个微服务之间提供：配置管理、服务发现、断路器、路由、微代理、事件总览、全局锁、决策竞选、分布式会话等集合服务
> * SpringBoot可以离开SpringCloud独立使用，开发项目，但SPringCloud离不开SpringBoot，属于依赖关系。
> * **SpringBoot专注于快速开发、方便的开发单个个体微服务，SpringCloud关注全局的服务治理框架**

SpringCloud版本惦

| SpringBoot | SpringCloud | 关系                                         |
| ---------- | ----------- | -------------------------------------------- |
| 1.2.x      | Angel版本   | 兼容SpringBoot 1.2.x                         |
| 1.3.x      | Brixton     | 兼容SpringBoot 1.3.x，也兼容SpringBoot 1.4.x |
| 1.4.x      | Camden      | 兼容SpringBoot 1.4.x，也兼容SpringBoot 1.5.x |
| 1.5.x      | Dalston     | 兼容SpringBoot 1.5.x，不兼容SpringBoot 2.0.x |
| 1.5.x      | Edgware     | 兼容SpringBoot 1.5.x，不兼容SpringBoot 2.0.x |
| 2.0.x      | Finchley    | 兼容SpringBoot 2.0.x，不兼容SpringBoot 1.5.x |
| 2.1.x      | Greenwich   |                                              |

# SpringCloud基本使用

## 创建父工程

* 新建父工程项目SpringCloud，切记Packageing是pom模式
* 主要是定义pom文件，将后续各个子模块公用的jar包等统一提取出来，类似于一个抽象父类

## 项目结构

![1](/assets/SpringCloud.assets/1.png)

## 操作步骤

1. 创建一个Maven项目，取名叫springcloud
2. 因为是一个父项目，所以删除src包，只剩下pom文件
3. 导入依赖（springcloud中的pom.xml）

```xml
<!--打包方式-->
<packaging>pom</packaging>

<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <junit.version>4.12</junit.version>
    <lombok.version>1.18.12</lombok.version>
    <log4j.version>1.2.17</log4j.version>
</properties>

<dependencyManagement>
    <dependencies>
        <!--springcloud的依赖-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Greenwich.SR1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <!--springboot-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-dependencies</artifactId>
            <version>2.1.4.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <!--数据库-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.11</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.10</version>
        </dependency>
        <!--springboot的启动器-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.3.2</version>
        </dependency>
        <!--日志测试-->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-core</artifactId>
            <version>1.2.3</version>
        </dependency>
        <!--junit-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
        </dependency>
        <!--lombok-->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
        </dependency>
        <!--log4j-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```

4. 在该父项目中新建一个maven项目，取名为springcloud-api
5. 导入依赖（springcloud-api中的pom.xml）

```xml
<!--当前的module自己需要的依赖，如果父依赖中已经配置了版本，这里就不需要写了-->
<dependencies>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-feign</artifactId>
        <version>1.4.6.RELEASE</version>
    </dependency>
</dependencies>
```

6. 之后父项目的pom文件中就会出现以下内容

```xml
<modules>
        <module>springcloud-api</module>
</modules>
```

7. 创建一个db01数据库

```sql
CREATE TABLE `dept`  (
  `deptno` bigint(0) NOT NULL AUTO_INCREMENT,
  `dname` varchar(60) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  `db_sourse` varchar(60) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL,
  PRIMARY KEY (`deptno`) USING BTREE
);

INSERT INTO `dept` VALUES (1, '开发部', 'db01');
INSERT INTO `dept` VALUES (2, '人事部', 'db01');
INSERT INTO `dept` VALUES (3, '财务部', 'db01');
INSERT INTO `dept` VALUES (4, '市场部', 'db01');
INSERT INTO `dept` VALUES (5, '运维部', 'db01');
INSERT INTO `dept` VALUES (6, '游戏部', 'db01');
```

8. 新建一个Dept类（springcloud-api下的com.example.springcloud.pojo）

```java
@Data
@NoArgsConstructor
@Accessors(chain = true)//链式写法
public class Dept implements Serializable {

    private Long deptno;//主键

    private String dname;
    //这个数据是存在哪个数据库的字段~微服务，一个服务对应一个数据库，同一个信息可能存在不同的数据库

    private String db_sourse;

    public Dept(String dname){
        this.dname = dname;
    }
    /*
    * 链式写法
    * Dept dept = new Dept();
    * dept.setDeptno(11).setDname("22").setDb_sourse("33");
    * */
}
```

9. 在父项目中新建一个maven项目，取名springcloud-provider-dept-8001
10. 导入依赖（springcloud-provider-dept-8001下的pom.xml）

```xml
<dependencies>
    <!--首先我们要拿到实体类，所以要配置api moudle-->
    <dependency>
        <groupId>org.example</groupId>
        <artifactId>springcloud-api</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
    <!--junit-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
    </dependency>
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-core</artifactId>
    </dependency>
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-test</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!--jetty-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
    <!--热部署工具-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
    </dependency>
</dependencies>
```

11. 在resources下新建application.yml（springcloud-provider-dept-8001）

```yml
server:
  port: 8001
#mybatis的配置
mybatis:
  type-aliases-package: com.example.springcloud.pojo
  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml
#spring的配置
spring:
  application:
    name: springcloud-provider-dept
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db01?userUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
    username: root
    password: 123456
```

12. 新建mybatis-config.xml（springcloud-provider-dept-8001的resource.mybatis）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybtis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!--开启缓存-->
        <setting name="cacheEnabled" value="true"/>
    </settings>
</configuration>
```

13. 新建DeptDao类（springcloud-provider-dept-8001下com.example.springcloud.dao）

```java
@Mapper
@Repository
public interface DeptDao {
    public boolean addDept(Dept dept);
    public Dept queryById(Long id);
    public List<Dept> queryAll();
}
```

14. 新建DeptMapper.xml（springcloud-provider-dept-8001下resources.mybatis.mapper）

````xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybtis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.springcloud.dao.DeptDao">
    <insert id="addDept" parameterType="Dept">
        insert into dept(dname,db_sourse) values(#{dname},DATABASE())
    </insert>
    <select id="queryById" resultType="Dept" parameterType="Long">
        select * from dept where deptno = #{deptno};
    </select>
    <select id="queryAll" resultType="Dept">
        select * from dept;
    </select>
</mapper>
````

15. 新建DeptService和DeptServiceImpl（springcloud-provider-dept-8001下com.example.springcloud.service）

```java
public interface DeptService {
    public boolean addDept(Dept dept);
    public Dept queryById(Long id);
    public List<Dept> queryAll();
}
```

```java
@Service
public class DeptServiceImpl implements DeptService {
    @Autowired
    private DeptDao deptDao;

    @Override
    public boolean addDept(Dept dept) {
        return deptDao.addDept(dept);
    }

    @Override
    public Dept queryById(Long id) {
        return deptDao.queryById(id);
    }

    @Override
    public List<Dept> queryAll() {
        return deptDao.queryAll();
    }
}
```

16. 新建DeptController（springcloud-provider-dept-8001下com.example.springcloud.controller）

```java
@RestController
public class DeptController {
    @Autowired
    private DeptService deptService;

    @PostMapping("/dept/add")
    public boolean addDept(@RequestBody Dept dept){
        return deptService.addDept(dept);
    }
    @GetMapping("/dept/get/{id}")
    public Dept queryById(@PathVariable("id") Long id){

        return deptService.queryById(id);
    }
    @GetMapping("/dept/list")
    public List<Dept> queryAll(){
        return deptService.queryAll();
    }
}
```

17. 编写启动类（springcloud-provider-dept-8001下com.example.springcloud）

```java
//启动类
@SpringBootApplication
public class DeptProvider_8001 {
    public static void main(String[] args) {
        SpringApplication.run(DeptProvider_8001.class,args);
    }
}
```

18. 运行springcloud-provider-dept-8001项目，访问http://localhost:8001/dept/list，出现以下内容则成功

> ```
> [{"deptno":1,"dname":"开发部","db_sourse":"db01"},{"deptno":2,"dname":"人事部","db_sourse":"db01"},{"deptno":3,"dname":"财务部","db_sourse":"db01"},{"deptno":4,"dname":"市场部","db_sourse":"db01"},{"deptno":5,"dname":"运维部","db_sourse":"db01"},{"deptno":6,"dname":"游戏部","db_sourse":"db01"}]
> ```

18. 在父项目中新建maven项目，取名为springcloud-consumer-dept-80

19. 导入依赖（springcloud-consumer-dept-80下pom.xml）

```xml
<!--实体类+web-->
<dependencies>
    <dependency>
        <groupId>org.example</groupId>
        <artifactId>springcloud-api</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
    </dependency>
</dependencies>
```

20. 新建application.yml（springcloud-consumer-dept-80下resources）

```yml
server:
  port: 80
```

21. 新建一个ConfigBean（springcloud-consumer-dept-80下com.example.springcloud.config）

```java
@Configuration
public class ConfigBean {//Configuration --spring applicationContext.xml

    @Bean
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```

22. 新建DeptConsumerController（springcloud-consumer-dept-80下com.example.springcloud.controller）

```java
@RestController
public class DeptConsumerController {
    //消费者不应该有service
    //RestTemplate...供我们直接调用就可以了！注册到spring中
    //(url,实体:Map,Class<T> responseType)
    @Autowired
    private RestTemplate restTemplate; //提供多种便捷访问远程http服务的方法，简单的restful模板

    private static final String REST_URL_PREFIX = "http://localhost:8001";

    @RequestMapping("/consumer/dept/get/{id}")
    public Dept get(@PathVariable("id") Long id){
        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/get/"+id,Dept.class);
    }

    @RequestMapping("/consumer/dept/add")
    public boolean add(Dept dept){
        return restTemplate.postForObject(REST_URL_PREFIX+"/dept/add",dept,Boolean.class);
    }

    @RequestMapping("/consumer/dept/list")
    public List<Dept> list(){
        return restTemplate.getForObject(REST_URL_PREFIX+"/dept/list",List.class);
    }
}
```

22. 新建一个启动类(springcloud-consumer-dept-80下com.example.springcloud)

```java
@SpringBootApplication
public class DeptConsumer_80 {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer_80.class,args);
    }
}
```

23. 启动springcloud-provider-dept-8001和springcloud-consumer-dept-80，访问http://localhost/consumer/dept/list

> [{"deptno":1,"dname":"开发部","db_sourse":"db01"},{"deptno":2,"dname":"人事部","db_sourse":"db01"},{"deptno":3,"dname":"财务部","db_sourse":"db01"},{"deptno":4,"dname":"市场部","db_sourse":"db01"},{"deptno":5,"dname":"运维部","db_sourse":"db01"},{"deptno":6,"dname":"游戏部","db_sourse":"db01"}]

# Eureka服务注册与发现

## 什么是Eureka

> Eureka是Netflix的一个子模块，也是核心模块之一。Eureka是一个基于REST的服务，用于定位服务，以实现远端中间层服务发现和故障转移，服务注册与发现对于微服务来说是非常重要的，有了服务发现与注册，只需要使用服务的标识符，就可以访问到服务，而不需要修改服务调用的配置文件了，功能类似于Dubbo的注册中心，比如Zookeeper。

## 操作步骤

通用步骤

> 1. 导入依赖
> 2. 编写配置文件
> 3. 开启这个功能@EnableXXXX
> 4. 配置类

1. 在父项目新建maven项目，取名为springcloud-eureka-7001，导入依赖

```xml
<!--导包-->
<dependencies>
    <!--Eureka-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-eureka-server</artifactId>
        <version>1.4.6.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
    </dependency>
</dependencies>
```

2. 新建application.yml（springcloud-eureka-7001下resources）

```yml
server:
  port: 7001
#Eureka配置
eureka:
  instance:
    hostname: localhost #Eureka服务端的实例名称
  client:
    register-with-eureka: false #表示是否向eureka注册中心注册自己
    fetch-registry: false #如果fetch-registry为false，则表示自己为注册中心
    service-url: #监控页面
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

3. 新建启动类（springcloud-eureka-7001下com.example.springcloud）

```java
@SpringBootApplication
@EnableEurekaServer//EnableEurekaService服务端的启动类，可以接受别人注册进来
public class EurekaService_7001 {
    public static void main(String[] args) {
        SpringApplication.run(EurekaService_7001.class,args);
    }
}
```

4. 启动springcloud-eureka-7001项目，访问http://localhost:7001/，出现eureka页面则成功

![eureka](/assets/SpringCloud.assets/eureka.png)

5. 导入依赖（springcloud-provider-dept-8001下的pom.xml）

```xml
<dependencies>
    <!--Eureka-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-eureka</artifactId>
        <version>1.4.6.RELEASE</version>
    </dependency>
    ...
</dependencies>
```

6. 在8001的application.yml配置eureka（springcloud-provider-dept-8001）

```yml
...
#Eureka的配置，服务注册到哪里
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka/
  instance:
    instance-id: springcloud-provider-dept-8001 #修改eureka上的默认描述信息
```

7. 在启动类开启eureka功能（springcloud-provider-dept-8001下com.example.springcloud）

```java
@SpringBootApplication
@EnableEurekaClient//在服务启动后自动注册到eureka中！
public class DeptProvider_8001 {
  ...
}
```

8. 启动springcloud-eureka-7001和springcloud-provider-dept-8001项目，访问http://localhost:7001/

![eureka-2](/assets/SpringCloud.assets/eureka-2-1608973085273.png)

9. 关闭springcloud-provider-dept-8001项目，等待几分钟。点击下面绿色的springcloud-provider-dept-8001链接，将不能访问

![eureka-3](/assets/SpringCloud.assets/eureka-3.png)

10. 添加依赖，完善监控信息（springcloud-provider-dept-8001下的pom.xml）

```xml
...
<!--actuator完善监控信息-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
...
```

11. 在application.yml添加info配置（springcloud-provider-dept-8001下的resources）

```yml
#info配置
info:
  app.name: example-springcloud
  company.name: blog.example.com
```

12. 启动springcloud-eureka-7001和springcloud-provider-dept-8001项目，访问http://localhost:7001/，点击springcloud-provider-dept-8001链接，出现

> {"app":{"name":"example-springcloud"},"company":{"name":"blog.example.com"}}

13. 上面出现红色的一段话

> 自我保护机制
>
> 某时刻某一个微服务不可以用了，eureka不会立即清理，依旧对该微服务的信息进行保存

14. 获取注册进来的微服务信息DeptController（springcloud-provider-dept-8001下com.example.springcloud.controller）

```java
@RestController
public class DeptController {
    @Autowired
    private DeptService deptService;

    //获取一些配置信息,得到具体的微服务
    @Autowired
    private DiscoveryClient discoveryClient;

	...
    
    //注册进来的微服务~，获取一些消息
    @GetMapping("/dept/discovery")
    public Object discovery(){
        //获取微服务列表的清单
        List<String> services = discoveryClient.getServices();
        System.out.println("discovery=>services:"+services);
        //得到具体的微服务信息,通过具体的微服务id
        List<ServiceInstance> instances = discoveryClient.getInstances("SPRINGCLOUD-PROVIDER-DEPT");
        for (ServiceInstance instance : instances
             ) {
            System.out.println(instance.getHost()+"\t"+instance.getPort()+"\t"+instance.getUri()+"\t"+instance.getServiceId());
        }
        return this.discoveryClient;
    }
}
```

15. 在启动类配置服务发现（springcloud-provider-dept-8001下com.example.springcloud）

```java
@SpringBootApplication
@EnableEurekaClient//在服务启动后自动注册到eureka中！
@EnableDiscoveryClient//服务发现
public class DeptProvider_8001 {
    public static void main(String[] args) {
        SpringApplication.run(DeptProvider_8001.class,args);
    }
}
```

16. 启动springcloud-eureka-7001和springcloud-provider-dept-8001项目，访问http://localhost:8001/dept/discovery

> 控制台输出
> discovery=>services:[springcloud-provider-dept]
> localhost	8001	http://localhost:8001	SPRINGCLOUD-PROVIDER-DEPT
>
> 浏览器输出
> {"discoveryClients":[{"services":["springcloud-provider-dept"],"order":0},{"services":[],"order":0}],"services":["springcloud-provider-dept"],"order":0}

## 搭建集群

1. 在父项目新建两个maven项目，分别为springcloud-eureka-7002和springcloud-eureka-7003，并导入依赖（跟7001一样）
2. 更改springcloud-eureka-700的application.yml

```yml
server:
  port: 7001
#Eureka配置
eureka:
  instance:
    hostname: eureka7001.com #Eureka服务端的实例名称
  client:
    register-with-eureka: false #表示是否向eureka注册中心注册自己
    fetch-registry: false #如果fetch-registry为false，则表示自己为注册中心
    service-url: #监控页面
      #集群（关联）：
      defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

3. 编写application.yml（springcloud-eureka-7002和springcloud-eureka-7003）

```yml
server:
  port: 7002
#Eureka配置
eureka:
  instance:
    hostname: eureka7002.com #Eureka服务端的实例名称
  client:
    register-with-eureka: false #表示是否向eureka注册中心注册自己
    fetch-registry: false #如果fetch-registry为false，则表示自己为注册中心
    service-url: #监控页面
      defaultZone: http://eureka7003.com:7003/eureka/, http://eureka7001.com:7001/eureka/
```

```yml
server:
  port: 7003
#Eureka配置
eureka:
  instance:
    hostname: eureka7003.com
  client:
    register-with-eureka: false #表示是否向eureka注册中心注册自己
    fetch-registry: false #如果fetch-registry为false，则表示自己为注册中心
    service-url: #监控页面
      defaultZone: http://eureka7001.com:7001/eureka/, http://eureka7002.com:7002/eureka/
```

4. 编写host文件（C:\Windows\System32\drivers\etc）

> ...
>
> 127.0.0.1	eureka7001.com
> 127.0.0.1	eureka7002.com
> 127.0.0.1	eureka7003.com

5. 编写启动类springcloud-eureka-7002和springcloud-eureka-7003，与springcloud-eureka-7001一样
6. 修改springcloud-provider-dept-8001的application.yml

```yml
...
#Eureka的配置，服务注册到哪里
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
  instance:
    instance-id: springcloud-provider-dept-8001 #修改eureka上的默认描述信息
...
```

7. 启动springcloud-eureka-7001，springcloud-eureka-7002，springcloud-eureka-7003，springcloud-provider-dept-8001

![eureka-4](/assets/SpringCloud.assets/eureka-4.png)

## 对比Zookeeper

> RDBMS(Mysql,Oracle,sqlServer) ==> ACID
>
> NoSQL(redis,mongodb) ==> CAP

### ACID是什么

> A(Atomicity)	原子性
>
> C(Consistency)	一致性
>
> I(Isolation)	隔离性
>
> D(Durability)	持久性

### CAP是什么

>C(Consistency)	强一致性
>
>A(Availability)	可用性
>
>p(Parition tolerance)	分区容错性

### CAP的三进二

> CA、AP、CP

### CAP理论的核心

> 一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求
>
> 根据CAP原理，将NoSQL数据库分成了满足了CA原则，满足CP原则和满足AP原则三大类：
>
> * CA：单点集群，满足一致性，可用性的系统，通常可扩展性较差
> * CP：满足一致性，分区容错性的系统，通常性能不是特别高
> * AP：满足可用性，分区容错性的系统，通常可能对一致性要求低一些

### 作为注册中心，Eureka比Zookeeper好在哪里

> 由于分区容错性P在分布式系统中是必须保证的，因此只能在A和C之间权衡
>
> Zookeeper保证的是CP
>
> Eureka保证的是AP
>
> Eureka可以很好的应对因网络故障导致部分节点失去联系的情况，而不会像Zookeeper那样使整个注册服务瘫痪

# Ribbon

## Ribbon是什么

> Springcloud Ribbon是基于Netflix Ribbon实现的一套客户端负载均衡的工具
>
> Ribbon是Netflix发布的云中间层服务开源项目，其主要功能是提供客户端实现负载均衡算法。Ribbon客户端组件提供一系列完善的配置项如连接超时，重试等。简单的说，Ribbon是一个客户端负载均衡器，我们可以在配置文件中Load Balancer后面的所有机器，Ribbon会自动的帮助你基于某种规则（如简单轮询，随机连接等）去连接这些机器，我们也很容易使用Ribbon实现自定义的负载均衡算法。

## Ribbon能干啥

> LB，即负载均衡（Load Balance），在微服务或分布式集群中经常用的一种应用
>
> 负载均衡简单的说就是将用户的请求平摊的分配到多个服务器上，从而达到系统的HA（高可用）
>
> 常见的负载均衡软件有Nginx，Lvs等‘
>
> dubbon、Springcloud中均给我们提供了负载均衡，SpringCloud的负载均衡算法可以自定义
>
> 负载均衡简单分类
>
> * 集中式LB
>   * 即在服务的消费方和提供方之间使用独立的LB设施，如Nginx：反向代理服务器，有该设施负责把访问请求通过某种策略转发至服务的提供方
> * 进程式LB
>   * 将LB逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选出一个合适的服务器
>   * Ribbon须臾进程内LB，他只是一个类库，继承于消费方进程，消费方通过它来获取到服务提供方的地址

## 基本操作

1. 导入依赖（springcloud-consumer-dept-80）

```xml
<!--Ribbon-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-ribbon</artifactId>
    <version>1.4.6.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
    <version>1.4.6.RELEASE</version>
</dependency>
...
```

2. 配置application.yml（springcloud-consumer-dept-80）

```yml
#Eureka配置
eureka:
  client:
    register-with-eureka: false #不想eureka中心注册自己
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

3. 在启动类开启eureka（springcloud-consumer-dept-80）

```java
@SpringBootApplication
@EnableEurekaClient
public class DeptConsumer_80 {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer_80.class,args);
    }
}
```

4. 配置负载均衡ConfigBean（springcloud-consumer-dept-80下com.example.springcloud.config）

```java
@Configuration
public class ConfigBean {//Configuration --spring applicationContext.xml

    //配置负载均衡实现RestTemplate
    @Bean
    @LoadBalanced //Ribbon
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }
}
```

5. 更改DeptConsumerController（springcloud-consumer-dept-80下com.example.springcloud.controller）

```java
@RestController
public class DeptConsumerController {
    //消费者不应该有service
    //RestTemplate...供我们直接调用就可以了！注册到spring中
    //(url,实体:Map,Class<T> responseType)
    @Autowired
    private RestTemplate restTemplate; //提供多种便捷访问远程http服务的方法，简单的restful模板

    //Ribbon 我们这里的地址，应该是一个变量，通过服务名来访问
    private static final String REST_URL_PREFIX = "http://SPRINGCLOUD-PROVIDER-DEPT";

    ...
}
```

6. 启动springcloud-eureka-7001，springcloud-eureka-7002，springcloud-provider-dept-8001，springcloud-consumer-dept-80，访问http://localhost/consumer/dept/list（Ribbon和Eureka整合之后，客户端可以直接调用，不用关心IP地址和端口号）

> [{"deptno":1,"dname":"开发部","db_sourse":"db01"},{"deptno":2,"dname":"人事部","db_sourse":"db01"},{"deptno":3,"dname":"财务部","db_sourse":"db01"},{"deptno":4,"dname":"市场部","db_sourse":"db01"},{"deptno":5,"dname":"运维部","db_sourse":"db01"},{"deptno":6,"dname":"游戏部","db_sourse":"db01"}]

## 实现负载均衡

1. 新建两个数据库db02，db03，内容与db01一样。db_sourse改为db02和db03
2. 新建两个maven项目，springcloud-provider-dept-8002，springcloud-provider-dept-8003

3. 导入依赖springcloud-provider-dept-8002，springcloud-provider-dept-8003与springcloud-provider-dept-8001一致

4. 将springcloud-provider-dept-8001的application.yml复制到springcloud-provider-dept-8002，springcloud-provider-dept-8003

> 数据库db01改为db02和db03，把有8001的地方改为8002和8003

5. 将springcloud-provider-dept-8001的mybatis包复制到springcloud-provider-dept-8002，springcloud-provider-dept-8003

6. 将springcloud-provider-dept-8001的java包复制到springcloud-provider-dept-8002，springcloud-provider-dept-8003

> 将启动类DeptProvider_8001改为DeptProvider_8002，DeptProvider_8003

7. 启动springcloud-eureka-7001，springcloud-provider-dept-8001，springcloud-provider-dept-8002，springcloud-provider-dept-8003，springcloud-consumer-dept-80，访问http://localhost:7001/

![ribbon](/assets/SpringCloud.assets/ribbon.png)

> 访问http://localhost/consumer/dept/list，一直刷新
>
> [{"deptno":1,"dname":"开发部","db_sourse":"db01"},{"deptno":2,"dname":"人事部","db_sourse":"db01"},{"deptno":3,"dname":"财务部","db_sourse":"db01"},{"deptno":4,"dname":"市场部","db_sourse":"db01"},{"deptno":5,"dname":"运维部","db_sourse":"db01"},{"deptno":6,"dname":"游戏部","db_sourse":"db01"}]
>
> [{"deptno":1,"dname":"开发部","db_sourse":"db02"},{"deptno":2,"dname":"人事部","db_sourse":"db02"},{"deptno":3,"dname":"财务部","db_sourse":"db02"},{"deptno":4,"dname":"市场部","db_sourse":"db02"},{"deptno":5,"dname":"运维部","db_sourse":"db02"},{"deptno":6,"dname":"游戏部","db_sourse":"db02"}]
>
> [{"deptno":1,"dname":"开发部","db_sourse":"db03"},{"deptno":2,"dname":"人事部","db_sourse":"db03"},{"deptno":3,"dname":"财务部","db_sourse":"db03"},{"deptno":4,"dname":"市场部","db_sourse":"db03"},{"deptno":5,"dname":"运维部","db_sourse":"db03"},{"deptno":6,"dname":"游戏部","db_sourse":"db03"}]
>
> 会在三个数据库循环查出数据

## 自定义负载均衡算法

1. ConfigBean配置（springcloud-consumer-dept-80下com.example.springcloud.config）

```java
@Configuration
public class ConfigBean {//Configuration --spring applicationContext.xml

    //配置负载均衡实现RestTemplate
    //IRule
    //RoundRobinRule 轮询
    //RandomRule 随机
    //AvailabilityFilteringRule：会先过滤掉，跳闸，访问故障的服务，对剩下的进行轮询
    //RetryRule：会先按照轮询获取服务，如果服务获取失败，则会在指定的时间内进行重试
    @Bean
    @LoadBalanced //Ribbon
    public RestTemplate getRestTemplate(){
        return new RestTemplate();
    }

    @Bean
    public IRule myIRule(){
        return new RandomRule();
    }
}
```

2. 启动springcloud-eureka-7001，springcloud-provider-dept-8001，springcloud-provider-dept-8002，springcloud-provider-dept-8003，springcloud-consumer-dept-80，访问http://localhost/consumer/dept/list，上面那三种数据会随机出现

3. 新建MyRule类（springcloud-consumer-dept-80下com.exampl.myrule）

```java
@Configuration
public class MyRule {
    @Bean
    public IRule myIRule(){
        return new RandomRule();
    }
}
```

4. 将第一步中myIRule()注释掉，在启动类加载自定义Ribbon类（springcloud-consumer-dept-80下com.example.springcloud）

```java
//Ribbon和Eureka整合之后，客户端可以直接调用，不用关心IP地址和端口号
@SpringBootApplication
@EnableEurekaClient
//在微服务启动时就能去加载我们自定义的Ribbon类
@RibbonClient(name = "SPRINGCLOUD-PROVIDER-DEPT",configuration = MyRule.class)
public class DeptConsumer_80 {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumer_80.class,args);
    }
}
```

5. 执行第二步操作，运行结果一样
6. 新建自定义轮询MyRandomRule类（springcloud-consumer-dept-80下com.exampl.myrule）

```java
public class MyRandomRule extends AbstractLoadBalancerRule {

    //自定义获取服务
    //每个服务，访问5此，换下一个服务(3个)
    //total = 0，默认=0，如果=5，指向下一个服务节点
    //index = 0，默认=0，如果total=5，index+1

    private int total = 0;//被调用的次数
    private int currentIndex = 0;//当前是谁在提供服务

    @SuppressWarnings({"RCN_REDUNDANT_NULLCHECK_OF_NULL_VALUE"})
    public Server choose(ILoadBalancer lb, Object key) {
        if (lb == null) {
            return null;
        } else {
            Server server = null;

            while(server == null) {
                if (Thread.interrupted()) {
                    return null;
                }

                List<Server> upList = lb.getReachableServers();//获取活着的服务
                List<Server> allList = lb.getAllServers();//获取全部的服务
                int serverCount = allList.size();
                if (serverCount == 0) {
                    return null;
                }

                //                int index = this.chooseRandomInt(serverCount);//生成区间随机数
                //                server = (Server)upList.get(index);//从活着的服务中，随机选择一个

                //----------------自定义-----------------
                if(total < 5){
                    server = upList.get(currentIndex);
                    total++;
                }else {
                    total = 0;
                    currentIndex++;
                    if (currentIndex > upList.size()-1){
                        currentIndex = 0;
                    }
                    server = upList.get(currentIndex);//从活着的服务中，获取指定的服务来进行操作
                }
                //--------------------------------------

                if (server == null) {
                    Thread.yield();
                } else {
                    if (server.isAlive()) {
                        return server;
                    }

                    server = null;
                    Thread.yield();
                }
            }

            return server;
        }
    }

    protected int chooseRandomInt(int serverCount) {
        return ThreadLocalRandom.current().nextInt(serverCount);
    }

    public Server choose(Object key) {
        return this.choose(this.getLoadBalancer(), key);
    }

    public void initWithNiwsConfig(IClientConfig clientConfig) {
    }
}
```

7. 修改MyRule类（springcloud-consumer-dept-80下com.exampl.myrule）

```java
@Configuration
public class MyRule {
   @Bean
   public IRule myIRule(){
       return new MyRandomRule();//默认是轮询，现在我们定义为MyRandomRule
   }
}
```

8. 执行第二步操作，一直刷新，出现的结果是每一组数据出现5次，换成下一组数据出现5次

# Feign负载均衡

## 简介

> feign是声明式的web service客户端，他让微服务之间的调用变得简单了，类似controller调用service。SpringCloud继承了Ribbon和Eureka，可用使用feign时提供负载均衡的http客户端
>
> 只需要创建一个接口，然后添加注解即可
>
> fegin，主要是社区，搭建都习惯面向接口编程。这是很多开发人员的规范。调用微服务访问两种方式
>
> 1. 微服务名字（ribbon）
> 2. 接口和注解（fegin）

## Fegin能干什么

> Feign旨在使编写Java Http客户端变得更加容易
>
> 前面在使用Ribbon+RestTemplate时，利用RestTemplate对Http请求的封装处理，形成了一套模板化的调用方法。。但是在实际开发中，由于对服务依赖的调用可能不止一处，往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用。所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义，在Feign的实现下，**我们只需要创建一个接口并使用注解的方式来配置它（类似于以前Dao接口上标注Mapper注解，现在是一个微服务接口上面标注一个Feign注解即可。）**即可完成对服务提供方的接口绑定，简化了使用Spring Cloud Ribbon时，自动封装服务调用客户端的开发量。

## Feign集成了Ribbon

> 利用Ribbon维护了MicroServiceCloud-Dept的服务列表信息，并且通过轮询实现了客户端的负载均衡，而Ribbon不同的是，通过Feign只需要定义服务绑定接口且以声明式的方法，优雅而且简单的实现了服务调用。

基本使用

1. 新建一个maven项目，springcloud-consumer-dept-feign
2. 导入依赖，与springcloud-consumer-dept-80一致，新增feign依赖

```xml
<!--feign-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-feign</artifactId>
    <version>1.4.6.RELEASE</version>
</dependency>
...
```

3. 将springcloud-consumer-dept-80下com.example.springcloud包下的所有内容和application.yml复制到springcloud-consumer-dept-feign中

4. 在spingcloud-api的pom.xml中导入feign依赖
5. 新建DeptClientService（spingcloud-api下com.example.springcloud.service）

```java
@Component
@FeignClient(name = "SPRINGCLOUD-PROVIDER-DEPT",fallbackFactory = DeptCilentServiceFallbackFactory.class)
public interface DeptClientService {

    @GetMapping("/dept/get/{id}")
    public Dept queryById(@PathVariable("id") Long id);

    @PostMapping("/dept/add")
    public boolean addDept(Dept dept);

    @GetMapping("/dept/list")
    public List<Dept> queryAll();

}
```

6. 修改DeptConsumerController类（springcloud-consumer-dept-feign下com.example.springcloud.controller）

```java
@RestController
public class DeptConsumerController {

    @Autowired
    private DeptClientService service;

    @RequestMapping("/consumer/dept/get/{id}")
    public Dept get(@PathVariable("id") Long id){
        return this.service.queryById(id);
    }

    @RequestMapping("/consumer/dept/add")
    public boolean add(Dept dept){
        return this.service.addDept(dept);
    }

    @RequestMapping("/consumer/dept/list")
    public List<Dept> list(){
        return this.service.queryAll();
    }
}
```

7. 更改启动类名称FeignDeptConsumer_80，修改内容（springcloud-consumer-dept-feign下com.example.springcloud）

```java
//Ribbon和Eureka整合之后，客户端可以直接调用，不用关心IP地址和端口号
@SpringBootApplication
@EnableEurekaClient
@EnableFeignClients(basePackages = {"com.example.springcloud"})
@ComponentScan("com.example.springcloud")
public class FeignDeptConsumer_80 {
    public static void main(String[] args) {
        SpringApplication.run(FeignDeptConsumer_80.class,args);
    }
}s
```

8. 运行springcloud-eureka-7001，springcloud-provider-dept-8001，springcloud-provider-dept-8002，springcloud-consumer-dept-feign，访问http://localhost/consumer/dept/list，默认的循环方式，db01、db02各出现一次

# Hystrix

## 服务雪崩

> 多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C，微服务B和微服务C又调用其他的微服务，这就是所谓的“扇出"、如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“雪崩效应"。
>
> 对于高流量的应用来说，单一的后端依赖可能会导致所有服务器上的所有资源都在几秒中内饱和。比失败更糟糕的是，这些应用程序还可能导致服务之间的延迟增加，备份队列，线程和其他系统资源紧张，导致整个系统发生更多的级联故障，这些都表示需要对故障和延迟进行隔离和管理，以便单个依赖关系的失败，不能取消整个应用程序或系统。

## 什么是Hystrix

> Hystrix是一个用于处理分布式系统的延迟和容错的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时，异常等，Hystrix能够保证在一个依赖出问题的情况下，不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性。
>
> “断路器”"本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝)，**向调用方返回一个服务预期的，可处理的备选响应(FallBack)，而不是长时间的等待或者抛出调用方法无法处理的异常，这样就可以保证了服务调用方的线程不会被长时间**，不必要的占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩

## Hystrix能干啥

> * 服务降级
> * 服务熔断
> * 服务限流
> * 接近实时的监控
> * ......

## 服务熔断是什么

> 熔断机制是对应雪崩效应的一种微服务链路保护机制。
>
> 当扇出链路的某个微服务不可用或者响应时间太长时，会进行服务的降级，**进而熔断该节点微服务的调用，快速返回错误的响应信息。**当检测到该节点微服务调用响应正常后恢复调用链路。在SpringCloud框架里熔断机制通过Hystrix实现。Hystrix会监控微服务间调用的状况，当失败的调用到一定阈值，缺省是5秒内20次调用失败就会启动熔断机制。熔断机制的注解是@HystrixCommand。

## 基本使用

1. 新建maven项目，springcloud-provider-dept-hystrix-8001
2. 导入依赖，将springcloud-provider-dept-8001的依赖复制到springcloud-provider-dept-hystrix-8001
3. 将springcloud-provider-dept-8001的application.yml和mybatis复制到springcloud-provider-dept-hystrix-8001中
4. 将springcloud-provider-dept-8001的java复制到springcloud-provider-dept-hystrix-8001中

5. 将springcloud-provider-dept-hystrix-8001中启动类名字改为DeptProviderHystrix_8001
6. 新增依赖（springcloud-provider-dept-hystrix-8001的pom.xml）

```xml
<!--hystrix-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-hystrix</artifactId>
    <version>1.4.6.RELEASE</version>
</dependency>
...
```

7. 将application.yml中instance-id改为springcloud-provider-dept-hystrix-8001（springcloud-provider-dept-hystrix-8001）

8. 删除DeptController的内容，并更改为以下内容（springcloud-provider-dept-hystrix-8001下com.example.springcloud.controller）

```java
@RestController
public class DeptController {

    @Autowired
    private DeptService deptService;

    @GetMapping("/dept/get/{id}")
    @HystrixCommand(fallbackMethod = "hystrixGet")
    public Dept get(@PathVariable("id") Long id){
        Dept dept = deptService.queryById(id);
        if (dept == null){
            throw new RuntimeException("id=>"+id+",不存在该用户，或者信息无法找到");
        }
        return dept;
    }

    //备选方案
    public Dept hystrixGet(Long id){
        return new Dept().setDeptno(id).setDname("id=>"+id+",没有对应的信息，null").setDb_sourse("no this database in MySQL");
    }
}
```

9. 在启动类添加对熔断的支持（springcloud-provider-dept-hystrix-8001下com.example.springcloud）

```java
//启动类
@SpringBootApplication
@EnableEurekaClient//在服务启动后自动注册到eureka中！
@EnableDiscoveryClient//服务发现
@EnableCircuitBreaker//添加对熔断的支持
public class DeptProviderHystrix_8001 {
    public static void main(String[] args) {
        SpringApplication.run(DeptProviderHystrix_8001.class,args);
    }
}
```

10 启动springcloud-eureka-7001，springcloud-eureka-7002，springcloud-provider-dept-hystrix-8001，springcloud-consumer-dept-80，访问http://localhost/consumer/dept/get/1，http://localhost/consumer/dept/get/2，http://localhost/consumer/dept/get/8

> {"deptno":1,"dname":"开发部","db_sourse":"db01"}
>
> {"deptno":2,"dname":"人事部","db_sourse":"db01"}
>
> {"deptno":8,"dname":"id=>8,没有对应的信息，null","db_sourse":"no this database in MySQL"}

## 服务降级

1. 新建DeptCilentServiceFallbackFactory类（springcloud-api下com.example.springcloud.service）

```java
@Component
public class DeptCilentServiceFallbackFactory implements FallbackFactory {
    @Override
    public DeptClientService create(Throwable throwable) {
        return new DeptClientService() {
            @Override
            public Dept queryById(Long id) {
                return new Dept().setDeptno(id).setDname("id=>"+id+"没有对应信息，客户端提供了降级的信息，这个服务现在已经关闭").setDb_sourse("没有数据");
            }

            @Override
            public boolean addDept(Dept dept) {
                return false;
            }

            @Override
            public List<Dept> queryAll() {
                return null;
            }
        };
    }
}
```

2. 修改DeptClientService类内容（springcloud-api下com.example.springcloud.service）

```java
@Component
@FeignClient(name = "SPRINGCLOUD-PROVIDER-DEPT",fallbackFactory = DeptCilentServiceFallbackFactory.class)
public interface DeptClientService {
	...
}
```

3. application.yml开启服务降级（springcloud-consumer-dept-feign）

```yml
...
#开启降级feign.hystrix
feign:
  hystrix:
    enabled: true
...
```

4. 启动springcloud-eureka-7001，springcloud-provider-dept-8001，springcloud-consumer-dept-feign，访问http://localhost/consumer/dept/get/1

> {"deptno":1,"dname":"开发部","db_sourse":"db01"}

5. 关闭springcloud-provider-dept-8001，访问http://localhost/consumer/dept/get/1

> {"deptno":1,"dname":"id=>1没有对应信息，客户端提供了降级的信息，这个服务现在已经关闭","db_sourse":"没有数据"}

6. 总结

> 服务熔断：服务端~   某个服务超时或异常时，引起熔断
> 服务降级：客户端，从整体网站请求负载考虑，当某个服务器熔断或关闭之后，服务将不再被调用
>         此时在客户端，我们可以准备一个FallBackFactory，返回一个默认值(缺省值)，整体的服务水平下降了，但是好歹能用，比直接挂掉强

## DashBoard流监控

1. 新建maven项目，springcloud-consumer-hystrix-dashboard

2. 导入依赖，将springcloud-consumer-dept-80的依赖复制到springcloud-consumer-hystrix-dashboard中，添加Hystrix依赖

> ```xml
> <!--hystrix-->
> <dependency>
>     <groupId>org.springframework.cloud</groupId>
>     <artifactId>spring-cloud-starter-hystrix</artifactId>
>     <version>1.4.6.RELEASE</version>
> </dependency>
> <dependency>
>     <groupId>org.springframework.cloud</groupId>
>     <artifactId>spring-cloud-starter-hystrix-dashboard</artifactId>
>     <version>1.4.6.RELEASE</version>
> </dependency>
> ...
> ```

3. 新建application.yml（springcloud-consumer-hystrix-dashboard）

```yml
server:
  port: 9001
```

4. 新建启动类（springcloud-consumer-hystrix-dashboard下com.example.springcloud）

```java
@SpringBootApplication
@EnableHystrixDashboard//开启
public class DeptConsumerDashboard_9001 {
    public static void main(String[] args) {
        SpringApplication.run(DeptConsumerDashboard_9001.class,args);
    }
}
```

5. 启动springcloud-consumer-hystrix-dashboard项目，访问http://localhost:9001/hystrix

![hystrix_dashboard](/assets/SpringCloud.assets/hystrix_dashboard.png)

6. 修改启动类（springcloud-provider-dept-hystrix-8001）

```java
...
public class DeptProviderHystrix_8001 {
 	...
    //增加一个Servlet
    @Bean
    public ServletRegistrationBean hystrixMetricsStreamServlet(){
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(new HystrixMetricsStreamServlet());
        registrationBean.addUrlMappings("/actuator/hystrix.stream");
        return registrationBean;
    }
}
```

7. 启动启动springcloud-eureka-7001，springcloud-provider-dept-hystrix-8001，springcloud-consumer-hystrix-dashboard，访问http://localhost:8001/dept/get/8，http://localhost:8001/actuator/hystrix.stream会出现ping、data的数据

8. 访问http://localhost:9001/hystrix，点击Monitor Stream

![dashboard](/assets/SpringCloud.assets/dashboard.png)

![dashboard_1](/assets/SpringCloud.assets/dashboard_1-1609132250399.png)

9. 多次访问http://localhost:8001/dept/get/1，http://localhost:8001/dept/get/8查看效果

10. 如何看

* 一圈

  实心圆:公有两种含义，他通过颜色的变化代表了实例的健康程度它的健康程度从绿色<黄色<橙色<红色递减

  该实心圆除了颜色的变化之外，它的大小也会根据实例的请求流量发生变化，流量越大，该实心圆就越大，所以通过该实心圆的展示，就可以在大量的实例中快速发现故障实例和高压力实例。

* 一线

  曲线：用来记录2分钟内流量的相对变化，可用通过他来观察到流量的上升和下降取势

* 整图说明

![dashboard_2](/assets/SpringCloud.assets/dashboard_2.png)

# Zuul路由网关

## 什么是Zuul

> zuul包含了对请求的路由和过滤两个最主要的功能
>
> 其中路由功能负责将外部请求转发到具体的微服务实例上，是实现外部访问统一入口的基础，而过滤器功能则负责对请求的处理过程进行干预，是实现请求校验，服务聚合等功能的基础。Zuul和Eureka进行整合，将Zuul自身注册为Eureka服务治理下的应用，同时从Eureka中获得其他微服务的消息，也即以后的访问微服务都是通过Zuul跳转后获得。
>
> 注意:Zuul服务最终还是会注册进Eureka提供:代理＋路由＋过滤三大功能!

## zuul能干啥

> * 路由
> * 过滤

## 基本使用

1. 新建maven项目，springcloud-zuul-6001
2. 导入依赖，将springcloud-consumer-hystrix-dashboard的依赖复制到springcloud-zuul-6001中，新增zuul依赖

```xml
<!--zuul-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-zuul</artifactId>
    <version>1.4.6.RELEASE</version>
</dependency>
...
```

3. 新建application.yml（springcloud-zuul-6001）

```yml
server:
  port: 6001
spring:
  application:
    name: apringcloud-zuul

eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
  instance:
    instance-id: zuul6001.com
    prefer-ip-address: true
info:
  app.name: example-springcloud
  company.name: blog.example.com
```

4. 新建启动类（springcloud-zuul-6001下com.example.springcloud）

```java
@SpringBootApplication
@EnableZuulProxy
public class ZuulApplication_6001 {
    public static void main(String[] args) {
        SpringApplication.run(ZuulApplication_6001.class,args);
    }
}
```

5. 启动springcloud-eureka-7001，springcloud-provider-dept-hystrix-8001，springcloud-zuul-6001,访问http://localhost:8001/dept/get/1，http://localhost:6001/springcloud-provider-dept/dept/get/1结果一致

6. 由于springcloud-provider-dept微服务名称会被暴露出来，所有需要进行配置，修改application.yml（springcloud-zuul-6001）

```yml
...
zuul:
  routes:
    mydept.serviceId: springcloud-provider-dept
    mydept.path: /mydept/**
```

7. 启动springcloud-eureka-7001，springcloud-provider-dept-hystrix-8001，springcloud-zuul-6001,访问，http://localhost:6001/mydept/dept/get/1，但http://localhost:6001/springcloud-provider-dept/dept/get/1也能访问

8. 不能使带springcloud-provider-dept的能访问，配置application.yml（springcloud-zuul-6001）

```yml
zuul:
  routes:
    mydept.serviceId: springcloud-provider-dept
    mydept.path: /mydept/**
  ignored-services: springcloud-provider-dept #不能在使用这个路径访问  *：隐藏全部
  prefix: /example #设置公共的前缀
```

9. 启动springcloud-eureka-7001，springcloud-provider-dept-hystrix-8001，springcloud-zuul-6001,访问http://localhost:6001/example/mydept/dept/get/1成功，不能访问http://localhost:6001/example/springcloud-provider-dept/dept/get/1

# config分布式配置

## 分布式系统面临的--配置文件的问题

> 微服务意味着要将单体应用中的业务拆分成一个个子服务，每个服务的粒度相对较小，因此系统中会出现大量的服务，由于每个服务都需要必要的配置信息才能运行，所以一套集中式的，动态的配置管理设施是必不可少的。SpringCloud提供了ConfigServer来解决这个问题，我们每一个微服务自己带着一个application.yml，那上百的的配置文件要修改起来，岂不是要发疯!

## 什么是SpringCloud config分布式配置中心

> Spring Cloud Config为微服务架构中的微服务提供集中化的外部配置支持,配置服务器为各个不同微服务应用的所有环节提供了一个中心化的外部配置。
>
> Spring Cloud Config 分为**服务端**和**客户端**两部分。
>
> 服务端也称为分布式配置中心，它是一个独立的微服务应用，用来连接配置服务器并为客户端提供获取配置信息，加密，解密信息等访问接口。
>
> 客户端则是通过指定的配置中心来管理应用资源，以及与业务相关的配置内容，并在启动的时候从配置中心获取和加载配置信息。配置服务器默认采用git来存储配置信息，这样就有助于对环境配置进行版本管理。并且可以通过git客户端工具来方便的管理和访问配置内容。

## SpringCloud config分布式配置中心能干嘛

> * 集中管理配置文件
> * 不同环境，不同配置，动态化的配置更新，分环境部署，比如/dev /test/ /prod /beta /release —
> * 运行期间动态调整配置，不再需要在每个服务部署的机器上编写配置文件，服务会向配置中心统一拉取配置自己的信息。
> * 当配置发生变动时，服务不需要重启，即可感知到配置的变化，并应用新的配置
> * 将配置信息以REST接口的形式暴露

## SpringCloud config分布式配置中心与github整合

> 由于Spring Cloud Config默认使用Git来存储配置文件（也有其他方式，比如支持SVN和本地文件)，但是最推荐的还是Git,而且使用的是http / https访问的形式;

## 基本使用

1. 在github上新建git仓库springcloud-config
2. 在本地新建一个文件拉取github的springcloud-config文件

> git pull git地址

3. 在拉取下来的springcloud-config文件中新建application.yml，并添加内容

```yml
spring:
  profiles:
    active:

---
spring:
  profiles: dev
  application:
    name: springcloud-config-dev

---
spring:
  profiles: test
  application:
   name: springcloud-config-test
```

4. 提交

> git add *
>
> git commit -m "first"
>
> git push

5. 去github上查看提交的文件
6. 新建maven项目，springcloud-config-server-3344并导入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-config-server</artifactId>
    <version>2.0.0.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-eureka</artifactId>
    <version>1.4.6.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

7. 新建application.yml（springcloud-config-server-3344）

```yml
server:
  port: 3344
spring:
  application:
    name: spring-config-server
  cloud:
    config:
      server:
        git:
          uri: git地址
          username: xxx
          password: xxx
          default-label: main
```

8. 新建启动类（springcloud-config-server-3344下com.example.springcloud）

```java
@SpringBootApplication
@EnableConfigServer
public class Config_Server_3344 {
    public static void main(String[] args) {
        SpringApplication.run(Config_Server_3344.class,args);
    }
}
```

9. 启动springcloud-config-server-3344，访问http://localhost:3344/application-dev.yml或http://localhost:3344/application-test.yml查看配置

> spring:
>   application:
>     name: springcloud-config-dev
>   profiles:
>     active: ''

10. 在拉取下来的springcloud-config文件中新建config-client.yml，并添加内容

```yml
spring:
  profiles:
    active: dev

---
#spring的配置
server:
  port: 8201
spring:
  profiles: dev
  application:
    name: springcloud-provider-dept

#Eureka的配置，服务注册到哪里
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/
---

#spring的配置
server:
  port: 8202
spring:
  profiles: test
  application:
    name: springcloud-provider-dept

#Eureka的配置，服务注册到哪里
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/

```

11. 提交到github
12. 新建maven项目，springcloud-config-client-3355并导入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

13. 新建application.yml和bootstrap.yml（springcloud-config-client-3355）

```yml
#bootstrap.yml
#系统级别的配置
spring:
  cloud:
    config:
      name:  config-client #需要从git上读取的资源名称，不要后缀
      profile: dev
      uri: http://localhost:3344
      label: main
```

```yml
#application.yml
#用户级别的配置
spring:
  application:
    name: springcloud-config-client-3355
```

14. 新建启动类（springcloud-config-client-3355）

```java
@SpringBootApplication
public class Config_Client_3355 {
    public static void main(String[] args) {
        SpringApplication.run(Config_Client_3355.class,args);
    }
}
```

15. 新建ConfigCilentController类（springcloud-config-client-3355下com.example.springcloud.controller）

```java
@RestController
public class ConfigCilentController {

    @Value("${spring.application.name}")
    private String applicationName;

    @Value("${eureka.client.service-url.defaultZone}")
    private String eurekaServer;

    @Value("${server.port}")
    private String port;

    @RequestMapping("/config")
    public String getConfig(){
        return "applicationName："+applicationName+"，"+
                "eurekaServer："+eurekaServer+"，"+
                "port："+port;
    }
}
```

16. 启动springcloud-config-server-3344，springcloud-config-client-3355，访问http://localhost:8201/config

> applicationName：springcloud-provider-dept，eurekaServer：http://eureka7001.com:7001/eureka/，port：8201

## 项目实战

1. 在拉取下来的springcloud-config文件中新建config-eureka.yml，并添加内容

```yml
spring:
  profiles:
    active: dev

---
server:
  port: 7001

#spring配置
spring:
  profiles: dev
  application:
    name: springcloud-config-eureka

#Eureka配置
eureka:
  instance:
    hostname: eureka7001.com #Eureka服务端的实例名称
  client:
    register-with-eureka: false #表示是否向eureka注册中心注册自己
    fetch-registry: false #如果fetch-registry为false，则表示自己为注册中心
    service-url:
      defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/

---
#spring配置
spring:
  profiles: test
  application:
    name: springcloud-config-eureka

#Eureka配置
eureka:
  instance:
    hostname: eureka7001.com #Eureka服务端的实例名称
  client:
    register-with-eureka: false #表示是否向eureka注册中心注册自己
    fetch-registry: false #如果fetch-registry为false，则表示自己为注册中心
    service-url:
      defaultZone: http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
```

2. 在拉取下来的springcloud-config文件中新建config-dept.yml，并添加内容

```yml
spring:
  profiles:
    active: dev

---
server:
  port: 8001
#mybatis的配置
mybatis:
  type-aliases-package: com.example.springcloud.pojo
  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml
#spring的配置
spring:
  profiles: dev
  application:
    name: springcloud-config-dept
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db01?userUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
    username: root
    password: 123456

#Eureka的配置，服务注册到哪里
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
  instance:
    instance-id: springcloud-provider-dept-8001 #修改eureka上的默认描述信息

#info配置
info:
  app.name: example-springcloud
  company.name: blog.example.com
  
---
server:
  port: 8001
#mybatis的配置
mybatis:
  type-aliases-package: com.example.springcloud.pojo
  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml
#spring的配置
spring:
  profiles: test
  application:
    name: springcloud-config-dept
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/db02?userUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
    username: root
    password: 123456

#Eureka的配置，服务注册到哪里
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/,http://eureka7003.com:7003/eureka/
  instance:
    instance-id: springcloud-provider-dept-8001 #修改eureka上的默认描述信息

#info配置
info:
  app.name: example-springcloud
  company.name: blog.example.com
```

3. 新建maven项目，springcloud-config-eureka-7001，所有内容与springcloud-eureka-7001一致，新增依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
...
```

4. 删除application.yml所有内容，并新建bootstrap.yml（springcloud-config-eureka-7001）

```yml
#application.yml
spring:
  application:
    name: springcloud-config-eureka-7001
```

```yml
#bootstrap.yml
spring:
  cloud:
    config:
      uri: http://localhost:3344
      name: config-eureka
      label: main
      profile: dev
```

5. 启动springcloud-config-server-3344，访问http://localhost:3344/config-eureka-dev.yml，看能不能查看到配置
6. 再启动springcloud-config-eureka-7001，访问http://localhost:7001/，出现eureka页面，成功

7. 新建maven项目，springcloud-config-dept-8001，所有内容与springcloud-provider-dept-8001一致，新增依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
...
```

8. 删除application.yml所有内容，并新建bootstrap.yml（springcloud-config-dept-8001）

```yml
#application.yml
spring:
  application:
    name: springcloud-config-dept-8001
```

```yml
#bootstrap.yml
spring:
  cloud:
    config:
      profile: dev
      name: config-dept
      label: main
      uri: http://localhost:3344
```

9. 启动springcloud-config-server-3344，springcloud-config-eureka-7001，springcloud-config-dept-8001，访问http://localhost:7001/，看8001是否注册进去，访问http://localhost:8001/dept/get/1，出现数据库db01的数据，成功

10. 修改bootstrap.yml（springcloud-config-dept-8001）

```yml
spring:
  cloud:
    config:
      profile: test
      name: config-dept
      label: main
      uri: http://localhost:3344
```

11. 启动springcloud-config-server-3344，springcloud-config-eureka-7001，springcloud-config-dept-8001访问http://localhost:8001/dept/get/1，出现数据库db02的数据，成功