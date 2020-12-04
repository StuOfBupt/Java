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