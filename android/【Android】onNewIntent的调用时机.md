[参考博客](https://my.oschina.net/xsjayz/blog/138447)

launchMode为**singleTask**的时候，通过Intent启到一个Activity,如果系统已经存在一个实例，系统就会将请求发送到这个实例上，但这个时候，系统就不会再调用
通常情况下我们处理请求数据的onCreate方法，而是调用onNewIntent方法。

不要忘记，系统可能会随时杀掉后台运行的 Activity ，如果这一切发生，那么系统就会调用 onCreate 方法，而不调用 onNewIntent 方法，一个好的解决方法就是在
onCreate 和 onNewIntent 方法中调用同一个处理数据的方法。

![](https://developer.android.com/guide/components/images/activity_lifecycle.png)

![](http://dl2.iteye.com/upload/attachment/0126/5549/2af0de16-8a4d-3e78-b705-32d50f049fb0.png)

**特别注意：**

当调用到onNewIntent(intent)的时候，需要在onNewIntent() 中使用setIntent(intent)赋值给Activity的Intent。否则，后续的getIntent()都是得到老的Intent。

