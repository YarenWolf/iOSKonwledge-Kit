# CocoaPods最新安装步骤

## 一、简介

* ### 什么是CocoaPods

  * CocoaPods是OS X和iOS下的一个第三方库管理工具，通过CocoaPods我们可以为项目添加所依赖的库。（这种库必须是CocoaPods本身可以找到且支持的），且可以方便管理这些库的版本（更新、回退）
* ### 好处

  * 在引入第三方库的时候它自动为我们完成各种各样的配置，包括配置编译阶段、连接器选项、甚至是ARC环境下的-fno-objc-arc配置等

  * 使用CocoaPods可以方便查找第三方库（pod search AFNetworking\)，这些库是比较标准的，因为在CocoaPods上可以找得到所以符合CocoaPods的规范比较标准

## 二、CocoaPods的安装步骤

### 1、升级机器的Ruby环境

```
终端输入：$ gem update --system
```

此时有可能出现You do not have write permissions for the /Library/Ruby/Gems/2.0.0 directory

明显告诉我们没有权限，用过linux的人知道需要加上sudo接下来会让你输入密码。不出意外接下来会提示

```
RubyGems system software updated
```

### 2、更换Ruby镜像

* 首先移除现有的Ruby镜像

```
终端输入：$ gem sources --remove https://rubygems.org/
```

* 然后添加国内镜像

```
终端输入：$ gem sources -a https://gems.ruby-china.org/
```

* 执行完毕之后输入gem sources -l来查看当前镜像

```
终端输入：$ gem sources -l
```

如果结果是

`*** CURRENT SOURCES ***https://gems.ruby-china.org/`

### 3、安装Cocopods

输入

```
终端输入：$ sudo gem install cocoapods
```

在我的MBP上出现了提示说权限不足，接下来再次输入

```
终端输入：$ sudo gem install -n /usr/local/bin cocoapods
```

安装成功

```
终端显示 21 gems installed
```

继续

```
pod setup
```

这个过程比较长



## 三、Cocopods的使用

### 1、查找

```
终端输入:pod search ReactiveObjc
```

会显示找到的最新库及其历史版本![](/assets/屏幕快照 2017-11-24 上午10.49.35.png)



### 2、用法

* 新建工程
* 命令行切换到工程目录下
* 新建一个Podfile

```
终端：touch Podfile
```

* 编辑Podfile

```
终端：vim Podfile
```

输入以下内容

```
target 'RAC' do

pod 'ReactiveObjC', '~> 3.0.0'

end 
```

* 终端安装库

```
终端：pod install
```

* 完成之后会看到一个在项目目录下多出一个RAC.xcworkspace文件
* 终端输入

```
终端：open RAC.xcworkspace
```



