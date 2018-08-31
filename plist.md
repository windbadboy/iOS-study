## plist
+ 一层一层的往下解析，最终目的是将所有的字典转成模型。
+ 如果plist中字典(dictionary)在一个数组(array)中，将转出来的模型也放在一个数组中，即将字典数组转成模型数组。
+ plist不支持存储自定义对象

+ info.plist
    - Bundle identifier:应用唯一标识
    - Bundle name:软件名称(显示在app图标下)
    - Bundle versions string, short:应用程序版本号

- 存储plist
```objc
    NSArray *array = @[@"xz",@10];
    NSString *path = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES)[0];
    
    //拼接文件名称
    //NSString *filePath = [path stringByAppendingString:@"/array.plist"];
    NSString *filePath = [path stringByAppendingPathComponent:@"array.plist"];
    [array writeToFile:filePath atomically:YES];
    
        //存储字典
    NSDictionary *dictionary = @{
                                 @"age":@"11"
                                 };
    
    NSString *filePath = [path stringByAppendingPathComponent:@"dict.plist"];
    [dictionary writeToFile:filePath atomically:YES];
```
- 读取plist
```objc
    NSString *path = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES)[0];
    NSString *filePath = [path stringByAppendingPathComponent:@"array.plist"];
    NSArray *array = [NSArray arrayWithContentsOfFile:filePath];
    NSLog(@"%@",array);
    //读取字典
        NSString *filePath = [path stringByAppendingPathComponent:@"dict.plist"];
    NSDictionary *dict = [NSDictionary dictionaryWithContentsOfFile:filePath];
    NSLog(@"%@",dict);
```

- 存储配置
```objc
    //NSUserDefaults是一个字典类型的plist文件
    //使用NSUserDefaults不用关心路径，会自动帮你存到Library/Preferences
    NSUserDefaults *userDefault = [NSUserDefaults standardUserDefaults];
    [userDefault setObject:@"Ted" forKey:@"name"];
    [userDefault setInteger:36 forKey:@"age"];
    [userDefault setBool:YES forKey:@"wedding"];
```
- 读取配置
```objc
    NSUserDefaults *userDefault = [NSUserDefaults standardUserDefaults];
    NSString *name = [userDefault objectForKey:@"name"];
    NSInteger age = [userDefault integerForKey:@"age"];
    BOOL wedding = [userDefault boolForKey:@"wedding"];
    NSLog(@"name is %@,age is %ld,wedding:%d",name,(long)age,wedding);
```