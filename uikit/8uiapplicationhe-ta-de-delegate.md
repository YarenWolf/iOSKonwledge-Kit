# UIApplication Delegate



* 很多原因会导致App受到干扰（打电话、内存不足），当App受到干扰时会产生一些系统事件。这时UIApplication会通知它的Delegate对象，让delegate代理来处理这些系统事件



* delegate事件包括：

* 应用程序的生命周期（程序启动关闭）
* 系统事件（来电话）
* 内存警告



