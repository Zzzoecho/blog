# Java

## 代码 -> 执行

1. `test.java`写代码
2. 命令行 `javac test.java` 编译出`test.class`文件
3. `java test` 启动 JVM(java 虚拟机)运行`test.class`文件, 将字节码转换成平台能够理解的形式来运行

运行时只需要指定类名

## 基础概念

JDK: 开发工具包, 包括了 java 运行环境, java 工具和基础的类库
JRE: java 运行时环境
JVM: Java 虚拟机(java virtual machina), 跨平台的核心, 所有 java 程序会先通过 javac 编译为 .class 文件, 然后在虚拟机上运行

优点:

1. 跨平台
2. 面向对象
3. 虚拟机
4. 内存自动回收
5. 多线程

`javac hello.java`

每个程序都有一个统一的入口 main()函数

文件名与类名(Class)一致, 首字母大写

```java
public class MyClass {
  public static void main (String[] args) {
    System.out.print('Hello World);
  }
}
```

### 访问修饰符

控制访问权限

- public: 公有
- default: 不写, 默认
- protected: 自己和继承的子元素可以用
- private: 只有自己可以用

```java
public class Hello {
  public static void main (String[] args) {
        System.out.println("Hello Java");
  }
}
```

## 类型

1 字节 = 8 位

基础类型(8):

- 整数:
  - byte(1 字节): -128~127
  - short(2 字节): -32768~~32767
  - int(4 字节): 20 亿左右
  - long(8 字节): 时间
- 浮点数: float(4), double(8)
- char
- boolean

0bxxx - 表二进制

java 中 A-Z, \_, \$, π 等都是字母. 可以用 Character 的 `isJavaIdentifierStart` 和 `isJavaIdentifierPart` 来检测

变量被声明后, 必须用赋值语句对其进行显式初始化. 否则会报错 `variable not initialized`

final -- 常量
static final -- 类常量(可以在一个类中的多个方法中使用), 定义在 main 的外部

引用类型()

栈内存: 先进后出, 用于实现方法(函数执行过程也是先进后出)
堆内存

### 数值类型之间的转换

二元操作式, 两边的操作数要转换为同一种类型, 按浮点数 > 整数, 字节多 > 字节少的优先级转换

强制类型转换: `int x = (int)Math.round(x);` (目标类型)变量名

枚举: enum

检测字符串是否相等 `s.equals(t)` , `s.equalsIgnoreCase(t)` 不区分大小写

**不能用 `==` 检测字符串, 字符串是引用类型, 只比较地址**

### Array

声明

- new type[n]; -> [0,0,0] type: 数组内元素的类型, n: 数组长度
- {a,b,c}; -> [a,b,c]

直接赋值两个变量会引用同一个数组, 拷贝 `Arrays.copyOf(arr, length)`, 多余的元素会被赋值为 0

排序 -- `Arrays.sort(arr)`
打印 -- `Arrays.toString(arr)`
打印二维数组 -- `Arrats.deepToString(arr)`

### Object

- Object 是 Java 所有对象的父对象
- 类默认继承Object

A extend B:

- A 拥有 B 的方法
- B 的属性

getClass -- 返回包含对象信息的类对象
getName -- 返回类名
getSuperclass -- 以Class对象的形式返回这个类的超类信息
`a.equals(b)` -- 判断两个对象是否有相同的引用

为了防范参数可能是null的情况, 可以用`Objects.equals(a, b)`, 如果有参数是null, 则返回false, 如果都不是null, 则调用`a.equals(b)`

- toString()
- hasCode 字符串的散列码由内容导出, StringBuffer是什么?

**修改了 equals 方法, 就必须重写 hashCode 方法**
HashMap, 底层: 数组中存放链表(1.8版本前) -> 红黑树(1.8以后).
元素的hashCode决定了元素在HashMap中存放的位置, 寻找时会先根据**hashCode**找到数组的位置, 再根据`==`先判断元素是不是同一个(性能), `equals`判断元素内容是否相等

### StringBuilder & StringBuffer

String在Java中是不可更改的, 每次'+'都是创建了一个新对象. StringBuilder 和 StringBuffer可以多次修改而不产生新的对象. 可变对象, 可以预分配缓冲区.

区别:
StringBuilder: 线程不安全, 速度快, 多数情况下使用
StringBuffer: 线程安全, 同步但速度慢.

### 集合

#### ArrayList

- 本质是一个可变的数组
- 数组长度不变、但是实际中需要一个可以自动变长的数组，所以就出现了 ArrayList

1. 自动扩容原理实现.

```java
  @Test
  public void tstAdd() throws Exception {
    /*1. 创建一个空集合, 集合中只能有对象类型, 此时是一个空集合常量*/
    List<Integer> list = new ArrayList<>();
    /*2. 第一次 add 的时候、创建一个长度为10的数组*/
    list.add(0);
    /*3. 填充9个数据、达到数组不能容纳的长度*/
    for (int i = 1; i < 10; i++) {
      list.add(i);
    }
    /*4. 第11个元素的时候，不够，自动扩容为当前的 1.5 倍*/
    list.add(10);
  }

```

2. 最佳实践: 知道需要多长的数组, 不用扩容

> `List<Integer> bestList = new ArrayList<>(10);`

扩容，以前的内存不够、要申请新的内存、然后拷贝以前的数据去新的内存、伤性能

**删除时注意类型 int & integer**
remove(index): 某个位置,返回被删除的值
remove(Object): 某个对象 , `== then equals` , 返回是否删除成功

##### 关于迭代删除

任何 for 循环 不能随便删除

```java
  @Test
  public void tstIteratorRm() throws Exception {
    List<Integer> list = fill(10);

    /*标准姿势*/
    Iterator<Integer> iterator = list.iterator();
    while (iterator.hasNext()) {
      Integer next = iterator.next();
      /*通过 iterator 删除 可以*/
      iterator.remove();
      /*永远不允许通过 list 删除*/
      list.remove(next);
    }

    for (int i = 0; i < list.size(); i++) {
      /*可以删除、不报错，不建议用*/
      if (list.get(i) == 2) {
        list.remove(list.get(i));
      }
    }

    for (Integer integer : list) {
      if (integer == 2) {
        list.remove(integer);
      }
    }
  }

  // jdk8 的姿势
  @Test
  public void tstIteratorRm() throws Exception {
    List<Integer> list = fill(10);
    list.removeIf(x -> x < 5);
    System.out.println(list);
  }
```

stream 流

```java
  @Test
  public void streamDm() throws Exception {
    List<Integer> list = fill(10);
    list.stream().map(integer -> integer * 2).filter(x -> x > 10).forEach(System.out::println);
  }

```

## 类

类的基本结构

1. 实例域(instance fields): 变量声明, 通常用 private 标记为私有. 只暴露出 public 的域访问器(不能直接返回引用类型, 会导致私有的实例域被更改, 先`clone`)和域更改器. 不可更改的要`final`
2. 构造函数(constructor): 与类同名, 无返回值, new 调用
3. 方法(methods): 隐式(implicit)参数 -- 方法名前的类对象, this, 显式(explicit)参数 -- 函数传参.

类的方法可以访问类下任何实例的私有域.

设计技巧:

1. 一定要保证数据私有
2. 一定要对数据初始化
3. 不要在类中使用过多的基本类型, 用其他的类代替多个有相关性的基本类型
4. 不是所有的域都需要独立的域访问器和域更改器
5. 将职责过多的类分解

### static

> 静态域: 类的所有实例共享的域, 既在类中只有一个这样的域, 而每个对象会拷贝实例域

静态变量: 在多个实例中共享 如: `Math.PI`, `System.out`
静态方法: 不能向实例实施操作的方法, 既没有this, 不能访问实例域. `Math.pow(x, a)`. 建议用类名来调用静态方法, 而不是实例后的对象. 容易误解, 而静态方法计算的结果跟实例对象也没关系. 不用构造实例对象也可以调用.

使用静态方法的场景:

- 方法不需要访问对象状态, 所需参数都是由显示参数提供
- 方法只需要访问类的静态域

### 方法

方法名 + 方法参数类型 -- 构成唯一方法的签名, 重载

默认域初始化: 数值 - 0, 布尔 - false, 对象引用 - null

### 构造器

构造器的第一个语句是 `this(...)`, 这个构造器将调用同一个类的另一个构造器. 可复用公共的构造器

一般访问控制符是 public, 有时也可以用 private, 在类里新建一个函数用于数据的校验, 如果校验通过则 new 一个

初始化数据域:

1. 在构造器中设置值
2. 在声明中赋值
3. 初始化块, 静态初始化块 `static { a = xx }`

## 包

// TODO:
> 一个 class 的 namespace

class 要求名称唯一, 既 packageName + className

- fullClassName:
- simpleClassName:
- import

本身提供了一工具包:

- java.lang: 核心 .
- java.util

## 继承

> 关键字 extends 表明: 正在构造的新类派生于一个已存在的类

已存在的类: 超类(superclass), 基类(base class), 父类(parent class)
新类: 子类(subclass), 派生类(derived class), 孩子类(child class)

>super: 1. 调用超类的方法 2.调用超类的构造器

构造器中的super: 调用超类中含有a,b,c,d参数的构造器的简写

子类构造器的第一条语句必须是使用super调用超类构造器, 如果没有则调用超类默认(无参数)的构造器. 如果超类没有不带参数的构造器, 那么会报错.

> 多态(polymorphism): 一个对象变量可以指示多种实际类型的现象
> 动态绑定(dynamic binding): 在运行时能够自动地选择调用哪个方法的现象

### 继承层次

> 继承层次(inheritance hierarchy): 由一个公共超类派生出来的所有类的集合
> 继承链(inheritance chain): 在继承层次中, 从某个特定的类到其祖先的路径

阻止继承: final类

强制类型转换: `类型 a = (类型) b`

- 只能在继承层次内进行类型转换
- 在将超类转换成子类之前, 应该用 `instanceof` 检查. `null instanceof C` -> 返回 false

### 抽象类

关键字 abstract , 用于占位, 具体实现在子类中
一个有多个抽象方法的类必须被声明为抽象类

抽象类不能被实例化

## 序列化与反序列化

> 序列化: 将对象转换成二进制数组
> 反序列化: 将二进制数组转换成对象

c 语言 GO python C++ 语言:

```c

数组:

val arr = [1, 2, 3]

c 语言:

arr : 是一个指针,指向数组在的

#include <stdio.h>

/**
* argc: 表示参数长度:
**/
int main(int argc, char *argv[]) {
    printf("%d%s\n", argc,*(argv+1));
}

```

c 要能执行:
-> 二进制的可执行文件



## 项目结构

- 



## 增删改查最佳实践

1. 定义对象的各种属性, 有枚举用枚举, 对于有些需要返回给前端但不希望存进数据库的属性可以用 `@Transient`



## 学习路线

<Java 核心技术> 前 6 章
