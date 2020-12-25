---
title: 集合
date: 2020/12/15 19:21
categories:
- [计算机, 计算机语言, Java]
tags: 
- Java
---

## Set

### 概念

无序的，不可重复的集合

要保证元素的唯一性，需要重写hashCode()和equals()方法

### 方法

>​	**添加：add()**
>
>​	**删除：remove()**

### 取值方式

```java
Set<String> set = new HashSet<>();
set.add("apple");
set.add("banana");
set.add("pear");
```

>**foreach遍历**
>
>```java
>for (String s : set){
>System.out.println(s);
>}
>```
>
>**迭代器遍历**
>
>```java
>Iterator<String> iterator = set.iterator();
>while (iterator.hasNext()){
>String s = iterator.next();
>System.out.println(s);
>}
>```

## List

### 概念

有序的，可重复的集合

### 方法

>**添加：add()**
>
>**删除：remove()**
>
>**获取：get()**
>
>**长度：size()**

### 取值方式

```java
List<String> list = new LinkedList<>();
list.add("apple");
list.add("banana");
list.add("pear");
```

>**for循环**
>
>```java
>for (int i = 0;i < list.size();i++){
>System.out.println(list.get(i));
>}
>```
>
>**foreach循环**
>
>```java
>for (String s : list){
>System.out.println(s);
>}
>```
>
>**迭代器循环**
>
>```java
>Iterator<String> iterator = list.iterator();
>while (iterator.hasNext()){
>String s = iterator.next();
>System.out.println(s);
>}
>```

## Map

### 概念

无序，键值对集合（映射关系），键不能重复，值可以重复，键可以为null，值也可为null。

### 方法

>**添加：put(key,value)**
>
>**获取：get(key)**
>
>**删除：remove(key)**

### 获取所有的值

```java
Map<String,String> map = new HashMap<>();
map.put("apple","good");
map.put("banana","better");
map.put("pear","best");
Set<String> keySet = map.keySet();
```

拿到所有的key，遍历key，根据key拿值

**foreach循环遍历**

```java
for (String key : keySet){
    System.out.println(map.get(key));
}
```

**迭代器遍历**

```java
while (iterator.hasNext()){
    String s = iterator.next();
    System.out.println(map.get(s));
}
```

### 获取所有的映射关系集合

```java
Set<Map.Entry<String,String>> entries = map.entrySet();
```

**for循环**

```java
for (Map.Entry<String,String> entry:entries){
    System.out.println(entry.getKey()+" "+entry.getValue());
}
```

**迭代器遍历**

```java
Iterator<Map.Entry<String,String>> iterator = entries.iterator();
while (iterator.hasNext()){
    Map.Entry<String,String> entry = iterator.next();
    System.out.println(entry.getKey()+" "+entry.getValue());
}
```

## 迭代器

### Iterator

#### 方法

>**next()：返回迭代中的下一个元素**
>
>**hasNext()：如果迭代器有下一个元素，返回true**

### ListIterator

#### 方法

>**next()**
>
>**hasNext()**
>
>**previous()：返回列表中的上一个元素** 
>
>**hasPrevious()：如果迭代在相反方向上遍历有更多元素，返回true**
>
>**add()：向指定的元素插入列表**

## **Array和List的相互转换**

### **array转list**

>```java
> String[] array = {"string1","string2","string3"};
>//Arrays.asList产生一个Arrays内置的类 其中数组用final表示！
>List<String> list = Arrays.asList(array);
>// list.add("string4");  所以不能往其中添加元素
>for(String s:list){
>    System.out.println(s);
>}
>```

### **list转array**

> ```java
> List<String> list = new ArrayList<>();
> list.add("t1");
> list.add("t2");
> list.add("t3");
> //toArray中不指定参数 只能转换为Object类型
> String[] strings = list2.toArray(new String[list2.size()]);
> for (String v:strings) {
>     System.out.println(v);
> }
> ```
