###自定义URL Scheme###

1、引言

URL Schemes 应用在 iOS 上已经很久了。对于使用者来说，在沙盒机制下的 iOS 中，如果想做到一定程度上的自动化就不可避免地要用到 URL Schemes。但因为 URL Schemes 的使用方式不像传统 iOS 使用者接触到的图形界面那样可以直观地点来点去，造成了对它有兴趣的人（尤其是对英文有恐惧的人）一定程度上理解的困难。


2、简介苹果的沙盒机制


苹果选择沙盒来保障用户的隐私和安全，但沙盒也阻碍了应用间合理的信息共享，于是有了 URL Schemes 这个解决办法。

一般来说，我们使用的智能设备上有许多我们的个人信息。比如：联系方式、银行卡/信用卡信息、支付宝/Paypal/各大商城的账户密码、照片甚至行程与位置信息等。

如果说，你设备上的每一个应用，不管是官方的还是你从任何商城安装的应用都可以随意地获取这些信息，那么你轻则收到骚扰信息和邮件、重则后果不堪设想。如何让这些信息不被其它应用随意使用，或者说，如何让这些信息仅在设备所有者本人知情并允许的情况下被使用，是所有智能设备与操作系统所要在乎的核心安全问题。

在 iOS 这个操作系统中，针对这个问题，苹果使用了名为「沙盒」的机制：应用只能访问它声明可能访问的资源。一切提交到 App Store 的应用都必须遵守这个机制。

在安全方面沙盒是个很好的解决办法，但是有些矫枉过正。敏感的个人信息我们不愿意透露，却不代表所有的信息我们都不想与其它应用共享。

比如说我们要一次性地（没错，只按一次）把多个事件放到日历中，这些事件包含日期时间以及持续时间等信息，如果 App 之间信息不能沟通，就无法做到这点。（在下文中的 x-callback-URL 的部分会详述整个过程）

类似于一次性添加多个日历事件这样的，我们在使用智能设备的过程中会遇到很多不必要的重复的步骤。大多数人对这些重复的步骤是不自觉的，就像当自己电脑里有一批文件需要批量重命名的时候，他们机械地重复着重命名的过程。但是当我们掌握了这些设备运行的模式，或者有了一些工具，我们就能将这些重复的步骤全部节省下来。在 iOS 上，我们可以利用的工具就是 URL Schemes。


3、URL Schemes 是什么

Custom URL scheme 的好处就是，你可以在其它程序中通过这个url打开应用程序。如Ａ应用程序注册了一个url scheme:myApp, 那么就在mobile浏览器中就可以通过<href=’myApp://’>打开你的应用程序Ａ。

对比网页url就比较好理解url scheme，。给出一个url “http://bxu2359670321.my3w.com/view/login.php”，它的格式：protocol :// hostname[:port] / path / [;parameters][?query]#fragment。
因此这个url的protocol就是http。对比URL Scheme，给出例子“weixin://dl/moments“，前面的weixin://就代表微信的scheme。你可以完全按照理解一个网页的 URL ——也就是它的网址——的方式来理解一个 iOS 应用的 URL。

 
###注意###

1、所有的网页都有url；但未必所有的应用都有自己的 URL Schemes

2、一个网址只对应一个网页，但并非每个 URL Schemes 都只对应一款应用。这点是因为苹果没有对 URL Schemes 有不允许重复的硬性要求

3、一般网页的 URL 比较好预测，而 iOS 上的 URL Schemes 因为没有统一标准，所以非常难猜，通过猜来获取 iOS 应用的 URL Schemes 是不现实的。（我推荐将Bundle identifier反转） 


###上干货###

1、注册自定义 URL Scheme


1）注册自定义 URL Scheme 的第一步是创建 URL Scheme — 在 Xcode Project Navigator 中找到并点击工程 info.plist 文件。当该文件显示在右边窗口，在列表上点击鼠标右键，选择 Add Row:
![](/assets/url scheme1.png)

2）点击左边剪头打开列表，可以看到 Item 0，一个字典实体。展开 Item 0，可以看到 URL Identifier，一个字符串对象。该字符串是你自定义的 URL scheme 的名字。建议采用反转Bundle idenmtifier的方法保证该名字的唯一性
![](/assets/url scheme2.png)

3）点击 Item 0 新增一行，从下拉列表中选择 URL Schemes，敲击键盘回车键完成插入。（注意 URL Schemes 是一个数组，允许应用定义多个 URL schemes。）展开该数据并点击 Item 0。你将在这里定义自定义 URL scheme 的名字。只需要名字，不要在后面追加 ://


![](/assets/url scheme3.png)

2、拿浏览器坐简单验证

在地址栏中熟入自定的url scheme。此时必须保证该浏览器所在设备上已经安装了具有自定义url scheme的App。
![](/assets/IMG_5739.PNG)

3、新建Xcode工程，做个App试试看，这里我就放一个Button，点击打开url
代码。


```
- (IBAction)open:(id)sender {
    NSString *url = @"zhunaer://?name=lbp&age=22";
    if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:url]]) {
           [[UIApplication sharedApplication] openURL:[NSURL URLWithString:url]];
    }else{
        NSLog(@"打不开");
    }
}
```
结果打不开，为什么？
因为在新建App的plist中没加query schemes


```
<key>LSApplicationQueriesSchemes</key>
<array>
<string>zhunaer</string>
</array>
```

4、如果需要在2个App之间传值，怎么办？可以用URL Scheme解决。

在被打开的App的Appdelegate.m中实现-(BOOL)application:(UIApplication *)application openURL:(nonnull NSURL *)url sourceApplication:(nullable NSString *)sourceApplication annotation:(nonnull id)annotation；



```
-(BOOL)application:(UIApplication *)application openURL:(nonnull NSURL *)url sourceApplication:(nullable NSString *)sourceApplication annotation:(nonnull id)annotation{
    NSLog(@"calling application bundle id: %@",sourceApplication);
    NSLog(@"url shceme:%@",[url scheme]);
    NSLog(@"参数:%@",[url query]);
    if ([sourceApplication isEqualToString:@"com.geek.test1"]) {
        return YES;
    }
    return NO;
    
}

```


在需要打开第三方App的点击事件处的url处后面加上参数，类似NSString *url = @"zhunaer://?name=lbp&age=22";

注意：在URL Scheme后加?然后跟网页的url的参数一样写法。

5、如何判断是指定App打开，或者某些App不让打开我们的App？
做了实验。

A：在需要打开第三方App的工程中将Bundle identifier改为“com.geek.test2”,其余不变

B：在被打开的App的AppDelegate.m中


```
-(BOOL)application:(UIApplication *)application openURL:(nonnull NSURL *)url sourceApplication:(nullable NSString *)sourceApplication annotation:(nonnull id)annotation{
    NSLog(@"calling application bundle id: %@",sourceApplication);
    NSLog(@"url shceme:%@",[url scheme]);
    NSLog(@"参数:%@",[url query]);
    if ([sourceApplication isEqualToString:@"com.geek.test1"]) {
        return YES;
    }
    return NO;
}
```

###实验结果###

依旧可以打开App，即使断点走入Return NO

结论：如果你想阻止其它应用调用你的应用，###创建一个与众不同的 URL scheme###。尽管这不能保证你的应用不会被调用，但至少大大降低了这种可能性。



