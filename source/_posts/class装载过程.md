---
class装载过程
---

### 1.加载（loading）

将class二进制文件读入内存，==保存在方法区==(类的元数据）,并在堆上创建==java.lang.Class对象==，用来封装类在方法区内的数据结构

class文件来源：

（1）从本地系统中直接加载
（2）通过网络下载.class文件
（3）从zip，jar等归档文件中加载.class文件
（4）从专用数据库中提取.class文件
（5）将java源文件动态编译为.class文件

### 2.验证（verification）

### 3.准备（preparation）

1. 初始化静态变量（类变量），在==方法区==分配内存并初始化为==默认值==（成员变量在堆上分配，属于对象实例范畴）
2. 如果是static final变量，则初始化为==指定值==。

### 4.解析（resolution）

当通过准备阶段之后，进入解析阶段。解析阶段是虚拟机将常量池内的符号引用替换为直接引用的过程，解析动作主要针对类或接口、字段、类方法、接口方法、方法类型、方法句柄和调用点限定符7类符号引用进行。符号引用就是一组符号来描述目标，可以是任何字面量。

直接引用就是直接指向目标的指针、相对偏移量或一个间接定位到目标的句柄。


### 5.初始化（initialization）

一般==主动使用的类==才进行初始化，包括：

1、 创建类的实例，也就是new的方式

2、 访问某个类或接口定义的静态变量，或者对该静态变量赋值（凡是被final修饰，其实更准确的说是在编译器把结果放入常量池的静态字段除外）。==如果通过类访问父类的静态变量，父类会初始化，子类不会==

```java
//A会被初始化
class A {
  static int a = 123;
  //static int a = new Random.nextInt(20);
  static {
    System.out.println("in a static block");
  }
}
public class Test(){
  public static void main(String[] args){
    System.out.println(A.a);
    //~out
    //in a static block
    //123
    //~end
  }
}
//不会初始化！！！
//因为变量a是final，会在编译的时候将123写入常量池
class A {
  static final int a = 123;
  static {
    System.out.println("in a static block");
  }
}
public class Test(){
  public static void main(String[] args){
    System.out.println(A.a);
    //~out
    //123
    //~end
  }
}
```



3、 调用类的静态方法

4、 反射（如 Class.forName(“com.gx.yichun”)）

5、 初始化某个类的子类，则其父类也会被初始化

6、 Java虚拟机启动时被标明为启动类的类（ JavaTest ），还有就是Main方法的类会首先被初始化

最后注意一点对于静态字段，只有直接定义这个字段的类才会被初始化（执行静态代码块），这句话在继承、多态中最为明显！为了方便理解下文会陆续通过例子讲解

> 初始化子类时需要先初始化父类
>
> ==接口的初始化不需要先初始化父接口==，直到使用时才初始化
>
> classA a 这样的语句不会触发初始化
>
> classA a =new classA() //初始化

### 6.使用（using）

### 7.卸载（unloading）



### *接口的装在过程

