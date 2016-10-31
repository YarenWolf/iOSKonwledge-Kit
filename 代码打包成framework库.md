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


参考链接：
http://www.jianshu.com/p/9f2d5cd546cc




