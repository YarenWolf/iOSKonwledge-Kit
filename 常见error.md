###Ios开发中常见错误总结

1、cell复用奔溃。
    
![](/assets/屏幕快照 2016-11-30 下午4.51.33.png)
解决方法：检查cell是否在项目中；选中你的xib文件然后把xcode的右边栏工具窗口打开在File inspector 中下面有个target membership 确保你的选择框是勾选上的。
