# iOSApp打包上线的时候报错

All object files and libraries for bitcode must be generated from xcode Archive or install build for architecture arm64

那么bitcode是什么：

Distribution  Guide-App Thinning\(iOS, wacthOS\)一节中，找到了下面这样的定义：

> Bitcode is an intermediate representationof a compiled program. Apps you upload to iTunes Connect that contain bitcodewill be compiled and linked on the App Store. Including bitcode will allowApple to re-optimize your app binary in the future without the need to submit anew version of your app to the store.

说的是bitcode是被编译程序的一种中间形式的代码，包含bitcode配置的程序将会在App store上被编译和链接，bitcode允许苹果在后期重新优化程序的二进制文件，而不需要重新提交一个新的版本到App store。



解决办法：

* 让第三方库支持bitcode
  * 在时间充裕的情况下可以跟这个SDK或者库的客服沟通交流。让他们提供一个支持bitcode的版本
* 关闭bitcode
  * 时间比较紧的情况下，人家即使愿意给你提供一个支持bitcode的版本也是需要时间，如果需要上架紧急，那么我们的Xcode工程可以直接关闭bitcode。



