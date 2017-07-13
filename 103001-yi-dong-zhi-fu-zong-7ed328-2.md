# 支付宝SDK支付

* 1、进入支付宝官网下载所需资源包：[https://doc.open.alipay.com/doc2/detail.htm?treeId=54&articleId=104509&docType=12](https://doc.open.alipay.com/doc2/detail.htm?treeId=54&articleId=104509&docType=1)
* 2、运行Demo，查看如何使用
* 3、将所需资源加入到自己的项目工程中

![](/assets/屏幕快照 2017-07-13 下午6.26.00.png)

* 4、Command+B 编译项目，查看是否正确
* 5、编译后发现报错![](/assets/屏幕快照 2017-07-13 下午8.32.03.png)

* 6、相信上图所示的错误很多人都遇到过，解决办法是将支付宝SDK的文件路径填到Build Settings选项中的Header Search Paths

,注意这里填写的路径要对应工程文件中的路径![](/assets/QQ20170713-203710@2x.png)

* 7、对支付宝支付做以封装，做到解耦

```
#import <Foundation/Foundation.h>

@class AliPayManager;

//支付宝支付结果
typedef NS_ENUM(NSInteger,ALiPay_State){
    ALiPay_State_Fail = 0,
    ALiPay_State_Success = 1
};

@protocol AliPayManagerDelegate <NSObject>

-(void)aliApiManager:(AliPayManager *)aliApiManager didPayedWithState:(ALiPay_State)payState;

@end

@interface AliPayManager : NSObject

+ (instancetype)sharedManager;

/**
 * 调起支付宝支付
 *
 * dic：字典 ，key的值应该和官方SDK文档的参数key一致
 */
-(void)alipayWithBasicInfo:(NSDictionary *)dic;

@property (nonatomic, weak) id<AliPayManagerDelegate> delegate;

@end



#import "AliPayManager.h"
#import "AlipayConstant.h"
#import "Order.h"
#import "RSADataSigner.h"
#import <AlipaySDK/AlipaySDK.h>

@implementation AliPayManager

+ (instancetype)sharedManager{
    static dispatch_once_t onceToken;
    static AliPayManager *instance;
    dispatch_once(&onceToken, ^{
        instance = [[AliPayManager alloc] init];
    });
    return instance;
}

- (void)dealloc {
    self.delegate = nil;
}

-(void)alipayWithBasicInfo:(NSDictionary *)dic{
    /*
     *生成订单信息及签名
     */
    //将商品信息赋予AlixPayOrder的成员变量
    Order* order = [Order new];

    // NOTE: app_id设置
    order.app_id = dic[@"app_id"];

    // NOTE: 支付接口名称
    order.method = dic[@"method"];

    // NOTE: 参数编码格式
    order.charset = dic[@"charset"];

    // NOTE: 当前时间点
    order.timestamp = dic[@"timestamp"];

    // NOTE: 支付版本
    order.version = dic[@"version"];

    //NOTE: 支付成功后的回调
    order.notify_url = dic[@"notify_url"];

    // NOTE: sign_type 根据商户设置的私钥来决定
    order.sign_type = dic[@"sign_type"];
    //字符串转字典
    NSData *data1 = [dic[@"biz_content"] dataUsingEncoding:NSUTF8StringEncoding];
    NSDictionary *bizContent = [NSJSONSerialization JSONObjectWithData:data1 options:NSJSONReadingMutableContainers error:nil ];

    // NOTE: 商品数据
    order.biz_content = [BizContent new];
    order.biz_content.body = bizContent[@"body"];
    order.biz_content.out_trade_no = bizContent[@"out_trade_no"];
    order.biz_content.product_code = bizContent[@"product_code"];
    order.biz_content.seller_id = bizContent[@"seller_id"];
    order.biz_content.subject = bizContent[@"subject"];
    order.biz_content.total_amount = bizContent[@"total_amount"];

    //将商品信息拼接成字符串
    NSString *orderInfo = [order orderInfoEncoded:NO];
    NSString *orderInfoEncoded = [order orderInfoEncoded:YES];

    // NOTE: 获取私钥并将商户信息签名，外部商户的加签过程请务必放在服务端，防止公私钥数据泄露；
    //       需要遵循RSA签名规范，并将签名字符串base64编码和UrlEncode
    RSADataSigner* signer = [[RSADataSigner alloc] initWithPrivateKey:rsaPrivateKey];
    NSString *signedString = [signer signString:orderInfo withRSA2:NO];

    // NOTE: 如果加签成功，则继续执行支付
    if (signedString != nil) {
        //应用注册scheme,在AliSDKDemo-Info.plist定义URL types
        NSString *appScheme = @"DEPUALIPAY";
        // NOTE: 将签名成功字符串格式化为订单字符串,请严格按照该格式
        NSString *orderString = [NSString stringWithFormat:@"%@&sign=%@",
                                 orderInfoEncoded, signedString];

        // NOTE: 调用支付结果开始支付
        [[AlipaySDK defaultService] payOrder:orderString fromScheme:appScheme callback:^(NSDictionary *resultDic) {
            //支付宝SDK支付成功
            if ([resultDic[@"resultStatus"]  isEqualToString:@"9000" ]) {
                if (self.delegate && [self.delegate respondsToSelector:@selector(aliApiManager:didPayedWithState:)]) {
                    [self.delegate aliApiManager:self didPayedWithState:ALiPay_State_Success];
                }
            }else{
                if (self.delegate && [self.delegate respondsToSelector:@selector(aliApiManager:didPayedWithState:)]) {
                    [self.delegate aliApiManager:self didPayedWithState:ALiPay_State_Fail];
                }
            }
        }];
    }
}


@end
```

* 8、如何使用？在需要使用支付的地方
* 让你的VC遵循AliPayManagerDelegate协议
* 调用支付方法

* 实现代理方法，  
  p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; font: 18.0px 'PingFang SC'; color: \#ffffff}  
  span.s1 {font-variant-ligatures: no-common-ligatures}  


  拿到支付状态，处理逻辑



