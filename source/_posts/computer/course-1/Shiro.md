---
title: Shiro
date: 2020/12/14
categories:
	- [计算机, 框架]
tags: Shiro
---



## 权限管理

1.什么是权限管理？

> 不同身份的用户进入到系统中能够完成的操作是不同的，我们对于不同的用户进行的可执行的操作的管理称之为权限管理。

## Shiro简介

### 认证授权流程

> 认证：对用户的身份进行检查（登录验证）
>
> 授权：对用户的权限进行检查（是否有对应的操作权限）

### 安全框架

>帮助我们完成用户身份认证及权限检查功能框架
>
>常用的安全框架
>
>+ Shrio：Apache Shiro是一个功能强大并且易用的Java安全框架（小而简单）
>+ spring Security：基于Spring的一个安全框架，依赖于Spring
>+ OAuth2：第三方授权登录
>+ 自定义安全认证中心
>

### Shiro

>* Apache Shiro是一个功能强大并且易用的Java安全框架
>
>* 可以完成用户认证、授权、密码以及会话管理
>
>* 可以在任何应用系统中使用（主要针对于单体项目的权限管理）

## Shiro的工作原理

### Shiro的核心功能

> Authentication：认证，验证用户是否有相应的身份-登录验证；
>
> Authorization：授权，即权限验证；对已经通过的用户检查是否具有某个权限或者角色，从而控制是否能进行某种操作
>
> Session Management：会话管理，用户在认证成功之后创建会话，在没有退出之前，之前用户的所有信息都将会保存在这个会话中，可以是普通的JavaSE应用，也可以是Web应用；
>
> Cryptography：加密，对敏感信息进行加密处理，shiro就提供这种加密机制；
>
> 支持的特性：
>
> * Web Support - Shiro提供l过滤器，可以通过过滤器拦截Web请求处理web应用的访问控制
> * Caching缓存支持，shiro可以缓存用户信息以及用户的角色授权信息，可以提高执行效率
> * Concurrency shiro支持多线程应用
> * Testing提供测试支持
> * Run As允许一个用户以另一种身份去访问
> * Remember me
>
> 说明：Shiro是一个安全框架，不提供用户以及权限的维护（用户权限的权限管理需要我们自己去设计）

### Shiro核心组件

![shiro](Shiro.assets/shiro.jpg)

Shiro三大核心组件：Subject、Security Manager、Realms

>* Subject，表示待认证和授权的用户
>* Security Manager，他是Shiro框架的核心，Shiro就是通过Security Manger来进行内部实例的管理，并通过它来提供安全框架的各种服务
>  * Authenticator，认证器
>  * Anthorizer，授权器
>  * SessionManager，会话管理器
>  * CacheManager，缓存管理器
>* Realms，相当于Shiro进行认证授权的数据源，充当了Shiro与安全数据之间的“桥梁”或者“连接器”，也就是说，当用户进行认证（登录）和授权（访问控制）验证时，Shiro会用应用配置的Realm中查找用户以及权限信息

## 基于JavaSE应用

### 创建Maven项目

### 导入Shiro依赖

```xml
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-core</artifactId>
    <version>1.4.1</version>
</dependency>
```

### 创建Shiro配置文件

>在resource目录下创建名为shiro.ini的文件
>
>在文件中完成用户角色以及权限的配置

```ini
[users]
zhangsan=123456,seller
lisi=123456,ckmanager
admin=222222,admin

[roles]
admin=*
seller=order-add,order-del,order-list
ckmanager=ck-add,ck-del,ck-list
```

Shiro的基本使用

```java
package com.hu.shiro;

import org.apache.shiro.SecurityUtils;
import org.apache.shiro.authc.UsernamePasswordToken;
import org.apache.shiro.mgt.DefaultSecurityManager;
import org.apache.shiro.realm.text.IniRealm;
import org.apache.shiro.subject.Subject;

import java.util.Scanner;

public class TestShiro {
    public static void main(String[] args) {
       Scanner scanner = new Scanner(System.in);
       System.out.println("请输入账号");
       String username = scanner.nextLine();
       System.out.println("请输入密码");
       String password = scanner.nextLine();

       //1.创建安全管理器
       DefaultSecurityManager securityManager = new DefaultSecurityManager();
       //2.创建realm
       IniRealm iniRealm = new IniRealm("classpath:shiro.ini");
       //3.将realm设置给安全管理器
       securityManager.setRealm(iniRealm);
       //4.将realm设置给SecurityUtils工具
       SecurityUtils.setSecurityManager(securityManager);
       //5.通过SecurityUtils工具过去Subject对象
       Subject subject = SecurityUtils.getSubject();

       //认证流程
       //1.将认证账号密码封装到token对象中
       UsernamePasswordToken token = new UsernamePasswordToken(username,password);
       //2.通过subject对象调用login方法进行认证申请
       boolean flag = false;
       try {
           subject.login(token);
           flag = true;
       }catch (Exception e){
           flag = false;
       }
       System.out.println(flag?"登录成功":"登录失败");
       //授权
       //判断是否有某个角色
       System.out.println(subject.hasRole("seller"));
       //判断是否有某个权限
       System.out.println(subject.isPermitted("order-del"));
    }
}
```

### Shiro认证流程

>1.通过subject.login(token)进行登录验证，就会将token包含的用户信息（账号和密码）传递给SecurityManager
>
>2.SecurityManger将会调用Authenticator进行身份验证
>
>3.Authenticator把token传递给对应的Realm
>
>4.Realm根据得到的token，调用doGetAuthenticationInfo方法进行认证（如果认证失败通过抛出异常提示认证器）
>
>5.将认证结果一层一层的返回到subject（如果subject.login抛出异常则表示认证失败 ）

## SpringBoot应用整合Shiro

### 创建SpringBoot应用

>lombok
>
>spring web
>
>thymeleaf

### 整合Druid和Mybatis

依赖

```xml
<!--druid-->
<dependency>
<groupId>com.alibaba</groupId>
<artifactId>druid-spring-boot-starter</artifactId>
<version>1.1.7</version>
</dependency>
<!--mysql-->
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>8.0.20</version>
</dependency>
<!--mybatis-->
<dependency>
<groupId>org.mybatis.spring.boot</groupId>
<artifactId>mybatis-spring-boot-starter</artifactId>
<version>2.1.1</version>
</dependency>
```

配置

>```yml
>spring:
>  datasource:
>    druid:
>      url: jdbc:mysql://localhost:3306/user
>      driver-class-name: com.mysql.jdbc.Driver
>      username: root
>      password: 123456
>      initial-size: 1
>      min-idle: 1
>      max-active: 20
>mybatis:
>  mapper-locations: classpath:mapper/*.xml
>```

3.整合Shiro

导入依赖

```xml
<!--shiro-->
<dependency>
    <groupId>org.apache.shiro</groupId>
    <artifactId>shiro-spring</artifactId>
    <version>1.4.1</version>
</dependency>
```

配置

> SpringBoot默认没有提供Shiro的自动配置
>
> ```java
> package com.example.demo.config;
> 
> import org.apache.shiro.realm.text.IniRealm;
> import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
> import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
> import org.springframework.context.annotation.Bean;
> import org.springframework.stereotype.Controller;
> 
> import java.util.HashMap;
> import java.util.Map;
> 
> @Configuration
> public class ShiroConfig {
> 
>     @Bean
>     public IniRealm getIniRealm(){
>         IniRealm iniRealm = new IniRealm("classpath:shiro.ini");
>         return iniRealm;
>     }
> 
>     @Bean
>     public DefaultWebSecurityManager getDefaultWebSecurityManager(IniRealm iniRealm){
>         DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
>         //securityManager要完成校验，需要realm
>         securityManager.setRealm(iniRealm);
>         return securityManager;
>     }
> 
>     @Bean
>     public ShiroFilterFactoryBean shiroFilter(DefaultWebSecurityManager securityManager){
>         ShiroFilterFactoryBean filter = new ShiroFilterFactoryBean();
>         //过滤器就是Shiro进行权限校验的核心，进行认证和授权是需要SecurityManager的
>         filter.setSecurityManager(securityManager);
> 
>         //设置shiro的拦截规则
>         //anon  匿名用户可访问
>         //authc 认证用户可访问
>         //user  使用RememberMe的用户可访问
>         //perms 对应权限可访问
>         //role  对应角色可访问
>         Map<String,String> filterMap = new HashMap<>();
>         filterMap.put("/","anon");
>         filterMap.put("/login.html","anon");
>         filterMap.put("/user/login","anon");
>         filterMap.put("/static/**","anon");
>         filterMap.put("/**","authc");
> 
>         filter.setFilterChainDefinitionMap(filterMap);
>         filter.setLoginUrl("/login.html");
>         //设置未授权的访问路径
>         filter.setUnauthorizedUrl("/login.html");
>        return filter;
>     }
> 
> }
> ```
>
> * 认证测试
>
>   * UserServiceImpl.java
>
>     ```java
>     package com.example.demo.service;
>     
>     import org.apache.shiro.SecurityUtils;
>     import org.apache.shiro.authc.UsernamePasswordToken;
>     import org.apache.shiro.subject.Subject;
>     import org.springframework.stereotype.Service;
>     
>     @Service
>     public class UserServiceImpl {
>         public void checkLogin(String username,String password) throws Exception{
>             Subject subject = SecurityUtils.getSubject();
>             UsernamePasswordToken token = new UsernamePasswordToken(username,password);
>             subject.login(token);
>         }
>     }
>     ```
>
>   * UserController
>
>     ```java
>     package com.example.demo.controller;
>     
>     import com.example.demo.service.UserServiceImpl;
>     import org.springframework.beans.factory.annotation.Autowired;
>     import org.springframework.stereotype.Controller;
>     import org.springframework.web.bind.annotation.RequestMapping;
>     
>     @Controller
>     @RequestMapping("/user")
>     public class UserController {
>     
>         @Autowired
>         private UserServiceImpl userService;
>     
>         @RequestMapping("/login")
>         public String login(String username,String password){
>             try {
>                 userService.checkLogin(username,password);
>                 System.out.println("登录成功");
>                 return "index";
>             } catch (Exception e) {
>                 System.out.println("登录失败");
>                 return "login";
>             }
>         }
>     
>     }
>     ```
>
>   * login.html
>
>     ```html
>     <form action="user/login">
>         <p>账号：<input type="text" name="username"></p>
>         <p>密码：<input type="text" name="password"></p>
>         <p><input type="submit" value="登录"></p>
>     </form>
>     ```

## SpringBoot应用整合Shiro-案例（JdbcRealm）

### JdbcRealm介绍

>如果使用JdbcRealm，则必须提供JdbcRealm所需的表结构（权限设计）

### JdbcRealm规定的表结构

>```mysql
>-- 创建用户表
>create table users(
>	id int primary key auto_increment,
>    username varchar(20) not null unique,
>    password varchar(20) not null,
>    password_salt varchar(20)
>);
>insert into users(username,password) values('zhangsan','123456');
>insert into users(username,password) values('lisi','123456');
>insert into users(username,password) values('wangwu','123456');
>insert into users(username,password) values('zhaoliu','123456');
>insert into users(username,password) values('chengqi','123456');
>
>-- 用户角色表
>create table user_roles(
>	id int primary key auto_increment,
>    username varchar(60) not null,
>    role_name varchar(100) not null
>);
>
>-- admin 系统管理员
>-- cmanager 仓库人员
>-- xmanager 销售人员
>-- kmanager 客服人员
>-- zmanager 行政人员
>
>insert into user_roles(username,role_name) values('zhangsan','admin');
>insert into user_roles(username,role_name) values('lisi','cmanager');
>insert into user_roles(username,role_name) values('wangwu','xmanager');
>insert into user_roles(username,role_name) values('zhaoliu','kmanager');
>insert into user_roles(username,role_name) values('chengqi','zmanager');
>
>-- 角色权限表
>create table roles_permissions(
>	id int primary key auto_increment,
>    role_name varchar(100) not null,
>    permission varchar(100) not null
>);
>
>-- 管理员
>insert into roles_permissions(role_name,permission) values('admin','*');
>-- 仓库人员
>insert into roles_permissions(role_name,permission) values('cmanager','sys:c:save');
>insert into roles_permissions(role_name,permission) values('cmanager','sys:c:delete');
>insert into roles_permissions(role_name,permission) values('cmanager','sys:c:update');
>insert into roles_permissions(role_name,permission) values('cmanager','sys:c:find');
>-- 销售人员
>insert into roles_permissions(role_name,permission) values('xmanager','sys:x:save');
>insert into roles_permissions(role_name,permission) values('xmanager','sys:x:delete');
>insert into roles_permissions(role_name,permission) values('xmanager','sys:x:update');
>insert into roles_permissions(role_name,permission) values('xmanager','sys:x:find');
>-- 客服人员
>insert into roles_permissions(role_name,permission) values('cmanager','sys:c:update');
>insert into roles_permissions(role_name,permission) values('cmanager','sys:c:find');
>-- 行政人员
>insert into roles_permissions(role_name,permission) values('zmanager','sys:z:find');
>```

### Springboot整合Shiro

> 创建Springboot应用
>
> 整合Druid和Mybatis
>
> 整合Shiro
>
> * 添加依赖
>
> * 配置shiro
>
>   ```java
>   package com.example.demo.config;
>   
>   import org.apache.shiro.realm.jdbc.JdbcRealm;
>   import org.apache.shiro.realm.text.IniRealm;
>   import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
>   import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
>   import org.springframework.context.annotation.Bean;
>   import org.springframework.context.annotation.Configuration;
>   
>   import javax.sql.DataSource;
>   import java.util.HashMap;
>   import java.util.Map;
>   
>   @Configuration
>   public class ShiroConfig {
>   
>       @Bean
>       public JdbcRealm getJdbcRealm(DataSource dataSource){
>           JdbcRealm jdbcRealm = new JdbcRealm();
>           //JdbcRealm会自动从数据查询用户及权限数据（数据库的表结构要符合JdbcRealm的规范）
>           jdbcRealm.setDataSource(dataSource);
>           //JdbcRealm默认开启认证功能，需要手动开启授权功能
>           jdbcRealm.setPermissionsLookupEnabled(true);
>           return jdbcRealm;
>       }
>   
>       @Bean
>       public DefaultWebSecurityManager getDefaultWebSecurityManager(JdbcRealm jdbcRealm){
>           DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
>           //securityManager要完成校验，需要realm
>           securityManager.setRealm(jdbcRealm);
>           return securityManager;
>       }
>   
>       @Bean
>       public ShiroFilterFactoryBean shiroFilter(DefaultWebSecurityManager securityManager){
>           ShiroFilterFactoryBean filter = new ShiroFilterFactoryBean();
>           //过滤器就是Shiro进行权限校验的核心，进行认证和授权是需要SecurityManager的
>           filter.setSecurityManager(securityManager);
>   
>           //设置shiro的拦截规则
>           //anon  匿名用户可访问
>           //authc 认证用户可访问
>           //user  使用RememberMe的用户可访问
>           //perms 对应权限可访问
>           //role  对应角色可访问
>           Map<String,String> filterMap = new HashMap<>();
>           filterMap.put("/","anon");
>           filterMap.put("/login.html","anon");
>           filterMap.put("/user/login","anon");
>           filterMap.put("/static/**","anon");
>           filterMap.put("/**","authc");
>   
>           filter.setFilterChainDefinitionMap(filterMap);
>           //filter.setLoginUrl("/");
>           //设置未授权的访问路径
>           filter.setUnauthorizedUrl("/login.html");
>           return filter;
>       }
>   }
>   ```

七、Shiro的标签使用

> 当用户认证进入到主页之后，需要显示用户信息以及当前用户的权限信息，Shiro就提供了一套标签用于页面来进行权限数据的呈现

Shiro提供了可供JSP使用的标签以及thymeleaf中标签

* JSP页面引用：

  ```jsp
  <%@ taglib prefix="shiro" uri="http://shiro.apache.org/tags" %>
  ```

* thymeleaf模板中引用

  在pom.xml文件中导入thymeleaf模板对shiro标签支持的依赖

  ```xml
  <dependency>
      <groupId>com.github.theborakompanioni</groupId>
      <artifactId>thymeleaf-extras-shiro</artifactId>
      <version>2.0.0</version>
  </dependency>
  ```

  在ShiroConfig配置Shiro的

  ```java
  @Bean
  public ShiroDialect getShiroDialect(){
      return new ShiroDialect();
  }
  ```

  thymeleaf模板中引入Shiro的命名空间

  ```html
  <html xmlns:th="http://www.thymeleaf.org"
        xmlns:shiro="http://www.pollix.at/thymeleaf/shiro ">  
  </html>
  ```

  常用标签

  >guest，判断用户是否是游客身份，如果是游客身份则显示此标签内容
  >
  >```html
  ><shiro:guest>
  >    欢迎游客访问，<a href="login.html">登录</a>
  ></shiro:guest>
  >```
  >
  >user，判断用户是否是认证身份，如果是认证身份则显示此标签内容
  >
  >principal，获取当前登录用户名
  >
  >```html
  ><shiro:user>
  >    用户[<shiro:principal/>]欢迎您！
  > </shiro:user>
  >```
  >
  >noAuthenticated/authenticated
  >
  >hasRole
  >
  >hasPermission
  >
  >```html
  ><!DOCTYPE html>
  ><html xmlns:th="http://www.thymeleaf.org"
  >      xmlns:shiro="http://www.pollix.at/thymeleaf/shiro ">
  ><head>
  >    <meta charset="UTF-8">
  >    <title>Title</title>
  ></head>
  ><body>
  >    index
  >    <hr/>
  >    <shiro:guest>
  >        欢迎游客访问，<a href="login.html">登录</a>
  >    </shiro:guest>
  >    <shiro:user>
  >        用户[<shiro:principal/>]欢迎您！
  >        当前用户为<shiro:hasRole name="admin">超级管理员</shiro:hasRole>
  >        <shiro:hasRole name="cmanager">仓库人员</shiro:hasRole>
  >        <shiro:hasRole name="xmanager">销售人员</shiro:hasRole>
  >        <shiro:hasRole name="kmanager">客服人员</shiro:hasRole>
  >        <shiro:hasRole name="zmanager">行政人员</shiro:hasRole>
  >    </shiro:user>
  >    <hr/>
  >    仓库管理
  >    <ul>
  >        <shiro:hasPermission name="sys:c:save"><li><a href="#">入库</a></li></shiro:hasPermission>
  >        <shiro:hasPermission name="sys:c:delete"><li><a href="#">出库</a></li></shiro:hasPermission>
  >        <shiro:hasPermission name="sys:c:update"><li><a href="#">修改</a></li></shiro:hasPermission>
  >        <shiro:hasPermission name="sys:c:find"><li><a href="#">查询</a></li></shiro:hasPermission>
  >    </ul>
  >    订单管理
  >    <ul>
  >        <shiro:hasPermission name="sys:x:save"><li><a href="#">添加订单</a></li></shiro:hasPermission>
  >        <shiro:hasPermission name="sys:x:delete"><li><a href="#">删除订单</a></li></shiro:hasPermission>
  >        <shiro:hasPermission name="sys:x:update"><li><a href="#">修改订单</a></li></shiro:hasPermission>
  >        <shiro:hasPermission name="sys:x:find"><li><a href="#">查询订单</a></li></shiro:hasPermission>
  >    </ul>
  >    客户管理
  >    <ul>
  >        <shiro:hasPermission name="sys:k:save"><li><a href="#">添加客户</a></li></shiro:hasPermission>
  >        <shiro:hasPermission name="sys:k:delete"><li><a href="#">删除客户</a></li></shiro:hasPermission>
  >        <shiro:hasPermission name="sys:k:update"><li><a href="#">修改客户</a></li></shiro:hasPermission>
  >        <shiro:hasPermission name="sys:k:find"><li><a href="#">查询客户</a></li></shiro:hasPermission>
  >    </ul>
  ></body>
  ></html>
  >```

## Springboot整合Shiro完成权限管理案例—自定义Realm

>使用JdbcRealm可以完成用户权限管理，但是我们必须提供Jdbc规定的数据表结构，如果我们的项目开发中，这个JdbcReal规定的数据表结构不能满足开发需求，如何处理？
>
>* 自定义数据库表结构
>* 自定义Realm实现认证和授权

### 数据库设计

>```mysql
>-- 用户信息表
>create table user(
>	id int primary key auto_increment,
>	username varchar(60) not null unique,
>	password varchar(20) not null,
>	password_salt varchar(60)
>);
>
>insert into user(username,password) values('zhangsan','123456');
>insert into user(username,password) values('lisi','123456');
>insert into user(username,password) values('wangwu','123456');
>insert into user(username,password) values('zhaoliu','123456');
>insert into user(username,password) values('chengqi','123456');
>
>-- 角色信息表
>create table role(
>	id int primary key auto_increment,
>	name varchar(60) not null
>);
>
>insert into role(name) values('admin');
>insert into role(name) values('cmanager');-- 仓库
>insert into role(name) values('xmanager');-- 销售
>insert into role(name) values('kmanager');-- 客服
>insert into role(name) values('zmanager');-- 行政
>
>-- 权限信息表
>create table permission(
>	id int primary key auto_increment,
>	`code` varchar(60) not null,
>	name varchar(60)
>);
>
>insert into permission(code,name) values('sys:c:save','入库');
>insert into permission(code,name) values('sys:c:delete','出库');
>insert into permission(code,name) values('sys:c:update','修改');
>insert into permission(code,name) values('sys:c:find','查询');
>
>insert into permission(code,name) values('sys:x:save','新增订单');
>insert into permission(code,name) values('sys:x:delete','删除订单');
>insert into permission(code,name) values('sys:x:update','修改订单');
>insert into permission(code,name) values('sys:x:find','查询订单');
>
>insert into permission(code,name) values('sys:k:save','新增客户');
>insert into permission(code,name) values('sys:k:delete','删除客户');
>insert into permission(code,name) values('sys:k:update','修改客户');
>insert into permission(code,name) values('sys:k:find','查询客户');
>
>
>-- 用户角色表
>create table user_role(
>	uid int not null,
>	rid int not null
>-- 	primary key(uid,rid),
>-- 	constraint FK_user foreign key(uid) references user(id),
>-- 	constraint FK_role foreign key(rid) references role(id),
>)
>
>insert into user_role(uid,rid) values(1,1);
>insert into user_role(uid,rid) values(1,2);
>insert into user_role(uid,rid) values(1,3);
>insert into user_role(uid,rid) values(1,4);
>insert into user_role(uid,rid) values(1,5);
>
>insert into user_role(uid,rid) values(2,2);
>insert into user_role(uid,rid) values(3,3);
>insert into user_role(uid,rid) values(4,4);
>insert into user_role(uid,rid) values(5,5);
>
>-- 角色权限表
>create table role_permission(
>	rid int not null,
>	pid int not null
>)
>-- 给仓库角色分配权限
>insert into role_permission(rid,pid) values(2,1);
>insert into role_permission(rid,pid) values(2,2);
>insert into role_permission(rid,pid) values(2,3);
>insert into role_permission(rid,pid) values(2,4);
>-- 给销售角色分配权限
>insert into role_permission(rid,pid) values(3,5);
>insert into role_permission(rid,pid) values(3,6);
>insert into role_permission(rid,pid) values(3,7);
>insert into role_permission(rid,pid) values(3,8);
>insert into role_permission(rid,pid) values(3,9);
>insert into role_permission(rid,pid) values(3,10);
>insert into role_permission(rid,pid) values(3,11);
>insert into role_permission(rid,pid) values(3,12);
>-- 给客服角色分配权限
>insert into role_permission(rid,pid) values(4,11);
>insert into role_permission(rid,pid) values(4,12);
>-- 给行政角色分配权限
>insert into role_permission(rid,pid) values(5,4);
>insert into role_permission(rid,pid) values(5,8);
>insert into role_permission(rid,pid) values(5,12);
>```

### DAO实现

>Shiro进行认证需要用户信息
>
>* 根据用户名查询用户信息
>
>Shiro进行授权管理需要当前用户的角色和权限
>
>* 根据用户名查询当前用户的角色列表（3张表连接查询）
>* 根据用户名查询当前用户的权限列表（5张表连接查询）

#### 创建Springboot项目，整合Mybatis

#### 更具用户名查用户信息

* 创建Bean

  ```java
  @Data
  public class User {
      private Integer id;
      private String username;
      private String password;
      private String pwdSalt;
  }
  ```

* 创建Dao

  ```java
  public interface UserDao {
      User queryUserByUsername(String username) throws Exception;
  }
  ```

* 映射配置

  ```xml
  <resultMap id="userMap" type="User">
      <id column="id" property="id"></id>
      <result column="username" property="username"/>
      <result column="password" property="password"/>
      <result column="password_salt" property="pwdSalt"/>
  </resultMap>
  <select id="queryUserByUsername" resultMap="userMap">
      select * from user where username = #{username};
  </select>
  ```

#### 根据用户名查询角色名列表

* 创建Dao

  ```java
  public interface RoleDao {
      Set<String> queryRoleNameByUsername(String username) throws Exception;
  }
  ```

* 映射配置

  ```xml
  <select id="queryRoleNameByUsername" resultSets="java.util.set" resultType="String">
      SELECT r.name FROM `user` u
      LEFT JOIN user_role ur ON u.id = ur.uid
      LEFT JOIN role r ON ur.rid = r.id
      WHERE u.username = #{username}
  </select>
  ```

### 整合Shiro

* 导入依赖

  ```xml
  <dependency>
      <groupId>com.github.theborakompanioni</groupId>
      <artifactId>thymeleaf-extras-shiro</artifactId>
      <version>2.0.0</version>
  </dependency>
  
  <!--shiro-->
  <dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-spring</artifactId>
      <version>1.4.1</version>
  </dependency>
  ```

* 配置Shiro-基于Java配置方式

* 自定义Realm

  ```java
  package com.example.demo.config;
  
  import com.example.demo.beans.User;
  import com.example.demo.dao.PermissionDao;
  import com.example.demo.dao.RoleDao;
  import com.example.demo.dao.UserDao;
  import org.apache.shiro.authc.*;
  import org.apache.shiro.authz.AuthorizationInfo;
  import org.apache.shiro.authz.SimpleAuthorizationInfo;
  import org.apache.shiro.realm.AuthorizingRealm;
  import org.apache.shiro.subject.PrincipalCollection;
  
  import javax.annotation.Resource;
  import java.util.Set;
  
  /**
   * 1.创建一个继承AuthorizingRealm类（实现Realm接口的类）
   * 2.重新doGetAuthorizationInfo和doGetAuthenticationInfo方法
   * 3.重新getName方法
   */
  public class MyRealm extends AuthorizingRealm {
  
      @Resource
      private UserDao userDao;
  
      @Resource
      private RoleDao roleDao;
  
      @Resource
      private PermissionDao permissionDao;
  
      @Override
      public String getName() {
          return "myRealm";
      }
  
      /**
       * 获取授权数据(将当前用户的角色以及权限信息查询出来)
       * @param principalCollection
       * @return
       */
      @Override
      protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
          //获取用户的用户名
          String username = (String) principalCollection.iterator().next();
          //根据用户名查询当前用户角色列表
          Set<String> roleNames = roleDao.queryRoleNameByUsername(username);
  
          //根据用户名查询当前用户权限列表
          Set<String> permission = permissionDao.queryPermissionByUsername(username);
          SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
          info.setRoles(roleNames);
          info.setStringPermissions(permission);
          return info;
      }
  
      /**
       * 获取认证数据（从数据库查询的用户的正确数据）
       * @param authenticationToken
       * @return
       * @throws AuthenticationException
       */
      @Override
      protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
          //参数authenticationToken就是传递的 subject.login(token)
          //从token获取用户名
          UsernamePasswordToken token = (UsernamePasswordToken) authenticationToken;
          String username = token.getUsername();
          //根据用户名，从数据库查询当前用户的安全数据
          User user = userDao.queryUserByUsername(username);
          if (user == null){
              return null;
          }
          AuthenticationInfo info = new SimpleAuthenticationInfo(
                  username,   //当前用户名
                  user.getPassword(),  //从数据库查询出来的安全密码
                  getName());
  
          return info;
      }
  }
  ```

## 加密

### 加密介绍

> 明文——（加密规则）——密文
>
> 加密规则可以自定义，在项目开发中我们通常使用BASE64和MD5编码方式
>
> * BASE64：可反编码的编码方式
>
>   明文——密文
>
>   密文——明文
>
> * MD5：不可逆的编码方式（非对称）
>
>   明文——密文

如果数据库用户的密码存储的密文，Shiro改如何验证

使用Shiro提供的加密功能，对输入的密码进行加密之后再进行认证

### Shiro使用加密认证

> 配置Shiro
>
> ```java
> package com.example.demo.config;
> 
> import at.pollux.thymeleaf.shiro.dialect.ShiroDialect;
> import org.apache.shiro.authc.credential.HashedCredentialsMatcher;
> import org.apache.shiro.realm.jdbc.JdbcRealm;
> import org.apache.shiro.realm.text.IniRealm;
> import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
> import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
> import org.springframework.context.annotation.Bean;
> import org.springframework.context.annotation.Configuration;
> 
> import javax.sql.DataSource;
> import java.util.HashMap;
> import java.util.Map;
> 
> @Configuration
> public class ShiroConfig {
> 
>    //...
> 
>     @Bean
>     public HashedCredentialsMatcher getHashedCredentialsMatcher(){
>         HashedCredentialsMatcher matcher = new HashedCredentialsMatcher();
>         //matcher就是用来指定加密规则
>         matcher.setHashAlgorithmName("md5");
>         //hash次数
>         matcher.setHashIterations(1);//次数循环次数要与用户注册时密码加密次数一致
>         return matcher;
>     }
> 
>     /**
>      * 自定义Realm
>      */
>     @Bean
>     public MyRealm getMyRealm(HashedCredentialsMatcher matcher){
>         MyRealm realm = new MyRealm();
>         realm.setCredentialsMatcher(matcher);
>         return realm;
>     }
> 
>     //...
> }
> 
> ```

### 用户注册密码加密处理

> register.html
>
> ```html
> <form action="user/register">
>     <p>账号：<input type="text" name="username"></p>
>     <p>密码：<input type="text" name="password"></p>
>     <p><input type="submit" value="注册"></p>
> </form>
> ```
>
> UserController.java
>
> ```java
> package com.example.demo.controller;
> 
> import com.example.demo.service.UserServiceImpl;
> import org.apache.shiro.crypto.hash.Md5Hash;
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.stereotype.Controller;
> import org.springframework.web.bind.annotation.RequestMapping;
> 
> import java.util.Random;
> 
> @Controller
> @RequestMapping("/user")
> public class UserController {
> 
>     @Autowired
>     private UserServiceImpl userService;
> 
>     @RequestMapping("/register")
>     public String register(String username,String password){
>         System.out.println("注册");
>         //注册的时候要对密码进行加密存储
>         Md5Hash md5Hash = new Md5Hash(password);
>         System.out.println(md5Hash.toHex());
> 
>         //加盐加密
>         int num = new Random().nextInt(90000)+10000;//10000-99999
>         System.out.println("salt:"+num);
>         Md5Hash md5Hash2 = new Md5Hash(password,num+"");
>         System.out.println(md5Hash2);
> 
>         //加盐加密+多次hash
>         Md5Hash md5Hash3 = new Md5Hash(password,num+"",3);
>         System.out.println(md5Hash3);
>         
>         //将用户信息保存到数据库时，保存加密后的米，如果生成的随机盐，盐也要保存
>         
>         return "login";
>     }
> }
> ```

### 如果密码进行了加密处理，则Realm再但会认证数据时需要返回盐

>MyRealm.java
>
>```java
>package com.example.demo.config;
>
>import com.example.demo.beans.User;
>import com.example.demo.dao.PermissionDao;
>import com.example.demo.dao.RoleDao;
>import com.example.demo.dao.UserDao;
>import org.apache.shiro.authc.*;
>import org.apache.shiro.authz.AuthorizationInfo;
>import org.apache.shiro.authz.SimpleAuthorizationInfo;
>import org.apache.shiro.realm.AuthorizingRealm;
>import org.apache.shiro.subject.PrincipalCollection;
>import org.apache.shiro.util.ByteSource;
>
>import javax.annotation.Resource;
>import java.util.Set;
>
>/**
> * 1.创建一个继承AuthorizingRealm类（实现Realm接口的类）
> * 2.重新doGetAuthorizationInfo和doGetAuthenticationInfo方法
> * 3.重新getName方法
> */
>public class MyRealm extends AuthorizingRealm {
>
>    @Resource
>    private UserDao userDao;
>
>    @Resource
>    private RoleDao roleDao;
>
>    @Resource
>    private PermissionDao permissionDao;
>
>    //...
>
>    /**
>     * 获取认证数据（从数据库查询的用户的正确数据）
>     * @param authenticationToken
>     * @return
>     * @throws AuthenticationException
>     */
>    @Override
>    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
>        //参数authenticationToken就是传递的 subject.login(token)
>        //从token获取用户名
>        UsernamePasswordToken token = (UsernamePasswordToken) authenticationToken;
>        String username = token.getUsername();
>        //根据用户名，从数据库查询当前用户的安全数据
>        User user = userDao.queryUserByUsername(username);
>        if (user == null){
>            return null;
>        }
>        /*AuthenticationInfo info = new SimpleAuthenticationInfo(
>                username,   //当前用户名
>                user.getPassword(),  //从数据库查询出来的安全密码
>                getName());*/
>        //如果数据库是加了盐的
>        AuthenticationInfo info = new SimpleAuthenticationInfo(
>                username,   //当前用户名
>                user.getPassword(),  //从数据库查询出来的安全密码
>                ByteSource.Util.bytes(user.getPwdSalt()),
>                getName());
>        return info;
>    }
>}
>```

## 退出登录

在shiro过滤器中进行配置，配置logOut对应的路径

```java
filterMap.put("/exit","logout");
```

在页面的退出按钮上，跳转到exit对应的url

```html
<a href="exit">退出</a>
```

## 授权

> 用户登录成功之后，要进行的操作就需要有对应的权限，在进行操作之前对权限进行检查—授权
>
> 权限控制通常有两种做法：
>
> * 不同身份的用户登录，我们显示不同的操作菜单（没有权限的菜单不显示）
> * 对所有用户显示所有菜单，当用户点击菜单以后再验证当前用户是否有次权限，如果没有则提示权限不足

### html授权

> 在菜单页面只显示当前用户拥有权限操作的菜单
>
> shiro标签
>
> ```html
> <shiro:hasPermission name="sys:c:save"><dd><a href="c_add.html"  >入库</a></dd></shiro:hasPermission>
> ```

### 过滤器授权

>在shiro过滤器中请求的url进行权限设置
>
>```java
>filterMap.put("/c_add.html","perms[sys:c:save]");
> //设置未授权的访问路径
>filter.setUnauthorizedUrl("/lesspermission.html");
>```

### 注解授权

> 配置Spring对Shiro的支持，ShiroConfig.java
>
> ```java
> @Bean
> public DefaultAdvisorAutoProxyCreator getAdvisorAutoProxyCreator(){
>     DefaultAdvisorAutoProxyCreator autoProxyCreator = new DefaultAdvisorAutoProxyCreator();
>     autoProxyCreator.setProxyTargetClass(true);
>     return autoProxyCreator;
> }
> 
> @Bean
> public AuthorizationAttributeSourceAdvisor getAuthorizationAttributeSourceAdvisor(DefaultWebSecurityManager securityManager){
>     AuthorizationAttributeSourceAdvisor advisor = new AuthorizationAttributeSourceAdvisor();
>     advisor.setSecurityManager(securityManager);
>     return advisor;
> }
> ```
>
> 在请求的控制器添加权限注解
>
> ```java
> package com.example.demo.controller;
> 
> import org.apache.shiro.authz.annotation.RequiresPermissions;
> import org.springframework.stereotype.Controller;
> import org.springframework.web.bind.annotation.RequestMapping;
> 
> @Controller
> @RequestMapping("/customer")
> public class CustomerController {
> 
>     @RequestMapping("/list")
>     /**
>      * 如果没有sys:k:find的权限，则不需要执行此方法
>      */
>     @RequiresPermissions("sys:k:find")
>     public String list(){
>         return "customer_list";
>     }
> }
> ```
>
> 通过全局异常处理，指定权限不足时的页面跳转
>
> ```java
> package com.example.demo.utils;
> 
> import org.apache.shiro.authz.AuthorizationException;
> import org.springframework.web.bind.annotation.ControllerAdvice;
> import org.springframework.web.bind.annotation.ExceptionHandler;
> 
> @ControllerAdvice
> public class GlobalExceptionhandler {
> 
>     @ExceptionHandler
>     public String doException(Exception e){
>         if (e instanceof AuthorizationException){
>             return "lesspermission";
>         }
>         return null;
>     }
> }
> ```

### 手动授权

>在代码中进行手动的权限校验
>
>```java
>Subject subject = SecurityUtils.getSubject();
>if (subject.isPermitted("sys:k:find")){
>    return "customer_list";
>}else {
>    return "lesspermission";
>}
>```

## 缓存使用

> 使用Shiro进行权限管理过程中，每次授权都会访问realm中的doGetAuthenticationInfo方法查询当前用户的角色及权限信息，如果系统的用户量比较大则会对数据库造成比较大的压力
>
> Shiro支持缓存以降低对数据库的访问压力（缓存的时授权信息）



### 缓存的使用

> 导入依赖
>
> ```xml
>  <dependency>
>      <groupId>org.springframework.boot</groupId>
>      <artifactId>spring-boot-starter-cache</artifactId>
> </dependency>
> 
> <dependency>
>     <groupId>net.sf.ehcache</groupId>
>     <artifactId>ehcache</artifactId>
> </dependency>
> 
> <dependency>
>     <groupId>org.apache.shiro</groupId>
>     <artifactId>shiro-ehcache</artifactId>
>     <version>1.4.0</version>
> </dependency>
> ```

### 配置缓存策略

> * 在resources目录下创建一个xml文件，ehcache.xml
>
> * ```xml
>   <?xml version="1.0" encoding="UTF-8" ?>
>   <ehcache updateCheck="false" dynamicConfig="false">
>       <diskStore path="C:\TEMP" />
>   
>       <cache name="user" timeToLiveSeconds="300" maxEntriesLocalHeap="1000"/>
>   
>       <defaultCache name="defaultCache"
>                     maxElementsInMemory="10000"
>                     eternal="false"
>                     timeToIdleSeconds="120"
>                     timeToLiveSeconds="120"
>                     overflowToDisk="false"
>                     maxElementsOnDisk="100000"
>                     diskPersistent="false"
>                     diskExpiryThreadIntervalSeconds="120"
>                     memoryStoreEvictionPolicy="LRU"/>
>       <!--缓存淘汰策略：当缓存空间比较紧张时，我们要存储新的数据进来，就必然删除一些老的数据
>           LRU 最近最少使用
>           FIFO 先进先出
>           LFU 最少使用
>       -->
>   
>   </ehcache>
>   ```

### 加入缓存

> ShiroConfig
>
> ```java
> @Bean
> public EhCacheManager getEhCacheManager(){
>     EhCacheManager ehCacheManager = new EhCacheManager();
>     ehCacheManager.setCacheManagerConfigFile("classpth:ehcache.xml");
>     return ehCacheManager;
> }
> @Bean
> public DefaultWebSecurityManager getDefaultWebSecurityManager(MyRealm realm){
>     DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
>     //securityManager要完成校验，需要realm
>     securityManager.setRealm(realm);
>     securityManager.setCacheManager(getEhCacheManager());
>     return securityManager;
> }
> ```

## session管理

> Shiro进行认证和授权时基于session

如果我们需要对session进行管理

* 自定义session管理器
* 将自定义的session管理器设置给SecurityManager

配置自定义SessionManager：ShiroConfig.java

>```java
>@Bean
>public DefaultWebSessionManager getDefaultWebSessionManager(){
>    DefaultWebSessionManager sessionManager = new DefaultWebSessionManager();
>    System.out.println(sessionManager.getGlobalSessionTimeout());//1800000
>    //配置sessionManager
>    sessionManager.setGlobalSessionTimeout(15*1000);
>    return sessionManager;
>}
>@Bean
>public DefaultWebSecurityManager getDefaultWebSecurityManager(MyRealm realm){
>    DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
>    //securityManager要完成校验，需要realm
>    securityManager.setRealm(realm);
>    //设置缓存
>    securityManager.setCacheManager(getEhCacheManager());
>    //设置session
>    securityManager.setSessionManager(getDefaultWebSessionManager());
>    return securityManager;
>}
>```

## RememberMe

> 将用户对页面的访问权限分为三个级别
>
> * 未认证—可访问的页面
>   * login.html、register.html
> * 曾认证—可访问的页面
>   * info.html
> * 已认证—可访问的页面
>   * 转账.html

### 在过滤器中设置“记住我”可访问的url

```java
 		//anon  匿名用户可访问
        //authc 认证用户可访问
        //user  使用RememberMe的用户可访问
        //perms 对应权限可访问   
		//logout 退出指定的url
Map<String,String> filterMap = new HashMap<>();
        filterMap.put("/","anon");
        filterMap.put("/login.html","anon");
//        filterMap.put("/index.html","anon");
        filterMap.put("/index.html","user");
        filterMap.put("/register.html","anon");
        filterMap.put("/user/login","anon");
        filterMap.put("/user/register","anon");
        filterMap.put("/static/**","anon");
        filterMap.put("/**","authc");
  		filterMap.put("/c_add.html","perms[sys:c:save]");

        filterMap.put("/exit","logout");
```

### 在ShiroConfig.java中配置基于Cookie的rememberMe管理器

> ```java
> @Bean
> public CookieRememberMeManager getCookieRememberMeManager(){
>     CookieRememberMeManager rememberMeManager = new CookieRememberMeManager();
>     //cookie必须设置name
>     SimpleCookie cookie = new SimpleCookie("rememberMe");
>     cookie.setMaxAge(30*24*3600);
>     rememberMeManager.setCookie(cookie);
>     return rememberMeManager;
> }
> 
> @Bean
> public DefaultWebSecurityManager getDefaultWebSecurityManager(MyRealm realm){
>     DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
>     //securityManager要完成校验，需要realm
>     securityManager.setRealm(realm);
>     //设置缓存
>     securityManager.setCacheManager(getEhCacheManager());
>     //设置session
>     securityManager.setSessionManager(getDefaultWebSessionManager());
>     //设置rememberMe管理器
>     securityManager.setRememberMeManager(getCookieRememberMeManager());
>     return securityManager;
> }
> ```

### 登录认证时设置token“记住我”

> 登录页面
>
> ```html
> <form action="user/login">
>     <p>账号：<input type="text" name="username"></p>
>     <p>密码：<input type="text" name="password"></p>
>     <p>记住我：<input type="checkbox" name="rememberMe"></p>
>     <p><input type="submit" value="登录"></p>
> </form>
> ```
>
> 控制器
>
> ```java
> package com.example.demo.controller;
> 
> import com.example.demo.service.UserServiceImpl;
> import org.apache.shiro.crypto.hash.Md5Hash;
> import org.springframework.beans.factory.annotation.Autowired;
> import org.springframework.stereotype.Controller;
> import org.springframework.web.bind.annotation.RequestMapping;
> 
> import java.util.Random;
> 
> @Controller
> @RequestMapping("/user")
> public class UserController {
> 
>     @Autowired
>     private UserServiceImpl userService;
> 
>     @RequestMapping("/login")
>     public String login(String username,String password,boolean rememberMe){
>         try {
>             userService.checkLogin(username,password,rememberMe);
>             System.out.println("登录成功");
>             return "index.html";
>         } catch (Exception e) {
>             System.out.println("登录失败");
>             return "login";
>         }
>     }
>    //...
> }
> ```
>
> service
>
> ```java
> package com.example.demo.service;
> 
> import org.apache.shiro.SecurityUtils;
> import org.apache.shiro.authc.UsernamePasswordToken;
> import org.apache.shiro.subject.Subject;
> import org.springframework.stereotype.Service;
> 
> @Service
> public class UserServiceImpl {
>        public void checkLogin(String username, String password, boolean rememberMe) throws Exception{
>            Subject subject = SecurityUtils.getSubject();
>            UsernamePasswordToken token = new UsernamePasswordToken(username,password);
>            token.setRememberMe(rememberMe);
>            subject.login(token);
>        }
> }
> ```

## Shiro多Realm配置

### 使用场景

> 当Shiro进行权限管理，数据来自不同的数据源时，我们可以给SecurityManager配置多个Realm

### 多个Realm的处理方式

> 1.链式处理
>
> * 多个Realm一次进行认证
>
> 2.分支处理
>
> * 根据不同的条件从多个Realm中选择一个进行认证处理

### 多Realm配置（链式处理）

定义多个Realm

> UserRealm.java
>
> ```java
> package com.example.demo.realm;
> 
> import lombok.extern.slf4j.Slf4j;
> import org.apache.shiro.authc.*;
> import org.apache.shiro.authz.AuthorizationInfo;
> import org.apache.shiro.realm.AuthorizingRealm;
> import org.apache.shiro.subject.PrincipalCollection;
> 
> @Slf4j
> public class UserRealm extends AuthorizingRealm {
> 
>     @Override
>     public String getName() {
>         return "UserRealm";
>     }
> 
>     @Override
>     protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
>         return null;
>     }
> 
>     @Override
>     protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
>         log.info("-------UserRealm--------");
> 
>         UsernamePasswordToken token = (UsernamePasswordToken) authenticationToken;
>         String username = token.getUsername();
> 
>         SimpleAuthenticationInfo info = new SimpleAuthenticationInfo(username,"123456",getName());
>         return info;
>     }
> }
> ```
>
> ManagerRealm.java
>
> ```java
> package com.example.demo.realm;
> 
> import lombok.extern.slf4j.Slf4j;
> import org.apache.shiro.authc.*;
> import org.apache.shiro.authz.AuthorizationInfo;
> import org.apache.shiro.realm.AuthorizingRealm;
> import org.apache.shiro.subject.PrincipalCollection;
> 
> @Slf4j
> public class ManagerRealm extends AuthorizingRealm {
> 
>     @Override
>     public String getName() {
>         return "ManagerRealm";
>     }
> 
>     @Override
>     protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
>         return null;
>     }
> 
>     @Override
>     protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
>         log.info("-------ManagerRealm--------");
> 
>         UsernamePasswordToken token = (UsernamePasswordToken) authenticationToken;
>         String username = token.getUsername();
> 
>         SimpleAuthenticationInfo info = new SimpleAuthenticationInfo(username,"222222",getName());
>         return info;
>     }
> }
> ```

在ShiroConfig.java中为Securitymanager配置多个Realm

> ```java
> package com.example.demo.config;
> 
> import com.example.demo.realm.ManagerRealm;
> import com.example.demo.realm.UserRealm;
> import org.apache.shiro.realm.Realm;
> import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
> import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
> import org.springframework.context.annotation.Bean;
> import org.springframework.context.annotation.Configuration;
> 
> import java.util.ArrayList;
> import java.util.Collection;
> import java.util.HashMap;
> import java.util.Map;
> 
> @Configuration
> public class ShiroConfig {
> 
>     @Bean
>     public UserRealm getUserRealm(){
>         return new UserRealm();
>     }
> 
>     @Bean
>     public ManagerRealm getManagerRealm(){
>         return new ManagerRealm();
>     }
> 
>     @Bean
>     public DefaultWebSecurityManager getDefaultWebSecurityManager(){
>         DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
>         //securityManager要完成校验，需要realm
>         Collection<Realm> realms = new ArrayList<>();
>         realms.add(getUserRealm());
>         realms.add(getManagerRealm());
>         securityManager.setRealms(realms);
>         return securityManager;
>     }
> 	//....
> }
> ```

测试代码

> login.html
>
> ```html
> <form action="user/login">
>     <p>账号：<input type="text" name="username"></p>
>     <p>密码：<input type="text" name="password"></p>
>     <input type="submit" value="登录">
> </form>
> ```
>
> UserController.java
>
> ```java
> package com.example.demo.controller;
> 
> import lombok.extern.slf4j.Slf4j;
> import org.apache.shiro.SecurityUtils;
> import org.apache.shiro.authc.UsernamePasswordToken;
> import org.apache.shiro.subject.Subject;
> import org.springframework.stereotype.Controller;
> import org.springframework.web.bind.annotation.RequestMapping;
> 
> @Controller
> @RequestMapping("user")
> @Slf4j
> public class UserController {
> 
>     @RequestMapping("/login")
>     public String login(String username,String password){
>       log.info("----UserController---");
>       try {
>           UsernamePasswordToken token = new UsernamePasswordToken(username,password);
>           Subject subject = SecurityUtils.getSubject();
>           subject.login(token);
>           return "index";
>       }catch (Exception e){
>           return "login";
>       }
>     }
> }
> ```

### 多Realm配置（分支处理）

> 根据不同的条件执行不同的Realm

实现 案例：用户不同身份登录执行不同的Realm

> 自定义Realm（UserRealm、ManagerRealm）
>
> * 当登录页面选择“普通用户”登录，则执行UserRealm的认证
> * 当登录页面选择“管理员”登录，则执行ManagerRealm的认证
>
> Realm的声明及配置
>
> 自定义Token
>
> ```java
> package com.example.demo.config;
> 
> import org.apache.shiro.authc.UsernamePasswordToken;
> 
> public class MyToken extends UsernamePasswordToken {
>     private String loginType;
> 
>     public MyToken(String username,String password,String loginType){
>         super(username,password);
>         this.loginType = loginType;
>     }
> 
>     public String getLoginType() {
>         return loginType;
>     }
> 
>     public void setLoginType(String loginType) {
>         this.loginType = loginType;
>     }
> }
> ```
>
> 自定义认证器
>
> ```java
> package com.example.demo.config;
> 
> import lombok.extern.slf4j.Slf4j;
> import org.apache.shiro.authc.AuthenticationException;
> import org.apache.shiro.authc.AuthenticationInfo;
> import org.apache.shiro.authc.AuthenticationToken;
> import org.apache.shiro.authc.pam.ModularRealmAuthenticator;
> import org.apache.shiro.realm.Realm;
> 
> import java.util.ArrayList;
> import java.util.Collection;
> 
> @Slf4j
> public class MymodularRealmAuthenticator extends ModularRealmAuthenticator {
>     @Override
>     protected AuthenticationInfo doAuthenticate(AuthenticationToken authenticationToken) throws AuthenticationException {
>         log.info("-----MymodularRealmAuthenticator---------");
> 
>         Collection<Realm> realms = this.getRealms();
>         MyToken myToken = (MyToken) authenticationToken;
> 
>         String loginType = myToken.getLoginType();
> 
>         log.info("-------------loginType:"+loginType);
>         Collection<Realm> typeRealms = new ArrayList<>();
>         for (Realm realm : realms){
>             if (realm.getName().startsWith(loginType)){
>                 typeRealms.add(realm);
>             }
>         }
>         if (typeRealms.size() == 1){
>             return this.doSingleRealmAuthentication((Realm) 				typeRealms.iterator().next(),authenticationToken);
>         }else {
>             return this.doMultiRealmAuthentication(typeRealms,authenticationToken);
>         }
> 
>     }
> }
> ```
>
> 配置自定义认证器
>
> ```java
> package com.example.demo.config;
> 
> import com.example.demo.realm.ManagerRealm;
> import com.example.demo.realm.UserRealm;
> import org.apache.shiro.realm.Realm;
> import org.apache.shiro.spring.web.ShiroFilterFactoryBean;
> import org.apache.shiro.web.mgt.DefaultWebSecurityManager;
> import org.springframework.context.annotation.Bean;
> import org.springframework.context.annotation.Configuration;
> 
> import java.util.ArrayList;
> import java.util.Collection;
> import java.util.HashMap;
> import java.util.Map;
> 
> @Configuration
> public class ShiroConfig {
> 
>     @Bean
>     public UserRealm getUserRealm(){
>         return new UserRealm();
>     }
> 
>     @Bean
>     public ManagerRealm getManagerRealm(){
>         return new ManagerRealm();
>     }
> 
>     @Bean
>     public MymodularRealmAuthenticator getMymodularRealmAuthenticator(){
>         return new MymodularRealmAuthenticator();
>     }
> 
>     @Bean
>     public DefaultWebSecurityManager getDefaultWebSecurityManager(){
>         DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
> 
>         //配置自定义认证器(防在realms设置之前)
>         securityManager.setAuthenticator(getMymodularRealmAuthenticator());
> 
>         //securityManager要完成校验，需要realm
>         Collection<Realm> realms = new ArrayList<>();
>         realms.add(getUserRealm());
>         realms.add(getManagerRealm());
>         securityManager.setRealms(realms);
>         return securityManager;
>     }
> 
>  	//...
> }
> ```

