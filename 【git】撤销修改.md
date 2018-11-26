参考自：[git-scm](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E7%BD%AE%E6%8F%AD%E5%AF%86)

## 修改最后一次commit

**场景再现**

1. 创建并提交a.txt 和 b.txt

![](https://github.com/Diabi/LearningNotes/blob/master/screenshot/1.jpg)

2. 查看log

![](https://github.com/Diabi/LearningNotes/blob/master/screenshot/2.jpg)


**现在需要更改最后一次提交，把b.txt从提交中去掉，具体实现如下：**

3. reflog -> reset -> rm

![](https://github.com/Diabi/LearningNotes/blob/master/screenshot/3.jpg)

4. commit --amend

![](https://github.com/Diabi/LearningNotes/blob/master/screenshot/4.jpg)
