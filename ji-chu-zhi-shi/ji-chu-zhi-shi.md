## 基础知识

* 代理
  * 设置代理
  * 遵守协议
  * 实现代理方法
  * 使用代理的情况
    * 上下级之间，下级要给上级传递数据时，可使用代理。
  * 写代理的过程
    * 定义协议:规定成为其代理的对象需要遵守的协议\(方法\)
    * 定义代理属性:设置代理属性`@property(nonatomic,weak) id<XZAddViewControllerDelegate> delegate;`
    * 调用代理方法:确定何时调用代理遵守的协议方法`[self.delegate AddAddVC:self contactModel:contactModel];`
    * 设置代理:`addVC.delegate = self;`
    * 遵守协议:`@interface XZContactTableViewController () <XZAddViewControllerDelegate>`
    * 实现代理\(方法\)
    * 应用程序启动流程
* 启动流程
  * 执行main
  * 执行UIApplicationMain
    * 创建UIApplication对象，并设置它的代理
    * 开启一个事件循环\(主运行循环，死循环，保证应用程序不退出\)
    * 加载info.plist，判断有没有指定Main
      {如果指定，加载Main.storyboard，加载剪会创建一个UIWindow，并把箭头指向的控制器的View添加到UIWindow，让UIWindow显示出来。
    * 每一个UIWindow都必须要有一个根控制器\(rootViewController\)
      }
    * 通知应用程序代理\(appDelegate\)，应用程序启动完毕调用didFinishLaunchingWithOptions。
* 高内聚:在同一个类中，把相同的代码写到同一个方法中。变化的东西当作参数传递。
  * 任何地点获取控制器:`[[UIApplication sharedApplication].keyWindow.rootViewController presentViewController:alertController animated:YES completion:nil];`
- 低耦合:类与类之间降低关联
* 数据顺传
  * 在接收数据的控制器定义属性接收数据；`@property (nonatomic,copy) NSString *accountName;`
  * 获取数据要接收的控制器
  * 把数据传给接收控制器对应的属性
    ```objc
    XZContactTableViewController *contactTC = segue.destinationViewController;
    contactTC.accountName = self.accountTextF.text;
    ```
- 控制器会懒加载（使用才会加载，比如设置控制器view的属性）
- 应用程序沙盒
  - layer:所有程序的资源和可以执行文件
    - Documents:保存持久化数据，itunes同步时会备份该目录。如游戏存档
    - tmp:存放监时数据
    - Library/Caches:保存持久化数据，一般存储体积较大
    - Library/Preference:存储偏好设置，itunes同步时会备份该目录



