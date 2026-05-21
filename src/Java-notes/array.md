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

>数组支持"foreach"循环：
>`for(int i:arr1) System.out.println(i);`
