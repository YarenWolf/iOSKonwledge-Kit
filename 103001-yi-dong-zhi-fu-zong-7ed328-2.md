# 支付宝SDK支付

* 1、进入支付宝官网下载所需资源包：[https://doc.open.alipay.com/doc2/detail.htm?treeId=54&articleId=104509&docType=12](https://doc.open.alipay.com/doc2/detail.htm?treeId=54&articleId=104509&docType=1)
* 2、运行Demo，查看如何使用
* 3、将所需资源加入到自己的项目工程中

![](/assets/屏幕快照 2017-07-13 下午6.26.00.png)



* 4、Command+B 编译项目，查看是否正确
* 5、编译后发现报错![](/assets/屏幕快照 2017-07-13 下午8.32.03.png)

* 6、相信上图所示的错误很多人都遇到过，解决办法是将支付宝SDK的文件路径填到Build Settings选项中的Header Search Paths

,注意这里填写的![](/assets/QQ20170713-203710@2x.png)

