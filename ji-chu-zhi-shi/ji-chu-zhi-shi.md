##  基础知识
- 代理
    - 设置代理
    - 遵守协议
    - 实现代理方法
- 应用程序启动流程
    - 执行main
    - 执行UIApplicationMain
        - 创建UIApplication对象，并设置它的代理
        - 开启一个事件循环(主运行循环，死循环，保证应用程序不退出)
        - 加载info.plist，判断有没有指定Main
        {如果指定，加载Main.storyboard，加载剪会创建一个UIWindow，并把箭头指向的控制器的View添加到UIWindow，让UIWindow显示出来。
        - 每一个UIWindow都必须要有一个根控制器(rootViewController)
        }
        - 通知应用程序代理(appDelegate)，应用程序启动完毕调用didFinishLaunchingWithOptions。
- 高内聚:在同一个类中，把相同的代码写到同一个方法中。变化的东西当作参数传递。
    - 任何地点获取控制器:`    [[UIApplication sharedApplication].keyWindow.rootViewController presentViewController:alertController animated:YES completion:nil];`