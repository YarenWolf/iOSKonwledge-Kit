###respondsToSelector&instancesRespondToSelector


```
BOOL flag1 = [Test1 instancesRespondToSelector:@selector(objectFun)];   //YES
        BOOL flag2 = [Test1 instancesRespondToSelector:@selector(classFun)];    //NO
        BOOL flag3 = [Test1 respondsToSelector:@selector(objectFun)];           //NO
        BOOL flag4 = [Test1 respondsToSelector:@selector(classFun)];            //YES
        Test1 *test = [[Test1 alloc] init];
        BOOL flag5 = [test respondsToSelector:@selector(objectFun)];            //YES
        BOOL flag6 = [test respondsToSelector:@selector(classFun)];             //NO
        BOOL flag7 = [test instancesRespondToSelector:@selector(objectFun)];    //非法
        BOOL flag8 = [test instancesRespondToSelector:@selector(classFun)];     //非法
        /**
         *  总结：
         *  instancesRespondToSelector只能写在类后面，respondsToSelector既可以写在类后面，也可以写在对象后面
         *  [类 instancesRespondToSelector:@selector(methodName)]用于检测是否实现了实例方法；效果等价于[类的实例  respondsToSelector:@selector(methodName)]
         *  [类 respondsToSelector:@selector(methodName)] 用于检测是否实现了类方法
         */
        
        /**
         * 判断对象类型
         * -(BOOL)isKindOfClass:classObj:是否是这个类或者是这个类子类的对象
         * -(BOOL)isMemberOfClass:classObj :是否是这个类的实例
         */

```


```
#define iPhone5 ([UIScreen instancesRespondToSelector:@selector(currentMode)] ? CGSizeEqualToSize(CGSizeMake(640, 1136), [[UIScreen mainScreen] currentMode].size) : NO)
#define iPhone6 ([UIScreen instancesRespondToSelector:@selector(currentMode)] ? CGSizeEqualToSize(CGSizeMake(750, 1334), [[UIScreen mainScreen] currentMode].size) : NO)
#define iPhone6pOr6sp ([UIScreen instancesRespondToSelector:@selector(currentMode)] ? CGSizeEqualToSize(CGSizeMake(1242, 2208), [[UIScreen mainScreen] currentMode].size) : NO)
```
