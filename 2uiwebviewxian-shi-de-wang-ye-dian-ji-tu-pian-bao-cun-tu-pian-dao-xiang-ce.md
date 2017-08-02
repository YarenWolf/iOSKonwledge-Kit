# 长按UIWebView上的图片保存到相册

> 不知道各位对于这个需求要如何解决？
>
> 可能有些人会想到js与原生交互，js监听图片点击事件，然后将图片的url传递给原生App端，然后原生App将图片保存到相册，这样子麻烦吗？超麻烦。（1）、js监听图片长按事件；（2）、js将图片url传递给原生；（3）、原生通过图片的url生成UIImage；（4）、保存UIImage到系统相册，巨麻烦啊，大哥，我很懒的好不好

#### 那么问题跑出来了，怎么办最简单？

* 鉴于个人道行尚浅，我就将自己的想法说出来

* 有个js的api：`Document.elementFromPoint()`

> The`elementFromPoint()`method of the[`Document`](https://developer.mozilla.org/en-US/docs/Web/API/Document)interface returns the topmost element at the specified coordinates.

所以根据这个提示，我们完全可以只在App原生端做一些代码开发，实现这个需求

#### 开发步骤

* 给UIWebView添加长按手势
* 监听手势动作，拿到坐标点\(x,y\)
* 弹出对话框，是否保存到相册
* UIWebView注入js:Document.elementFromPoint\(x,y\).src拿到img标签的src
* 拿到图片的url，生成UIImage
* 图片保存到相册

#### 有巨坑

* 长按手势事件不能每次都响应，据我猜测UIWebView本身就有很多事件，所以实现下UIGestureRecognizerDelegate代理方法。长按手势准确率100%

```
- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer{
    return YES;
}
```

附上关键的js官方文档：\[Document.elementFromPoint\(\)\]\([https://developer.mozilla.org/en-US/docs/Web/API/Document/elementFromPoint\](https://developer.mozilla.org/en-US/docs/Web/API/Document/elementFromPoint%29\)

如果需要下载Demo：\[Demo\]\([https://github.com/FantasticLBP/SaveImageToAlbumsFromUIWebView\](https://github.com/FantasticLBP/SaveImageToAlbumsFromUIWebView%29\)

```
//
//  ViewController.m
//  WebView长按图片保存到相册
//
//  Created by 杭城小刘 on 2017/8/2.
//  Copyright © 2017年 杭城小刘. All rights reserved.
//

#import "ViewController.h"

@interface ViewController ()<UIGestureRecognizerDelegate,NSURLSessionDelegate>
@property (weak, nonatomic) IBOutlet UIWebView *webView;

@end

@implementation ViewController

#pragma mark -- life cycle
- (void)viewDidLoad{
    [super viewDidLoad];

    NSString *htmlURL = [[NSBundle mainBundle] pathForResource:@"saveImage" ofType:@"html"];
    [self.webView loadRequest:[NSURLRequest requestWithURL:[NSURL fileURLWithPath:htmlURL]]];
    //给UIWebView添加手势
    UILongPressGestureRecognizer* longPressed = [[UILongPressGestureRecognizer alloc] initWithTarget:self action:@selector(longPressed:)];
    longPressed.delegate = self;
    [self.webView addGestureRecognizer:longPressed];
}

#pragma mark -- UIGestureRecognizerDelegate
- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer{
    UIActivityTypeAddToReadingList
    return YES;
}

- (void)longPressed:(UILongPressGestureRecognizer*)recognizer{
    if (recognizer.state != UIGestureRecognizerStateBegan) {
        return;
    }
    CGPoint touchPoint = [recognizer locationInView:self.webView];
    NSString *imgURL = [NSString stringWithFormat:@"document.elementFromPoint(%f, %f).src", touchPoint.x, touchPoint.y];
    NSString *urlToSave = [self.webView stringByEvaluatingJavaScriptFromString:imgURL];
    if (urlToSave.length == 0) {
        return;
    }

    UIAlertController *alertVC =  [UIAlertController alertControllerWithTitle:@"大宝贝儿" message:@"你真的要保存图片到相册吗？" preferredStyle:UIAlertControllerStyleAlert];
    UIAlertAction *okAction = [UIAlertAction actionWithTitle:@"真的啊" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
            [self saveImageToDiskWithUrl:urlToSave];
    }];
    UIAlertAction *cancelAction = [UIAlertAction actionWithTitle:@"大哥，我点错了，不好意思" style:UIAlertActionStyleDefault handler:nil];
    [alertVC addAction:okAction];
    [alertVC addAction:cancelAction];
    [self presentViewController:alertVC animated:YES completion:nil];
}

#pragma mark - private method
- (void)saveImageToDiskWithUrl:(NSString *)imageUrl{
    NSURL *url = [NSURL URLWithString:imageUrl];

    NSURLSessionConfiguration * configuration = [NSURLSessionConfiguration defaultSessionConfiguration];

    NSURLSession *session = [NSURLSession sessionWithConfiguration:configuration delegate:self delegateQueue:[NSOperationQueue new]];

    NSURLRequest *imgRequest = [NSURLRequest requestWithURL:url cachePolicy:NSURLRequestReturnCacheDataElseLoad timeoutInterval:30.0];

    NSURLSessionDownloadTask  *task = [session downloadTaskWithRequest:imgRequest completionHandler:^(NSURL * _Nullable location, NSURLResponse * _Nullable response, NSError * _Nullable error) {
        if (error) {
            return ;
        }
        NSData * imageData = [NSData dataWithContentsOfURL:location];
        dispatch_async(dispatch_get_main_queue(), ^{

            UIImage * image = [UIImage imageWithData:imageData];
            UIImageWriteToSavedPhotosAlbum(image, self, @selector(imageSavedToPhotosAlbum:didFinishSavingWithError:contextInfo:), NULL);
        });
    }];
    [task resume];
}

#pragma mark 保存图片后的回调
- (void)imageSavedToPhotosAlbum:(UIImage*)image didFinishSavingWithError:  (NSError*)error contextInfo:(id)contextInfo{
    NSString*message =@"嘿嘿";
    if(!error) {
        UIAlertController *alertControl = [UIAlertController alertControllerWithTitle:@"提示" message:@"成功保存到相册" preferredStyle:UIAlertControllerStyleAlert];

        UIAlertAction *action = [UIAlertAction actionWithTitle:@"确定" style:UIAlertActionStyleDestructive handler:nil];
        [alertControl addAction:action];
        [self presentViewController:alertControl animated:YES completion:nil];
    }else{
        message = [error description];
        UIAlertController *alertControl = [UIAlertController alertControllerWithTitle:@"提示" message:message preferredStyle:UIAlertControllerStyleAlert];
        UIAlertAction *action = [UIAlertAction actionWithTitle:@"确定" style:UIAlertActionStyleCancel handler:nil];
        [alertControl addAction:action];
        [self presentViewController:alertControl animated:YES completion:nil];
    }
}

@end
```



