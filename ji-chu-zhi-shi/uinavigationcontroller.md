## UINavigationController

- 创建导航栏控制器:`UINavigationController *nav = [[UINavigationController alloc] initWithRootViewController:viewController];`
- 跳转下一级(入栈):`[self.navigationController pushViewController:vc2 animated:YES];`
- 返回上一级（出栈）:`[self.navigationController popViewControllerAnimated:YES];`
- 导航条的内容由栈顶控制器的navigationItem属性决定。`    self.navigationItem.leftBarButtonItem = [[UIBarButtonItem alloc] initWithTitle:@"left side" style:0 target:self action:@selector(leftClick)];`
- 导航器跳转分为2种
    - 手动型跳转:需要根据标识手工执行代码(通过控制器拖线的方式进行跳转)
    - 自动型跳转:一般用于跳转前不做任何事情(通过控件拖线的方式进行跳转)