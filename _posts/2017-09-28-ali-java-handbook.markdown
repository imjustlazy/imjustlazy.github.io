---
layout:     post
title:      "阿里巴巴Java开发手册阅读记录"
subtitle:   "Alibaba java handbook！"
date:       2017-09-28 20:00:00
author:     "石头人m"
header-img: "img/about-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Java
---


# 一、编程规约

## (一) 命名规约
  1.中括号是数组类型的一部分，数组定义如下：String[] args;请勿使用String args[]的方式来定义。

  2.POJO 类中的任何布尔类型的变量，都不要加is，否则部分框架解析会引起序列化错误。<br>
   反例：定义为基本数据类型boolean isSuccess；的属性，它的方法也是isSuccess()，RPC框架在反向解析的时候，“以为”对应的属性名称是success，导致属性获取不到，进而抛出异常。

## (二) 常量定义
  1.不允许出现任何魔法值（即未经定义的常量）直接出现在代码中。

  2.long 或者Long 初始赋值时，必须使用大写的L，不能是小写的l，小写容易跟数字1混淆，造成误解。

  3.常量的复用层次有五层：跨应用共享常量、应用内共享常量、子工程内共享常量、包内共享常量、类内共享常量。<br>
      1） 跨应用共享常量：放置在二方库中，通常是client.jar 中的const 目录下。<br>
      2） 应用内共享常量：放置在一方库的modules 中的const 目录下。<br>
      3） 子工程内部共享常量：即在当前子工程的const 目录下。<br>
      4） 包内共享常量：即在当前包下单独的const 目录下。<br>
      5） 类内共享常量：直接在类内部private static final 定义。<br>

## (三) 格式规约
  1.单行字符数限制不超过120 个，超出需要换行，换行时，遵循如下原则：<br>
	  1） 换行时相对上一行缩进4 个空格。<br>
	  2） 运算符与下文一起换行。<br>
	  3） 方法调用的点符号与下文一起换行。<br>
	  4） 在多个参数超长，逗号后进行换行。<br>
	  5） 在括号前不要换行，见反例。<br>

  2.方法体内的执行语句组、变量的定义语句组、不同的业务逻辑之间或者不同的语义之间插入一个空行。相同业务逻辑和语义之间不需要插入空行。没有必要插入多行空格进行隔开。

例：
```java
public static void main(String args[]) {
    // 缩进4 个空格
    String say = "hello";
    // 运算符的左右必须有一个空格
    int flag = 0;
    // 关键词if 与括号之间必须有一个空格，括号内f 与左括号，1 与右括号不需要空格
    if (flag == 0) {
    	System.out.println(say);
    }
    
    // 左大括号前加空格且不换行；左大括号后换行
    if (flag == 1) {
    	System.out.println("world");
    // 右大括号前换行，右大括号后有else，不用换行
    } else {
    	System.out.println("ok");
    // 右大括号做为结束，必须换行
    }
}
```

## (四) OOP 规约
  1.避免通过一个类的对象引用访问此类的静态变量或静态方法，无谓增加编译器解析成本，直接用类名来访问即可。

  2.所有的覆写方法，必须加@Override 注解。<br>
   getObject()与get0bject()的问题。一个是字母的O，一个是数字的0，加@Override可以准确判断是否覆盖成功。另外，如果在抽象类中对方法签名进行修改，其实现类会马上编译报错。

  3.Object 的equals 方法容易抛空指针异常，应使用常量或确定有值的对象来调用equals。
```java
    正例： "test".equals(object)。
    反例： object.equals("test");
```
说明：推荐使用java.util.Objects#equals （JDK7 引入的工具类）

  4.所有的相同类型的包装类对象之间值的比较，全部使用equals 方法比较。<br>
    说明：对于Integer var=?在-128 至127 之间的赋值，Integer 对象是在IntegerCache.cache产生，会复用已有对象，这个区间内的Integer 值可以直接使用==进行判断，但是这个区间之外的所有数据，都会在堆上产生，并不会复用已有对象，这是一个大坑，推荐使用equals 方法进行判断。

  5.序列化类新增属性时，请不要修改serialVersionUID 字段，避免反序列失败；如果完全不兼容升级，避免反序列化混乱，那么请修改serialVersionUID 值。<br>
    说明：注意serialVersionUID 不一致会抛出序列化运行时异常。

  6.使用索引访问用String 的split 方法得到的数组时，需做最后一个分隔符后有无内容的检查，否则会有抛IndexOutOfBoundsException 的风险。
```java
说明：
    String str = "a,b,c,,"; String[] ary = str.split(",");
    //预期大于3，结果是3
    System.out.println(ary.length);
```

  7.循环体内，字符串的联接方式，使用StringBuilder 的append 方法进行扩展。
```java
反例：
	String str = "start";
	for(int i=0; i<100; i++){
		str = str + "hello";
	}
```
   说明：反编译出的字节码文件显示每次循环都会new 出一个StringBuilder 对象，然后进行append 操作，最后通过toString 方法返回String 对象，造成内存资源浪费。

  8.final 可􁨀高程序响应效率，声明成final 的情况：<br>
    1） 不需要重新赋值的变量，包括类属性、局部变量。<br>
    2） 对象参数前加final，表示不允许修改引用的指向。<br>
    3） 类方法确定不允许被重写。<br>

   9.慎用Object 的clone 方法来拷贝对象。<br>
	说明：对象的clone 方法默认是浅拷贝，若想实现深拷贝需要重写clone 方法实现属性对象的拷贝。


## (五) 集合处理
  1. Map/Set 的key 为自定义对象时，必须重写hashCode 和equals。<br>
     正例：String 重写了hashCode 和equals 方法，所以我们可以非常愉快地使用String 对象作为key 来使用。

  2.ArrayList 的subList 结果不可强转成ArrayList，否则会抛出ClassCastException异常：java.util.RandomAccessSubList cannot be cast to java.util.ArrayList ;<br>
	说明：subList 返回的是 ArrayList 的内部类 SubList，并不是 ArrayList ，而是 ArrayList的一个视图，对于SubList 子列表的所有操作最终会反映到原列表上。

  3.在subList 场景中，高度注意对原集合元素个数的修改，会导致子列表的遍历、增加、删除均产生ConcurrentModificationException 异常。

  4.使用集合转数组的方法，必须使用集合的toArray(T[] array)，传入的是类型完全一样的数组，大小就是list.size()。直接使用toArray 无参方法存在问题，此方法返回值只能是Object[]类，若强转其它类型数组将出现ClassCastException 错误。
```java
正例：
    List<String> list = new ArrayList<String>(2);
    list.add("guan");
    list.add("bao");
    String[] array = new String[list.size()];
    array = list.toArray(array);
```
说明：使用toArray 带参方法，入参分配的数组空间不够大时，toArray 方法内部将重新分配内存空间，并返回新数组地址；如果数组元素大于实际所需，下标为[ list.size() ]的数组元素将被置为null，其它数组元素保持原值，因此最好将方法入参数组大小定义与集合元素个数一致。


  5.使用工具类Arrays.asList()把数组转换成集合时，不能使用其修改集合相关的方法，它的add/remove/clear 方法会抛出UnsupportedOperationException 异常。<br>
说明：asList 的返回对象是一个Arrays 内部类，并没有实现集合的修改方法。Arrays.asList体现的是适配器模式，只是转换接口，后台的数据仍是数组。
```java
String[] str = new String[] { "a", "b" };
List list = Arrays.asList(str);
```
第一种情况：list.add("c"); 运行时异常。<br>
第二种情况：str[0]= "gujin"; 那么list.get(0)也会随之修改。

  6.泛型通配符<? extends T>来接收返回的数据，此写法的泛型集合不能使用add 方法。<br>
	说明：苹果装箱后返回一个<? extends Fruits>对象，此对象就不能往里加任何水果，包括苹果。

  7.不要在foreach 循环里进行元素的remove/add 操作。remove 元素请使用Iterator方式，如果并发操作，需要对Iterator 对象加锁。<br>
```java
反例：
    List<String> a = new ArrayList<String>();
    a.add("1");
    a.add("2");
    for (String temp : a) {
        if("1".equals(temp)){
            a.remove(temp);
        }
    }
说明：这个例子的执行结果会出乎大家的意料，那么试一下把“1”换成“2”，会是同样的结果吗？
正例：
    Iterator<String> it = a.iterator();
    while(it.hasNext()){
        String temp = it.next();
        if(删除元素的条件){
            it.remove();
        }
    }
```

  8.在JDK7 版本以上，Comparator 要满足自反性，传递性，对称性，不然Arrays.sort，
Collections.sort 会报IllegalArgumentException 异常。<br>
说明：<br>
  1） 自反性：x，y 的比较结果和y，x 的比较结果相反。<br>
  2） 传递性：x>y,y>z,则x>z。<br>
  3） 对称性：x=y,则x,z 比较结果和y，z 比较结果相同。<br>
反例：下例中没有处理相等的情况，实际使用中可能会出现异常：<br>
```java
new Comparator<Student>() {
    @Override
    public int compare(Student o1, Student o2) {
    	return o1.getId() > o2.getId() ? 1 : -1;
    }
}
```

  9.使用entrySet 遍历Map 类集合KV，而不是keySet 方式进行遍历。<br>
	说明：keySet 其实是遍历了2 次，一次是转为Iterator 对象，另一次是从hashMap 中取出key所对应的value。而entrySet 只是遍历了一次就把key 和value 都放到了entry 中，效率更高。如果是JDK8，使用Map.foreach 方法。<br>
	正例：values()返回的是V 值集合，是一个list 集合对象；keySet()返回的是K 值集合，是一个Set 集合对象；entrySet()返回的是K-V 值组合集合。

  10.高度注意Map 类集合K/V 能不能存储null 值的情况，如下表格：<br>


| 集合类                         |      Key               |  Value                 |     Super               |      说明   
| ----------------------------- |:---------------------:| ----------------------:| -----------------------:| ----------------------:|
|Hashtable                     |   不允许为null    |  不允许为null      |     Dictionary         |    线程安全<br>
|ConcurrentHashMap    |   不允许为null    | 不允许为null       |  AbstractMap        | 线程局部安全<br>
|TreeMap                       |   不允许为null    | 允许为null           |  AbstractMap       |  线程不安全<br>
|HashMap                      |   允许为null        |允许为null           |   AbstractMap       | 线程不安全<br>
	反例：很多同学认为ConcurrentHashMap 是可以置入null 值。在批量翻译场景中，子线程分发时，出现置入null 值的情况，但主线程没有捕获到此异常，导致排查困难。

  11.合理利用好集合的有序性(sort)和稳定性(order)，避免集合的无序性(unsort)和不
稳定性(unorder)带来的负面影响。<br>
	说明：稳定性指集合每次遍历的元素次序是一定的。有序性是指遍历的结果是按某种比较规则
依次排列的。如：ArrayList 是order/unsort；HashMap 是unorder/unsort；TreeSet 是
order/sort。



## (六) 并发处理
	