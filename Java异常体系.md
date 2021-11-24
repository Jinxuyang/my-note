## Java异常体系

```ascii
                     ┌───────────┐
                     │  Object   │
                     └───────────┘
                           ▲
                           │
                     ┌───────────┐
                     │ Throwable │
                     └───────────┘
                           ▲
                 ┌─────────┴─────────┐
                 │                   │
           ┌───────────┐       ┌───────────┐
           │   Error   │       │ Exception │
           └───────────┘       └───────────┘
                 ▲                   ▲
         ┌───────┘              ┌────┴──────────┐
         │                      │               │
┌─────────────────┐    ┌─────────────────┐┌───────────┐
│OutOfMemoryError │... │RuntimeException ││IOException│...
└─────────────────┘    └─────────────────┘└───────────┘
                                ▲
                    ┌───────────┴─────────────┐
                    │                         │
         ┌─────────────────────┐ ┌─────────────────────────┐
         │NullPointerException │ │IllegalArgumentException │...
         └─────────────────────┘ └─────────────────────────┘
```

异常对象都是派生于Throwable,后又分为两个分支Error和Exception

Error描述了Java运行时系统的内部错误和我资源耗尽错误,程序不应该抛出这种类型的对象,这样的问题除了告诉用户,尽力使程序安全终止之外,就无能为力了



在设计程序时,需要关注Exception,Exception又分为两个个分支,一个分支派生于RuntimeException,另一个分支包含其他异常.

由程序错误导致需要修改代码的异常属于RuntimeException,而程序本身没有异常由于I/O错误这类问题导致的异常属于其他异常

派生于RuntimeException的异常包含下面几种情况：

- 错误的类型转换。

- 数组访问越界。
-  访问null指针。

不是派生于RuntimeException的异常包括：

- 试图在文件尾部后面读取数据。
- 试图打开一个不存在的文件。
- 试图根据给定的字符串查找Class对象，而这个字符串表示的类并不存在。

> 如果出现RuntimeException异常，那么就一定是你的问题

Java语言规范将派生于Error类或RuntimeException类的所有异常称为非受查（unchecked）异常

所有其他的异常称为受查（checked）异常



一个方法必须声明所有可能抛出的受查异常，而非受查异常要么不可控制（Error），要么就应该避免发生（RuntimeException）。

如果方法没有声明所有可能发生的受查异常，编译器就会报错。



## throw,throws

Java抛出异常有两种情况

1. 系统自动抛出异常比如`int a = 1/0;`

2. 使用throw手动抛出

​		throw 是抛出了一种异常,异常确确实实发生了

​		throw用在方法上,表示这个方法可能出现的异常,将会由调用这个方法的方法进行处理



## try-catch-finally

```java
try {
   //code 
} catch (Exception e) {
    //resovle exception
} finally {
    // do someting 
}
```

当捕获到对应的异常时会执行catch代码块中的代码

无论如何finally代码块都会被执行**(不确定,有待试验,在抛出Error异常后会被执行吗?)**



## try-with-resource

当方法实现了Closeable或AutoCloseable接口后,使用

```java
try(Xxx xx = ...) {
    //code
}
```

程序正常退出或2存在一个异常时,后会自动调用xx.close()方法



> 只要需要关闭资源，就要尽可能使用带资源的try语句。

## 异常机制技巧

1. 异常处理不能代替简单的测试

   ```java
   if(!s.empty()) s.pop;
   ```

   ```java
   try {
       s.pop();
   } catch (EmptyStackException e) {
       //code
   }
   
   ```

   > 在测试的机器上，调用isEmpty的版本运行时间为646毫秒。捕获EmptyStackException的版本运行时间为21739毫秒

​		可以看出，与执行简单的测试相比，捕获异常所花费的时间大大超过了前者，因此使用异常的基本规则是：		只在异常情况下使用异常机制。

2. 在检测错误时，“苛刻”要比放任更好

   > 返回一个虚拟的数值，还是抛出一个异常，哪种处理方式更好？例如，当栈空时，Stack.pop是返回一个null，还是抛出一个异常？我们认为：在出错的地方抛出一个EmptyStackException异常要比在后面抛出一个NullPointerException异常更好。

3. 不要羞于传递异常

   ```java
   public void readStuff(String filename) throws IOException {
       InputStream in = new FileInputStream(filename);
       ...
   }
   ```

   

   传递异常要比捕获这些异常更好：让高层次的方法通知用户发生了错误，或者放弃不成功的命令更加适宜。

​		2、3可以归纳为“早抛出，晚捕获”



## 断言

断言假设确信某个属性符合要求

例如需要计算

```java
double y = Math.sqrt(x);
```

这里我们确信x是一个非负数,因为x是另一个计算的结果,这个结果不可能是负数

可以

```java
if (x < 0) throw new IllegalArgumentException("x < 0");
```

这样这段代码会一直保留在程序中,即使测试完毕,如果代码中包含大量这样的检查会使程序运行变慢



断言机制允许在测试起劲啊向代码中插入一些检查语句,当代码发布时这些语句会被自动删除.



### 使用

```java
assert 条件;
assert 条件:表达式;
```

如果条件为false就会抛出`AssertionError`,第二种情况表达式将会被传入`AssertionError`并转换为一个消息字符串

### 启用

默认情况下断言被禁(当方法上有@org.junit.jupiter.api.Test注解时会被打开)用可以使用`java -enableasserttions MyApp`或`-ea`打开

