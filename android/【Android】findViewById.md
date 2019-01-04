### 问题

findViewById是如何在一个复杂的布局中找到特定的View的

### 源码跟踪

```java
[Activity.java]

@Nullable
public <T extends View> T findViewById(@IdRes int id) {
    return getWindow().findViewById(id);
}

[Window.java]

@Nullable
public <T extends View> T findViewById(@IdRes int id) {
    return getDecorView().findViewById(id);
}

[View.java]

@Nullable
public final <T extends View> T findViewById(@IdRes int id) {
    if (id == NO_ID) {
        return null;
    }
    return findViewTraversal(id);
}

protected <T extends View> T findViewTraversal(@IdRes int id) {
    if (id == mID) {
        return (T) this;
    }
    return null;
}
```

咋一看有点蹊跷，一层一层剖开源码却没有跟踪到一个递归或者迭代的过程，最终在View.java的findViewTraversal方法中找到了
最终返回的View对象，而条件是id == mID。仔细思考一下，这里忽略了一个关键的点，ViewGroup继承自View，并且布局继承自
ViewGroup，可以推测ViewGroup中重写了View中的某个find相关方法，该方法中有一个迭代或递归过程。

```java
[ViewGroup.java]

@Override
protected <T extends View> T findViewTraversal(@IdRes int id) {
    if (id == mID) {
        return (T) this;
    }

    final View[] where = mChildren;
    final int len = mChildrenCount;

    for (int i = 0; i < len; i++) {
        View v = where[i];

        if ((v.mPrivateFlags & PFLAG_IS_ROOT_NAMESPACE) == 0) {
            v = v.findViewById(id);

            if (v != null) {
                return (T) v;
            }
         }
    }

    return null;
}
```

可以看到，ViewGroup重写了View的findViewTraversal方法，该方法的实现中包含了对布局所有子控件的遍历，这里面的递归过程是这样的：

findViewById -> findViewTraversal -> findViewById -> findViewTraversal -> ...

边界条件为：找到目标id控件，即id == mID。
