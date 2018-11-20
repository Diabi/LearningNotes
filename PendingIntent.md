## PendingIntent

### 和Activity的区别

首先看官方解释：*An Intent is something that is used right now; a PendingIntent is something that may create an Intent in the future. You will use a PendingIntent with Notifications, AlarmManager, etc.*

Intent的作用是表明要启动哪个Activity，Service等，PendingIntent可以看作是对Intent的进一步封装，它是对Intent的描述，我们可以把这个描述交给别的程序，别的程序根据这个描述在后面的时间做你安排做的事情

### 用法相关

* 下面是一个发送SMS短信的例子：
```java
String msg ="你好";    
String number = "135****6784";    
SmsManager sms = SmsManager.getDefault();        
PendingIntent pi = PendingIntent.getBroadcast(SmsActivity.this, 0, new Intent(XXX), 0);    
sms.sendTextMessage(number, null, msg, pi, null);    
Toast.makeText(SmsActivity.this, "发送成功", Toast.LENGHT_LONG).show();
```

* 方法
```java
SmsManager.sendTextMessage(String destinationAddress, String scAddress, String text, PendingIntent sentIntent, PendingIntent deliveryIntent)
```
```PendingIntent sentIntent```：当短信发出时，成功的话sendIntent会把其内部的描述的intent广播出去，当然失败之后会产生错误代码，并通过 ```android.app.PendingIntent.OnFinished```进行回调（"Callback")；

```PendingIntent deliveryIntent```：是当消息已经传递给收信人后所进行的Intent广播；

如果你的BroadcastReveiver注册接收相应的消息，你就会收到相应的Intent，这时候就可以根据Intent的Action，执行相应的动作，这就是上面说的in the future的含义；

### 获取PendingIntent实例

有三个静态方法可以获得PendingIntent实例：
```java
public static PendingIntent getBroadcast(Context context, int requestCode, Intent intent, int flags)  
public static PendingIntent getActivity(Context context, int requestCode, Intent intent, int flags)  
public static PendingIntent getService(Context context, int requestCode, Intent intent, int flags) 
```

### 未完待续...
