### 什么是Copy-On-Write？

wikipedia上面有很详尽的解释：[Copy-On-Write](https://zh.wikipedia.org/wiki/%E5%AF%AB%E5%85%A5%E6%99%82%E8%A4%87%E8%A3%BD)

写入时复制（英语：Copy-on-write，简称COW）是一种计算机程序设计领域的优化策略。其核心思想是，如果有多个调用者（callers）同时请求相同资源（如内存
或磁盘上的数据存储），他们会共同获取相同的指针指向相同的资源，直到某个调用者试图修改资源的内容时，系统才会真正复制一份专用副本（private copy）给
该调用者，而其他调用者所见到的最初的资源仍然保持不变。这过程对其他的调用者都是透明的（transparently）。此作法主要的优点是如果调用者没有修改该资
源，就不会有副本（private copy）被建立，因此多个调用者只是读取操作时可以共享同一份资源。


### Java中的``java.util.ConcurrentModificationException``异常

java中，List在遍历的时候，如果被修改了会抛出java.util.ConcurrentModificationException异常

```java
public class Test {
    static ArrayList<Integer> list = new ArrayList<Integer>();
    public static void main(String[] args)  {
        list.add(1);
        list.add(2);
        list.add(3);
        list.add(4);
        list.add(5);
        Thread thread1 = new Thread(){
            public void run() {
                Iterator<Integer> iterator = list.iterator();
                while(iterator.hasNext()){
                    Integer integer = iterator.next();
                    System.out.println(integer);
                }
            };
        };
        Thread thread2 = new Thread(){
            public void run() {
                Iterator<Integer> iterator = list.iterator();
                while(iterator.hasNext()){
                    Integer integer = iterator.next();
                    if(integer==2)
                        list.add(6);  
                }
            };
        };
        thread1.start();
        thread2.start();
    }
}
```

这段代码将会抛出以下异常：

```java
Exception in thread "Thread-0" java.util.ConcurrentModificationException
	at java.util.ArrayList$Itr.checkForComodification(Unknown Source)
	at java.util.ArrayList$Itr.next(Unknown Source)
	at Test$1.run(Test.java:20)
```

因为线程threads2试图在thread1还在读取list的值时去写list

### CopyOnWriteArray

CopyOnWriteArrayList这是一个ArrayList的线程安全的变体，其原理大概可以通俗的理解为:初始化的时候只有一个容器，很常一段时间，这个容器数据、数量
等没有发生变化的时候，大家(多个线程)，都是读取(假设这段时间里只发生读取的操作)同一个容器中的数据，所以这样大家读到的数据都是唯一、一致、安全的，
但是后来有人往里面增加了一个数据，这个时候CopyOnWriteArrayList 底层实现添加的原理是先copy出一个容器(可以简称副本)，再往新的容器里添加这个新的
数据，最后把新的容器的引用地址赋值给了之前那个旧的的容器地址，但是在添加这个数据的期间，其他线程如果要去读取数据，仍然是读取到旧的容器里的数据。

所谓的"写时复制"可以从源码上看出本质

```java

    public boolean add(E e) {
        synchronized (lock) {
            Object[] elements = getArray();
            int len = elements.length;
            Object[] newElements = Arrays.copyOf(elements, len + 1); // 生成一个副本
            newElements[len] = e;
            setArray(newElements);                                   // 更改指针
            return true;
        }
    }

    public E remove(int index) {
        synchronized (lock) {
            Object[] elements = getArray();
            int len = elements.length;
            E oldValue = get(elements, index);
            int numMoved = len - index - 1;
            if (numMoved == 0)
                setArray(Arrays.copyOf(elements, len - 1));
            else {
                Object[] newElements = new Object[len - 1];
                System.arraycopy(elements, 0, newElements, 0, index);  // 生成副本
                System.arraycopy(elements, index + 1, newElements, index,
                                 numMoved);
                setArray(newElements);                                 // 更改指针
            }
            return oldValue;
        }
    }
 ```

### CopyOnWriteArray使用场景

* 读多写少（白名单，黑名单，商品类目的访问和更新场景），为什么？因为写的时候会复制新集合
* 集合不大，为什么？因为写的时候会复制新集合
* 实时性要求不高，为什么，因为有可能会读取到旧的集合数据
