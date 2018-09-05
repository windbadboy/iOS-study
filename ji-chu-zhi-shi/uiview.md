## UIView

UIView有一个transform属性，能够改变控件的形状。
```objc
    [UIView animateWithDuration:0.5 animations:^{
        //平移
        //self.imageView.transform = CGAffineTransformTranslate(self.imageView.transform, 0, 100);
        //旋转
        //self.imageView.transform = CGAffineTransformRotate(self.imageView.transform, M_PI_2);
        //缩放（1代表原始大小，）
        self.imageView.transform = CGAffineTransformScale(self.imageView.transform, 1.5, 1.5);
        
    }];
```