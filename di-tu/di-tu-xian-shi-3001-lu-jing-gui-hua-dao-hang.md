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
    
    
2、在所需要的控制器里面加入地图显示用户当前定位、显示目标地址（用大头针标示出来）、路径规划导航（百度地图、高德地图、系统地图），详细说明见代码注释



```

//
//  HotelLocationMapVC.m
//  住哪儿
//
//  Created by geek on 2016/12/25.
//  Copyright © 2016年 geek. All rights reserved.
//

#import "HotelLocationMapVC.h"
#import <BaiduMapAPI_Map/BMKMapComponent.h>         //百度地图基本头文件
#import <BaiduMapAPI_Location/BMKLocationService.h> //百度地图定位头文件
#import <BaiduMapAPI_Search/BMKPoiSearch.h>         //百度地图搜索头文件
#import <MapKit/MapKit.h>                           //打开系统地图所需的头文件

@interface HotelLocationMapVC ()<BMKMapViewDelegate,BMKLocationServiceDelegate,BMKPoiSearchDelegate>
@property (nonatomic, strong) UIButton *myLodactionButton;      //我的位置按钮
@property (nonatomic, strong) UIButton *hotelLocationButton;    //酒店位置按钮
@property (nonatomic, strong) UIButton *navigationButton;       //开始导航按钮
@property (nonatomic, strong) BMKMapView *mapView;              //地图基本
@property (nonatomic, strong) BMKLocationService *locService;   //定位服务
@property (nonatomic, strong) BMKPoiSearch *poiSearch;          //搜索服务
@property (nonatomic, strong) NSMutableArray *dataArray;
@property (nonatomic, strong) UIAlertController *alertController;
@property (nonatomic,assign) CLLocationCoordinate2D coordinate;  // 要导航的坐标
@end

@implementation HotelLocationMapVC

#pragma mark - lice cyele
- (void)viewDidLoad {
    [super viewDidLoad];
    [self setupUI];
}

-(void)viewWillDisappear:(BOOL)animated{
    [super viewWillDisappear:animated];
    [SVProgressHUD dismiss];
    self.mapView.delegate = nil;
}

#pragma mark - private method
-(void)setupUI{
    self.title = @"酒店位置";
    self.view.backgroundColor = [UIColor whiteColor];
    
    [self.view addSubview:self.mapView];
    [self.locService startUserLocationService];
    
    [self.view addSubview:self.navigationButton];
    [self.view addSubview:self.myLodactionButton];
    [self.view addSubview:self.hotelLocationButton];
}

-(void)startNavigation{
    NSLog(@"开始导航");
    [self presentViewController:self.alertController animated:YES completion:nil];
}

-(void)myPosition{
    NSLog(@"我的位置");
}

-(void)hotelPosition{
    NSLog(@"酒店位置");
    
}

#pragma mark - BMKLocationServiceDelegate
- (void)didUpdateBMKUserLocation:(BMKUserLocation *)userLocation{
    self.mapView.showsUserLocation = YES;
    [self.mapView updateLocationData:userLocation];
    self.mapView.centerCoordinate = userLocation.location.coordinate;
    self.mapView.zoomLevel = 18;
    
    //周边云检索参数信息类
    BMKNearbySearchOption *option = [[BMKNearbySearchOption alloc] init];;
    
    option.pageIndex = 0;
    option.pageCapacity = 50;
    option.location = userLocation.location.coordinate;
    option.keyword = self.destination;
    
    BOOL flag = [self.poiSearch poiSearchNearBy:option];
    if (flag) {
        NSLog(@"搜索成功");
        [self.locService stopUserLocationService];
    }else{
        NSLog(@"搜索失败");
    }
    
}

#pragma mark - BMKPoiSearchDelegate
- (void)onGetPoiResult:(BMKPoiSearch*)searcher result:(BMKPoiResult*)poiResult errorCode:(BMKSearchErrorCode)errorCode{
    if (errorCode == BMK_SEARCH_NO_ERROR) {
        
        //只将搜索到的第一个点显示到界面上
        BMKPoiInfo *info = poiResult.poiInfoList.firstObject;
        //初始化一个点的注释
        BMKPointAnnotation *annotoation = [[BMKPointAnnotation alloc] init];
        self.coordinate = CLLocationCoordinate2DMake(info.pt.latitude, info.pt.longitude);
        annotoation.coordinate = info.pt;
        annotoation.title = info.name;
        annotoation.subtitle = self.destination;
        annotoation.subtitle = info.address;
        [self.mapView addAnnotation:annotoation];
        [self.mapView selectAnnotation:annotoation animated:YES];
        
        /*
         *将搜索到的所有结果显示出来
        for (BMKPoiInfo *info in poiResult.poiInfoList) {
            [self.dataArray addObject:info];
            
            //初始化一个点的注释
            BMKPointAnnotation *annotoation = [[BMKPointAnnotation alloc] init];
            annotoation.coordinate = info.pt;
            annotoation.title = info.name;
            annotoation.subtitle = self.destination;
            annotoation.subtitle = info.address;
            [self.mapView addAnnotation:annotoation];
            [self.mapView selectAnnotation:annotoation animated:YES];
        }
         */
    }
}

- (void)onGetPoiDetailResult:(BMKPoiSearch*)searcher result:(BMKPoiDetailResult*)poiDetailResult errorCode:(BMKSearchErrorCode)errorCode{
    NSLog(@"详情%@",poiDetailResult.name);
}

#pragma mark - BMKMapViewDelegate
- (BMKAnnotationView *)mapView:(BMKMapView *)mapView viewForAnnotation:(id <BMKAnnotation>)annotation{
    if ([annotation isKindOfClass:[BMKPointAnnotation class]]) {
        BMKPinAnnotationView *newAnnotation = [[BMKPinAnnotationView alloc] initWithAnnotation:annotation reuseIdentifier:@"an"];
        newAnnotation.pinColor = BMKPinAnnotationColorRed;
        newAnnotation.animatesDrop = YES;
        return newAnnotation;
    }
    return  nil;
}

- (void)mapView:(BMKMapView *)mapView didSelectAnnotationView:(BMKAnnotationView *)view {
    
    //poi详情检索信息类
    BMKPoiDetailSearchOption *option = [[BMKPoiDetailSearchOption alloc] init];
    
    BMKPoiInfo *info = self.dataArray.firstObject;
    
    //poi的uid，从poi检索返回的BMKPoiResult结构中获取
    option.poiUid = info.uid;
    
    BOOL flag = [self.poiSearch poiDetailSearch:option];
    
    if (flag) {
        NSLog(@"检索成功");
    }
    else {
        NSLog(@"检索失败");
    }
}

#pragma mark - lazy load
-(BMKMapView *)mapView{
    if (!_mapView) {
        _mapView = [[BMKMapView alloc] initWithFrame:CGRectMake(0, 0, BoundWidth, BoundHeight)];
        _mapView.showsUserLocation = YES;
        _mapView.showIndoorMapPoi = YES;
        _mapView.zoomLevel = 21;
        _mapView.rotateEnabled = NO;
        _mapView.delegate = self;
    }
    return _mapView;
}

-(BMKLocationService *)locService{
    if (!_locService) {
        _locService = [[BMKLocationService alloc] init];
        _locService.delegate = self;
    }
    return _locService;
}

-(BMKPoiSearch *)poiSearch{
    if (!_poiSearch) {
        _poiSearch = [[BMKPoiSearch alloc] init];
        _poiSearch.delegate = self;
    }
    return _poiSearch;
}

- (NSMutableArray *)dataArray {
    if (!_dataArray) {
        _dataArray = [NSMutableArray array];
    }
    return _dataArray;
}

-(UIButton *)navigationButton{
    if (!_navigationButton) {
        _navigationButton = [UIButton buttonWithType:UIButtonTypeCustom];
        _navigationButton.frame = CGRectMake(BoundWidth-120, BoundHeight-64-140, 100, 30);
        [_navigationButton setTitle:@"开始导航" forState:UIControlStateNormal];
        ;_navigationButton.titleLabel.font = [UIFont systemFontOfSize:14];
        [_navigationButton setTitleColor:[UIColor whiteColor] forState:UIControlStateNormal];
        _navigationButton.backgroundColor = [UIColor colorWithRed:0 green:0 blue:0 alpha:0.7];
        _navigationButton.layer.cornerRadius = 5;
        [_navigationButton clipsToBounds];
        [_navigationButton addTarget:self action:@selector(startNavigation) forControlEvents:UIControlEventTouchUpInside];
    }
    return _navigationButton;
}

-(UIButton *)myLodactionButton{
    if (!_myLodactionButton) {
        _myLodactionButton = [UIButton buttonWithType:UIButtonTypeCustom];
        _myLodactionButton.frame = CGRectMake(BoundWidth-120, BoundHeight-64-100, 100, 30);
        [_myLodactionButton setTitle:@"我的位置" forState:UIControlStateNormal];
        _myLodactionButton.titleLabel.font = [UIFont systemFontOfSize:14];
        [_myLodactionButton setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
        _myLodactionButton.backgroundColor = [UIColor whiteColor];
        _myLodactionButton.layer.cornerRadius = 5;
        [_myLodactionButton clipsToBounds];
        [_myLodactionButton addTarget:self action:@selector(myPosition) forControlEvents:UIControlEventTouchUpInside];
    }
    return _myLodactionButton;
}

-(UIButton *)hotelLocationButton{
    if (!_hotelLocationButton) {
        _hotelLocationButton = [UIButton buttonWithType:UIButtonTypeCustom];
        _hotelLocationButton.frame = CGRectMake(BoundWidth-120, BoundHeight-64-60, 100, 30);
        [_hotelLocationButton setTitle:@"酒店位置" forState:UIControlStateNormal];
        _hotelLocationButton.titleLabel.font = [UIFont systemFontOfSize:14];
        [_hotelLocationButton setTitleColor:[UIColor blackColor] forState:UIControlStateNormal];
        _hotelLocationButton.backgroundColor = [UIColor whiteColor];
        _hotelLocationButton.layer.cornerRadius = 5;
        [_hotelLocationButton clipsToBounds];
        [_hotelLocationButton addTarget:self action:@selector(hotelPosition) forControlEvents:UIControlEventTouchUpInside];
    }
    return _hotelLocationButton;
}

-(UIAlertController *)alertController{
    if (!_alertController) {
        _alertController = [UIAlertController alertControllerWithTitle:@"请选择地图" message:nil
                                                        preferredStyle:UIAlertControllerStyleActionSheet ];
        UIAlertAction *appleAction = [UIAlertAction actionWithTitle:@"苹果自带地图" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
            MKMapItem *currentLocation = [MKMapItem mapItemForCurrentLocation];
            MKMapItem *toLocation = [[MKMapItem alloc] initWithPlacemark:[[MKPlacemark alloc] initWithCoordinate:self.coordinate addressDictionary:nil]];
            toLocation.name = self.destination;
            [MKMapItem openMapsWithItems:@[currentLocation,toLocation] launchOptions:@{MKLaunchOptionsDirectionsModeKey:MKLaunchOptionsDirectionsModeDriving,
                                                                                       MKLaunchOptionsShowsTrafficKey:[NSNumber numberWithBool:YES]}];
            
            
        }];
        UIAlertAction *tecentAction = [UIAlertAction actionWithTitle:@"高德地图" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
            NSString *urlString = [[NSString stringWithFormat:@"iosamap://navi?sourceApplication=%@&backScheme=%@&poiname=%@&lat=%f&lon=%f&dev=0&style=2",@"住哪儿",@"YGche",self.destination,self.coordinate.latitude,self.coordinate.longitude] stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
            if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:urlString]]) {
                 [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlString]];
            }else{
                [SVProgressHUD showInfoWithStatus:@"您的手机未安装高德地图"];
            }
           
        }];
        UIAlertAction *baiduAction = [UIAlertAction actionWithTitle:@"百度地图" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
            NSString *urlString = [[NSString stringWithFormat:@"baidumap://map/direction?origin={{我的位置}}&destination=latlng:%f,%f|name:%@&mode=driving&coord_type=gcj02",self.coordinate.latitude,self.coordinate.longitude,self.destination] stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
            if ([[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlString]]) {
                 [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlString]];
            }else{
                 [SVProgressHUD showInfoWithStatus:@"您的手机未安装百度地图"];
            }
        }];
        UIAlertAction *cancelAction = [UIAlertAction actionWithTitle:@"退出" style:UIAlertActionStyleCancel handler:nil];
        [_alertController addAction:appleAction];
        [_alertController addAction:tecentAction];
        [_alertController  addAction:baiduAction];
        [_alertController addAction:cancelAction];
    }
    return _alertController;
}

@end

```
如果要看完整Demo的，传送门：[住哪儿App](https://github.com/FantasticLBP/Hotels)


    
    
    