## UITextField
- 调用`- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string`,必须返回yes后，才会修改文本。