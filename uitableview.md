## UITableView
* 继承：继承自`UIScrollView`
* 基本知识
    - 需要设置`dataSource`显示数据。
    - 任何对象都可以成为数据源(dataSource)
    - UITableView每一行显示的一定是一个Cell
    - `cell.accessoryType=UITableViewAccessoryDisclosureIndicator`:设置cell右边的>
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