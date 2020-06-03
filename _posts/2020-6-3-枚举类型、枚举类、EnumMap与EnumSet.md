---
layout:     post
title:      枚举类型、枚举类、EnumMap与EnumSet
subtitle:   java
date:       2020-6-3
author:     leon7777
header-img: img/user.jpg
catalog: 	 true
tags:
    - java
---
# 枚举类型、枚举类、EnumMap与EnumSet
1、[枚举类型](#枚举类型)  
2、[java枚举类](#java枚举类)  
3、[EnumMap](#EnumMap)  
4、[EnumSet](#EnumSet)  
## 枚举类型
枚举时一个被命名的整型常数的集合，用于声明一组带标识符的常数。枚举在日常生活中很常见，比如性别{男，女}星期{星期一，星期二、、、星期天}  
等这种一个变量有几种固定的取值时，就可以将它定义为枚举类型
## java的枚举类
### 声明枚举
声明枚举时必须使用enum关键字，然后定义枚举的名称、可访问性、基础类型和成员等。  
枚举声明的语法如下：
```java
enum-modifiers enum enumname:enum-base {
    enum-body,
}
```
enum-modifiers修饰符：public\private\internal  
enumname表示声明的枚举名称  
enum-base表示枚举的基础类型<font color="red">（注意：不定义时默认为int类型）</font>  
enum-body表示枚举的成员，它是枚举类型的命名常数  
<font color="red" >任意两个枚举成员不能具有相同的名称，且它的常数值必须在该枚举的基础类型的范围之内，多个枚举成员之间使用逗号分隔。</font>  
#### 例子
下列是英雄联盟英雄的分类，构造了一个HeroType枚举类  
```java
public enum HeroType {
    TANK,WIZARD,ASSASSIN,WARRIOR,RANGED,PUSH,FARMING;
}
```
之后通过<font color="red">枚举类型名直接引用常量</font>，如HeroType.TANK等  
```java
public class HeroTypeTest {
    public static void main(String[] args)
    {
//        for(HeroType s: HeroType.values()){
//            System.out.println(s);
//        }
        HeroType hero=HeroType.ASSASSIN;
        switch (hero){
            case ASSASSIN:
                System.out.println("1");
                break;
            case TANK:
                System.out.println("2");
                break;
            case WARRIOR:
                System.out.println("3");
                break;
            case PUSH:
                System.out.println("4");
                break;
            case RANGED:
                System.out.println("5");
                break;
            case WIZARD:
                System.out.println("6");
                break;
            case FARMING:
                System.out.println("7");
                break;
        }
    }
}
```
### 枚举类
java中的每一个枚举都继承自java.lang.Enum类。当定义一个枚举类型时，每一个枚举类型成员都可以看作时Enum类的实例，这些枚举成员默认都被final、public，static修饰，当使用枚举类型成员时，直接使用枚举名称调用成员即可。  
所有枚举实例都可以调用Enum类的方法，常用方法如图所示  

方法   |   方法描述  
-----  |------
values()|以数据形式返回枚举类型的所有成员
valueOf()|将普通字符串转换为枚举实例
compareTo()|比较两个枚举成员再定义时的顺序
ordinal()|获取枚举成员的索引位置

以下例子为通过values方法直接将枚举类型进行循环输出
```java
public class HeroTypeTest {
    public static void main(String[] args)
    {
        for(HeroType s: HeroType.values(){
            System.out.println(s);
        }
    }
}
```
Java 中的 enum 还可以跟 Class 类一样覆盖基类的方法。这里不进行多余解释
## EnumMap
EnumMap时专门为为枚举类型量身定做的Map实现。它与其他Map相比更高效。  
原因：HashMap只能接受同一枚举类型的实例作为键值，并且由于枚举类型实例的数量相对固定并且有限，所以EnumMap使用数组存放与枚举类型对应的值，使得EnumMap的效率非常高。  
## EnumSet
EnumSet是枚举类型的高性能Set实现，它要求放入它的枚举常量必须属于同一枚举类型。
