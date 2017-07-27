# iOS 只对URL中的中文进行转码



```
// 对url中的中文进行转码（如果已知url中的中文没有进行utf-8转码）
url = [url stringByAddingPercentEncodingWithAllowedCharacters:[NSCharacterSet URLQueryAllowedCharacterSet]];
```

如果知道url中的中文既可能已经转码，也可能没有转码，那么使用如下的方法，当不管url中的中文是否已经utf-8转码了，都可以解决将中文字符转为utf-8的问题，且不是二次转码

```
  NSLog(@"原url:%@", url);
    NSString *encodedString = (NSString *)
    CFBridgingRelease(CFURLCreateStringByAddingPercentEscapes(kCFAllocatorDefault,

                                                              (CFStringRef)url,

                                                              (CFStringRef)@"!$&'()*+,-./:;=?@_~%#[]",

                                                              NULL,

                                                              kCFStringEncodingUTF8));

    NSLog(@"转码url:%@",  encodedString);
```



