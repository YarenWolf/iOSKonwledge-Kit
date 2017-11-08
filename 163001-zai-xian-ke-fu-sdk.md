# 在线客服-UDESK



http://www.udesk.cn/doc/ios/udesksdk\_ios/\#sdk\_3

接入步骤：

* 导入头文件 \#import "Udesk.h"

* 触发事件

  ```
  -(void)jumpToChat{
    
      UdeskOrganization *organization = [[UdeskOrganization alloc] initWithDomain:UDeskDomain appKey:UDeskAppKey appId:UDeskAppId];
      NSString *sdk_token = [NSString stringWithFormat:@"%u",arc4random()];

      UdeskCustomer *customer = [UdeskCustomer new];
      customer.sdkToken = [UserManager getUserObject].huanxinId;
      customer.nickName = [UserManager getUserObject].name;
      [UdeskManager initWithOrganization:organization customer:customer];
    

      UdeskSDKStyle *sdkStyle = [UdeskSDKStyle customStyle];
      sdkStyle.navigationColor = GlobalMainColor;
      sdkStyle.titleColor = [UIColor whiteColor];
      sdkStyle.navBackButtonColor = [UIColor whiteColor];
      UdeskSDKManager *chat = [[UdeskSDKManager alloc] initWithSDKStyle:sdkStyle];
      [chat pushUdeskInViewController:self completion:nil];
  }
  ```





