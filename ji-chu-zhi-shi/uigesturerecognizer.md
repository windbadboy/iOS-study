## UIGestureRecognizer

为了完成手势识别，必须借助于手势识别器。--UIGestureRecognizer
UIGestureRecognizer是一个抽象类，定义了所有手势的基本行为，使用它的子类才能处理具体手势。

UITapGestureRecognizer(敲击)
UIPinchGestureRecognizer(捏合，用于缩放)
UIPanGestureRecognizer(拖拽)

UIImageView默认不支持用户交互。

```objc
    //创建轻扫手势
    UISwipeGestureRecognizer *swipe = [[UISwipeGestureRecognizer alloc]initWithTarget:self action:@selector(swipeGes:)];
    //设置轻扫方向
    swipe.direction = UISwipeGestureRecognizerDirectionLeft | UISwipeGestureRecognizerDirectionRight;
    
    [self.imageView addGestureRecognizer:swipe];
    
    //创建长按手势
    UILongPressGestureRecognizer *longGes = [[UILongPressGestureRecognizer alloc] initWithTarget:self action:@selector(longGes:)];
    
    //添加长按手势
    [self.imageView addGestureRecognizer:longGes];
    
    
    //创建点按手势
    UITapGestureRecognizer *tapGes = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(tap)];
    tapGes.delegate = self;
     //添加点按手势
    [self.imageView addGestureRecognizer:tapGes];
    

    //创建平移手势
    UIPanGestureRecognizer *pan = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(pan:)];
    //添加手势
    [self.imageView addGestureRecognizer:pan];

- (void)pan:(UIPanGestureRecognizer *) pan {
    //获取偏移量
    //获取的偏移量是相对于最原始的点
    CGPoint transP = [pan translationInView:self.imageView];
    self.imageView.transform = CGAffineTransformMakeTranslation(transP.x, transP.y);
    
    
}
```