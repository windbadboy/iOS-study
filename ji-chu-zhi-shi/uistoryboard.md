## UIStoryBoard

-加载Main.storyboard流程
```objc
//创建窗口
self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
//设置窗口的根控制器
//从storyboard当中加载控制器
UIStoryboard *sb = [UIStoryboard storyboardWithName:@"Main" bundle:nil];
UIViewController *vc = [sb instantiateInitialViewController];
self.window.rootViewController = vc;
```