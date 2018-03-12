#### 1、深拷贝与浅拷贝

深拷贝同浅拷贝的区别：浅拷贝是指针拷贝，对一个对象进行浅拷贝，相当于对指向对象的指针进行复制，产生一个新的指向这个对象的指针，那么就是有两个指针指向同一个对象，这个对象销毁后两个指针都应该置空。

深拷贝是对一个对象进行拷贝，相当于对对象进行复制，产生一个新的对象，那么就有两个指针分别指向两个对象。当一个对象改变或者被销毁后拷贝出来的新的对象不受影响。

实现深拷贝需要实现NSCoying协议，实现- \(id\)copyWithZone:\(NSZone \*\)zone 方法。当对一个property属性含有copy修饰符的时候，在进行赋值操作的时候实际上就是调用这个方法。

父类实现深拷贝之后，子类只要重写copyWithZone方法，在方法内部调用父类的copyWithZone方法，之后实现自己的属性的处理

父类没有实现深拷贝，子类除了需要对自己的属性进行处理，还要对父类的属性进行处理。





#### 2、KVO，NSNotification，delegate及block

 KVO就是cocoa框架实现的观察者模式，一般同KVC搭配使用，通过KVO可以监测一个值的变化，比如View的高度变化。是一对多的关系，一个值的变化会通知所有的观察者。

 NSNotification是通知，也是一对多的使用场景。在某些情况下，KVO和NSNotification是一样的，都是状态变化之后告知对方。NSNotification的特点，就是需要被观察者先主动发出通知，然后观察者注册监听后再来进行响应，比KVO多了发送通知的一步，但是其优点是监听不局限于属性的变化，还可以对多种多样的状态变化进行监听，监听范围广，使用也更灵活。

delegate 是代理，就是我不想做的事情交给别人做。比如狗需要吃饭，就通过delegate通知主人，主人就会给他做饭、盛饭、倒水，这些操作，这些狗都不需要关心，只需要调用delegate（代理人）就可以了，由其他类完成所需要的操作。所以delegate是一对一关系。

block是delegate的另一种形式，是函数式编程的一种形式。使用场景跟delegate一样，相比delegate更灵活，而且代理的实现更直观。

KVO一般的使用场景是数据，需求是数据变化，比如股票价格变化，我们一般使用KVO（观察者模式）。

delegate一般的使用场景是行为，需求是需要别人帮我做一件事情，比如买卖股票，我们一般使用delegate。

 Notification一般是进行全局通知，比如利好消息一出，通知大家去买入。

delegate是强关联，就是委托和代理双方互相知道，你委托别人买股票你就需要知道经纪人，经纪人也不要知道自己的顾客。

Notification是弱关联，利好消息发出，你不需要知道是谁发的也可以做出相应的反应，同理发消息的人也不需要知道接收的人也可以正常发出消息。



#### 3、 多线程-将一个函数在主线程执行的4种方法

*  GCD方法，通过向主线程队列发送一个block块，使block里的方法可以在主线程中执行。 

```
dispatch_async(dispatch_get_main_queue(), ^{
    //需要执行的方法
}); 
```

* NSOperation 方法 

```
NSOperationQueuemainQueue = [NSOperationQueue mainQueue];//主队列 
NSBlockOperationoperation = [NSBlockOperation blockOperationWithBlock:^{ 
    //需要执行的方法 
}]; 
[mainQueue addOperation:operation];
```

*  NSThread 方法 

```
[self performSelector:@selector(method) onThread:[NSThread mainThread] withObject:nil waitUntilDone:YES modes:nil];
[self performSelectorOnMainThread:@selector(method) withObject:nil waitUntilDone:YES];
[[NSThread mainThread] performSelector:@selector(method) withObject:nil];
```

*  RunLoop方法

```
[[NSRunLoop mainRunLoop] performSelector:@selector(method) withObject:nil];
```



