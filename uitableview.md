## UITableView

* 继承：继承自`UIScrollView`
* 基本知识
  * 需要设置`dataSource`显示数据。
  * 任何对象都可以成为数据源\(dataSource\)
  * UITableView每一行显示的一定是一个Cell
  * `cell.accessoryType=UITableViewAccessoryDisclosureIndicator`:设置cell右边的&gt;
  * 如果要重复利用cell，需要给cell加一个identifer（标识），需要新的cell时，会在cell缓存池寻找标识一样的cell.
  * `[tableView registerClass:[UITableViewCell class] forCellReuseIdentifier:ID]`:根据ID标识注册对应的cell类型为UITableViewCell
  * `dequeueReusableCellWithIdentifier:ID`:这个方法首先根据ID标识去缓存池取可循环利用的cell,如果没有，会判断有没有根据ID标识注册对应的cell类型，如果有注册，会自动创建这种类型的cell，并且绑定ID标识。
  * 从StoryBoard创建的cell不会调用initwith...方法，而是调用awakeFromNib方法。
* 常用属性
  * `rowHeight`:设置每一行cell的高度\(例:`self.tableView.rowHeight=70;`）
  * `sectionHeaderHeight`:设置每组标题高度
  * `sectionFooterHeight`:设置每组尾标题高度
  * `separatorColor`:分隔线颜色设置\(设置为透明色:`self.tableView.separatorColor = [UIColor clearColor];`）
  * `tableHeaderView`:设置表头控件
  * `tableFooterView`:设置表尾控件
  * `accessoryType`:设置cell右边的指示样式
  * `accessoryView`:设置cell右边指示样式为控件\(UIView\)，如果Type和View同时设置，Vie优先级较高。
  * `contentView`:cell在其上面显示。
  * `backgroundColor`:设置cell背景颜色
  * `backgroundView`:设置背景控件
  * `selectedBackgroundView`:设置选中的背景控件
* 使用流程
  * 设置数据源\(`dataSrouce`\)
  * 遵守协议\(`UITableViewDataSource`\)
  * 实现协议方法\(2个required方法\)
* 代理\(delegate\):`UITableViewDelegate`，监听tableview的各种事件。
* 常用方法
 - 数据源常用方法
```objc
//五个常用方法
//返回组数
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
//返回每组行数
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
//返回cell（行内容）
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
//返回头文字
- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section
//返回尾文字
- (NSString *)tableView:(UITableView *)tableView titleForFooterInSection:(NSInteger)section
```

 - 代理常用方法

  
 ```objc
 //cellForRowAtIndexPath可以获取选中的cell对象
 MyCell *cell = [tableView cellForRowAtIndexPath:indexPath];
 ```

* 性能优化
  * 首先去缓存池中找可循环利用的cell（标识要一样）
  * 如果缓存池没有可用cell，创建新cell
* 自定义cell
  * 从代码加载
    * 创建数据模型
    * 加载字典数据\(可选）:`NSArray *dictArray = [NSArray arrayWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"tgs.plist" ofType:nil]];`
  * 从xib加载
    * 创建xib布局文件，部署控件；
    * 创建继承自`UITableViewCell`的类，让xib的类继承自它。
    * 加载xib：
      ```objc
      [self.tableView registerNib:[UINib nibWithNibName:NSStringFromClass([BBCell class]) bundle:nil] forCellReuseIdentifier:CELLID];
      ```
    * cell存放所需数据数据模型
    * cell通过setter方法设置数据
* 不等高Cell：
  ```objc
    CGSize textMaxSize = CGSizeMake(textW, MAXFLOAT);
    NSDictionary *textAttr = @{NSFontAttributeName : [UIFont systemFontOfSize:14]};
    CGFloat textH = [cellModel.text boundingRectWithSize:textMaxSize options:NSStringDrawingUsesLineFragmentOrigin attributes:textAttr context:nil].size.height;
  ```
* 左滑删
  * 系统自带:

```objc
- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath {
    [self.wineGroups removeObjectAtIndex:indexPath.row];

    [self.tableView deleteRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationLeft];
}

//删除文字
- (NSString *)tableView:(UITableView *)tableView titleForDeleteConfirmationButtonForRowAtIndexPath:(NSIndexPath *)indexPath {
    return @"❎";
}
```

* 自定义删除内容和数量
  ```objc
  - (NSArray<UITableViewRowAction *> *)tableView:(UITableView *)tableView editActionsForRowAtIndexPath:(NSIndexPath *)indexPath {
   UITableViewRowAction *action = [UITableViewRowAction rowActionWithStyle:UITableViewRowActionStyleNormal title:@"移除" handler:^(UITableViewRowAction * _Nonnull action, NSIndexPath * _Nonnull indexPath) {
       [self.wineGroups removeObjectAtIndex:indexPath.row];
       [self.tableView deleteRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationLeft];
   }];
   //退出编辑模式
   self.tableView.editing = NO;
   return @[action,action];
  }
  ```

  * cell编辑模式
    * 进入/退出编辑模式:`[self.tableView setEditing:!self.tableView.isEditing animated:YES];`
    * 批量删除:
      ```objc
      //允许编辑时选择多个cell
      self.tableView.allowsMultipleSelectionDuringEditing = YES;
      NSMutableArray *willDeleteWine = [NSMutableArray array];
      //indexPathsForSelectedRows表示选中的cell.
      for (NSIndexPath *indexPath in self.tableView.indexPathsForSelectedRows) {
       [willDeleteWine addObject:self.wineGroups[indexPath.row]];
      }
      [self.wineGroups removeObjectsInArray:willDeleteWine];
      [self.tableView reloadData];
      ```



