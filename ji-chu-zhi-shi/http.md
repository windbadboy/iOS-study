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
    
```objc

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



//使用urlsession代理
- (void)delegate {
    //01确定url
    NSURL *url = [NSURL URLWithString:@"http://www.badteacher.club/indexsa.php?a=admin"];
    
    //02创建request对象
    NSURLRequest *request = [NSURLRequest requestWithURL:url];
    
    //03创建session,delegateQueuel如果传nil，默认在子线程中执行。
    NSURLSession *session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration] delegate:self delegateQueue:[NSOperationQueue mainQueue]];
    
    //04根据session创建task
    NSURLSessionDataTask *dataTask = [session dataTaskWithRequest:request];
    
    //05执行task
    [dataTask resume];
}

- (void)URLSession:(NSURLSession *)session dataTask:(NSURLSessionDataTask *)dataTask didReceiveResponse:(NSURLResponse *)response completionHandler:(void (^)(NSURLSessionResponseDisposition))completionHandler{
    NSLog(@"didReceiveResponse");
    //处理回调
    completionHandler(NSURLSessionResponseAllow);
    
}
- (void)URLSession:(NSURLSession *)session dataTask:(NSURLSessionDataTask *)dataTask didReceiveData:(NSData *)data{
    
        NSLog(@"didReceiveData");
    [self.resultData appendData:data];
    
}
- (void)URLSession:(NSURLSession *)session task:(NSURLSessionTask *)task didCompleteWithError:(NSError *)error{
    
        NSLog(@"didCompleteWithError");
    NSLog(@"%@",[[NSString alloc] initWithData:self.resultData encoding:NSUTF8StringEncoding]);
    
}
```

- 下载内容直接写入沙盒文件
```objc
    //拼接文件的存储路径（沙盒）+文件名
    NSString *cache = [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) lastObject];
    NSString *fileName = [response suggestedFilename];
    NSString *fullPath = [cache stringByAppendingPathComponent:fileName];
    
    //创建空文件
    [[NSFileManager defaultManager] createFileAtPath:fullPath contents:nil attributes:nil];
    
    //创建文件句柄指针指向该文件
    self.handle = [NSFileHandle fileHandleForWritingAtPath:fullPath];
    [self.handle writeData:data];
    [self.handle closeFile];
```