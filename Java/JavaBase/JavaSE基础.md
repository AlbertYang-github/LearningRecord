# JavaSE基础
## 基本数据类型
### 占用存储空间的大小
<table>
<tr>
<td>类型</td>
<td>占用存储空间的大小</td>
</tr>
<tr>
<td>boolean</td>
<td>没有明确指定, 只能取true或false</td>
</tr>
<tr>
<td>char</td>
<td>16 bits</td>
</tr>
<tr>
<td>byte</td>
<td>8 bits</td>
</tr>
<tr>
<td>short</td>
<td>16 bits</td>
</tr>
<tr>
<td>int</td>
<td>32 bits</td>
</tr>
<tr>
<td>long</td>
<td>64 bits</td>
</tr>
<tr>
<td>float</td>
<td>32 bits</td>
</tr>
<tr>
<td>double</td>
<td>64 bits</td>
</tr>
</table>

**整数默认类型：int** <br/>
**小数默认类型：double**

### 类型转换
* 将浮点数转换为整型时，截取小数点之间的值，并非四舍五入；可以使用java.lang.Math中的round()方法进行四舍五入

* 如果对char、byte或short做算数或位运算时，在运算之前都会自动转换为int，最终结果也是int类型

* 表达式中最大的数据类型决定表达式最终结果的数据类型

## 初始化
### 初始化顺序
**在类的内部，变量定义的先后顺序决定了初始化的顺序；即时变量定义散布于方法定义之间，它们仍旧会在任何方法（包括构造器）被调用那个之前得到初始化。**

### 继承体系中初始化块以及构造方法的初始化顺序
**静态初始化块最先加载，非静态初始化块与构造方法次之**
```
public class MyTest {
    public static void main(String[] args) {
        new C();
    }
}

class A {
    static {
        System.out.println("A 的静态初始化块");
    }
    {
        System.out.println("A 的非静态初始化块");
    }
    public A() {
        System.out.println("A 的无参构造器");
    }
}

class B extends A {
    static {
        System.out.println("B 的静态初始化块");
    }
    {
        System.out.println("B 的非静态初始化块");
    }
    public B() {
        System.out.println("B 的无参构造器");
    }
    public B(String msg) {
        this();
        System.out.println("B 的带参构造器，器参数值：" + msg);
    }
}

class C extends B {
    static {
        System.out.println("C 的静态初始化块");
    }
    {
        System.out.println("C 的非静态初始化块");
    }
    public C() {
        super("C调用父类带参构造器");
        System.out.println("C 的无参构造器");
    }
}
```
执行结果：
```
A 的静态初始化块
B 的静态初始化块
C 的静态初始化块
A 的非静态初始化块
A 的无参构造器
B 的非静态初始化块
B 的无参构造器
B 的带参构造器，器参数值：C调用父类带参构造器
C 的非静态初始化块
C 的无参构造器
```

## 可变长参数列表
### 概述
有了可变长参数，就不用显示地编写数组语法，当指定参数时，编译器实际上会填充数组；获取的仍旧是一个数组，这就是为什么可以使用foreach来迭代的该参数的原因。
- 将0个参数传递给可变长参数列表是可行的

## 关键字
### final
- final数据不允许被修改
- final方法不允许被覆盖
- final类不允许被继承

## 静态导入
**静态导入某个类之后，调用该类下的静态方法时可以直接使用方法名，不需要写为包名.方法名**

## 三大特性（封装、继承、多态）
### 封装
**通过合并特征和行为来创建新的数据类型**
```
特征 --> 属性（域）
行为 --> 方法
数据类型 --> 类
```
### 继承
**继承是复用类的一种方式（另一种方式是组合，即：新的类是由现有对象组成的）**
* 子类实例化后无法使用父类中的private修饰成员变量和方法
* 子类构造方法第一句会默认（隐式）使用super关键字来实例化父类

### 多态
**同一消息可以根据发送对象的不同而采用多种不同的行为方式**
```
同一消息 --> 引用类型都是基类类型
发送对象 --> 对象本身的类型
不同的行为方式 --> 对象各自的方法实现
```
- 任何域访问操作都将由编译器解析，因此不存在多态

### 内部类
- 在外部类之外构造内部类
```
Outer.Inner inner = new Outer().new Inner()
```
- 在外部类之外构造静态内部类
```
Outer.Inner inner = new Outer.Inner();
```