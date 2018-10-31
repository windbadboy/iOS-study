
- json数据格式
    - {}:表示字典(dictionary)
    - []:表示数组(array)
```objc

- (void) getJson{
    [[[NSURLSession sharedSession] dataTaskWithURL:[NSURL URLWithString:@"http://www.badteacher.club/iostest.php?username=ted"] completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
        
        NSLog(@"%@",[[NSString alloc]initWithData:data encoding:NSUTF8StringEncoding]);
        [NSJSONSerialization JSONObjectWithData:data options:kNilOptions error:nil];
    }] resume];
}

- (void) other{
    //01 加载数据
    NSArray *array = [NSArray arrayWithContentsOfFile: [[NSBundle mainBundle] pathForResource:@"apps.plist" ofType:nil] ];
    //02 把数据以json方式保存
    NSData *jsonData = [NSJSONSerialization dataWithJSONObject:array options:NSJSONWritingPrettyPrinted error:nil];
    NSLog(@"%@",[[NSString alloc] initWithData:jsonData encoding:NSUTF8StringEncoding]);
    //03 写文件
    [jsonData writeToFile: @"/Users/ted/Desktop/test.json" atomically:YES];
}



- (void) other2{
    //01加载数据
    NSData *data = [NSData dataWithContentsOfFile:[[NSBundle mainBundle] pathForResource:@"test.json" ofType:nil] ];
    
    //02反序列化(json -> oc)
    NSArray *array = [NSJSONSerialization JSONObjectWithData:data options:kNilOptions error:nil];
    NSLog(@"%@",array[0]);
}
```