```
##简短代码实现原生定位##
```



    1、首先在info.plist文件中加入权限声明。请求用户获取定位能力
    ![](/assets/屏幕快照 2017-04-12 上午8.58.56.png)

    2、大体思路。

    定位需要用户设备打开定位功能。这个可以根据这句代码判断。[CLLocationManager locationServicesEnables]如果为真则设备开启定位功能，否则没有开启。
    判断用户是否为该应用设置允许定位可以根据CLLocationManagerDelegate的代理方法判断。具体见下面代码



    ```
    #import <Foundation/Foundation.h>
    @class LocationManager;
    @protocol LocationManagerDelegate <NSObject>

    -(void)locationManager:(LocationManager *)locationManager didGotLocation:(NSString *)location;

    @end

    @interface LocationManager : NSObject

    @property (nonatomic, assign) id<LocationManagerDelegate> delegate;

    /**
     * 单例模式实例化对象
     */
    +(LocationManager *)sharedInstance;
    /**
     * 开始定位
     */
    -(void)autoLocate;
    @end


    #import "LocationManager.h"
    #import <CoreLocation/CoreLocation.h>

    @interface LocationManager()<CLLocationManagerDelegate>
    @property (nonatomic, strong) CLLocationManager *locationManager;
    @end

    @implementation LocationManager

    +(LocationManager *)sharedInstance{
        static LocationManager *instance = nil;
        static dispatch_once_t predict;
        dispatch_once(&predict, ^{
            instance = [[self alloc] init];
        });
        return instance;
    }

    #pragma mark - private method
    -(void)autoLocate{
        if ([CLLocationManager locationServicesEnabled]) {
            self.locationManager = [[CLLocationManager alloc] init];
            self.locationManager.delegate = self;
            [self.locationManager startUpdatingLocation];
            if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 8.0){
                [self.locationManager requestWhenInUseAuthorization];
            }
        }
    }


    #pragma mark - CLLocationManagerDelegate

    -(void)locationManager:(CLLocationManager *)manager didFailWithError:(NSError *)error{
        UIAlertController *alertVC = [UIAlertController alertControllerWithTitle:@"打开定位开关" message:@"定位服务未开启，请进入系统【设置】>【隐私】>【定位服务】中打开开关，并允许母子健康手册使用定位服务" preferredStyle:UIAlertControllerStyleAlert];
        UIAlertAction * ok = [UIAlertAction actionWithTitle:@"打开定位" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
            //打开该App的权限设置
            NSURL *settingsURL = [NSURL URLWithString:UIApplicationOpenSettingsURLString];
            [[UIApplication sharedApplication] openURL:settingsURL];
        }];
        UIAlertAction * cancel = [UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:^(UIAlertAction * _Nonnull action) {

        }];
        [alertVC addAction:cancel];
        [alertVC addAction:ok];
        [[UIApplication sharedApplication].keyWindow.rootViewController presentViewController:alertVC animated:YES completion:nil];
    }

    -(void)locationManager:(CLLocationManager *)manager didUpdateLocations:(NSArray<CLLocation *> *)locations{
        [manager stopUpdatingLocation];
        CLLocation *currentLocation = [locations lastObject];
        CLGeocoder *geocoder = [[CLGeocoder alloc] init];
        [geocoder reverseGeocodeLocation:currentLocation completionHandler:^(NSArray<CLPlacemark *> * _Nullable placemarks, NSError * _Nullable error) {
            CLPlacemark *place = placemarks[0];
            if (self.delegate && [self.delegate respondsToSelector:@selector(locationManager:didGotLocation:)]) {
                [self.delegate locationManager:self didGotLocation:place.locality];
            }
        }];
    }

    @end

    ```



    ###How to use###

    1、在你的文件中导入#import "LocationManager.h"
    2、遵循LocationManagerDelegate协议<LocationManagerDelegate>
    3、定义属性。@property (nonatomic, strong) LocationManager *locationManager;
    4、触发定位事件

    ```
    //自动定位
    -(void)autoLocate{
    self.locationManager = [LocationManager sharedInstance];
    self.locationManager.delegate = self;
    [self.locationManager autoLocate];
    }

    ```

    5、在代理方法中拿到定位城市，更新UI

    ```
    #pragma mark - LocationManagerDelegate
    -(void)locationManager:(LocationManager *)locationManager didGotLocation:(NSString *)location{
        self.conditionView.cityName = location;
    }

    ```











