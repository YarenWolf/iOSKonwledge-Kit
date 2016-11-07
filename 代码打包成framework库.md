[](http://www.jianshu.com/p/9f2d5cd546cc)###代码打包成.framework库



 1、打开xcode新建工程，选择如下Framework&Library下的Cocoa Touch Framework。

![](/assets/1.png)



 2、编写代码

![](/assets/2.png)



 3、在生成的工程的.h文件中将需要暴露给外部的文件全部以<framework名/头文件名.h>的形式写出来。

![](/assets/3.png)



 4、在工程设置中选择需要支持的最低版本号选择出来。



![](/assets/5.png)

![](/assets/6.png)



 5、选中工程的TARGETS的Build Phases，找到Headers，将需要暴露给外部的.h文件加到如下图的位置。

![](/assets/7.png)



 6、在X-code的Product->Scheme->Edit scheme。将Build Configuration选择不同的Release和Debug版本。

然后按下“Command+B”快捷键编译整个工程，你需要分别在模拟器和真机上运行，会生成可以在真机和模拟器上使用的.frameWork。

![](/assets/屏幕快照 2016-10-25 下午12.23.21.png)

![](/assets/屏幕快照 2016-10-25 下午12.23.32.png)



 7、如果工程结构下的Products为红色则代表没成功，黑色为成功。

![](/assets/11.png)



 8、如何使用？



 （1）、将frameWork加入到项目中。

 ![](/assets/12.png)

 （2）、因为我们做的是动态库，在使用的时候需要额外加一个步骤,要把Framework同时添加到‘Embedded Binaries’中

![](/assets/13.png)

 （3）、![](/assets/15.png)

 （4）、代码中使用

 ```

#import <MyLibrary/MyLibrary.h> //导入库头文件

...

 [MyPrinter show]; //使用

 ```

 9、常见问题

 有时候自己打包好的FrameWork在使用的时候会爆出一些奇怪的错误，如下图所示。

 ![](/assets/9609286F-8E77-4E6D-99A6-59E4AED22EB7.png)

 这些错误其实是自己在打包Framework的时候没有将自己提供给外部调用的.h文件中引用到的.h文件放到Build Phases->Header->Public中去。

 ![](/assets/屏幕快照 2016-10-31 下午1.36.09.png)

10、自己打包好的Framework引入项目工程报错。见下图。
![](/assets/屏幕快照 2016-11-02 下午10.28.21.png)

解决办法：这种情况一般是自己打包的动态库的模拟器和真机版本没有合并起来。另外需要注意自己打包动态库的时候需要选择是Release版本还是Debug版本。将工程中的Product-》scheme-》Edit scheme中的设置一致。在Release模式下将framework工程分别选择模拟器和真机编译一遍，这样会生成2个Release模式下的动态Framework包，同样在Debug模式下将工程分别在模拟器和真机编译一遍，生成2个Debug模式下的Framework包。


    接下来才是正式步骤，打开终端。输入“cd framework的目录下，”
分别输入：
lipo -create 【模拟器打包path】 【真机打包path】 -output 【导出兼容版本path】


11、在制作Framework的时候需要可能需要在项目中混编swift和oc。新建好swift文件后需要在swift文件导入一些oc写的头文件，但是Framework工程不允许使用pch文件的格式。所以解决办法：1、删除桥接头文件，如Xxx-Bridging.h，删到垃圾桶亦可；也把Build Settings中指定了桥接头文件路径配置清空。2、把需要导入的oc文件加入到Build Phases的Headers中。


参考链接：
http://www.jianshu.com/p/9f2d5cd546cc
http://www.cocoachina.com/ios/20141126/10322.html
https://my.oschina.net/hantianyu/blog/481525
http://www.cnblogs.com/yajunLi/p/6005077.html





