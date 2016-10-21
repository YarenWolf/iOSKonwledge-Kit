###拨打完电话不回回到原来应用，且留在通讯录，直接拨打电话不给出提示。
```
NSMutableString * str=[[NSMutableString alloc] initWithFormat:@"tel:%@",@"186xxxx6979"];
[[UIApplication sharedApplication] openURL:[NSURL URLWithString:str]];

```


###打完电话还留在原来的程序中，也会给出提示
```
NSMutableString * str=[[NSMutableString alloc] initWithFormat:@"tel:%@",@"186xxxx6979"];
 UIWebView * callWebview = [[UIWebView alloc] init];
 [callWebview loadRequest:[NSURLRequest requestWithURL:[NSURL URLWithString:str]]];
 [self.view addSubview:callWebview];
```


###打完电话可以跳回原来的程序中，且给出提示
```
NSMutableString * str=[[NSMutableString alloc] initWithFormat:@"telprompt://%@",@"186xxxx6979"];
 [[UIApplication sharedApplication] openURL:[NSURL URLWithString:str]]
```