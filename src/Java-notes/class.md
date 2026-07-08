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

| 访问权限       | 本类  | 本包  | 子类（它包） | 它包  |
| ---------- | --- | --- | ------ | --- |
| public     | √   | √   | √      | √   |
| protected  | √   | √   | √      | X   |
| **包级(默认)** | √   | √   | X      | X   |
| private    | √   | X   | X      | X   |
静态方法不能使用`this`, `super`关键字和访问成员方法/属性
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

第一次加载子类时的执行顺序：
1. 父静态变量、静态初始化块（先装入类）
2. 子静态变量、静态初始化块 
3. 父实例变量（实例化对象）
4. 父实例块
5. 父构造
6. 子实例变量
7. 子实例块
8. 子构造

## 析构？

>==Java does **not have explicit destructors**== like those found in C++. Instead, memory management is handled automatically by the Java Virtual Machine (JVM) through a process called **garbage collection**. There is an inherited method called `finalize`, but this is called entirely at the discretion of the garbage collector and cannot be manually invoked. It has been officially deprecated and marked for removal.

## 抽象类

即使函数不含抽象方法，也可以被声明为抽象类。
## 继承

java 中用 `extends` 关键字表示继承

静态函数和属性只能被隐藏，不会被重写。出现这些情况时，隐藏的成员会被使用：
1. Reference declared as parent type       Animal a = new Dog(); → a.x reveals
2. Cast to parent type                     ((Animal)dog).x reveals
3. Passed as parent type argument          void f(Animal a) → a.x reveals
4. Parent's own method uses its members⭐    Animal.whoAmI() always sees Animal.type
5. Explicit parent class access            Animal.method() / Animal.field


*构造函数不能被继承*

实现父类抽象方法和重写普通方法，它们遵循的规则是完全一模一样的：
1. 签名（Signature）：必须相同
2. 可见性（访问权限）：可以不同，但只能“扩大”，不能“缩小”
3. 返回值（Return Type）：子类重写方法的返回值，可以是父类方法返回值的“子类”


### 实现接口

Java 不能多继承，但可以实现多个[接口](./interface)

## 内部类

在一个代码块（通常是函数体或方法体内部）定义的类
- **作用域：** 仅在定义它的代码块或方法内部有效。
- **访问权限：** 不能使用 `public`、`private` 等访问修饰符。
- **变量访问：** 可以直接访问外部类的成员变量和方法；它还可以访问定义它的方法中的局部变量，但这些变量必须是***隐式或显式声明为 `final` 的***（effectively final）。

:::info

局部变量生命周期很短，方法执行完就销毁了；而内部类对象可能还存活在堆内存中。为了防止“变量没了对象还在”的尴尬，Java 在底层把这个变量**复制了一份**给匿名类。为了保证复制的那份数据和原本的数据绝对一致，Java 强行规定这个变量不能被修改。

:::

```
Method/Block
│
└── class Local {         ← born here, dies here
        - sees outer fields
        - sees effectively final locals
        - can implement interfaces
        - can be instantiated multiple times
        - invisible outside this block
    }
```

## 匿名内部类

```java
new 父类名/接口名() { // 1. 在这里实现接口的方法，或者重写父类的方法 // 2. 这里也可以定义自己的属性和方法（但外部通常无法直接调用） };

public void start(Stage primaryStage) {
  // Omitted
  btEnlarge.setOnAction(
    new EventHandler<ActionEvent>() {
	  @Override
      public void handle(ActionEvent e) {
        circlePane.enlarge();
      }
    });
}
```

## lambda