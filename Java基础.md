##### 1、JDK & JRE

- JDK(Java Development Kit）：面向开发人员使用的SDK（ Software Development Kit ）
- JRE（Java Runtime Environment）：指Java运行环境，面向Java程序的使用者，编写的Java程序必须要有JRE才能够运行

##### 2、equals和==

- ==：

  - 对于基本类型：比较值是否相等
  - 对于引用：比较引用是否相同

  ```java
  String x = "string";
  String y = "string";
  String z = new String("string");
  System.out.println(x==y); 				// true
  System.out.println(x==z); 				// false
  System.out.println(x.equals(y));  // true
  System.out.println(x.equals(z));  // true
  ```

  x、y指向的是同一个常量引用，因此x==y

  z在堆区重新开辟了内存空间，因此z ==  x 为false

- equals

  - **equals本质是等同于==**

    equals源码：

    ```java
    public boolean equals(Object obj){
      return (this == obj);
    }
    ```

  - x.equals(z) 为true的原因：String类重写了equals

    ```java
    public boolean equals(Object anObject){
      if(this == anObject)
       	return true;
      if (anObject instanceof String) {
            String anotherString = (String)anObject;
            int n = value.length;
            if (n == anotherString.value.length) {
                char v1[] = value;
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0) {
                    if (v1[i] != v2[i])
                        return false;
                    i++;
                }
                return true;
            }
        }
        return false;
    }
    ```

- 总结：**== 对于基本类型来说是值比较，对于引用类型来说是比较的是引用；而 equals 默认情况下是引用比较**，只是很多类重新了 equals 方法，比如 String、Integer 等把它变成了值比较，所以一般情况下equals 比较的是值是否相等。

##### 3、final关键字

- 修饰类：类无法被继承
- 修饰方法：方法无法被重写
- 修饰变量：变量第一次被赋值后不能被修改

##### 4、Java字符串类

- String
  - 不是基本数据类型，是被final修饰的类
  - 每次操作后会生成新的String对象（String.substring），而StringBuffer、StringBuilder都是在原有基础上修改字符
- StringBuffer
  - 线程安全的
  - 方法被sychronized修饰
- StringBuilder
  - 非线程安全

##### 5、String str="i" & String str=new String("i")

- 作用不一样
  - str = ”i“ Java虚拟机将其分配到常量池中
  - str = new String("i") Java虚拟机将其分配到堆区

##### 6、抽象类是否必须要有抽象方法

- 不是

```java
abstract class A{
  public static void sayHi(){
    System.out.println("Hi");
  }
}
```

##### 7、接口和抽象类区别

- 实现上：
  - 接口需要用关键字implement
  - 抽象类需要用关键字extends
  - 只能继承一个抽象类，但可以实现多个接口
-  抽象类可以有构造函数，接口没有
- 接口中默认访问修饰符为public，抽象类可以设置任意访问修饰符

##### 8、BIO、NIO、AIO

- BIO（Block IO）：同步阻塞式IO，简单方便但是并发能力低
- NIO（Non IO）：同步非阻塞式IO，客户端与服务端通过通道（channel）通信，实现多路复用
- AIO（Asynchronous IO）：异步非阻塞IO，实现机制基于事件和回调机制

##### 9、反射

- 反射是指程序可以在运行时分析任意的对象，访问、检测和修改它本身状态或行为的一种能力，特别是针对泛型类型。

- 反射机制提供了以下功能：

  - 在运行时判断任意一个对象的类型
  - 在运行时构造任意一个类对象
  - 在运行时判断任意一个类所具有的成员变量和方法
  - 在运行时调用任意一个对象的方法

- Class反射API

  - Class.forName()

    ```java
    String name = "java.util.Collections";
    Class<?> cl = Class.forName(name);
    ```

  - 获取泛型类型变量

    ```java
    TypeVariable[] getTypeParameters();
    ```

    

  - 获取当前类的超类，若当前类为Object，返回null

    ```java
    Type getGenericSuperclass();
    ```

    

  - 获取当前类的接口的泛型类型

    ```java
    Type[] getGenericInterfaces();
    ```

    

  - 获取方法

    ```java
    Method[] getDeclaredMethods()
    ```

    

  - 

- Method反射API

  - 获取泛型返回值

    ```java
    Type getGenericReturnType();
    ```

    

  - 获取泛型参数类型

    ```java
    Type[] getGenericParameterTypes();
    ```

    

  - 获取方法的修饰符

    ```java
    Modifier[] getModifiers();
    ```

    

  - 

  

##### 8、Java访问修饰符

- private：仅针对本类可见
- public：所有类可见
- protected：对本包和所有子类可见
- 默认（不加修饰符）：对本包可见

##### 9、克隆

- Object.clone（）

  - **Object中的clone方法是protected的，只有子类才能访问，因此无法直接调用anObject.clone()，必须重新定义clone为public才能允许所有方法克隆对象**【即便默认的clone能够满足需求，也需要重新定义clone，将其访问修饰符定义为public以便其他对象能够调用clone方法】

    ```java
    @override
    public T clone() throw CloneNotSupportedException{
      	return (T)super.clone();
    }
    ```

    

  - 该方法中只是对逐个域进行拷贝，因为它对这个对象一无所知

- Cloneable接口

  - 该接口是个**标记接口**（不包含任何方法，唯一的作用就是可以进行类型检查 obj instanceof Cloneable），他并没有指定clone方法，该方法是从Object继承而来，这个接口**只是作为一个标记**，指示类的设计者了解克隆过程。
  - 如果对象进行clone调用，但是没有实现这个接口，则会生成受查异常

- 实现深拷贝的一个例子

  ```java
  class Employee implements Cloneable{
    ...	
    public Employee clone() throw CloneNotSupportedException{
      Employee cloned = (Employee)super.clone();
      cloned.hireDay = (Date).hireDay.clone();
      return cloned;
  	}
  }
  ```

  