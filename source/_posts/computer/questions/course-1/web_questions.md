---
title: web面试题
date: 2020/12/17 12:05
categories:
	- [计算机, 面试, 面试题]
tags:
	- 面试题
---



## Get和Post的区别

get和post都是http的请求方式，用户通过不同的请求方式来完成对资源的不同操作，get、post、put、delete分别对应着资源的查、改、增、删四个操作，一般来说get用来获取资源，post用于更新资源

> 1. **get请求提交的数据会在地址栏显示出来，post请求不会**
> 2. **由于地址栏长度有限，导致get传输的数据有限，而post不会**
> 3. **安全性，post安全性比get高**

## 对Servlet的理解

> **Servlet是用Java程序编写的服务端程序，而这些Servlet都要实现Servlet接口，其主要功能是用于交互式的浏览和修改数据，生成动态网页**
>
> **HttpServlet重写doget和dopost方法或者重写service方式可以实现对get和post请求的响应**

## Servlet的生命周期

> 加载Servlet的生命周期，调用init()进行初始化，然后调用service()方法来处理客户端的请求，最后调用destroy()终止

## forward与redirect的区别

> 1. forward地址栏不会发生改变，redirect地址栏会发生改变
> 2. forward是服务器上的行为，redirect是客户端的行为
> 3. forward是一次请求完成的，redirect是两次请求完成的
> 4. forward效率较高

## JSP与Servlet的相同点与不同点

> 相同点：JSP是Servlet的扩展，所有的JSP文件最终都会被翻译成一个继承HttpServlet类，也就是说JSP最终也是一个Servlet
>
> 不同点：JSP侧重于视图，Servlet侧重于控制逻辑

## JSP的九大内置对象与四大作用域

九大内置对象

> **Request：客户端的请求**
>
> **Response：网页传回客户端的响应**
>
> **PageContext：网页属性的管理**
>
> **Session：会话**
>
> **Application：servlet正在执行的内容**
>
> **Out：传递回应的输出**
>
> **Config：servlet的架构不见**
>
> **Page：Jsp网页本身**
>
> **Exception：针对错误的网页**

四大作用域

> **page：只在一个页面保留数据**
>
> **request：只在一个请求中保存数据**
>
> **Session：再一次会话中保存数据，仅供单个用户使用**
>
> **Application：在整个服务器中保存数据，全部用户共享**

## Cookie与Session的区别

**cookie和session都是会话跟踪技术**

不同点

> 1. **cookie的数据是存在客户端的，session的数据是存在服务器上的**
> 2. **cookie是不安全的**
> 3. **session会在一定时间内存放在服务器上，当访问增多时，会占用服务器的性能**
> 4. **单个cookie的保存数据不能超过4k,很多浏览器一个站点最多存放20个cookie**

建议

> **将登录信息等重要信息保存在session中，其他信息如需保留，可以放在cookie中，如：购物车**
>
> **购物车最好使用cookie，范式cookie实在客户端禁用的，只是要我们需要使用cookie+数据库的方式实现，当从cookie中不能取出数据时，就从数据库中取**