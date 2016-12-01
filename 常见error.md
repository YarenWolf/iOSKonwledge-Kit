###Ios开发中常见错误总结

1、cell复用奔溃。
    
![](/assets/屏幕快照 2016-11-30 下午4.51.33.png)
解决方法：检查cell是否在项目中；选中你的xib文件然后把xcode的右边栏工具窗口打开在File inspector 中下面有个target membership 确保你的选择框是勾选上的。


2、今天遇到一个奇葩问题，一个功能选择时间，本地用各种机型测试没问题，上线App Store，下载下来发现iphone5c没问题，iphone6s plus有问题，百思不得其解。最后再项目设置中选择Release模式下Debug，发现奔溃在一个第三方库的地方，一个UIColor居然是assign修饰的，这不坑爹吗？总结了下：项目测试后需要在Release模式下导出一个ipa来测试。github上不上1000的库不敢用，要么用的时候代码一行行过一遍。
