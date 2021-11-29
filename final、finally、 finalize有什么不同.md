## final

final 用于修饰类、变量、方法

final 修饰的 class 代表不可以继承扩展

final 的变量是不可以修改的

而 final 的方法也是不可以重写的

## finally

是Java保证某些代码一定被执行的代码块，可以使用try-finally 或者 try-catch-finally 来进行类似关闭 JDBC 连接、保证 unlock 锁等动作

## finalize()

finalize()是Object对象的一个方法，用于实例被垃圾回收器回收的时触发的操作。

Java 9 中，甚至明确将 Object.finalize() 标记为 deprecated

因为你无法保证 finalize 什么时候执行，执行的是否符合预期。

