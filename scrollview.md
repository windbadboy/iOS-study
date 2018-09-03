## ScrollView
+ 属性
    - `contentSize`:决定了可滚动的范围
    - `scrollEnable`:是否可以滚动
    - `delegate`:当UIScrollView发生一系列的滚动操作时，会自动通知它的代理(delegate)对象，给它的代理发送相应的消息，让代理得知它的滚动情况。
+ 其它知识点
    - delegate都是弱引用
    - 任何OC对象均可成为代理
    - 凡是在导航控制器下的scrollView，会自动设置一个上面的内边距64,如不想设置，可设置属性`self.automaticallyAdjustsScrollViewInsets = NO;`
    - 隐藏导航控制器的导航条:`self.navigationController.navigationBar.hidden = YES;`
    - 设置导航条的透明度: