---
layout:     post
title:      关于继承以及构造方法、关键字super和this的一道题
subtitle:   java
date:       2020-5-24
author:     leon7777
header-img: img/user.jpg
catalog: 	 true
tags:
    - java
---
# java学习-关于继承以及构造方法、关键字super和this的一道题
## 题目 这段程序的输出为？
```java
public class Test extends TT

{

public static void main(String args[])

{

    Test t = new Test("Tom");

}

public Test(String s)

{

    super(s);

    System.out.println("How do you do?");

}

public Test()

{

    this("I am Tom");

}

}

class TT

{

public TT()

{

    System.out.println("What a pleasure!");

}

public TT(String s)

{

this();

    System.out.println("I am "+s);

}
}
```
## 我的初步认知
因为之前学java很久之前，看这段代码时还带着函数式编程的想法，认为主函数里只是new了一个对象，而没有输出，认为不会有输出。
## 进一步认识
认为答案课本不过没这么简单，便去请教了一位大佬，大佬痛心疾首的给我解释了这段程序的输出过程，让我了解了自己对知识浅薄的认识。
## 解疑
从第一句我们了解到Test继承的是父类TT
```java
public class Test extends TT
```
1、Test在主类中new了一个对象，便结束了，此时就要了解**构造方法是在实例化的时候自动执行**
。  s
2、又因为对象带参数，执行带参数的构造方法（Test （String s））。  
```java
public Test(String s)

{

    super(s);

    System.out.println("How do you do?");

}
```
3、此时到又因为这个（Test （String s））中有super(s)，此时又要知道**super此时指向的是父类即TT**，所以此时调用的是TT的构造函数，又因为super带参数，调用的方法为
```java
public TT(String s)

{

this();

    System.out.println("I am "+s);

}
```
4、到这个方法中发现有this（），又要知道**this指向的是当前对象**，又因为不带参数，所以调用的是
```java
public TT()

{

    System.out.println("What a pleasure!");

}
```
5、回到Test中执行输出 "How do you do?"

## 结果  
What a pleasure!  
I am Tom  
How do you do?  
## 知识梳理
### 1、实例化对象时会自动执行构造方法  
### 2、super的用法  
super可以理解为是指向自己超（父）类对象的一个指针，而这个超类指的是离自己最近的一个父类。

#### (1)、普通的直接引用  
与this类似，super相当于是指向当前对象的父类，这样就可以用super.xxx来引用父类的成员。
#### (2)、子类中的成员变量或方法与父类中的成员变量或方法同名
```java
class Country {
    String name;
    void value() {
       name = "China";
    }
}
  
class City extends Country {
    String name;
    void value() {
    name = "Shanghai";
    super.value();      //调用父类的方法
    System.out.println(name);
    System.out.println(super.name);
    }
  
    public static void main(String[] args) {
       City c=new City();
       c.value();
       }
}
```
运行结果：

Shanghai  
China  

#### (3)、引用构造函数  
super（参数）：调用父类中的某一个构造函数（应该为构造函数中的第一条语句）。  
this（参数）：调用本类中另一种形式的构造函数（应该为构造函数中的第一条语句）。  
```java
class Person { 
    public static void prt(String s) { 
       System.out.println(s); 
    } 
   
    Person() { 
       prt("父类·无参数构造方法： "+"A Person."); 
    }//构造方法(1) 
    
    Person(String name) { 
       prt("父类·含一个参数的构造方法： "+"A person's name is " + name); 
    }//构造方法(2) 
} 
    
public class Chinese extends Person { 
    Chinese() { 
       super(); // 调用父类构造方法（1） 
       prt("子类·调用父类”无参数构造方法“： "+"A chinese coder."); 
    } 
    
    Chinese(String name) { 
       super(name);// 调用父类具有相同形参的构造方法（2） 
       prt("子类·调用父类”含一个参数的构造方法“： "+"his name is " + name); 
    } 
    
    Chinese(String name, int age) { 
       this(name);// 调用具有相同形参的构造方法（3） 
       prt("子类：调用子类具有相同形参的构造方法：his age is " + age); 
    } 
    
    public static void main(String[] args) { 
       Chinese cn = new Chinese(); 
       cn = new Chinese("codersai"); 
       cn = new Chinese("codersai", 18); 
    } 
}
```
运行结果：

父类·无参数构造方法： A Person.  
子类·调用父类”无参数构造方法“： A chinese coder.    
父类·含一个参数的构造方法： A person's name is codersai   
子类·调用父类”含一个参数的构造方法“： his name is codersai  
父类·含一个参数的构造方法： A person's name is codersai  
子类·调用父类”含一个参数的构造方法“： his name is codersai  
子类：调用子类具有相同形参的构造方法：his age is 18  

从本例可以看到，可以用super和this分别调用父类的构造方法和本类中其他形式的构造方法。  

例子中Chinese类第三种构造方法调用的是本类中第二种构造方法，而第二种构造方法是调用父类的，因此也要先调用父类的构造方法，再调用本类中第二种，最后是重写第三种构造方法。
### 3、this的用法
this是自身的一个对象，代表对象本身，可以理解为：指向对象本身的一个指针。  
大致有三种用法：
#### (1)、普通的直接引用
this相当于是指向当前对象本身
#### (2)、形参与成员名字重名，用this来区分：
```java
class Person {
    private int age = 10;
    public Person(){
    System.out.println("初始化年龄："+age);
}
    public int GetAge(int age){
        this.age = age;
        return this.age;
    }
}
 
public class test1 {
    public static void main(String[] args) {
        Person Harry = new Person();
        System.out.println("Harry's age is "+Harry.GetAge(12));
    }
}       
```
运行结果：  

初始化年龄：10  
Harry's age is 12  

可以看到，这里age是GetAge成员方法的形参，this.age是Person类的成员变量。
#### (3)、引用构造函数
this（参数）：调用本类中另一种形式的构造函数（应该为构造函数中的第一条语句）。  
例子在super的第三种用法中。
### 4、this和super的异同
(1)、super（参数）：调用基类中的某一个构造函数（应该为构造函数中的第一条语句）   

(2)、this（参数）：调用本类中另一种形成的构造函数（应该为构造函数中的第一条语句）  

(3)、super:　它引用当前对象的直接父类中的成员（用来访问直接父类中被隐藏的父类中成员数据或函数，基类与派生类中有相同成员定义时如：super.变量名    super.成员函数据名（实参）  

(4)、this：它代表当前对象名（在程序中易产生二义性之处，应使用this来指明当前对象；如果函数的形参与类中的成员数据同名，这时需用this来指明成员变量名）  

(5)、调用super()必须写在子类构造方法的第一行，否则编译不通过。每个子类构造方法的第一条语句，都是隐含地调用super()，如果父类没有这种形式的构造函数，那么在编译的时候就会报错。  

(6)、super()和this()类似,区别是，super()从子类中调用父类的构造方法，this()在同一类内调用其它方法。  

(7)、super()和this()均需放在构造方法内第一行。  

(8)、尽管可以用this调用一个构造器，但却不能调用两个。  

(9)、this和super不能同时出现在一个构造函数里面，因为this必然会调用其它的构造函数，其它的构造函数必然也会有super语句的存在，所以在同一个构造函数里面有相同的语句，就失去了语句的意义，编译器也不会通过。

(10)、this()和super()都指的是对象，所以，均不可以在static环境中使用。包括：static变量,static方法，static语句块。

(11)、从本质上讲，this是一个指向本对象的指针, 然而super是一个Java关键字。