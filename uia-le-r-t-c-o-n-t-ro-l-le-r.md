

```
UIAlertController *alert = [UIAlertController alertControllerWithTitle:@"验证原密码" message:@"为保障您的数据安全，修改密码前请填写原密码" preferredStyle:UIAlertControllerStyleAlert];
            [alert addTextFieldWithConfigurationHandler:^(UITextField * _Nonnull textField) {
                 textField.secureTextEntry = YES;
                NSLog(@"结果：%@",textField.text);
            }];
            
            UIAlertAction *cancel = [UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleDefault handler:nil];
            UIAlertAction *ok = [UIAlertAction actionWithTitle:@"确定" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
                UserInfo *userInfo = [UserManager getUserObject];
               UITextField *passwordTextField = alert.textFields.firstObject;
                if (![passwordTextField.text isEqualToString:userInfo.password]) {
                    [SVProgressHUD showErrorWithStatus:@"密码错误，请重新输入。"];
                }else{
                    NSLog(@"");
                }
            }];
            [alert addAction:cancel];
            [alert addAction:ok];
            [self presentViewController:alert animated:YES completion:nil];

```

