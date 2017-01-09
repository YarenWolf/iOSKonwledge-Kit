###单例
```
static SingleTest *singlTest = nil;
/**
 *@synchronized 的作用是创建一个互斥锁，保证此时没有其它线程对self对象进行修改。这个是objective-c的一个锁定令牌，防止self对象在同一时间内被其它线程访问，起到线程的保护作用。 一般在公用变量的时候使用，如单例模式或者操作类的static变量中使用。
 大概就是如果线程A访问一对象时，线程B必须等线程A访问完毕后，线程B才能够去操作。@synchronized(id anObject)是最简单的方法，会自动对参数对象加锁，保证临界区内的代码线程安全。
 */

/*
 * 方式1:传统的写法，这样没考虑到线程安全问题。
 */
+(SingleTest *)sharedInstanceMethod1{
 if (!singlTest) {
 singlTest = [[self alloc] init];
 }
 return singlTest;
}


+(SingleTest *)sharedInstanceMethod2{
 @synchronized (self) {
 if (!singlTest) {
 singlTest = [[self alloc] init];
 }
 }
 return singlTest;
}

+(SingleTest *)sharedInstanceMethod3{
 static dispatch_once_t token;
 dispatch_once(&token, ^
 if (!singlTest) {
 singlTest = [[self allocWithZone:NULL] init];
 }
 });
 return singlTest;
}

+(id)allocWithZone:(struct _NSZone *)zone{
 return [SingleTest sharedInstanceMethod3];
}

-(id) copyWithZone:(struct _NSZone *)zone {
 return [SingleTest sharedInstanceMethod3] ;
}

```