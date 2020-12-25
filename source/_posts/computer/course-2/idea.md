---
title: Idea
date: 2020/12/17 12:38
categories:
	- [计算机, 工具]
tags:
	- Idea
---




# 快捷方法

## 补全for循环

>length.for
>for(int i=0;i<length;i++){}

## 补全返回类型及变量

> **方法().var**

## 输出

> **sout**
>
> **System.out.println();**

## main方法

> **psvm 或 main**

## 方法注释

> 在方法上面一行输入/**回车，就会出现以下注释
>
> ```java
>   /**
>      * 
>      * @param 
>      * @param 
>      * @return
>      */
> ```

# 快捷键

## 代码快捷键

### 重写方法

> **Ctrl+O**

### 构造函数

> **ALT+INSERT**

### 自动导入变量定义

> **Ctrl+Alt+V**

### 大小写转化

> **Ctrl+Shift+U**

### 查看源码

> **Ctrl+B**

### 纠错，导包，提示

> **Alt+Enter**

## 功能快捷键

### 格式化代码

>**Ctrl+Shift+L**

### 删除一行

>**Ctrl+y**

### 注释多行

>  **Ctrl+Shift+/**

### 注释单行

> **Ctrl+/**

### 复制到下一行

> **Ctrl+D**

### 全局搜索

> **CTRL+SHIFT +F**

### 上/下移一行

> **Alt+Shift+Up/Down**

### 替换文本

> **Ctrl+R**

### 方法展开、折叠

**当前方法**

> **Ctrl+"+/-"**

**全部**

> **Ctrl+Shift+"+/-"**

# 其他快捷键

## 复制路径

> **Ctrl+Shift+C**

# 其他操作

## 设置作者注释

1. **准备好模板**

   ```java
   /**
    * @Author 张三
    * @Description 
    * @Date $date$ $time$
    **/
   ```

2. **添加模板**

   点击File → settings→Editor→live Templates

   1. 点击“+”，先添加一个群组,点击Template Group，取名为MyTemplate

   2. 添加一个模板名称，点击Live Template

   3. Abbreviation框填入“*”，Description框填入“注释模板”，Template text框填入刚才的模板

   4. 点击Define，选择Java→Declaration

   5. 点击Edit variables进行如下配置，点击OK

      | Name | Expression |
      | :--: | :--------: |
      | date |   date()   |
      | time |   time()   |

3. 测试

   在类名前输入*再按下Tab键，如下便成功

   ```java
   /**
    * @Author 张三
    * @Description 
    * @Date 2020/10/27 16:06
    **/
   ```

   在方法名前面输入@Param跟@return就会有值

## 设置方法注释

此操作在方法外也能获取参数

1. 准备好模板

   ```java
   *
   $params$
   * @return 
   * @Author zhangsan 
   * @Date $date$       
   **/
   ```

2. 添加模板

   >Abbreviation框填入 "method"(自定义)
   >
   >Options下面的Expand with设置为Enter
   
   |  Name  |     Expression     |
   | :----: | :----------------: |
   | params |                    |
   | return | methodReturnType() |
   
   > params中Expression填入以下脚本
   
   ```java
   groovyScript("def result=''; def params=\"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); for(i = 0; i < params.size(); i++) {result+=' * @param ' + params[i] + ((i < params.size() - 1) ? '\\n' : '')}; return result", methodParameters())
   ```

3. 测试

   >idea默认的生成注释方式为 /*+模板名+快捷键
   >
   >在方法名上输入/*method+enter
   
   ```java
   /**
   * @param 参数
   * @return 结果
   * @Author zhangsan
   * @Date 2020/11/18
   **/
   ```
