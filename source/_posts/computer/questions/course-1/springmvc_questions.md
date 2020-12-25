---
title: SpringMVC面试题
date: 2020/12/17 11:59
categories: 
	- [计算机, 面试, 面试题]
tags:
	- 面试题
---

### springmvc的执行流程

> 1. 发送请求到前端控制器DispatcherServlet
> 2. 前端控制器请求处理映射器HandlerMapping查找Handler
> 3. 处理映射器向前端控制器返回Handler
> 4. 前端控制器调用处理适配器去执行Handler
> 5. Handler执行完给适配器返回ModelAndView
> 6. 处理适配器向前端控制器返回ModelAndView
> 7. 前端控制器请求视图解析器ViewResolver去进行视图解析
> 8. 视图解析器向前端控制器返回View视图
> 9. 前端控制器进行视图渲染
> 10. 前端控制器向用户响应结果