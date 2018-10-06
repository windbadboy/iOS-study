##线程
- 什么是线程
    - 一个进程要想执行任务，必须得有线程（每1个进程至少要有1条线程）
    - 一个进程（程序）的所有任务都在线程中执行
- 线程的串行
    - 一个线程中任务的执行是串行的
    - 如果要在1个线程中执行多个任务，那么只能一个一个地按顺序执行这些任务
    - 也就是说，在同一时间内，1个线程只能执行1个任务。
- 进程和线程的比较
    - 线程是cpu调用(执行任务)的最小单位
    - 进程是cpu分配资源和调度的单位
    - 一个程序可以对应多个进程，一个进程中可以有多个线程，但至少要有一个线程。
    - 同一个进程内的线程共享进程的资源
- 多线程
    - 1个进程中可以开启多条线程，每条线程可以并行（同时）执行不同的任务
    - 同一时间，CPU只能处理1条线程，只有1条线程在工作（执行）
    - 多线程并发（同时）执行，其实是CPU快速地在多条线程之间调度（切换）
- 主线程
    - 一个iOS程序运行后，默认会开启1条线程，称为“主线程”或“UI线程”
    - 主线程作用
        - 显示\刷新UI界面
        - 处理UI事件（比如点击事件、滚动事件、拖拽事件等）
- 子线程:除了主线程外的其它进程称为子线程。（后台线程、非主线程）

```swift
        //print main thread
        let mainThread = Thread.main
        print(mainThread)
        //print current using thread
        print(Thread.current)
        //judge whether is in main thread,return true or false
        print(Thread.isMainThread)
```

- 线程的状态
    - 新建:new一个对象
    - runnable:处于可调度线程池
    - running:运行
    - block:阻塞，不会做任何事情,可切换至runnable状态
    - dead

- 原子性、非原子性
    - atomic:原子性，安全的(内部会对setter方法加锁）
    - noatomic:非原子性，不安全的

```objc
    NSURL *url = [NSURL URLWithString:@"http://img3.duitang.com/uploads/item/201509/30/20150930210729_CYP3F.jpeg"];
    
    //02把图片的二进制数据下载到本地
    NSDate *start = [NSDate date];
    NSData *imageData = [NSData dataWithContentsOfURL:url];
    NSDate *end = [NSDate date];
    NSLog(@"%f",[end timeIntervalSinceDate:start]);
    //03把图片的二进制数据转换为UIimage
    UIImage *image = [UIImage imageWithData:imageData];
    
    //04返回主线程执行
   // [self performSelectorOnMainThread:@selector(showPics:) withObject:image waitUntilDone:YES];
   // [self performSelector:@selector(showPics:) onThread:[NSThread mainThread] withObject:image waitUntilDone:YES];
    [self.imageView performSelector:@selector(setImage:) onThread:[NSThread mainThread] withObject:image waitUntilDone:YES];

}


- (void) showPics:(UIImage *)image {
    NSLog(@"%@",[NSThread currentThread]);
     self.imageView.image = image;
}

```    
- GCD
    - 核心概念
        - 任务:执行什么操作
        - 队列:用来存放任务
    - GCD使用步骤
        - 创建队列
        - 封装任务把任务添加到队列
            - 使用函数来封装任务（同步|异步）
                - 同步：不具备开启新线程的能力，以同步（执行完当前任务后才会往后执行）方式执行
                - 异步：具备开启新线程的能力，以异步方式执行
                    - 并发队列：多个任务并发（同时）执行
                        自己创建队列:`dispatch_queue_create concurrent`
                        全局并发队列:`dispatch_get_global_queue`
                    - 串行队列：让任务一个接着一个地执行。（一个任务执行完毕后，再执行下一个任务）
                        自己创建的队列:`dispatch_queue_create_serial`
                        主队列:`dispatch_get_main_queue`
                    
```objc
//异步函数 + 并发执行（会开多条新线程,并发执行）
- (void)asyncConcurrent {
    
    /**
     创建队列

     param label 给队列起个名字
     param attr#> 类型
                    DISPATCH_QUEUE_CONCURRENT   并发队列
                    DISPATCH_QUEUE_SERIAL   串行队列
     return 队列
     */
    dispatch_queue_t queue = dispatch_queue_create("club.badteacher.www.DownloadQueue", DISPATCH_QUEUE_CONCURRENT);
    
    
    /**
     封装任务，把任务添加到队列

     
     param queue#> 队列
     param void 代码块
     return 无
     */
    dispatch_async(queue, ^{
        NSLog(@"1----%@",[NSThread currentThread]);
    });
    dispatch_async(queue, ^{
        NSLog(@"2----%@",[NSThread currentThread]);
    });
    dispatch_async(queue, ^{
        NSLog(@"3----%@",[NSThread currentThread]);
    });
    dispatch_async(queue, ^{
        NSLog(@"4----%@",[NSThread currentThread]);
    });
}
```

- 队列组
```objc
    //create a group
    dispatch_group_t group = dispatch_group_create();
    
    //create a queue
    dispatch_queue_t queue = dispatch_queue_create("downloadPic", DISPATCH_QUEUE_CONCURRENT);
    
    //execute download task
    dispatch_group_async(group, queue, ^{
        NSURL *url = [NSURL URLWithString:@"http://img3.duitang.com/uploads/item/201509/30/20150930210729_CYP3F.jpeg"];
        NSData *data = [NSData dataWithContentsOfURL:url];
        self.image1 = [UIImage imageWithData:data];
        NSLog(@"---%@---",[NSThread currentThread]);


    });
    dispatch_group_async(group, queue, ^{
        NSURL *url = [NSURL URLWithString:@"http://img3.duitang.com/uploads/item/201509/30/20150930210729_CYP3F.jpeg"];
        NSData *data = [NSData dataWithContentsOfURL:url];
        self.image2 = [UIImage imageWithData:data];
    });
    
    dispatch_group_notify(group, queue, ^{
        // start context
        UIGraphicsBeginImageContext(CGSizeMake(375, 667));
        
        //drawing
        [self.image1 drawInRect:CGRectMake(0, 0, 300, 150)];
         [self.image2 drawInRect:CGRectMake(150, 0, 300, 150)];
        
        //take the pic
        UIImage *image = UIGraphicsGetImageFromCurrentImageContext();
        
        //close context
        UIGraphicsEndImageContext();
        dispatch_async(dispatch_get_main_queue(), ^{
            self.imageView.image = image;
            NSLog(@"---%@---",[NSThread currentThread]);
        });
    });
```

- 单例模式

```objc
//01 提供一个全局的静态变量（对外界隐藏）
static XZTool *_instance;

//02 重写alloc方法，保证永远只分配一次存储空间
+ (instancetype)allocWithZone:(struct _NSZone *)zone {
    //此方法在多线程时可能引发安全问题
//    @synchronized (self) {
        //    if (_instance == nil) {
        //        _instance = [super allocWithZone:zone];
        //    }
//    }

    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        _instance = [super allocWithZone:zone];
    });
    return _instance;
}
//03 提供类方法
+ (instancetype)shareTool {
    return [[super alloc] init];
}

//04 提供copy方法
//04 重写copy方法

- (id)copy {
    return _instance;
}

- (id)mutableCopy {
    return _instance;
}
```

- 操作队列
    - 自定义队列:`[[NSOperationQueue alloc] init]`
    - 主队列:串行队列，和主线程相关(主阶列中的任务在主线程中扫行) 