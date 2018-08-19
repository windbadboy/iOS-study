## NSNotificationCenter
- 每一个应用程序都有一个通知中心(NSNotificationCenter)实例，专门负责协助不同对象之间的消息通信。
- 任何一个对象都可以向通知中心发布通知(NSNotification)，描述自己在做什么。其他感兴趣的对象(Observer)可以申请在某个特定通知发布时(或在某个特写的对象发布通知时）收到这个通知。
- 通知应该在发布之前进行监听。
```objc
    //获取通知中心对象
    NSNotificationCenter *center = [NSNotificationCenter defaultCenter];
    //让控制器成为监听者
    /*
     第1个参数:让谁成为监听者
     第2个参数:将通知信息发送到指定的方法(方法名后加个“:”,会将通知对象传递给指定方法）
     第3个参数:接收通知消息的名称(nil为接收所有对象)
     第4个参数:接收通知消息的对象(nil为接收所有对象)
     
     */
    [center addObserver:self selector:@selector(plusClick:) name:@"plusClickNotification" object:nil];
    [center addObserver:self selector:@selector(minusClick:) name:@"minusClickNotification" object:nil];

    //发布通知
    /*
     第1个参数:发送通知消息的名称
     第2个参数:发送通知消息的对象(nil表示匿名发送)
     第3个参数:发送通知消息的附带信息(nil表示不发送附带消息)
     */
    [[NSNotificationCenter defaultCenter] postNotificationName:@"plusClickNotification" object:self];
    //移除监听
     [[NSNotificationCenter defaultCenter] removeObserver:self];
```