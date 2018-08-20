## UIWindow
- UIWindow是一种特殊的UIView,通常在一个app中至少有一个UIWindow
- iOS程序启动完毕后，创建的第一个视图控件就是UIWindow，接着创建控制器的view，最后将控制器的view添加到UIWindow上，于是控制器的view就显示在屏幕上了。
- 一个iOS程序之所以能显示到屏幕上，完全是因为它有UIWindow
- 人工创建UIWindow
    - 创建窗口:`self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];`
    - 设置窗口的根控制器:
```objc
    UIViewController *vc = [[UIViewController alloc] init];
    vc.view.backgroundColor = [UIColor redColor];
    self.window.rootViewController = vc;
```
    - 显示窗口:`[self.window makeKeyAndVisible];`