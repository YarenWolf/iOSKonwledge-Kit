\(modified\) 相册选取照片、拍照 — GitBook Editor

[iOSKonwledge-Kit](#)

[Primary version](#)

[Create a branch](#)

H1

H2

H3

Save

1

* [TOC](#)
* [FILES](#)

介绍

第一章 初识swift

第二章 玩转儿swift 第一回合

第三章 玩转儿swift 第二回合

第四章 玩转儿swift 第三回合

第五章 玩转儿Swift3新特性

续：开发tips

正则表达式

提高代码质量

毛玻璃-UIBlurEffect

单例

Webview

ios10某些App无法获取数据权限

UILabel自适应策略

GCD

TableView

ios拨打电话号码

static、constant

命令行 plist-

&gt;

json json-

&gt;

plist

代码打包成.framework库\(上\)

PrefixHeader.pch文件

iOS开发常用快捷键

教你搞定导航控制器全屏滑动返回效果

常用宏

MVC、MVVM、MVP

做一个 App 前需要考虑的几件事

UIViewController-Swizzled

通知

UINavigationController系列

可以滑动的导航

代码打包成.framework库\(下\)

根据图片url计算图片尺寸

swift、OC混编

iOS 之改变状态栏颜色

版本管理

数据源转换

环信

UIbutton文字右对齐

App引导页

ios具体设备型号

数组

Frmae和Bounds的区别

ios图片上传，压缩问题

NavigationController管理VC

UIWebView缓存方面的坑

StoryBoard

常见Error

UIDatePicker的使用

自定义Picker

UIAlertController带输入框

伸缩式header bar--BLKFlexibleHeightBar

UIImageView

ios10适配引申开来关于条件编译

X-code工作规范

注释规范

UISegmentedControl的使用

clipsToBounds 

&

 masksToBounds

使用safari对webview进行调试

NSTimer

Bool值

instancesRespondToSelector

&

respondsToSelector

URL常见问题

工程管理

手势

数组排序

第三方分享

校验

UIWebVIew滚动监听

ios10之后App无法请求网络权限问题

地图

相册选取照片、拍照

OC学习

Runtime篇

开发tips-OC版

地图

开发tips-Swift版

IPV6

网络篇

UINavigationController

UISteper渐进控件

URL Schemes

Add an article

1、使用场景：点击个人中心的用户头像，弹出“从相册选取”、“拍照”。用户选择相应的权限后进行操作。

  


2、UIAlertController弹出选择：

  


\`\`\`

UIAlertController \*alert = \[UIAlertController alertControllerWithTitle:nil message:nil preferredStyle:UIAlertControllerStyleActionSheet\];

UIAlertAction \*photo = \[UIAlertAction actionWithTitle:@"拍照" style:UIAlertActionStyleDestructive handler:^\(UIAlertAction \* \_Nonnull action\) {

}\];

UIAlertAction \*library = \[UIAlertAction actionWithTitle:@"从相册中选择" style:UIAlertActionStyleDefault handler:^\(UIAlertAction \* \_Nonnull action\) {

}\];

UIAlertAction \*cancel = \[UIAlertAction actionWithTitle:@"退出" style:UIAlertActionStyleCancel handler:nil\];

\[alert addAction:photo\];

\[alert addAction:library\];

\[alert addAction:cancel\];

\[self presentViewController:alert animated:YES completion:nil\];

  


\`\`\`

  


  


3、从相册中选择：

  


iOS10 需要在info.plist中声明权限。

\*\*

Privacy - Photo Library Usage Description -“住哪儿”需要访问您的相册信息

\*\*

  


在vc中遵循因此我们要实现UIImagePickerControllerDelegate,UINavigationControllerDelegate协议

  


sourceType有三种类型，是一个枚举值

  


  


\`\`\`

typedef NS\_ENUM\(NSInteger, UIImagePickerControllerSourceType\) {

UIImagePickerControllerSourceTypePhotoLibrary, //相册库

UIImagePickerControllerSourceTypeCamera, //拍照

UIImagePickerControllerSourceTypeSavedPhotosAlbum //从“时刻”中选择

}

\_\_

TVOS

\_

PROHIBITED;

  


  


  


\`\`\`

  


  


  


  


\`\`\`

UIImagePickerController \*imagePicker = \[\[UIImagePickerController alloc\] init\];

imagePicker.sourceType = UIImagePickerControllerSourceTypeSavedPhotosAlbum; //选择类型，表示从相册中选取照片

imagePicker.delegate = self; //指定代理，实现UIImagePickerControllerDelegate，UINavigationControllerDelegate

imagePicker.allowsEditing = YES; //设置在相册中选取照片后，是否跳到编辑模式进行图片剪裁

\[self presentViewController:imagePicker animated:YES completion:nil\];

\`\`\`

  


4、拍照

&lt;

li

&gt;

需要监测设备是否支持拍照。

&lt;

/

li

&gt;

  


  


\`\`\`

if \(\[UIImagePickerController isSourceTypeAvailable:UIImagePickerControllerSourceTypeCamera\]\) {

  


}else{

  


}

\`\`\`

  


&lt;

li

&gt;

iOS10 需要声明相机权限

&lt;

/

li

&gt;

Privacy - Camera Usage Description - “住哪儿”需要访问您的相机权限

  


5、实现代理方法

  


  


  


\`\`\`

\#

pragma mark - UIImagePickerControllerDelegate

-

\(void\)imagePickerController:\(UIImagePickerController

\*

\)picker didFinishPickingImage:\(UIImage

\*

\)image editingInfo:\(nullable NSDictionary

&lt;

NSString

\*

,id

&gt;

\*

\)editingInfo {

\[UserManager setUserImage:image\];

\[self dismissViewControllerAnimated:YES completion:nil\];

\[self.tableView reloadData\];

}

\`\`\`

  


  


  


  


1、使用场景：点击个人中心的用户头像，弹出“从相册选取”、“拍照”。用户选择相应的权限后进行操作。

2、UIAlertController弹出选择：

```
UIAlertController *alert = [UIAlertController alertControllerWithTitle:nil message:nil preferredStyle:UIAlertControllerStyleActionSheet];
            UIAlertAction *photo = [UIAlertAction actionWithTitle:@"拍照" style:UIAlertActionStyleDestructive handler:^(UIAlertAction * _Nonnull action) {

            }];
            UIAlertAction *library = [UIAlertAction actionWithTitle:@"从相册中选择" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {

            }];
            UIAlertAction *cancel = [UIAlertAction actionWithTitle:@"退出" style:UIAlertActionStyleCancel handler:nil];
            [alert addAction:photo];
            [alert addAction:library];
            [alert addAction:cancel];
            [self presentViewController:alert animated:YES completion:nil];
```

3、从相册中选择：

iOS10 需要在info.plist中声明权限。**Privacy-PhotoLibraryUsageDescription-“住哪儿”需要访问您的相册信息**

在vc中遵循因此我们要实现UIImagePickerControllerDelegate,UINavigationControllerDelegate协议

sourceType有三种类型，是一个枚举值

```
typedef NS_ENUM(NSInteger, UIImagePickerControllerSourceType) {
    UIImagePickerControllerSourceTypePhotoLibrary,        //相册库
    UIImagePickerControllerSourceTypeCamera,        //拍照
    UIImagePickerControllerSourceTypeSavedPhotosAlbum   //从“时刻”中选择
} __TVOS_PROHIBITED;
```

```
 UIImagePickerController *imagePicker = [[UIImagePickerController alloc] init];
                imagePicker.sourceType = UIImagePickerControllerSourceTypeSavedPhotosAlbum;     //选择类型，表示从相册中选取照片
                imagePicker.delegate = self;        //指定代理，实现UIImagePickerControllerDelegate，UINavigationControllerDelegate
                imagePicker.allowsEditing = YES;           //设置在相册中选取照片后，是否跳到编辑模式进行图片剪裁
                [self presentViewController:imagePicker animated:YES completion:nil];
```

4、拍照

需要监测设备是否支持拍照。

```
if ([UIImagePickerController isSourceTypeAvailable:UIImagePickerControllerSourceTypeCamera]) {

}else{

}
```

iOS10 需要声明相机权限  
Privacy - Camera Usage Description - “住哪儿”需要访问您的相机权限

5、实现代理方法

```
#pragma mark - UIImagePickerControllerDelegate
- (void)imagePickerController:(UIImagePickerController *)picker didFinishPickingImage:(UIImage *)image editingInfo:(nullable NSDictionary
<
NSString *,id
>
 *)editingInfo {
    [UserManager setUserImage:image];
    [self dismissViewControllerAnimated:YES completion:nil];
    [self.tableView reloadData];
}
```



