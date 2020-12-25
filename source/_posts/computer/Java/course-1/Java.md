---
title: Java基础
date: 2020/12/15 19:21
categories:
- [计算机, 计算机语言, Java]
tags: 
- Java
---

# 基本数据类型

| 数据类型 |    占用字节     |     取值范围     |    默认值    | 包装类型  |
| :------: | :-------------: | :--------------: | :----------: | :-------: |
| boolean  | 只有true和false |   true、false    |    false     |  Boolean  |
|   byte   |     1(8位）     |     -128~127     |      0       |   Byte    |
|  short   |     2(16位)     |   -32768~32767   |      0       |   Short   |
|   int    |     4(32位)     |   -2^31~2^31-1   |      0       |  Integer  |
|   long   |        8        |   -2^63~2^63-1   |     0.0l     |   Long    |
|  float   |        4        |  3.4E-45~1.4E38  |     0.0f     |   Float   |
|  double  |        8        | 4.9E-324~1.8E308 |      0       |  Double   |
|   char   |        2        |     0~65535      | \u0000(空格) | Character |

# 访问控制修饰符

| 修饰符  | 当前类 | 同包 | 子类 | 其他包 |
| :-----: | :----: | :--: | :--: | :----: |
| public  |   √    |  √   |  √   |   √    |
| protect |   √    |  √   |  √   |   ×    |
| default |   √    |  √   |  ×   |   ×    |
| private |   √    |  ×   |  ×   |   ×    |

# Character类

* 方法

> **isDigit()**
> 是否是一个数字字符
>
> **isWhitespace()**
> 是否是一个空白字符
>
> **isUpperCase()**
> 是否是大写字母
>
> **isLowerCase()**
> 是否是小写字母
>
> **toUpperCase() **
> 指定字母的大写形式
>
> **toLowerCase()**
> 指定字母的小写形式
>
> **toString()**
> 返回字符的字符串形式，字符串的长度仅为1

# String类

## 常用方法

> char charAt(int index)：返回指定索引处的 char 值。
>
> int compareTo(String anotherString)：按字典顺序比较两个字符串。
>
> String concat(String str)：将指定字符串连接到此字符串的结尾。
>
> byte[] getBytes()：使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。
>
> int indexOf(int ch)：返回指定字符在此字符串中第一次出现处的索引。
>
> boolean matches(String regex)：告知此字符串是否匹配给定的正则表达式。
>
> String replace(char oldChar, char newChar)：返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。
>
> String[] split(String regex)：根据给定正则表达式的匹配拆分此字符串。
>
> char[] toCharArray()：将此字符串转换为一个新的字符数组。
>
> String substring(int beginIndex, int endIndex)：返回一个新字符串，它是此字符串的一个子字符串。
>
> contains(CharSequence chars)：判断是否包含指定的字符系列。
>
> isEmpty()：判断字符串是否为空。

## 其他方法

>int compareTo(Object o)：把这个字符串和另一个对象比较。
>
>int compareToIgnoreCase(String str)：按字典顺序比较两个字符串，不考虑大小写。
>
>static String copyValueOf(char[] data)：返回指定数组中表示该字符序列的 String。
>
>static String copyValueOf(char[] data, int offset, int count)：返回指定数组中表示该字符序列的 String。
>
>boolean equals(Object anObject)：将此字符串与指定的对象比较。
>
>boolean equalsIgnoreCase(String anotherString)：将此 String 与另一个 String 比较，不考虑大小写。
>
>byte[] getBytes(String charsetName)：使用指定的字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。
>
>void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin)：将字符从此字符串复制到目标字符数组。
>
>int hashCode()：返回此字符串的哈希码。
>
>int indexOf(int ch, int fromIndex)：返回在此字符串中第一次出现指定字符处的索引，从指定的索引开始搜索。
>
>int indexOf(String str)：返回指定子字符串在此字符串中第一次出现处的索引。
>
>int indexOf(String str, int fromIndex)：返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始。
>
>int lastIndexOf(int ch)：返回指定字符在此字符串中最后一次出现处的索引。
>
>int lastIndexOf(int ch, int fromIndex)：返回指定字符在此字符串中最后一次出现处的索引，从指定的索引处开始进行反向搜索。
>
>int lastIndexOf(String str)：返回指定子字符串在此字符串中最右边出现处的索引。
>
>int lastIndexOf(String str, int fromIndex)：返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索。
>
>int length()：返回此字符串的长度。
>
>boolean regionMatches(boolean ignoreCase, int toffset, String other, int ooffset, int len)：测试两个字符串区域是否相等。
>
>boolean regionMatches(int toffset, String other, int ooffset, int len)：测试两个字符串区域是否相等。
>
>String replaceAll(String regex, String replacement)：使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。 String replaceFirst(String regex, String replacement)：使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。
>
>String[] split(String regex, int limit)：根据匹配给定的正则表达式来拆分此字符串。
>
>boolean startsWith(String prefix)：测试此字符串是否以指定的前缀开始。
>
>boolean startsWith(String prefix, int toffset)：测试此字符串从指定索引开始的子字符串是否以指定前缀开始。
>
>CharSequence subSequence(int beginIndex, int endIndex)：：返回一个新的字符序列，它是此序列的一个子序列。
>
>String substring(int beginIndex)：返回一个新的字符串，它是此字符串的一个子字符串。
>
>String toLowerCase()：使用默认语言环境的规则将此 String 中的所有字符都转换为小写。
>
>String toLowerCase(Locale locale)：：使用给定 Locale 的规则将此 String 中的所有字符都转换为小写。
>
>String toString()：：返回此对象本身（它已经是一个字符串！）。
>
>String toUpperCase()：使用默认语言环境的规则将此 String 中的所有字符都转换为大写。
>
>String toUpperCase(Locale locale)：使用给定 Locale 的规则将此 String 中的所有字符都转换为大写。
>
>String trim()：返回字符串的副本，忽略前导空白和尾部空白。
>
>static String valueOf(primitive data type x)：返回给定data type类型x参数的字符串表示形式。

## 例子

1. split

> **split(String regex, int limit)**
>
> **regex** -- 正则表达式分隔符。
>
> **limit** -- 分割的份数。

```java
String s = "1,2,3,4,5";
String num[] = s.split(",");
```

2. subSequence

> **subSequence(int beginIndex, int endIndex)**
>
> **beginIndex** -- 起始索引（包括）。
>
> **endIndex** -- 结束索引（不包括）。

```java
String s = "HelloWord";
System.out.println(s.subSequence(1, 4));
```

> 输出：ell

# StringBuffer类

* 方法

> **public StringBuffer append(String s)**
> 将指定的字符串追加到此字符序列。
>
> **public StringBuffer reverse()**
> 将此字符序列用其反转形式取代。
>
> **public delete(int start, int end)**
> 移除此序列的子字符串中的字符。
>
> **public insert(int offset, int i)**
> 将 int 参数的字符串表示形式插入此序列中。
>
> **replace(int start, int end, String str)**
> 使用给定 String 中的字符替换此序列的子字符串中的字符。

# String与StringBuilder

>**StringBuilder拼接字符串耗时耗空间，需要用StringBuilder**
>

```java
String s;
StringBuilder sb = new StringBuilder(s);
```

>**拼接sb.append(字符串)**
>

```java
String s = sb.toString();
```

# Math类

>**xxxValue()**
>将 Number 对象转换为xxx数据类型的值并返回。
>
>**compareTo()**
>将number对象与参数比较。
>
>**equals()**
>判断number对象是否与参数相等。
>
>**valueOf()**
>返回一个 Number 对象指定的内置数据类型
>
>**toString()**
>以字符串形式返回值。
>
>**parseInt()**
>将字符串解析为int类型。
>
>**abs()**
>返回参数的绝对值。
>
>**ceil()**
>返回大于等于( >= )给定参数的的最小整数，类型为双精度浮点型。
>
>**floor()**
>返回小于等于（<=）给定参数的最大整数 。
>
>**rint()**
>返回与参数最接近的整数。返回类型为double。
>
>**round()**
>它表示**四舍五入**，算法为 **Math.floor(x+0.5)**，即将原来的数字加上 0.5 后再向下取整，所以，Math.round(11.5) 的结果为12，Math.round(-11.5) 的结果为-11。
>
>**min()**
>返回两个参数中的最小值。
>
>**max()**
>返回两个参数中的最大值。
>
>**pow()**
>返回第一个参数的第二个参数次方。
>
>**sqrt()**
>求参数的算术平方根。
>
>**random()**
>返回一个随机数

# Arrays 类

> **Arrays.asList()**
> 可以从 Array 转换成 List。可以作为其他集合类型构造器的参数
>
> **Arrays.binarySearch()**
> 在一个已排序的或者其中一段中快速查找
>
> **Arrays.copyOf()**
> 如果你想扩大数组容量又不想改变它的内容的时候可以使用这个方法
>
> **Arrays.copyOfRange()**
> 可以复制整个数组或其中的一部分
>
> **Arrays.deepEquals()**
> **Arrays.deepHashCode()**
> Arrays.equals/hashCode的高级版本，支持子数组的操作
>
> **Arrays.equals()**
> 如果你想要比较两个数组是否相等，应该调用这个方法而不是数组对象中的 equals方法（数组对象中没有重写equals()方法，所以这个方法之比较引用而不比较内容）。这个方法集合了Java 5的自动装箱和无参变量的特性，来实现将一个变量快速地传给 equals()方法——所以这个方法在比较了对象的类型之后是直接传值进去比较的
>
> **Arrays.fill()**
> 用一个给定的值填充整个数组或其中的一部分
>
> **Arrays.hashCode()**
> 用来根据数组的内容计算其哈希值（数组对象的hashCode()不可用）。这个方法集合了Java 5的自动装箱和无参变量的特性，来实现将一个变量快速地传给 Arrays.hashcode方法——只是传值进去，不是对象
>
> **Arrays.sort()**
> 对整个数组或者数组的一部分进行排序。也可以使用此方法用给定的比较器对对象数组进行排序
>
> **Arrays.toString()**
> 打印数组的内容

# 日期时间

## 基本方法

> **boolean after(Date date)**
> 若当调用此方法的Date对象在指定日期之后返回true,否则返回false。
>
> **boolean before(Date date)**
> 若当调用此方法的Date对象在指定日期之前返回true,否则返回false。
>
> **int compareTo(Date date)**
> 比较当调用此方法的Date对象和指定日期。两者相等时候返回0。调用对象在指定日期之前则返回负数。调用对象在指定日期之后则返回正数。
>
> **boolean equals(Object date)**
> 当调用此方法的Date对象和指定日期相等时候返回true,否则返回false。
>
> **long getTime( )** 
> 返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数。
>
> **void setTime(long time)**
> 用自1970年1月1日00:00:00 GMT以后time毫秒数设置时间和日期。
>
> **String toString( )** 
> 把此 Date 对象转换为以下形式的 String： dow mon dd hh:mm:ss zzz yyyy 其中： dow 是一周中的某一天 (Sun, Mon, Tue, Wed, Thu, Fri, Sat)。

## 日期和时间的格式化编码

| **字母** | **描述**                 | **示例**                |
| :------- | :----------------------- | :---------------------- |
| G        | 纪元标记                 | AD                      |
| y        | 四位年份                 | 2001                    |
| M        | 月份                     | July or 07              |
| d        | 一个月的日期             | 10                      |
| h        | A.M./P.M. (1~12)格式小时 | 12                      |
| H        | 一天中的小时 (0~23)      | 22                      |
| m        | 分钟数                   | 30                      |
| s        | 秒数                     | 55                      |
| S        | 毫秒数                   | 234                     |
| E        | 星期几                   | Tuesday                 |
| D        | 一年中的日子             | 360                     |
| F        | 一个月中第几周的周几     | 2 (second Wed. in July) |
| w        | 一年中第几周             | 40                      |
| W        | 一个月中第几周           | 1                       |
| a        | A.M./P.M. 标记           | PM                      |
| k        | 一天中的小时(1~24)       | 24                      |
| K        | A.M./P.M. (0~11)格式小时 | 10                      |
| z        | 时区                     | Eastern Standard Time   |
| '        | 文字定界符               | Delimiter               |
| "        | 单引号                   | `                       |

## String与Date自定义格式转化

```java
Date date = new Date()
```

>获取当前时间返回一个毫秒值

```java
Long time = System.currentTimeMills();
date.setTime(time);
```

>将date转化为String

```java
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
String s = sdf.format(date);
```

>将String转化为date

```java
String s = "yyyy-MM-dd HH:mm:ss";
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
date = sdf.parse(s);
```

# 正则表达式

| 字符          | 说明                                                         |
| :------------ | :----------------------------------------------------------- |
| \             | 将下一字符标记为特殊字符、文本、反向引用或八进制转义符。例如，"n"匹配字符"n"。"\n"匹配换行符。序列"\\\\"匹配"\\"，"\\("匹配"("。 |
| ^             | 匹配输入字符串开始的位置。如果设置了 **RegExp** 对象的 **Multiline** 属性，^ 还会与"\n"或"\r"之后的位置匹配。 |
| $             | 匹配输入字符串结尾的位置。如果设置了 **RegExp** 对象的 **Multiline** 属性，$ 还会与"\n"或"\r"之前的位置匹配。 |
| *             | 零次或多次匹配前面的字符或子表达式。例如，zo* 匹配"z"和"zoo"。* 等效于 {0,}。 |
| +             | 一次或多次匹配前面的字符或子表达式。例如，"zo+"与"zo"和"zoo"匹配，但与"z"不匹配。+ 等效于 {1,}。 |
| ?             | 零次或一次匹配前面的字符或子表达式。例如，"do(es)?"匹配"do"或"does"中的"do"。? 等效于 {0,1}。 |
| {*n*}         | *n* 是非负整数。正好匹配 *n* 次。例如，"o{2}"与"Bob"中的"o"不匹配，但与"food"中的两个"o"匹配。 |
| {*n*,}        | *n* 是非负整数。至少匹配 *n* 次。例如，"o{2,}"不匹配"Bob"中的"o"，而匹配"foooood"中的所有 o。"o{1,}"等效于"o+"。"o{0,}"等效于"o*"。 |
| {*n*,*m*}     | *m* 和 *n* 是非负整数，其中 *n* <= *m*。匹配至少 *n* 次，至多 *m* 次。例如，"o{1,3}"匹配"fooooood"中的头三个 o。'o{0,1}' 等效于 'o?'。注意：您不能将空格插入逗号和数字之间。 |
| ?             | 当此字符紧随任何其他限定符（*、+、?、{*n*}、{*n*,}、{*n*,*m*}）之后时，匹配模式是"非贪心的"。"非贪心的"模式匹配搜索到的、尽可能短的字符串，而默认的"贪心的"模式匹配搜索到的、尽可能长的字符串。例如，在字符串"oooo"中，"o+?"只匹配单个"o"，而"o+"匹配所有"o"。 |
| .             | 匹配除"\r\n"之外的任何单个字符。若要匹配包括"\r\n"在内的任意字符，请使用诸如"[\s\S]"之类的模式。 |
| (*pattern*)   | 匹配 *pattern* 并捕获该匹配的子表达式。可以使用 **$0…$9** 属性从结果"匹配"集合中检索捕获的匹配。若要匹配括号字符 ( )，请使用"\("或者"\)"。 |
| (?:*pattern*) | 匹配 *pattern* 但不捕获该匹配的子表达式，即它是一个非捕获匹配，不存储供以后使用的匹配。这对于用"or"字符 (\|) 组合模式部件的情况很有用。例如，'industr(?:y\|ies) 是比 'industry\|industries' 更经济的表达式。 |
| (?=*pattern*) | 执行正向预测先行搜索的子表达式，该表达式匹配处于匹配 *pattern* 的字符串的起始点的字符串。它是一个非捕获匹配，即不能捕获供以后使用的匹配。例如，'Windows (?=95\|98\|NT\|2000)' 匹配"Windows 2000"中的"Windows"，但不匹配"Windows 3.1"中的"Windows"。预测先行不占用字符，即发生匹配后，下一匹配的搜索紧随上一匹配之后，而不是在组成预测先行的字符后。 |
| (?!*pattern*) | 执行反向预测先行搜索的子表达式，该表达式匹配不处于匹配 *pattern* 的字符串的起始点的搜索字符串。它是一个非捕获匹配，即不能捕获供以后使用的匹配。例如，'Windows (?!95\|98\|NT\|2000)' 匹配"Windows 3.1"中的 "Windows"，但不匹配"Windows 2000"中的"Windows"。预测先行不占用字符，即发生匹配后，下一匹配的搜索紧随上一匹配之后，而不是在组成预测先行的字符后。 |
| *x*\|*y*      | 匹配 *x* 或 *y*。例如，'z\|food' 匹配"z"或"food"。'(z\|f)ood' 匹配"zood"或"food"。 |
| [*xyz*]       | 字符集。匹配包含的任一字符。例如，"[abc]"匹配"plain"中的"a"。 |
| [^*xyz*]      | 反向字符集。匹配未包含的任何字符。例如，"[^abc]"匹配"plain"中"p"，"l"，"i"，"n"。 |
| [*a-z*]       | 字符范围。匹配指定范围内的任何字符。例如，"[a-z]"匹配"a"到"z"范围内的任何小写字母。 |
| [^*a-z*]      | 反向范围字符。匹配不在指定的范围内的任何字符。例如，"[^a-z]"匹配任何不在"a"到"z"范围内的任何字符。 |
| \b            | 匹配一个字边界，即字与空格间的位置。例如，"er\b"匹配"never"中的"er"，但不匹配"verb"中的"er"。 |
| \B            | 非字边界匹配。"er\B"匹配"verb"中的"er"，但不匹配"never"中的"er"。 |
| \c*x*         | 匹配 *x* 指示的控制字符。例如，\cM 匹配 Control-M 或回车符。*x* 的值必须在 A-Z 或 a-z 之间。如果不是这样，则假定 c 就是"c"字符本身。 |
| \d            | 数字字符匹配。等效于 [0-9]。                                 |
| \D            | 非数字字符匹配。等效于 [^0-9]。                              |
| \f            | 换页符匹配。等效于 \x0c 和 \cL。                             |
| \n            | 换行符匹配。等效于 \x0a 和 \cJ。                             |
| \r            | 匹配一个回车符。等效于 \x0d 和 \cM。                         |
| \s            | 匹配任何空白字符，包括空格、制表符、换页符等。与 [ \f\n\r\t\v] 等效。 |
| \S            | 匹配任何非空白字符。与 [^ \f\n\r\t\v] 等效。                 |
| \t            | 制表符匹配。与 \x09 和 \cI 等效。                            |
| \v            | 垂直制表符匹配。与 \x0b 和 \cK 等效。                        |
| \w            | 匹配任何字类字符，包括下划线。与"[A-Za-z0-9_]"等效。         |
| \W            | 与任何非单词字符匹配。与"[^A-Za-z0-9_]"等效。                |
| \x*n*         | 匹配 *n*，此处的 *n* 是一个十六进制转义码。十六进制转义码必须正好是两位数长。例如，"\x41"匹配"A"。"\x041"与"\x04"&"1"等效。允许在正则表达式中使用 ASCII 代码。 |
| \*num*        | 匹配 *num*，此处的 *num* 是一个正整数。到捕获匹配的反向引用。例如，"(.)\1"匹配两个连续的相同字符。 |
| \*n*          | 标识一个八进制转义码或反向引用。如果 \*n* 前面至少有 *n* 个捕获子表达式，那么 *n* 是反向引用。否则，如果 *n* 是八进制数 (0-7)，那么 *n* 是八进制转义码。 |
| \*nm*         | 标识一个八进制转义码或反向引用。如果 \*nm* 前面至少有 *nm* 个捕获子表达式，那么 *nm* 是反向引用。如果 \*nm* 前面至少有 *n* 个捕获，则 *n* 是反向引用，后面跟有字符 *m*。如果两种前面的情况都不存在，则 \*nm* 匹配八进制值 *nm*，其中 *n* 和 *m* 是八进制数字 (0-7)。 |
| \nml          | 当 *n* 是八进制数 (0-3)，*m* 和 *l* 是八进制数 (0-7) 时，匹配八进制转义码 *nml*。 |
| \u*n*         | 匹配 *n*，其中 *n* 是以四位十六进制数表示的 Unicode 字符。例如，\u00A9 匹配版权符号 (©)。 |

# 逻辑运算符

>**&：逻辑与，无论左边为真还是假，后面都要执行**
>
>**&&： 短路与，只要左边为假，后面都不执行**
>
>**|：  逻辑或，无论左边是真还是假，后面都要执行**
>
>**||：  短路或，只要左边为真，后面都不执行**



