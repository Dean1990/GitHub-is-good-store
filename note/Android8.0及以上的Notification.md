### Android8.0及以上的Notification

****

> **发布于[Deanlib](http://deanlib.com)  转载请注明出处 http://deanlib.com**

在新版本上（Android8.0及以上）开发时，会遇到一些问题，比如，不显示通知，`Notification` 声音不可控，删除 channel 删到崩溃。

**闲下来测试一番，总结 `Notification` 如下：**

首先，Android8.0 及以上版本使用 `Notification` 需要为其设置 `NotificationChannel` ，理解为频道，为 `Notification` 归类，统一管理，设置声音，震动等都需要通过 `NotificationChannel` 进行设置

```java 
NotificationChannel channel = new ...
channel.setSound(null,null);//静音
channel.enableVibration(true);//震动
channel.setVibrationPattern(new long[]{100,200,300});//震动模式
channel.enableLights(true);//呼吸灯
channel.setLightColor(Color.rgb(0,0,0));//呼吸灯颜色
```

`NotificationChannel` 构造函数参数解释

- id ：唯一标示
- name ：显示在应用详情中的名称，不唯一，可重复，即使在同一组中也可重复（下面会提到组的概念）。
- importance ：级别，重要性 NotificationManager 共提供了7个级别的常量
  - NotificationManager.IMPORTANCE_UNSPECIFIED
  - NotificationManager.IMPORTANCE_NONE
  - NotificationManager.IMPORTANCE_MIN
  - NotificationManager.IMPORTANCE_LOW
  - NotificationManager.IMPORTANCE_DEFAULT
  - NotificationManager.IMPORTANCE_HIGH
  - NotificationManager.IMPORTANCE_MAX

`NotificationChannel` 构造函数里 `id` 是唯一的，使用相同 `id` ，不同 `name` new出的新对象代表的是同一个 channel ，`name` 会被最后一个 `NotificationManager.createNotificationChannel(channel)` 中的 channel `name` 覆盖。

还有一个为 `NotificationChannel` 归类的组的概念，`NotificationChannelGroup` , 他的构造函数只有 `id` 和 `name` ，同样的，和上面 `NotificationChannel` 的含义一样，略。

将 `NotificationChannel` 添加到 `NotificationChannelGroup` 的方式，不是 group.add 或 group.set ，而是

```java
NotificationChannel channel = new ...
channel.setGroup("group1");//传参 NotificationChannelGroup的id
```

由于是上面这种方式关联二者，所以 `NotificationChannelGroup` 必须先创建

```java 
NotificationManager manager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
NotificationChannelGroup group = new NotificationChannelGroup("group1","This is Group 1");
manager.createNotificationChannelGroup(group);
```

再通过 `channel.setGroup("group1")` 做关联，否则会报异常

```
Caused by: java.lang.IllegalArgumentException: NotificationChannelGroup doesn't exist
```

并且 `channel.setGroup("group1")` 必须在 `NotificationManager.createNotificationChannel(channel)` 之前设置，否则无效，所以，他们的顺序是

```java
manager.createNotificationChannelGroup(group);
channel.setGroup("group1");
manager.createNotificationChannel(channel);
```

当 `NotificationChannelGroup` 在没有添加任何 `NotificationChannel` 时，在应用详情中不会显示该组

---

`NotificationChannelGroup` 好像记仇，channel 只要加进去的，就永远删不掉，尝试使用

```java
channel.setGroup(null);//置空
channel.setGroup("group2");//设置到其他组
```

都不管用，通过

```
manager.deleteNotificationChannel("channel1");//传参 channel的id
```

也只是能隐藏 channel，如果还用相同的 `id` 创建 channel ，新 channel 还在第一次加入的那个组中，只有卸载应用才能删除 channel ，在开发时，一定要注意这一点，在调试时，可以采用不同的 `id` 以避免出现诡异的现象。

我理解这是一个注册的机制，应用安装后，只要 `NotificationManager.createNotificationXXX` 代码运行一次就注册了，即使应用以后的版本注释了该段代码，他依然有效，想要"删除"，需要主动调用 `NotificationManager.deleteNotificationXXX` ，想要彻底删除只能卸载应用～

还要提一点， `NotificationCompat.Builder` 的构造函数中 `channelId` 必须是注册过的 `channelId`，否则在 `NotificationManager.notify` 时，不会出现通知，也不会报像 `channel doesn't exist` 的异常。

----

2019/1/18.

*Dean.King*

**Beijing**