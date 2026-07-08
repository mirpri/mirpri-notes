# Constants and variables

## 类型

### char

Java 的 `char` 类型是 **16 位（2 字节）**，它采用的是 **Unicode** 编码。Unicode 的前 128 个字符（`\u0000` 到 `\u007F`）与 ASCII **完全一致**。所以可以直接把一个 ASCII 字符赋值给 `char`。

| **特性**   | **C++ char**                             | **Java char**        |
| -------- | ---------------------------------------- | -------------------- |
| **占用空间** | 1 字节 (8 bits)                            | 2 字节 (16 bits)       |
| **默认编码** | 依赖系统（通常是 ASCII 或扩展 ASCII）                | 固定为 Unicode (UTF-16) |
| **取值范围** | -128 到 127 (signed) 或 0 到 255            | 0 到 65535 (始终无符号)    |
| **汉字支持** | 一个 `char` 存不下汉字（需要 `char[]` 或 `wchar_t`） | `char` 能存下绝大多数常用汉字   |
Java 规定除了 `char` 之外的所有数值类型都是有符号的

:::warning

char能存下大多数常用的汉字，但有些生僻字码点已经超过了 `U+FFFF`，需要用 **2 个 `char`** 组合在一起才能表示，此时不能把它赋值给char（Too many characters in character literal）

:::
### byte

只用一个字节，不涉及任何字符编码的概念。
### double & float

>`Double` (大写) 是个类，提供了一些方法，与 `double` 概念不同


## 字面值

字面值是直接出现在程序中的常量值。

- 以0开头表示八进制，如`035`；以0x或0X开头表示十六进制。
- 以l或L结尾表示long类型，如`29L`；无后缀表示int类型。
- 浮点数是包含小数点的十进制数，后跟可选的指数部分。如 `18.` `1.8e1` `.18E2`
- 以d或D结尾或者无后缀表示double类型；以f或F结尾表示float类型

> 如果转换过程可能会导致数据丢失，不会自动进行类型转换而会报错。
> ```
> jshell> float x =1.
|  Error:
|  incompatible types: possible lossy conversion from double to float
|  float x =1.;
|           ^^
> ```

:::warning

在同一个方法内部，内层作用域**不允许声明与外层作用域（包括方法参数）同名的局部变量**，但 Java 允许局部变量遮蔽类的成员变量。
即：内层局部变量不能和外层局部变量同名

```java
public static void m(int i){  
  for(int i = 0 ; i < 10; i++){  // Variable 'i' is already defined in the scope
    System.out.println(i);  
  }  
}
```

:::

## `var`

声明局部变量，自动推断类型。不能用于：类的属性、方法参数或返回值、无初始化或null初始化
# Array

## 声明和创建

### 声明数组

```java
int[] arr1;
int arr2[]; // Not preferred
```

声明后，`arr1` `arr2` 为 `null` ，是未指向任何对象的引用。
### 创建数组

```java
arr1 = new int[10];
// arr1 ==> int[10] { 0, 0, 0, 0, 0, 0, 0, 0, 0, 0 }
arr2 = new int[]{1,2,3,4,5}
// arr2 ==> int[5] { 1, 2, 3, 4, 5 }
```

数组元素被赋默认值：`0` / `'\u0000'` / `false`

合并声明和创建：

```java
elementType[] arrayRefVar = new elementType[arraySize];
```

## 数组复制

直接用赋值 `arr2 = arr1` 只是将引用指向 `arr1` 的对象，不能实现数组复制。

复制数组的方法：
- 使用循环来复制每个元素
- 使用System.arraycopy方法：两个数组都预先实例化了
- 调用数组的clone方法复制：被复制的数组变量可以没有实例化

>Clone 是继承自 Object 的方法，因此所有对象都具有 clone 方法

>数组支持"foreach"循环：`for(int i:arr1) System.out.println(i);`
>String is not Iterable, 要先用`.toCharArray()`转换
