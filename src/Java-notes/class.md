# Class

## Object 类

Java 中所有类都继承自 `java.lang.Object` 类

### toString
`toString()` 是Object类的方法，返回描述该对象的字符串

### equals

默认实现为直接用`==`判断引用是否相等

若用 `Object.quals(a,b)` 来比较：
```
public static boolean equals(Object a, Object b) {
    return (a == b) || (a != null && a.equals(b));
}
```
## 可见性

- private： 只能被当前类定义的函数访问，即只能在类体里访问。
- 包级：无修饰符的成员，只能被同一包中的类访问。（*java 特性*）
- protected：子类、**同一包**中的类的函数可以访问。
- public：  所有类的函数都可以访问。

| 访问权限       | 本类  | 本包  | 子类  | 它包  |
| ---------- | --- | --- | --- | --- |
| public     | √   | √   | √   | √   |
| protected  | √   | √   | √   | X   |
| **包级(默认)** | √   | √   | X   | X   |
| private    | √   | X   | X   | X   |

## 初始化块

在 Java 中，**类的静态初始化块（Static Initialization Block）** 是指用 `static` 关键字修饰的、被大括号 `{}` 包围的一段代码。
它的核心作用是：**专门用来初始化静态变量（类变量），或者在类第一次被加载到内存时，执行一些只需要运行一次的准备工作。**
你可以把它理解为**“类的构造函数”**（普通的构造函数是用来初始化对象的，而静态块是用来初始化整个类的）。

静态初始化块：

1. **执行时机极早：** 当 JVM（Java 虚拟机）第一次加载这个类的时候，静态块就会自动执行。**它比任何对象的创建（`new`）、任何普通代码块和构造函数都要早。**   
2. **绝对只执行一次：** 不管你在这个程序里 `new` 了 100 个还是 1000 个该类的对象，静态初始化块在整个程序运行期间**只会在类加载时执行一次**。    
3. **只能访问静态资源：** 在静态块里面，你只能访问类的静态变量和静态方法。
4. **按顺序执行：** 如果一个类里写了多个静态初始化块，JVM 会严格按照它们在代码里从上到下的先后顺序依次执行。

实例初始化块：

- **调用频率：** 每次创建对象（`new`）时，它都会被执行一次。这与静态初始化块（只执行一次）完全不同。    
- **执行顺序：** 
	1. 父类静态块 $\rightarrow$ 子类静态块（仅类加载时）。    
    2. **父类实例初始化块** $\rightarrow$ 父类构造函数。    
    3. **子类实例初始化块** $\rightarrow$ 子类构造函数。    
- **它的位置：** 编译器实际上会把这些大括号里的代码，**自动复制到每一个构造函数的最开头**（在 `super()` 之后）。

```java
class SuperClass {
    static int i = 10;
    
    // 1. 静态初始化块
    static {
        System.out.println("1. static in SuperClass");
    }
    
    // 2. 实例初始化块
    {
        System.out.println("2. SuperClass instance block is called");
    }

    // 3. 构造函数
    public SuperClass() {
        System.out.println("3. SuperClass Constructor is called");
    }

    public static void main(String[] args) {
        System.out.println("--- 第一次 new ---");
        new SuperClass();
        
        System.out.println("--- 第二次 new ---");
        new SuperClass();
    }
}
```

**运行结果会是这样的：**

```
1. static in SuperClass
--- 第一次 new ---
2. SuperClass instance block is called
3. SuperClass Constructor is called
--- 第二次 new ---
4. SuperClass instance block is called
5. SuperClass Constructor is called
```

既然它在构造函数之前跑，我直接把代码写在构造函数第一行不就行了？但在以下场景，它非常有用：

1. **提取构造函数间的公共代码：** 如果你的类有 4、5 个重载的构造函数，且每个构造函数都有一段相同的初始化逻辑（比如给某个复杂对象赋初值），你可以把这段逻辑写在 `{}` 里。这样你就不需要每个构造函数都去调用同一个私有方法了。    
2. **匿名内部类的初始化：** 匿名内部类是没有名字的，所以它**没有构造函数**。如果你想在创建匿名内部类时执行一些逻辑，大括号 `{}` 就是你唯一的选择。

## 析构？

>==Java does **not have explicit destructors**== like those found in C++. Instead, memory management is handled automatically by the Java Virtual Machine (JVM) through a process called **garbage collection**. There is an inherited method called `finalize`, but this is called entirely at the discretion of the garbage collector and cannot be manually invoked. It has been officially deprecated and marked for removal.

## 继承

java 中用 `extends` 关键字表示继承

*构造函数不能被继承*

实现父类虚函数（抽象方法）”和“重写（Override）普通方法”，它们遵循的规则是完全一模一样的：
1. 签名（Signature）：必须相同
2. 可见性（访问权限）：可以不同，但只能“扩大”，不能“缩小”
3. 返回值（Return Type）：子类重写方法的返回值，可以是父类方法返回值的“子类”

### 实现接口

Java 不能多继承，但可以实现多个[接口](./interface)