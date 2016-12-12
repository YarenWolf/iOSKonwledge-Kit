###NSTimer


```
if ([self.readTimer isValid]) {
        [self.readTimer invalidate];
        self.readTimer = nil;
    }
```
这样写不能将NSTimer释放掉，因为NStimer运行的时候是被加载到NSRunLoop中，如果该界面不释放的话，及时置为nil，它要是用到的话还是会去初始化一下。
