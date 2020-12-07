# JDK各个版本新特性整理


学习整理笔记

<!--more-->

## JDK 5 
（1）泛型

  通过引入泛型，我们将获得编译时类型的安全和运行时更小地抛出ClassCastExceptions的可能。

（2）foreach，更简洁的for循环

（3）自动拆箱装箱

  自动装箱：Java自动将原始类型值转换成对应的对象，比如将int的变量转换成Integer对象，这个过程叫做装箱，反之将Integer对象转换成int类型值，这个过程叫做拆箱。原始类型：byte,short,char,int,long,float,double,boolean。对应的封装类：Byte,Short,Character,Integer,Long,Float,Double,Boolean。
   
（4）枚举

（5）静态导入

（6）元数据

（7）线程池

（8）Java Generics
## JDK 6 

（1）DestTop类和SystemTray类

（2）使用JAXB2来实现对象与XML之间的映射

（3）StAX

（4）使用Complier API

（5）轻量级的Http Server API

（6）插入式注解处理API(Pluggable Annotation Processing API)

（7）用Console开发控制台程序

（8）对脚本语言的支持

（9）Common Annotations

## JDK 7 
（1）switch中可以使用字串了

（2）泛型实例化类型自动推断

（3）新增一些取环境信息的工具方法

（4）Boolean类型反转，空指针安全,参与位运算

（5）两个char间的equals

（6）安全的加减乘除

（7）二进制变量的表示,支持将整数类型用二进制来表示，用0b开头。

（8）Try-with-resource语句

（9）使用一个catch语言来处理多种异常类型

## JDK 8 
（1）接口的默认方法

（2）Lambda 表达式

（3）函数式接口

（4）方法与构造函数引用

（5）Lambda 作用域

（6）访问局部变量

（7）访问对象字段与静态变量

（8）Date API

（9）多重Annotation 注解

（10）访问接口的默认方法
## JDK 9 
（1）Java 平台级模块系统

（2）Linking

（3）JShell : 交互式 Java REPL

（4）改进的 Javadoc

（5）集合工厂方法

（6）改进的 Stream API

（7）私有接口方法

（8）HTTP/2

（9）多版本兼容 JAR

## JDK 10 
（1）局部变量类型推断

（2）将JDK多存储库合并为单储存库

（3）垃圾回收接口

（4）并行Full GC 的G1

（5）应用数据共享

（6）线程局部管控

（7）移除Native-Header Generation Tool （javah）

（8）Unicode 标签扩展

（9）备用内存设备上分配堆内存

（10）基于实验JAVA 的JIT 编译器

（11）Root 证书

（12）基于时间的版本控制

## JDK 11

（1）Local Var

  Lambda表达式中，可以使用var关键字来标识变量，变量类型由编译器自行推断。

（2）HttpClient

  JDK11的新的HttpClient支持HTTP/2和WebSocket，并且可以使用异步接口。

（3）List API

  对于List接口，新增了一个of(T…)接口，用于快速创建List对象。

（4）读写文件

  对Files类增加了writeString和readString两个静态方法，可以直接把String写入文件，或者把整个文件读出为一个String。

（5）String API

  String新增了strip()方法，和trim()相比，strip()可以去掉Unicode空格，例如，中文空格。

  新增isBlank()方法，可判断字符串是不是“空白”字符串。

  新增lines()方法，可以非常方便地按行分割字符串。

  新增repeat()方法，可以指定重复次数。
