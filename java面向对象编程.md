



## 面向对象基础

- 要特别注意的是，如果我们自定义了一个构造方法，那么，编译器就*不再*自动创建默认构造方法：

- 没有在构造方法中初始化字段时，引用类型的字段默认是`null`，数值类型的字段用默认值，`int`类型默认值是`0`，布尔类型默认值是`false`：

- 特殊情况

  - 在变量的定义处有初值，构造函数又含有初值。最后决定于构造函数的值。

- 继承

  - 继承是面向对象编程中非常强大的一种机制，它首先可以复用代码。当我们让`Student`从`Person`继承时，`Student`就获得了`Person`的所有功能，我们只需要为`Student`编写新增的功能。

  - Java使用`extends`关键字来实现继承：

    - ```java
      class Person {
          private String name;
          private int age;
      
          public String getName() {...}
          public void setName(String name) {...}
          public int getAge() {...}
          public void setAge(int age) {...}
      }
      
      class Student extends Person {
          // 不要重复name和age字段/方法,
          // 只需要定义新增score字段/方法:
          private int score;
      
          public int getScore() { … }
          public void setScore(int score) { … }
      }
      ```

  - Java只允许一个class继承自一个类，因此，一个类有且仅有一个父类。只有`Object`特殊，它没有父类。

  - protected继承：

    - 子类无法访问父类的`private`字段或者`private`方法
    - 因此，`protected`关键字可以把字段和方法的访问权限控制在继承树内部，一个`protected`字段和方法可以被其子类，以及子类的子类所访问，后面我们还会详细讲解。

  - super

    - `super`关键字表示父类（超类）。子类引用父类的字段时，可以用`super.fieldName`。例如：

      ```java
      class Student extends Person {
          public String hello() {
              return "Hello, " + super.name;
          }
      }
      ```

      实际上，这里使用`super.name`，或者`this.name`，或者`name`，效果都是一样的。编译器会自动定位到父类的`name`字段。

    - 必须使用`super`的情况

      ```java
      public class Main {
          public static void main(String[] args) {
              Student s = new Student("Xiao Ming", 12, 89);
          }
      }
      
      class Person {
          protected String name;
          protected int age;
      
          public Person(String name, int age) {
              this.name = name;
              this.age = age;
          }
      }
      
      class Student extends Person {
          protected int score;
      
          public Student(String name, int age, int score) {
              this.score = score;
          }
      }
      
      // 报错
      ```

      如果父类没有默认的构造方法，子类就必须显式调用`super()`并给出参数以便让编译器定位到父类的一个合适的构造方法。

      解决方法：

      ```java
      class Student extends Person {
          protected int score;
      
          public Student(String name, int age, int score) {
              super(name, age); // 调用父类的构造方法Person(String, int)
              this.score = score;
          }
      }	
      ```

  - 子类向父类转换（直接转换类型）

  - 父类向子类转换，会报错
  
  - `Class instanceof Object`检查是否为class的一个实例
  
- 多态

  - 在继承关系中，子类如果定义了一个与父类方法签名完全相同的方法，被称为覆写（Override）。
  - 多态具有一个非常强大的功能，就是允许添加更多类型的子类实现功能扩展，却不需要修改基于父类的代码。
  - `object`基类提供了三个方法：（由上一条，所有的类中都有以下三个方法，自定义类也是）
    - `toString()`：把instance输出为`String`；
    - `equals()`：判断两个instance是否逻辑相等；
    - `hashCode()`：计算一个instance的哈希值。
  - 像调用父类的被override的方法，需要用super来调用
  - `final`
    - `final`修饰的方法不可以被子类`Override`基类的方法
    - `finla`修饰的类不可以被继承
    - `final`修饰的变量在初始化后不可以被修改

- 抽象类

  - 如果父类的方法本身不需要实现任何功能，仅仅是为了定义方法签名，目的是让子类去覆写它，那么，可以把父类的方法声明为抽象方法

  - 把一个方法声明为`abstract`，表示它是一个抽象方法，本身没有实现任何方法语句。因为这个抽象方法本身是无法执行的，所以，被`abstrct`修饰的类也无法被实例化

  - 使用`abstract`修饰的类就是抽象类。我们无法实例化一个抽象类：

    ```
    Person p = new Person(); // 编译错误
    ```

  - ### 面向抽象编程

    当我们定义了抽象类`Person`，以及具体的`Student`、`Teacher`子类的时候，我们可以通过抽象类`Person`类型去引用具体的子类的实例：

    ```
    Person s = new Student();
    Person t = new Teacher();
    ```

    这种引用抽象类的好处在于，我们对其进行方法调用，并不关心`Person`类型变量的具体子类型：

    ```
    // 不关心Person变量的具体子类型:
    s.run();
    t.run();
    ```

    同样的代码，如果引用的是一个新的子类，我们仍然不关心具体类型：

    ```
    // 同样不关心新的子类是如何实现run()方法的：
    Person e = new Employee();
    e.run();
    ```

    这种尽量引用高层类型，避免引用实际子类型的方式，称之为面向抽象编程。

    面向抽象编程的本质就是：

    - 上层代码只定义规范（例如：`abstract class Person`）；
    - 不需要子类就可以实现业务逻辑（正常编译）；
    - 具体的业务逻辑由不同的子类实现，调用者并不关心。

- 接口

  - 如果一个抽象类没有字段，所有方法全部都是抽象方法：

    ```
    abstract class Person {
        public abstract void run();
        public abstract String getName();
    }
    ```

    就可以把该抽象类改写为接口：`interface`。

    在Java中，使用`interface`可以声明一个接口：

    ```
    interface Person {
        void run();
        String getName();
    }
    ```

  - 当一个具体的`class`去实现一个`interface`时，需要使用`implements`关键字。举个例子：

    ```
    class Student implements Person {
        private String name;
    
        public Student(String name) {
            this.name = name;
        }
    
        @Override
        public void run() {
            System.out.println(this.name + " run");
        }
    
        @Override
        public String getName() {
            return this.name;
        }
    }
    ```

    一个类可以实现多个`interface`，例如：

    ```
    class Student implements Person, Hello { // 实现了两个interface
        ...
    }
    ```

- 包

  - `package "name"`用来声明包名称

  - 包的引用

    - 直接写出完整类名
    - 使用`import`导入（在写`import`的时候，可以使用`*`，表示把这个包下面的所有`class`都导入进来（但不包括子包的`class`））

  - Java编译器寻找类名的顺序

    - 如果是完整类名，就直接根据完整类名查找这个`class`
    - 如果是简单类名，按下面的顺序依次查找：
      - 查找当前`package`是否存在这个`class`；
      - 查找`import`的包是否包含这个`class`；
      - 查找`java.lang`包是否包含这个`class`。

  - 为了避免名字冲突，我们需要确定唯一的包名。推荐的做法是使用倒置的域名来确保唯一性。例如：

    - org.apache
    - org.apache.commons.log
    - com.liaoxuefeng.sample

    子包就可以根据功能自行命名。



## Java核心类

