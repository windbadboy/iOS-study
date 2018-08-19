## KVO

```objc
        //为每一个酒模型添加KVO监听
        for (WineModel *wineModel in self.wineGroups) {
            [wineModel addObserver:self forKeyPath:@"numCount" options:NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld context:nil];
        }

    //当KVO监听的属性值发生改变时调用此方法
    - (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context {
    NSLog(@"something changed.%@",change);
    NSLog(@"new value is %d",[change[NSKeyValueChangeNewKey] intValue]);
}        
                    //移除KVO监听
    for (WineModel *wineModel in self.wineGroups) {
        [wineModel removeObserver:self forKeyPath:@"numCount"];
    }        
```