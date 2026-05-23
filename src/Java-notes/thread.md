# Multithreading

## Runnable, Thread & Executor

- `Runnable`: 包含`run`方法，定义线程执行的方法
- `Thread`: 线程类，实现了`run`接口
- `Executors`: 线程池，复用线程，避免反复创建线程开销
>**线程池模式：** 线程绑定到一个**永不结束的循环**（`Worker.run()`，而非`Thread.run()`方法） ，只有超时或关闭时，才允许线程退出循环死亡。

创建线程的方法：
- 定义Thread类的子类并覆盖run方法	```
- 实现接口Runnable的run方法，传递给Thread构造线程
```java
class t1 implements Runnable {  
  @Override  
  public void run() {  
    System.out.println("t1!");  
  }  
}  
  
class t2 extends Thread {  
  @Override  
  public void run() {  
    System.out.println("t2!");  
  }  
}  
  
public class ThreadTest {  
  public void main() {  
    Runnable task = new t1();  
    Thread thread1 = new Thread(task);  
    thread1.start();  
  
    Thread thread2 = new t2();  
    thread2.start();  
  }  
}
```


## 线程同步

只读的线程是安全的，不用上锁。

### synchronized

隐式地在**对象实例**或**类**上加锁

**同步方法**
- 静态方法中，对整个类上锁
- 动态方法中，对该对象上锁

上锁后，锁住了该对象/类所有的同步方法。
类的锁和对象的锁是独立的。即静态锁不影响实例方法，对象锁不影响静态方法。

```java
public synchronized void A() {
	// ...
}

// 等价于：
public void A() {
    synchronized(this) {
	    // ...
    }
}

public static synchronized void B() {
	// ...
}

// 等价于：
public static void B() {
	synchronized(B.class) {
		// ...
	}
}
```

**同步语句块**
对表达式对象加锁，`expr`必须是对一个对象的引用
```java
synchronized (expr) {   statements;  }
```

#### wait/notify
### Lock

显式的加锁

`ReentrantLock`: 可重入的锁

#### await/signal
