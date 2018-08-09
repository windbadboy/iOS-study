## UITableView
* 继承：继承自`UIScrollView`
* 基本知识
    - 需要设置`dataSource`显示数据。
    - 任何对象都可以成为数据源(dataSource)
    - UITableView每一行显示的一定是一个Cell
    - `cell.accessoryType=UITableViewAccessoryDisclosureIndicator`:设置cell右边的>
    - 如果要重复利用cell，需要给cell加一个identifer（标识），需要新的cell时，会在cell缓存池寻找标识一样的cell.
    - `[tableView registerClass:[UITableViewCell class] forCellReuseIdentifier:ID]`:根据ID标识注册对应的cell类型为UITableViewCell
    - `dequeueReusableCellWithIdentifier:ID`:这个方法首先根据ID标识去缓存池取可循环利用的cell,如果没有，会判断有没有根据ID标识注册对应的cell类型，如果有注册，会自动创建这种类型的cell，并且绑定ID标识。
* 常用属性
    - `rowHeight`:设置每一行cell的高度(例:`self.tableView.rowHeight=70;`）
    - `sectionHeaderHeight`:设置每组标题高度
    - `sectionFooterHeight`:设置每组尾标题高度
    - `separatorColor`:分隔线颜色设置(设置为透明色:`self.tableView.separatorColor = [UIColor clearColor];`）
    - `tableHeaderView`:设置表头控件
    - `tableFooterView`:设置表尾控件
    - `accessoryType`:设置cell右边的指示样式
    - `accessoryView`:设置cell右边指示样式为控件(UIView)，如果Type和View同时设置，Vie优先级较高。
    - `contentView`:cell在其上面显示。
    - `backgroundColor`:设置cell背景颜色
    - `backgroundView`:设置背景控件
    - `selectedBackgroundView`:设置选中的背景控件
* 使用流程
    + 设置数据源(`dataSrouce`)
    + 遵守协议(`UITableViewDataSource`)
    + 实现协议方法(2个required方法)

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
* 性能优化
    - 首先去缓存池中找可循环利用的cell（标识要一样）
    - 如果缓存池没有可用cell，创建新cell
    
