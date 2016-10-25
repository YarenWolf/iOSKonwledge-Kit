###代码打包成.framework库

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
    