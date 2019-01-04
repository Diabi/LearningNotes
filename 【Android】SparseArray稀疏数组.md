[参考博客](https://juejin.im/entry/57c3e8c48ac24700634bd3cf)

### 简单介绍

```
SparseArray maps integers to Objects and, unlike a normal array of Objects, its indices can contain gaps. 
SparseArray is intended to be more memory-efficient than a HashMap, because it avoids auto-boxing keys and 
its data structure doesn't rely on an extra entry object for each mapping.
```

这段注释基本解释了该类的作用：
* 使用int[]数组存放key
* 避免了HashMap中基本数据类型需要装箱的步骤
* 不使用额外的结构体(Entry)，单个元素的存储成本下降。

### 与HashMap比较的优缺点

优势

* 避免了基本数据类型的装箱操作
* 不需要额外的结构体，单个元素的存储成本更低
* 数据量小的情况下，随机访问的效率更高

缺点

* 插入操作需要复制数组，增删效率降低
* 数据量巨大时，复制数组成本巨大，gc()成本也巨大
* 数据量巨大时，查询效率也会明显下降

综上，SparseArray的适用场景：
* key为普通数据类型int
* 数据量小
