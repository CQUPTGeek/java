# Java 入门课程 - 第一节课

## 1. 课程介绍

帮助大家快速入门Java编程语言，为后续的深入学习打下基础。  

这里再给大家一个我写的java后端学习路线:[java后端](http://www.wapgyj.top/index.php/java/)

## 2. Java 语言概述

### 2.1 什么是 Java？
Java 是一种面向对象的编程语言，由 Sun Microsystems 于 1995 年推出，现在由 Oracle 维护。Java 具有以下特点：
- **跨平台**：一次编写，到处运行（Write Once, Run Anywhere，WORA）。（编译方式，JVM）
- **面向对象**：支持封装、继承、多态等特性。
- **自动内存管理**：采用垃圾回收机制（Garbage Collection，GC）。
- **安全性高**：提供了强大的安全机制。
- **庞大的生态系统**：拥有丰富的第三方库和工具。

## 3. Java 开发环境搭建

### 3.1 安装 JDK
- 下载地址：[https://www.oracle.com/java/technologies/javase-downloads.html](https://www.oracle.com/java/technologies/javase-downloads.html)
- 安装 JDK 17（建议）
- 配置环境变量（Windows）：
  1. 设置 `JAVA_HOME` 为 JDK 安装路径。
  2. 在 `Path` 变量中添加 `%JAVA_HOME%\bin`。
  3. 在命令行输入 `java -version` 和 `javac -version` 进行验证。

### 3.2 配置开发工具
推荐使用以下 IDE：
- IntelliJ IDEA（推荐）
- Eclipse
- VS Code（需安装 Java 扩展）

## 4. Java 基础语法

### 4.1 第一个 Java 程序
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

### 4.2 Java 变量与数据类型
```java
int a = 10;      // 整数类型
double b = 3.14; // 浮点类型
char c = 'A';    // 字符类型
boolean d = true;// 布尔类型
String e = "Hello"; // 字符串类型
long l=100l;
float f=3.14f;
```

**需要特别注意的是java里没有专门的unsigned int 但是可以通过Integer,Long类的方法来模拟无符号数操作**
```java
public class UnsignedIntExample {
    public static void main(String[] args) {
        int unsignedInt1 = -1;
        int unsignedInt2 = 1;

        // 无符号比较
        int result = Integer.compareUnsigned(unsignedInt1, unsignedInt2);
        System.out.println("Comparison result: " + result);

        // 无符号转换为字符串
        String unsignedString = Integer.toUnsignedString(unsignedInt1);
        System.out.println("Unsigned string: " + unsignedString);
    }
}
```
**浮点数精度问题**  

float 和 double 无法精确表示货币等值（需用 BigDecimal）  

**类型转换规则**  

自动转换（低 → 高）：byte → short → int → long → float → double  

强制转换（高 → 低）：需显式声明，可能丢失精度或溢出。
```java
int i = 128;
byte b = (byte)i;  // 溢出，结果为 -128
```
### 4.3 java修饰符

Java的修饰符主要分为两种

- 访问修饰符
- 非访问修饰符

**访问修饰符**

Java中，可以使用访问控制符来保护对类、变量、方法和构造方法的访问。Java 支持 4 种不同的访问权限。

- **default** (即默认，什么也不写）: 在同一包内可见，不使用任何修饰符。使用对象：类、接口、变量、方法。
- **private** : 在同一类内可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**
- **public** : 对所有类可见。使用对象：类、接口、变量、方法
- **protected** : 对同一包内的类和所有子类可见。使用对象：变量、方法。 **注意：不能修饰类（外部类）**。
  
#### 受保护的访问修饰符 - protected 

**概述**   

`protected` 访问修饰符在 Java 中用于控制类成员（变量、方法、构造器）的访问权限，其访问规则需要从以下两个方面来分析：

###### 1. 子类与基类在同一包中  

当子类与基类处于同一包中时，被声明为 `protected` 的变量、方法和构造器能被同一个包中的任何其他类访问。

```java
// 定义一个基类，位于 com.example 包中
package com.example;

// 基类
class BaseClass {
    // 定义一个 protected 修饰的方法
    protected void protectedMethod() {
        System.out.println("这是基类的 protected 方法");
    }
}

// 定义一个子类，同样位于 com.example 包中
package com.example;

// 子类继承自 BaseClass
class SubClass extends BaseClass {
    public void callProtectedMethod() {
        // 子类可以直接调用基类的 protected 方法
        protectedMethod();
    }
}

// 定义一个测试类，也在 com.example 包中
package com.example;

public class TestSamePackage {
    public static void main(String[] args) {
        // 创建基类对象
        BaseClass base = new BaseClass();
        // 同一包中的类可以直接调用基类的 protected 方法
        base.protectedMethod();

        // 创建子类对象
        SubClass sub = new SubClass();
        // 子类调用自身方法，该方法内部调用基类的 protected 方法
        sub.callProtectedMethod();
    }
}
```
##### 2.子类与基类不在同一包中  
当子类与基类不在同一包中时，在子类中，子类实例可以访问其从基类继承而来的 protected 方法，但不能访问基类实例的 protected 方法。  

 ```java
// 定义基类，位于 com.example.base 包中
package com.example.base;

// 基类
public class BaseClass {
    // 定义一个 protected 修饰的方法
    protected void protectedMethod() {
        System.out.println("这是基类的 protected 方法");
    }
}

// 定义子类，位于 com.example.sub 包中
package com.example.sub;

import com.example.base.BaseClass;

// 子类继承自 BaseClass
public class SubClass extends BaseClass {
    public void callInheritedProtectedMethod() {
        // 子类实例可以访问从基类继承而来的 protected 方法
        protectedMethod();
    }

    public void tryAccessBaseInstanceProtectedMethod() {
        // 创建基类实例
        BaseClass base = new BaseClass();
        // 以下代码会编译错误，子类不能访问基类实例的 protected 方法
        // base.protectedMethod(); 
    }
}

// 定义测试类，位于 com.example.test 包中
package com.example.test;

import com.example.sub.SubClass;

public class TestDifferentPackage {
    public static void main(String[] args) {
        // 创建子类对象
        SubClass sub = new SubClass();
        // 调用子类中访问继承的 protected 方法的方法
        sub.callInheritedProtectedMethod();
    }
}
```


**非访问修饰符**

**static：**


1：static是静态意思，可以修饰成员变量和成员方法;static修饰成员变量表示该成员变量在内存中只存储一份，可以被共享访问，修改。

2：static修饰的成员属性，成员方法，不属于任何一个对象，它属于一个类模板公用，我们调用它时，直接使用类名加 "."就可以直接访问了

**final：**

final意味着不可变：不可变的是变量的引用而非引用指向对象的内容（String）

1、被final修饰的类不可以被继承

2、被final修饰的方法不可以被重写

3：被final修饰的变量不可以被改变

4：final修饰的变量往往和static搭配

### 4.4 Java 运算符
```java
int sum = 5 + 3;  // 加法
int diff = 5 - 3; // 减法
int product = 5 * 3; // 乘法
int quotient = 5 / 3; // 除法（整数）
int remainder = 5 % 3; // 取余
int i=1<<2; //左移
int j=4>>2; //右移
```

### 4.5 Java 条件语句
```java
public class Test {
   public static void main(String args[]){
      int x = 30;
 
      if( x == 10 ){
         System.out.print("Value of X is 10");
      }else if( x == 20 ){
         System.out.print("Value of X is 20");
      }else if( x == 30 ){
         System.out.print("Value of X is 30");
      }else{
         System.out.print("这是 else 语句");
      }
   }
}
```


### 4.6 Java 循环语句
```java
for (int i = 0; i < 5; i++) {
    System.out.println("循环次数: " + i);
}

int i = 0;
while (i < 5) {
    System.out.println("while 循环: " + i);
    i++;
}
int[] arr=new int[5];
for(int j:arr){
   System.out.println("for-each循环"+j);
}
```
```java
public class Test {
   public static void main(String args[]){
      //char grade = args[0].charAt(0);
      char grade = 'C';
 
      switch(grade)
      {
         case 'A' :
            System.out.println("优秀"); 
            break;
         case 'B' :
         case 'C' :
            System.out.println("良好");
            break;
         case 'D' :
            System.out.println("及格");
            break;
         case 'F' :
            System.out.println("你需要再努力努力");
            break;
         default :
            System.out.println("未知等级");
      }
      System.out.println("你的等级是 " + grade);
   }
}
```
## 5. Java 引用类型

### 5.1 引用类型概述
在 Java 中，除基本数据类型（`int`、`double`、`char` 等）外，其他类型都属于 **引用类型**，包括 **对象、数组、接口、枚举、字符串** 等。

- **引用类型存储的是对象的地址，而不是实际的值**。
- **所有的引用类型默认值为 `null`**（除特殊情况外）。
- **使用 `new` 关键字创建对象**。

### 5.2 引用类型示例

#### 5.2.1 类与对象
```java
class Person {
    String name;
    int age;

    void sayHello() {
        System.out.println("Hello, my name is " + name);
    }
}

public class Main {
    public static void main(String[] args) {
        Person p = new Person(); // 创建对象
        p.name = "Alice";
        p.age = 20;
        p.sayHello(); // 调用方法
    }
}
```

#### 5.2.2 数组的使用

数组是一种存储多个同类型数据的引用类型。Java 中的数组分为 **一维数组** 和 **多维数组**。

##### 一维数组示例：
```java
int[] numbers = new int[5]; // 创建一个长度为5的数组
numbers[0] = 10; // 赋值
numbers[1] = 20;
numbers[2] = 30;
numbers[3] = 40;
numbers[4] = 50;

// 遍历数组
for (int i = 0; i < numbers.length; i++) {
    System.out.println("第 " + i + " 个元素: " + numbers[i]);
}
```

##### 一维数组的初始化：
```java
int[] arr1 = new int[]{1, 2, 3, 4, 5}; // 静态初始化
int[] arr2 = {6, 7, 8, 9, 10}; // 省略 new 关键字的静态初始化
```

##### 多维数组示例：
```java
int[][] matrix = new int[2][3]; // 创建一个 2x3 的二维数组
matrix[0][0] = 1;
matrix[0][1] = 2;
matrix[0][2] = 3;
matrix[1][0] = 4;
matrix[1][1] = 5;
matrix[1][2] = 6;

// 遍历二维数组
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
```


#### 5.2.3 字符串（特殊的引用类型）
```java
String str1 = "Hello"; // 直接赋值
String str2 = new String("World"); // 使用 new 关键字创建
System.out.println(str1 + " " + str2);
```

#### 5.2.4 接口
```java
interface Animal {
    void makeSound();
}

class Dog implements Animal {
    public void makeSound() {
        System.out.println("Woof Woof");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal myDog = new Dog(); // 接口引用
        myDog.makeSound();
    }
}
```
#### 5.2.5 空引用  
引用变量可以为 null，表示它不指向任何对象。在使用引用变量之前，通常需要检查它是否为 null，以避免 NullPointerException
```java
Person p = null;
if (p != null) {
    p.introduce();
}
```
#### 5.2.6 垃圾回收  

当一个对象没有任何引用指向它时，Java 的垃圾回收机制会在适当的时候回收该对象所占用的内存  
```java
Person p1 = new Person("王五", 25);
p1 = null; // 此时对象没有引用指向它，可能会被垃圾回收
```

