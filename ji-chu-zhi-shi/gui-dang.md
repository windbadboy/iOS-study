## 归档
- 存储归档文件:
```objc
    Person *per = [[Person alloc] init];
    per.name = @"ls";
    per.age = 18;
    per.dog = [[Dog alloc] init];
    per.dog.name = @"coucou";
    //获取文件路径
    NSString *path = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES)[0];
    //拼接文件名
    NSString *filePath = [path stringByAppendingPathComponent:@"Per.data"];
    NSLog(@"%@",filePath);
    
    //归档数据
    //当执行NSKeyedArchiver,会自动调用保存对象的encodeWithCoder方法
    //调用encodeWithCoder的目的是告诉它保存该对象的哪些属性
    [NSKeyedArchiver archiveRootObject:per toFile:filePath];
```

- 读取归档文件:
```objc
    //解档数据
    //获取文件路径
    NSString *path = NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES)[0];
    //拼接文件名
    NSString *filePath = [path stringByAppendingPathComponent:@"Per.data"];
    
    //当执行NSKeyedUnarchiver,会自动调用保存对象的initWithCoder方法。
    //调用initWithCoder方法的目的是告诉他读取该对象的哪些属性。
    Person *per = [NSKeyedUnarchiver unarchiveObjectWithFile:filePath];
    NSLog(@"name is %@,age is %ld,dog's name is %@",per.name,per.age,per.dog.name);
```
- 需要实现的方法

```objc
- (void)encodeWithCoder:(NSCoder *)aCoder {
    [aCoder encodeObject:self.name forKey:@"name"];
    [aCoder encodeInteger:self.age forKey:@"age"];
    [aCoder encodeObject:self.dog forKey:@"dog"];
}

- (instancetype)initWithCoder:(NSCoder *)aDecoder {
    if (self = [super init]) {
        self.name = [aDecoder decodeObjectForKey:@"name"];
        self.age = [aDecoder decodeIntegerForKey:@"age"];
        self.dog = [aDecoder decodeObjectForKey:@"dog"];
    }
    return self;
}
```