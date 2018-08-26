## UINavigationController

- 创建导航栏控制器:`UINavigationController *nav = [[UINavigationController alloc] initWithRootViewController:viewController];`
- 跳转下一级(入栈):`[self.navigationController pushViewController:vc2 animated:YES];`
- 返回上一级（出栈）:`[self.navigationController popViewControllerAnimated:YES];`
- 导航条的内容由栈顶控制器的navigationItem属性决定。`    self.navigationItem.leftBarButtonItem = [[UIBarButtonItem alloc] initWithTitle:@"left side" style:0 target:self action:@selector(leftClick)];`
