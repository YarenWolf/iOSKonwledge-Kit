##简短代码实现原生定位##


1、首先在info.plist文件中加入权限声明。请求用户获取定位能力
![](/assets/屏幕快照 2017-04-12 上午8.58.56.png)


2、在你需要定位的那个ViewController中导入头文件。


```
#import <CoreLocation/CoreLocation.h>
```

3、大体思路。

定位需要用户设备打开定位功能。这个可以根据这句代码判断。[CLLocationManager locationServicesEnables]如果为真则设备开启定位功能，否则没有开启。
判断用户是否为该应用设置允许定位可以根据CLLocationManagerDelegate的代理方法判断。具体见下面代码



```
#import "ChooseRegionViewController.h"
#import <CoreLocation/CoreLocation.h>
@interface ChooseRegionViewController ()<CLLocationManagerDelegate>

@end
@implementation ChooseRegionViewController

#pragma mark - life cycle
- (void)viewDidLoad {
    [super viewDidLoad];
    [self setupUI];
    [self autoLocate];
}

#pragma mark - privat method
-(void)setupUI{
    self.title = @"选择区域";
    NSString *city = [ProjectUtil getCity];
    [self.regionButton setTitle:city forState:UIControlStateNormal];
}

-(void)autoLocate{
    if ([CLLocationManager locationServicesEnabled]) {
        self.locationManager = [[CLLocationManager alloc] init];
        self.locationManager.delegate = self;
        [self.locationManager startUpdatingLocation];
    }
}
#pragma mark - CLLocationManagerDelegate
- (void)locationManager:(CLLocationManager *)manager
       didFailWithError:(NSError *)error{
    UIAlertController *alertVC = [UIAlertController alertControllerWithTitle:@"打开定位开关" message:@"定位服务未开启，请进入系统【设置】>【隐私】>【定位服务】中打开开关，并允许母子健康手册使用定位服务" preferredStyle:UIAlertControllerStyleAlert];
    UIAlertAction * ok = [UIAlertAction actionWithTitle:@"打开定位" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
        //打开定位设置
        NSURL *settingsURL = [NSURL URLWithString:UIApplicationOpenSettingsURLString];
        [[UIApplication sharedApplication] openURL:settingsURL];
    }];
    UIAlertAction * cancel = [UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action) {
        
    }];
    [alertVC addAction:cancel];
    [alertVC addAction:ok];
    [self presentViewController:alertVC animated:YES completion:nil];
}

-(void)locationManager:(CLLocationManager *)manager didUpdateLocations:(NSArray<CLLocation *> *)locations{
    [manager stopUpdatingLocation];
    CLLocation *currentLocation = [locations lastObject];
    CLGeocoder * geoCoder = [[CLGeocoder alloc] init];
    
     __block __typeof(self) weakSelf = self;
    //反编码
    [geoCoder reverseGeocodeLocation:currentLocation completionHandler:^(NSArray<CLPlacemark *> * _Nullable placemarks, NSError * _Nullable error) {
        if (placemarks.count > 0) {
            CLPlacemark *placeMark = placemarks[0];
            if (!placeMark.locality) {
                NSLog(@"无法获取到定位城市");
            }else{
                [ProjectUtil saveCity:placeMark.locality];
                [weakSelf.regionButton setTitle:[NSString stringWithFormat:@"%@版",[placeMark.locality stringByReplacingOccurrencesOfString:@"市" withString:@""]] forState:UIControlStateNormal];
                NSMutableDictionary *dic = [NSMutableDictionary dictionaryWithObject:[UIFont systemFontOfSize:17] forKey:NSFontAttributeName];
            }
        }
        else if (error == nil && placemarks.count == 0) {
            NSLog(@"No location and error return");
        }
        else if (error) {
            NSLog(@"location error: %@ ",error);
        }
        
    }];
}


@end

```



