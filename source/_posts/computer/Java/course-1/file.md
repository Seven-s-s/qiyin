---
title: file
date: 2020/12/15 19:21
categories:
- [计算机, 计算机语言, Java]
tags: 
- Java

---

# 方法

>**File file = new File(路径);**
>
>**createNewFile()：创建文件，如果有没有该文件就创建文件并返回true，否则返回false**
>
>**mkdir()：创建目录，如果有该目录则创建该目录并返回true，否则返回false**
>
>**mkdirs()：创建多级目录，如果有这多级目录则创建多级目录并返回true，否则返回false**
>
>**isDirectory()：判断file是否为目录**
>
>**isFile()：判断file是否为文件**
>
>**exists()：判断file是否存在**
>
>**getAbsolutePath()：返回该路径的绝对路径名字符串**
>
>**getPath()：返回该路径的路径名字符串**
>
>**getName()：返回该路径表示的文件或目录**
>
>**list()：返回该目录中的文件和目录的名称字符串数组**
>
>**listFiles()：返回该目录中的文件和目录的file对象数组**
>
>**delete()：删除该目录，如果目录中有内容，不能直接删除，先要删除目录中的内容，在删除目录**

# 例子

1. 在D盘下创建文件file.txt

   ```java
   File file = new File("D://file.txt");
   try {
      boolean b = file.createNewFile();
   } catch (IOException e) {
       e.printStackTrace();
   }
   System.out.println(b);
   ```

2. 在D盘下创建目录file

   ```java
   File file = new File("D://file");
   boolean b = file.mkdir();
   System.out.println(b);
   ```

   

3. 在D盘下创建目录file，在file目录下在创建目录files

   ```java
   File file = new File("D://file//files");
   boolean b = file.mkdirs();
   System.out.println(b);
   ```
