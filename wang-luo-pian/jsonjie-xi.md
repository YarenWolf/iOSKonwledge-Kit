###JSON解析###


1、在日常开发的时候经常需要解析网络请求到的json，因此这句代码很常见


```
NSDictionary *dic = [NSJSONSerialization JSONObjectWithData:responseObject options:NSJSONReadingMutableLeaves error:nil];
```


但是参数中的options是一个系统的枚举值，现在对这几个参数做一下总结。


