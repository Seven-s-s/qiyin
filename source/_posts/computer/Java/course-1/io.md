---
title: IO流
date: 2020/12/15 19:21
categories:
- [计算机, 计算机语言, Java]
tags: 
- Java
---


# 字节流

## 输出流

### 方法

> **FileOutputStream fos = new FileOutputStream(文件名);**
>
> **write()：往文件里面写数据**
>
> **getBytes()：返回字符串对应的字节数组**
>
> **close()：释放资源**

### 换行符

>**Window：\r\n**
>
>**Linux：\n**
>
>**Mac：\r**

### 例子

向file.txt文件写入HelloWorld

```java
FileOutputStream fos = new FileOutputStream("D://file.txt");
String s = "Hello World";
fos.write(s.getBytes());
fos.close();
```



## 输入流

### 方法

>**FileInputStream fis = new FileInputStream(文件名);**
>
>**read()：读取一个字节，如果有参数并且是byte数组，则返回的数据长度**
>
>**close()：释放资源**

### 例子

> **向file.txt读取数据**
>
> **方法一**
>
> ```java
> FileInputStream fis = new FileInputStream("D://file.txt");
> byte b[] = new byte[1024];
> int length = fis.read(b);
> System.out.println(new String(b));
> System.out.println(length);
> fis.close();
> ```
>
> **方法二**
>
> ```java
> FileInputStream fis = new FileInputStream("D://file.txt");
> int data = 0;
> while ((data=fis.read())!=-1) {
>  System.out.print((char)data);
> }
> fis.close();
> ```

# 字符流

## 输出流

### 方法

> **FileWriter fw = new FileWriter();**
>
> **write()：写数据**
>
> **flush()：刷新**
>
> **close()：释放资源**

#### 例子

> **向file.txt写入Hello World**
>
> ```java
> FileWriter fw = new FileWriter("D://file.txt");
> String s = "Hello World";
> fw.write(s);
> fw.flush();//不刷新将不会写入
> fw.close();//关闭资源前会自动刷新
> ```

## 输入流

### 方法

> **FileReader fr = new FileReader();**
>
> **read()：读取数据**

### 例子

> 方法一
>
> ```java
> FileReader fr = new FileReader("D://file.txt");
> char c[] = new char[1024];
> fr.read(c);
> System.out.println(new String(c));
> ```
>
> 方法二
>
> ``````java
> FileReader fr = new FileReader("D://file.txt");
> int data = 0;
> while ((data=fr.read())!=-1) {
>  System.out.print((char)data);
> }
> fr.close();
> ``````
