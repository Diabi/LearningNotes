参考自[git-scm](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E9%87%8D%E7%BD%AE%E6%8F%AD%E5%AF%86)

## git reset命令的工作流程简析

 初始状态图

![](https://git-scm.com/book/en/v2/images/reset-start.png)

### 第一步 -> 移动HEAD

reset 做的第一件事是移动 HEAD 的指向。 这与改变 HEAD 自身不同（checkout 所做的）；reset 移动 HEAD 指向的分支。 这意味着如果 HEAD 设置为 master 
分支（例如，你正在 master 分支上），运行```git reset 9e5e64a```将会使master指向9e5e64a。

![](https://git-scm.com/book/en/v2/images/reset-soft.png)

无论你调用了何种形式的带有一个提交的 reset，它首先都会尝试这样做。 使用 reset --soft，它将仅仅停在那儿。

现在看一眼上图，理解一下发生的事情：它本质上是撤销了上一次 ```git commit``` 命令。 当你在运行 ```git commit``` 时，Git 会创建一个新的提交，并移动 
HEAD 所指向的分支来使其指向该提交。 当你将它 reset 回 HEAD~（HEAD的父结点）时，其实就是把该分支移动回原来的位置，而不会改变索引和工作目录。 现在你
可以更新索引并再次运行 git commit 来完成 ```git commit --amend``` 所要做的事情了

### 第二步 -> 更新索引

![](https://git-scm.com/book/en/v2/images/reset-mixed.png)

如果指定--mixed 选项，reset将会在这时停止。这也是默认行为，所以如果没有指定任何选项（在本例中只是```git reset HEAD~```），这就是命令将会停止的地方。
现在再看一眼上图，理解一下发生的事情：它依然会撤销一上次提交，但还会取消暂存所有的东西。于是，我们```回滚到了所有git add和git commit的命令执行之前```。

<span style="color:red;">这是比font标签更好的方式。可以试试。</span>
