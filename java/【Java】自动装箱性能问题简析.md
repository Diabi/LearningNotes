[参考博客](https://droidyue.com/blog/2015/04/07/autoboxing-and-autounboxing-in-java/)

### 什么是自动装箱和拆箱

自动装箱就是Java自动将原始类型值转换成对应的对象，比如将int的变量转换成Integer对象，这个过程叫做装箱，反之将Integer对象转换成int类型值，
这个过程叫做拆箱。因为这里的装箱和拆箱是自动进行的非人为转换，所以就称作为自动装箱和拆箱。

### 何时发生自动装箱和拆箱

自动装箱和拆箱在Java中很常见，比如我们有一个方法，接受一个对象类型的参数，如果我们传递一个原始类型值，那么Java会自动讲这个原始类型值转换
成与之对应的对象。最经典的一个场景就是当我们向ArrayList这样的容器中增加原始类型数据时或者是创建一个参数化的类，比如下面的ThreadLocal。

```java
ArrayList<Integer> intList = new ArrayList<Integer>();
intList.add(1); //autoboxing - primitive to object
intList.add(2); //autoboxing

ThreadLocal<Integer> intLocal = new ThreadLocal<Integer>();
intLocal.set(4); //autoboxing

int number = intList.get(0); // unboxing
int local = intLocal.get(); // unboxing in Java
```

### 自动装箱的弊端

自动装箱有一个问题，那就是在一个循环中进行自动装箱操作的情况，如下面的例子就会创建多余的对象，影响程序的性能。

```java
Integer sum = 0;
 for(int i=1000; i<5000; i++){
   sum+=i;
}
```

上面的代码sum+=i可以看成sum = sum + i，但是+这个操作符不适用于Integer对象，首先sum进行自动拆箱操作，进行数值相加操作，最后发生自动装箱操作转
换成Integer对象。其内部变化如下

```java
int result = sum.intValue() + i;
Integer sum = new Integer(result);
```

由于我们这里声明的sum为Integer类型，在上面的循环中会**创建将近4000个无用的Integer对象**，在这样庞大的循环中，会降低程序的性能并且加重了垃圾回收的工
作量。因此在我们编程时，需要注意到这一点，正确地声明变量类型，避免因为自动装箱引起的性能问题。
