# 微信支付总结

* 查看微信支付Demo，看到一句代码用来判断iphone和ipad的界面显示

```
 if ([[UIDevice currentDevice] userInterfaceIdiom] == UIUserInterfaceIdiomPhone) {
        self.viewController = [[[SendMsgToWeChatViewController alloc] initWithNibName:@"ViewController_iPhone" bundle:nil] autorelease];
    } else {
        self.viewController = [[[SendMsgToWeChatViewController alloc] initWithNibName:@"ViewController_iPad" bundle:nil] autorelease];
    }
```

![](/assets/1377427-359d12bff546cf2f.png)

图片引自：[http://www.jianshu.com/p/1c1c834b6d52](http://www.jianshu.com/p/1c1c834b6d52)

#### App端接入微信支付的步骤

* 去微信的平台申请商家的信息，需要Bundle id等信息
* 下载微信支付Demo运行查看开发流程
* 初次下载的微信支付Demo编译会报错，首先解决错误。![](/assets/屏幕快照 2017-07-04 下午3.27.04.png)解决方案：

* 选中Target，在Build Phases中Link Binary with Libraries中搜索CFNetwork.framework

* 再次运行会看到模拟器上熟悉的地球图片，然后又出错

![](/assets/屏幕快照 2017-07-04 下午3.30.58.png)

* 解决办法:选中Targets-&gt;Build Settings-&gt;Other Linker Flags-&gt;点击加入-all\_load和-Objc

* 这样微信支付Demo就顺利跑起来了

总结：其实这些错误信息在下载下来的资源包中的README.txt中早已写明了

正式开发步骤：

* 我们自己的工程中引入微信支付SDk
* 在Targets-&gt;Build Setting-&gt;Link Binary With Libraries添加所需要的依赖库
  * SystemConfiguration.framework
  * libz.tbd
  * libsqlite3.0.tbd
  * CoreTelephony.framework
  * QuartzCore.framework
* 设置URL Scheme

  * 在微信平台上注册App信息的时候会给我们的App分配一个唯一标识，然后将这个唯一标识填写在下图的这个地方![](/assets/QQ20170704-154424@2x.png)

* 在AppDelegate,m中注册AppID

```
    [WXApi registerApp:WX_APPID withDescription:@"微信支付"];
```

* 同时在AppDelegate.m中写

```
//微信支付
- (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url {
    return  [WXApi handleOpenURL:url delegate:[WXApiManager sharedManager]];
}

- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
    return [WXApi handleOpenURL:url delegate:[WXApiManager sharedManager]];
}
```

* 做进一步封装，达到解耦

```
#import <Foundation/Foundation.h>
#import "WXApi.h"

//微信支付结果
typedef NS_ENUM(NSInteger,WXPay_State){
    WXPay_State_Fail = 0,
    WXPay_State_Success = 1
};

@class WXApiManager;

@protocol WXApiManagerDelegate <NSObject>

@optional

- (void)managerDidRecvGetMessageReq:(GetMessageFromWXReq *)request;

- (void)managerDidRecvShowMessageReq:(ShowMessageFromWXReq *)request;

- (void)managerDidRecvLaunchFromWXReq:(LaunchFromWXReq *)request;

- (void)managerDidRecvMessageResponse:(SendMessageToWXResp *)response;

- (void)managerDidRecvAuthResponse:(SendAuthResp *)response;

- (void)managerDidRecvAddCardResponse:(AddCardToWXCardPackageResp *)response;


-(void)wxApiManager:(WXApiManager *)wxApiManager didPayedWithState:(WXPay_State)payState;

@end

@interface WXApiManager : NSObject<WXApiDelegate>

@property (nonatomic, assign) id<WXApiManagerDelegate> delegate;

+ (instancetype)sharedManager;

/**
 * 调起微信支付
 *
 * dic：字典 ，key的值应该和官方SDK文档的参数key一致
 */
-(void)wechatpayWithBasicInfo:(NSDictionary *)dic;

@end

#import <Foundation/Foundation.h>
#import "WXApi.h"

//微信支付结果
typedef NS_ENUM(NSInteger,WXPay_State){
    WXPay_State_Fail = 0,
    WXPay_State_Success = 1
};

@class WXApiManager;

@protocol WXApiManagerDelegate <NSObject>

@optional

- (void)managerDidRecvGetMessageReq:(GetMessageFromWXReq *)request;

- (void)managerDidRecvShowMessageReq:(ShowMessageFromWXReq *)request;

- (void)managerDidRecvLaunchFromWXReq:(LaunchFromWXReq *)request;

- (void)managerDidRecvMessageResponse:(SendMessageToWXResp *)response;

- (void)managerDidRecvAuthResponse:(SendAuthResp *)response;

- (void)managerDidRecvAddCardResponse:(AddCardToWXCardPackageResp *)response;


-(void)wxApiManager:(WXApiManager *)wxApiManager didPayedWithState:(WXPay_State)payState;

@end

@interface WXApiManager : NSObject<WXApiDelegate>

@property (nonatomic, assign) id<WXApiManagerDelegate> delegate;

+ (instancetype)sharedManager;

/**
 * 调起微信支付
 *
 * dic：字典 ，key的值应该和官方SDK文档的参数key一致
 */
-(void)wechatpayWithBasicInfo:(NSDictionary *)dic;

@end

#import "WXApiManager.h"
#import "WechatPayConstant.h"

@implementation WXApiManager


+(instancetype)sharedManager {
    static dispatch_once_t onceToken;
    static WXApiManager *instance;
    dispatch_once(&onceToken, ^{
        instance = [[WXApiManager alloc] init];
    });
    return instance;
}

- (void)dealloc {
    self.delegate = nil;
}


-(void)wechatpayWithBasicInfo:(NSDictionary *)dic{
    PayReq* req             = [[PayReq alloc] init];
    NSMutableString *stamp  = [dic objectForKey:@"timestamp"];
    req.partnerId           = [dic objectForKey:@"partnerid"];
    req.prepayId            = [dic objectForKey:@"prepayid"];
    req.nonceStr            = [dic objectForKey:@"noncestr"];
    req.timeStamp           = stamp.intValue;
    req.package             = [dic objectForKey:@"package"];
    req.sign                = [dic objectForKey:@"sign"];
    [WXApi sendReq:req];
}

#pragma mark - WXApiDelegate
- (void)onResp:(BaseResp *)resp {
    if ([resp isKindOfClass:[SendMessageToWXResp class]]) {
        if (_delegate
            && [_delegate respondsToSelector:@selector(managerDidRecvMessageResponse:)]) {
            SendMessageToWXResp *messageResp = (SendMessageToWXResp *)resp;
            [_delegate managerDidRecvMessageResponse:messageResp];
        }
    } else if ([resp isKindOfClass:[SendAuthResp class]]) {
        if (_delegate
            && [_delegate respondsToSelector:@selector(managerDidRecvAuthResponse:)]) {
            SendAuthResp *authResp = (SendAuthResp *)resp;
            [_delegate managerDidRecvAuthResponse:authResp];
        }
    } else if ([resp isKindOfClass:[AddCardToWXCardPackageResp class]]) {
        if (_delegate
            && [_delegate respondsToSelector:@selector(managerDidRecvAddCardResponse:)]) {
            AddCardToWXCardPackageResp *addCardResp = (AddCardToWXCardPackageResp *)resp;
            [_delegate managerDidRecvAddCardResponse:addCardResp];
        }
    }else if([resp isKindOfClass:[PayResp class]]){
        //支付返回结果，实际支付结果需要去微信服务器端查询
        switch (resp.errCode) {
            case WXSuccess:
                if (self.delegate && [self.delegate respondsToSelector:@selector(wxApiManager:didPayedWithState:)]) {
                    [self.delegate wxApiManager:self didPayedWithState:WXPay_State_Success];
                }
                break;
            default:
                if (self.delegate && [self.delegate respondsToSelector:@selector(wxApiManager:didPayedWithState:)]) {
                    [self.delegate wxApiManager:self didPayedWithState:WXPay_State_Fail];
                }
                break;
        }
    }

}

- (void)onReq:(BaseReq *)req {
    if ([req isKindOfClass:[GetMessageFromWXReq class]]) {
        if (_delegate
            && [_delegate respondsToSelector:@selector(managerDidRecvGetMessageReq:)]) {
            GetMessageFromWXReq *getMessageReq = (GetMessageFromWXReq *)req;
            [_delegate managerDidRecvGetMessageReq:getMessageReq];
        }
    } else if ([req isKindOfClass:[ShowMessageFromWXReq class]]) {
        if (_delegate
            && [_delegate respondsToSelector:@selector(managerDidRecvShowMessageReq:)]) {
            ShowMessageFromWXReq *showMessageReq = (ShowMessageFromWXReq *)req;
            [_delegate managerDidRecvShowMessageReq:showMessageReq];
        }
    } else if ([req isKindOfClass:[LaunchFromWXReq class]]) {
        if (_delegate
            && [_delegate respondsToSelector:@selector(managerDidRecvLaunchFromWXReq:)]) {
            LaunchFromWXReq *launchReq = (LaunchFromWXReq *)req;
            [_delegate managerDidRecvLaunchFromWXReq:launchReq];
        }
    }
}

@end

```



