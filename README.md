## autoresizing

* 6条线的含义

  * 外面4条是固定和父控件之间的间距，上和左优先级较高；
  * 里面2根表示随父控件拉伸而变化其尺寸；

* autoresizing和autolayout是不兼容的

```objc
UIViewAutoresizingFlexibleLeftMargin    距离父控件的左边是可以拉伸的
UIViewAutoresizingFlexibleBottomMargin    距离父控件的底部是可以拉伸的
UIViewAutoresizingFlexibleRightMargin    距离父控件的右边是可以拉伸的
UIViewAutoresizingFlexibleTopMargin    距离父控件的顶部是可以拉伸的
UIViewAutoresizingFlexibleHeight    高度会跟随父控件的高度进行拉伸
UIViewAutoresizingFlexibleWidth    宽度会跟随父控件的宽度进行拉伸
```





