---
title: java基础语法
date: 2019-10-27 22:01:10
tags: java
---

# Java 程序的基本结构

---

## 类名的要求

- 符合变量的命名规范
- **习惯以大写字母开头**

# 变量和数据类型

---

## 变量

```c
int x = 1;
```

上述语句定义了一个整型`int`类型的变量，名称为`x`，初始值为`1`。

不写初始值，就相当于给它指定了默认值。默认值总是`0`。

> 注： Java变量的命名规范不同于C/C++，它可以使用`$`作为开头。但一般不用

## 基本数据类型

- 整数类型：byte，short，int，long
- 浮点数类型：float，double
- 字符类型：char
- 布尔类型：boolean
- 常量

### 整型

---

对于整型类型，Java只定义了带符号的整型

- byte：-128 ~ 127
- short: -32768 ~ 32767
- int: -2147483648 ~ 2147483647
- long: -9223372036854775808 ~ 9223372036854775807

> java可以使用`0b`或者`0B`开头表示二进制数字

### 浮点型

---

- 对于`float`类型，需要加上`f`后缀。不加f表示为double类型。
- 浮点数可表示的范围非常大，`float`类型可最大表示3.4x1038，而`double`类型可最大表示1.79x10308。

### 布尔类型

---

布尔类型`boolean`只有`true`和`false`两个值，布尔类型总是关系运算的计算结果：

```java
boolean b1 = true;
boolean b2 = false;
boolean isGreater = 5 > 3; // 计算结果为true
int age = 12;
boolean isAdult = age >= 18; // 计算结果为false
```

- Java语言对布尔类型的存储并没有做规定，因为理论上存储布尔类型只需要1 bit，但是通常JVM内部会把`boolean`表示为4字节整数。
- 整数表达式不能转换成布尔类型，比如`x=0`不代表x为`false`

### 字符类型

---

字符类型`char`表示一个字符。Java的`char`类型除了可表示标准的ASCII外，还可以表示一个Unicode字符（）：

```java
public class Main {
    public static void main(String[] args) {
        char a = 'A';
        char zh = '中';
        System.out.println(a);
        System.out.println(zh);
    }
}
```

- 一般不使用char类型，除非需要处理`UTF-16`代码单元

### 常量

---

定义变量的时候，如果加上`final`修饰符，这个变量就变成了常量：

```java
final double PI = 3.14; // PI是一个常量
double r = 5.0;
double area = PI * r * r;
PI = 300; // compile error!
```

根据习惯，常量名通常全部大写。

### var关键字

---

有些时候，类型的名字太长，写起来比较麻烦。例如：

```java
StringBuilder sb = new StringBuilder();
```

这个时候，如果想省略变量类型，可以使用`var`关键字：

```java
var sb = new StringBuilder();
```

编译器会根据赋值语句自动推断出变量`sb`的类型是`StringBuilder`。对编译器来说，语句：

```java
var sb = new StringBuilder();
```

实际上会自动变成：

```java
StringBuilder sb = new StringBuilder();
```

因此，使用`var`定义变量，仅仅是少写了变量类型而已。

### String类型

---

- 常见的转义字符包括：
  - `\"` 表示字符`"`
  - `\'` 表示字符`'`
  - `\\` 表示字符`\`
  - `\n` 表示换行符
  - `\r` 表示回车符
  - `\t` 表示Tab
  - `\u####` 表示一个Unicode编码的字符

- String类型的方法不会对String对象进行修改，只会返回修改后的String对象

- 空串为`null`（注意：这也是所有的引用类型的初始值）

  - 判断是否为空串的方法
    - `str.equal("");`
    - `str.length==0;`

- 构建字符串

  ```java
  StringBuilder str = new StringBuilder();
  str.append("Hello");
  str.append(" ").append("World!");
  String res = str.toString();
  ```

- 将基本类型格式化进入`String`类型的字符串对象中(类似于C/C++中的`sprintf`函数)

  ```java
  int x=3,y=4;
  String str = String.format("%d:%d",x,y);
  /**
  str="3:4"
  */
  ```

  

### 数组类型

---

> 定义一个数组类型的变量，使用数组类型“类型[]”，例如，`int[]`。和单个基本类型变量不同，数组变量初始化必须使用`new int[5]`表示创建一个可容纳5个`int`元素的数组。

#### Java的数组有几个特点：

- 数组所有元素初始化为默认值，整型都是`0`，浮点型是`0.0`，布尔型是`false`；
- 数组一旦创建后，大小就不可改变。

#### 也可以在定义数组时直接指定初始化的元素，这样就不必写出数组大小，而是由编译器自动推算数组大小。例如：

```java
public class Main {
    public static void main(String[] args) {
        int[] ns = new int[] { 68, 79, 91, 85, 62 }; //new int[]{} 为匿名数组
        System.out.println(ns.length); // 编译器自动推算数组大小为5
    }
}
```

#### 还可以进一步简写为：

```java
int[] ns = { 68, 79, 91, 85, 62 };
```

### JAVA数组的遍历

---

```java
//方式一
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int i=0; i<ns.length; i++) {
            int n = ns[i];
            System.out.println(n);
        }
    }
}
/************************************************/
//方式二
public class Main {
    public static void main(String[] args) {
        int[] ns = { 1, 4, 9, 16, 25 };
        for (int n : ns) {
            System.out.println(n);
        }
    }
}
```

### 二维数组

- 不同于C/C++的数组，Java中的二维数组实际就是一维数组

- 一维数组可以单独使用，因此可以交换两行

- 使用`for each`遍历二维数组的方法

  ```java
  
  public class Main {
      public static void main(String[] args) {
          int[][] x = new int[][] {{1,2},{3,4,5},{6,7,8,9}};
          for (int[] mem:x) {
              for (int sin:mem) {
                  System.out.print(sin);
              }
              System.out.println("");
          }
      }
  
  }
  ```

  

## Java大数

```java
BigInteger x = BigInteger.valueOf(123456);
x=x.add(BigInteger.valueOf(147));
/*
待续***
*/
```



# 流程控制

---

## 输入和输出

### 输出

---

- `println`是print line的缩写，表示输出并换行。因此，如果输出后不想换行，可以用`print()`：
- 格式化输出：`System.out.printf();`
  - 可以按`%s`格式化输出`String`类型
  - 

### 输入

---

```java

import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in); // 创建Scanner对象
        System.out.print("Input your name: "); // 打印提示
        String name = scanner.nextLine(); // 读取一行输入并获取字符串
        System.out.print("Input your age: "); // 打印提示
        int age = scanner.nextInt(); // 读取一行输入并获取整数
        System.out.printf("Hi, %s, you are %d\n", name, age); // 格式化输出
    }
}
```

- 先通过import语句导入`Java.util.Scanner`
- `new`一个`Scanner`类型的对象，其参数为`System.in`为标准输入流

### 文件的输入输出

---

- 使用shell方式（不使用IDE）

  > 直接在编译时重定向输入输出文件
  >
  > e.g: 	java test.java < input,txt > output.txt

- 使用`IDEA`开发环境

  ```java
  //记得要捕获异常--输入
  Scanner in = new Scanner(Paths.get("input.txt"),"utf-8"); 
  //此时input.txt文件必须放在项目的根目录，否则就要写绝对/相对路径
  ```

  ```java
  import java.io.*;
  import java.nio.charset.StandardCharsets;
  
  public class Main {
      public static void main(String[] args) {
          PrintWriter out;
          try {
              out = new PrintWriter("output.txt","utf-8");
              out.println(456);
              out.close();
          } catch (IOException io) {
              io.printStackTrace();
          }
      }
  
  }
  ```



### 对象的判等

---

- 如果是基本类型，使用`==`完全OK

- 如果是引用类型，则需要使用`equals()`方法。引用类型使用`==`只能判断指向的对象是否相同，而`equals()`方法则可以判断内容是否相同

  ```java
  public class hello {
      public static void main(String[] args) {
           String s1 = "hello";
           String s2 = "HELLO".toLowerCase();
           System.out.println(s1==s2);
           System.out.println(s1.equals(s2));
      }
  }
  
  //输出
  /*
  false
  true
  */
  
  ```

