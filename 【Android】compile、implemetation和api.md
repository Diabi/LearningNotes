[参考博客](https://blog.csdn.net/Next_Second/article/details/78428086)

## Android gradle 3.0.0 中依赖指令compile、implementation、api 的区别

### 1. api

完全等同于compile指令，没区别，你将所有的compile改成api，完全没有错。

### 2. implementation

这个指令的特点就是，对于使用了该命令编译的依赖，对该项目有依赖的项目将无法访问到使用该命令编译的依赖中的任何程序，
也就是将该依赖隐藏在内部，而不对外部公开。


简单的说，就是使用implementation指令的依赖不会传递。举个简单的例子，有一个module为testsdk，testsdk依赖于gson：

```java
implementation 'com.google.code.gson:gson:2.8.2'
```

另一个module为app，app依赖于testsdk：
```java
implementation project(':testsdk')
```

这时候，因为testsdk使用的是implementation 指令来依赖gson，所以**app里边不能引用gson**。

但是，如果testsdk使用的是api来引用gson：

```java
api 'com.google.code.gson:gson:2.8.2'
```

则与gradle3.0.0之前的compile指令的效果完全一样，app的module也可以引用gson。这就是api和implementation的区别。

### 3. 简单总结

* api：和 compile 的作用一样，当前 module 会暴露其依赖的其他 module 内容。
* implementation：只在内部使用了该 module，不会向外部暴露其依赖的 module 内容。

### 4. 使用建议

依赖首先应该设置为implementation的，如果没有错，那就用implementation，如果有错，那么使用api指令。使用implementation会使编译速度有所增快。
