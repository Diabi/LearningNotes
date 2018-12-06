参考自[git-scm](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E7%BD%AE%E6%8F%AD%E5%AF%86)

## git reset命令的工作流程简析

 初始状态图

![](https://git-scm.com/book/en/v2/images/reset-start.png)

### 第一步 -> 移动HEAD

reset 做的第一件事是移动 HEAD 的指向。 这与改变 HEAD 自身不同（checkout 所做的）；reset 移动 HEAD 指向的分支。 这意味着如果 HEAD 设置为 master 
分支（例如，你正在 master 分支上），运行```git reset 9e5e6a4```将会使master指向9e5e6a4。

![](https://git-scm.com/book/en/v2/images/reset-soft.png)

无论你调用了何种形式的带有一个提交的 reset，它首先都会尝试这样做。 使用 reset --soft，它将仅仅停在那儿。

现在看一眼上图，理解一下发生的事情：它本质上是撤销了上一次 ```git commit``` 命令。 当你在运行 ```git commit``` 时，Git 会创建一个新的提交，并移动 
HEAD 所指向的分支来使其指向该提交。 当你将它 reset 回 HEAD~（HEAD的父结点）时，其实就是把该分支移动回原来的位置，而不会改变索引和工作目录。 现在你
可以更新索引并再次运行 git commit 来完成 ```git commit --amend``` 所要做的事情了

### 第二步 -> 更新索引

![](https://git-scm.com/book/en/v2/images/reset-mixed.png)

如果指定--mixed 选项，reset将会在这时停止。这也是默认行为，所以如果没有指定任何选项（在本例中只是```git reset HEAD~```），这就是命令将会停止的地方。
现在再看一眼上图，理解一下发生的事情：它依然会撤销一上次提交，但还会取消暂存所有的东西。于是，我们```回滚到了所有git add和git commit的命令执行之前```。

### 第三步 -> 更新工作目录

![](https://git-scm.com/book/en/v2/images/reset-hard.png)

现在让我们回想一下刚才发生的事情。 你撤销了最后的提交、git add 和 git commit 命令以及工作目录中的所有工作。

**必须注意，--hard 标记是 reset 命令唯一的危险用法，它也是 Git 会真正地销毁数据的仅有的几个操作之一。 其他任何形式的 reset 调用都可以轻松撤消，但是 --hard 选项不能，因为它强制覆盖了工作目录中的文件。 在这种特殊情况下，我们的 Git 数据库中的一个提交内还留有该文件的 v3 版本，我们可以通过 reflog 来找回它。但是若该文件还未提交，Git 仍会覆盖它从而导致无法恢复。**

### 对比--soft --mixed --hard

初始状态

![](https://github.com/Diabi/LearningNotes/blob/master/screenshot/5.jpg)

两个提交

* 执行```git reset --soft HEAD~```

![](https://github.com/Diabi/LearningNotes/blob/master/screenshot/6.jpg)

撤销commit，不撤销add

* 执行```git reset --mixed HEAD~```

![](https://github.com/Diabi/LearningNotes/blob/master/screenshot/7.jpg)

撤销commit，撤销add

* 执行```git reset --hard HEAD~```

![](https://github.com/Diabi/LearningNotes/blob/master/screenshot/8.jpg)

撤销commit，撤销add，强制覆盖工作目录，此处表现为直接删除b文件
