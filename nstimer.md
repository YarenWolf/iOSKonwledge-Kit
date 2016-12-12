###NSTimer


```
if ([self.readTimer isValid]) {
        [self.readTimer invalidate];
        self.readTimer = nil;
    }
```
这样写不能将NSTimer释放掉，因为NStimer运行的时候是被加载到NSRunLoop中，如果该界面不释放的话，及时置为nil，它要是用到的话还是会去初始化一下。




注意：

```
NSTimer *timer = [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(secondsPlus) userInfo:nil repeats:YES];
```
这个方法写的NSTimer不需要写fire方法，但需要在用的地方初始化一下就自动调用selector方法。如果懒加载写了的话在用的地方需要“使用”一下，比如fire或者“打印一下self.timer.isValid”

