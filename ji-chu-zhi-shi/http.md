- NSURLRequest:一个NSURLRequest对象就代表一个请求，它包含的信息有：
    - 一个NSURL对象
    - 请求方法、请求头、请求体
    - 请求超时
    ![](/assets/URL请求.png)
    
    
```objc
//中文转码
NSString *string = @"https://ip.cn/index.php?username=小强";
string = [string stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];


- (void)get{
    //01创建url
    NSURL *url = [NSURL URLWithString:@"https://ip.cn/index.php?ip=1.1.1.1"];
    //02建立request
    NSURLRequest *request = [NSURLRequest requestWithURL:url];
    //03 创建会话对象
    NSURLSession *session = [NSURLSession sharedSession];
    
    //04根据会话对象来创建请求task（任务）
    NSURLSessionDataTask *dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
        NSLog(@"%@",[[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);
        NSLog(@"%@",[NSThread currentThread]);
    }];
    
    //05执行请求任务
    [dataTask resume];
}

- (void) sendSyn {
    //01创建url
    NSURL *url = [NSURL URLWithString:@"https://ip.cn/index.php?ip=1.1.1.1"];
    //02建立request
    NSURLRequest *request = [NSURLRequest requestWithURL:url];
    //03发送数据
    NSURLResponse *response = nil;
    NSError *error = nil;
    NSData *data = [NSURLConnection sendSynchronousRequest:request returningResponse:&response error:&error];
    //http response header
    NSLog(@"%@",response);
    //http response content(data)
    NSLog(@"%@",[[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);
}

- (void)sendPost {
    NSURL *url = [NSURL URLWithString:@"https://ip.cn/index.php"];
    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
    //设置请求方法
    request.HTTPMethod = @"POST";
    //设置请求体
    request.HTTPBody = [@"ip=1.1.1.1" dataUsingEncoding:NSUTF8StringEncoding];
    [NSURLConnection sendAsynchronousRequest:request queue:[[NSOperationQueue alloc] init] completionHandler:^(NSURLResponse * _Nullable response, NSData * _Nullable data, NSError * _Nullable connectionError) {
        NSLog(@"%@",[[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);
        NSLog(@"%@",[NSThread currentThread]);
    }];
}

- (void)sendAsync {
    //01创建url
    NSURL *url = [NSURL URLWithString:@"https://ip.cn/index.php?ip=1.1.1.1"];
    //02建立request
    NSURLRequest *request = [NSURLRequest requestWithURL:url];
    
    //03使用NSURLConnection发送异步请求
    
    [NSURLConnection sendAsynchronousRequest:request queue:[NSOperationQueue mainQueue] completionHandler:^(NSURLResponse * _Nullable response, NSData * _Nullable data, NSError * _Nullable connectionError) {
        NSLog(@"%@",response);
    }];
}
    - (void)sendRequestWithDelegate {
    //01创建url
    NSURL *url = [NSURL URLWithString:@"https://ip.cn/index.php?ip=1.1.1.1"];
    //02建立request
    NSURLRequest *request = [NSURLRequest requestWithURL:url];
    //03设置代理
    [[NSURLConnection alloc]initWithRequest:request delegate:self];
    
}

//接收到服务器的响应
- (void)connection:(NSURLConnection *)connection didReceiveResponse:(NSURLResponse *)response{
    self.fileData = [NSMutableData data];
    NSLog(@"didReceiveResponse");
}

//接收到服务器返回数据，该方法可能会调用多次
- (void)connection:(NSURLConnection *)connection didReceiveData:(NSData *)data {
    NSLog(@"didReceiveData");
    [self.fileData appendData:data];
}

//请求完成的时候调用
- (void)connectionDidFinishLoading:(NSURLConnection *)connection {
    NSLog(@"connectionDidFinishLoading");
    NSLog(@"%@",[[NSString alloc] initWithData:self.fileData encoding:NSUTF8StringEncoding]);
}

//失败时调用
- (void)connection:(NSURLConnection *)connection didFailWithError:(NSError *)error{
    NSLog(@"didFailWithError");
}
    
```
    
- NSURLSession
    使用NSURLSession创建Task，执行Task