## UIApplication

- 每一个应用程序都有自己的UIApplication对象，而且是单例的。
- 通过[UIApplication sharedApplication]可以获得这个单例对象
- 一个iOS程序启动后创建的第一个对象就是UIApplication对象
- 常用属性
    - `@property(nonatomic) NSInteger applicationIconBadgeNumber;`:设置应用程序图标右上角的红色提醒数字
- 常用方法
    - `- (BOOL)prefersStatusBarHidden;`:状态栏可见性