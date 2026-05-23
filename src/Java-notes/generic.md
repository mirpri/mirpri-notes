# Generic

泛型集合类型没有**协变性**。
引入泛型之前，数组被设计成了**协变的**。

```java
// 因为数组是协变的，所以这一句编译完全合法
Fruit[] fruits = new Apple[10]; 

// 我们往“水果数组”里放一个“香蕉”，逻辑上没毛病吧？编译也能通过。
fruits[0] = new Banana(); 

// 💥 灾难发生！运行到上面那句时，程序会直接抛出 ArrayStoreException！
```


## 通配泛型

### 上界通配符`? extends`
```java
// ✅ 使用 ? extends 开启安全的协变 
List<? extends Fruit> fruits = new ArrayList<Apple>();
```
- 不能添加除了 `null` 之外的任何元素。
- 允许读取，均解释为`Fruit`。
### 无界通配符 `?`
`? extends Object` 的简写
### 下界通配符 `? super`
```java
// 这个 list 可能是 List<Apple>，也可能是 List<Fruit>，或者是 List<Object> 
List<? super Apple> list = new ArrayList<Fruit>();
```
- 存（写）：允许存入 `Apple` 以及子类。    
- 取（读）：解释为最顶层的 `Object` 。

## 泛型擦除

泛型是用 **类型擦除（type erasure）** 方法实现的。泛型的作用就是使得编译器在编译时通过类型参数来检测代码的类型匹配性。**当编译通过，意味着代码里的类型都是匹配的。因此，所有的类型参数使命完成而全部被擦除。** 这种方法使得泛型代码向后兼容使用原始代码的遗留代码.

:::warning
泛型信息在运行时是不可用的
```java
list1 instanceOf ArrayList<String> // Error
new E() // Error (E 是泛型)
new ArrayList<String>[10] // Error, 禁止创建泛型数组
```
:::

## Restrictions

- 泛型不能是基本类型
- 