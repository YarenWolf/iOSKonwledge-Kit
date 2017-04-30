###定位、目的地显示、路径规划、导航###


1、首先说说如何接入百度地图
    
    1)首先打开百度地图开开放平台网站，。找到相关下载模块，根据需求下载所需的SDK包。我这里选择的是“全部下载”
    
    2)其次需要在网站上申请密钥，申请步骤可查看官方文档。
    [申请密钥](http://lbsyun.baidu.com/index.php?title=iossdk/guide/key)
    
    3)在工程中接入百度地图。步骤说明：[百度地图接入](http://lbsyun.baidu.com/index.php?title=iossdk/guide/buildproject)
    
    使用CocoaPods。
    在当前工程文件（.xcodeproj）所在文件夹下，打开terminal，创建Podfile
    touch Podfile
    编辑Podfile内容如下：
    pod 'BaiduMapKit' #百度地图SDK
    在Podfile所在的文件夹下输入命令：
    pod install （这个可能比较慢，请耐心等待……）

    
    
    手动配置.framework形式开发包
    
    在工程的3dparty目录下“New Group”，将所需的包引入
    在 TARGETS->Build Phases-> Link Binary With Libaries中点击“+”按钮，在弹出的窗口中点击“Add Other”按钮，选择BaiduMapAPI_**.framework添加到工程中。
    注意：静态库采用objective-c++的形式开发，所以工程中至少要有一个.mm文件，或者修改Project -> Edit Active Target -> Build Setting 中找到 Compile Sources As，并将其设置为"Objective-C++"
    引入所需的系统库。CoreLocation.framework和QuartzCore.framework、OpenGLES.framework、SystemConfiguration.framework、CoreGraphics.framework、Security.framework、libsqlite3.0.tbd（xcode7以前为 libsqlite3.0.dylib）、CoreTelephony.framework 、libstdc++.6.0.9.tbd（xcode7以前为libstdc++.6.0.9.dylib）。在Xcode的Project -> Active Target ->Build Phases ->Link Binary With Libraries，添加这几个系统库即可。
    如果项目需要基础地图功能则需引入mapapi.bundle资源文件。
    
    
2、在需要显示地图的VC里
    
    
    