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

## 可见性

在同一个方法内部，内层作用域**不允许声明与外层作用域（包括方法参数）同名的局部变量**。，但 Java 允许**局部变量遮蔽类的成员变量**。

```java
public static void m(int i){  
  for(int i = 0 ; i < 10; i++){  // Variable 'i' is already defined in the scope
    System.out.println(i);  
  }  
}
```