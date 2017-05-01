###摇一摇根据城市位置推荐酒店###

1、实现摇一摇并震动需要导入头文件。#import <AudioToolbox/AudioToolbox.h>
2、当前城市定位，可以看我之前的文字[快速定位](https://my.oschina.net/u/1778933/blog/884823)
3、让vc支持摇一摇。

```
    [self becomeFirstResponder];
    [UIApplication sharedApplication].applicationSupportsShakeToEdit = YES;
```

4、关键代码
在移动的时候将手机震动，并将view显示出来，并请求接口，将酒店显示出来，点击进入到酒店详情界面。

```
#pragma mark - UIResponder
-(void)motionBegan:(UIEventSubtype)motion withEvent:(UIEvent *)event{
    AudioServicesPlaySystemSound(kSystemSoundID_Vibrate);
    self.bgView.alpha = 0;
    self.bgView.hidden = YES;
    [UIView animateWithDuration:1.0 animations:^{
        [self getRandomHotel];
        self.bgView.alpha = 1;
        self.bgView.hidden = NO;
        self.hotelImage.image = [UIImage imageNamed:@"My_about"];
        self.label.text = @"您已经成功摇到一个酒店，不喜欢？换个姿势再来一次";
        [self.label sizeToFit];
        
        self.hotelImage.contentMode =  UIViewContentModeScaleAspectFill;
        self.hotelImage.autoresizingMask = UIViewAutoresizingFlexibleHeight;
        self.hotelImage.clipsToBounds  = YES;
        
        //动态设置uilabel的高度
        self.hotelLabel.numberOfLines = 0;
        self.hotelLabel.lineBreakMode = NSLineBreakByWordWrapping;
        
    } completion:^(BOOL finished) {

    }];
    NSLog(@"摇一摇开始");
    return ;
}

-(void)motionCancelled:(UIEventSubtype)motion withEvent:(UIEvent *)event{
    NSLog(@"取消摇一摇");
    return ;
}

-(void)motionEnded:(UIEventSubtype)motion withEvent:(UIEvent *)event{
    if (motion ==UIEventSubtypeMotionShake ){
        NSLog(@"摇一摇结束");
    }
    return ;
}
```

全部代码：


```

//
//  ShakeViewController.m
//  住哪儿
//
//  Created by geek on 2017/4/30.
//  Copyright © 2017年 geek. All rights reserved.
//

#import "ShakeViewController.h"
#import "HotelDetailVC.h"
#import "HotelsModel.h"
#import "LocationManager.h"
#import <AudioToolbox/AudioToolbox.h>

@interface ShakeViewController ()<LocationManagerDelegate>
@property (nonatomic, strong) UIImageView *imageView;
@property (nonatomic, strong) UILabel *label;
@property (nonatomic, strong) UIView *bgView;
@property (nonatomic, strong) UILabel *hotelLabel;
@property (nonatomic, strong) UIImageView *hotelImage;
@property (nonatomic, strong) HotelsModel *model;
@property (nonatomic, strong) LocationManager *locationManager;
@property (nonatomic, strong) NSString *cityName;
@property (nonatomic, assign) BOOL showHotel;
@end

@implementation ShakeViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    [self setupUI];
    [self autoLocate];
}

- (void)setupUI {
    [self.view addSubview:self.imageView];
    [self becomeFirstResponder];
    [UIApplication sharedApplication].applicationSupportsShakeToEdit = YES;
    
    [self.view addSubview:self.label];
    [self.view addSubview:self.bgView];
    [self.bgView addSubview:self.hotelImage];
    [self.bgView addSubview:self.hotelLabel];
}

-(void)autoLocate{
    self.locationManager = [LocationManager sharedInstance];
    self.locationManager.delegate =  self;
    [self.locationManager autoLocate];
}

-(void)getRandomHotel{
    NSString *url = [NSString stringWithFormat:@"%@%@",Base_Url,@"/controller/api/RandomHotel.php"];
    NSMutableDictionary *paras = [NSMutableDictionary dictionary];
    paras[@"key"] = AppKey;
    paras[@"city"] = self.cityName;
    
    [SVProgressHUD showWithStatus:@"正在获取酒店数据"];
    [AFNetPackage getJSONWithUrl:url parameters:paras success:^(id responseObject) {
        NSDictionary *dic = [NSJSONSerialization JSONObjectWithData:responseObject options:NSJSONReadingMutableLeaves error:nil];
        if ([dic[@"code"] integerValue] == 200) {
            [SVProgressHUD dismiss];
            NSDictionary *dic = [NSJSONSerialization JSONObjectWithData:responseObject options:NSJSONReadingMutableLeaves error:nil];
            self.model = [HotelsModel yy_modelWithJSON:dic[@"data"]];
            self.hotelLabel.text = self.model.hotelName;
            [self.hotelLabel sizeToFit];
            [self.hotelImage sd_setImageWithURL:[NSURL URLWithString: [NSString stringWithFormat:@"%@/%@",Base_Url,self.model.image1]] placeholderImage:[UIImage imageNamed:@"jpg-1"]];
        }
    } fail:^{
        [SVProgressHUD dismiss];
    }];
}

-(void)watchDetail{
    HotelDetailVC *vc = [[HotelDetailVC alloc] init];
    vc.startPeriod = [[NSDate date] todayString];
    vc.leavePerios = [[NSDate date] GetTomorrowDayString];
    vc.model = self.model;
    [self.navigationController pushViewController:vc animated:YES];
}

#pragma mark - LocationManagerDelegate
-(void)locationManager:(LocationManager *)locationManager didGotLocation:(NSString *)location{
    self.cityName = location;
}

#pragma mark - UIResponder
-(void)motionBegan:(UIEventSubtype)motion withEvent:(UIEvent *)event{
    AudioServicesPlaySystemSound(kSystemSoundID_Vibrate);
    self.bgView.alpha = 0;
    self.bgView.hidden = YES;
    [UIView animateWithDuration:1.0 animations:^{
        [self getRandomHotel];
        self.bgView.alpha = 1;
        self.bgView.hidden = NO;
        self.hotelImage.image = [UIImage imageNamed:@"My_about"];
        self.label.text = @"您已经成功摇到一个酒店，不喜欢？换个姿势再来一次";
        [self.label sizeToFit];
        
        self.hotelImage.contentMode =  UIViewContentModeScaleAspectFill;
        self.hotelImage.autoresizingMask = UIViewAutoresizingFlexibleHeight;
        self.hotelImage.clipsToBounds  = YES;
        
        //动态设置uilabel的高度
        self.hotelLabel.numberOfLines = 0;
        self.hotelLabel.lineBreakMode = NSLineBreakByWordWrapping;
        
    } completion:^(BOOL finished) {

    }];
    NSLog(@"摇一摇开始");
    return ;
}

-(void)motionCancelled:(UIEventSubtype)motion withEvent:(UIEvent *)event{
    NSLog(@"取消摇一摇");
    return ;
}

-(void)motionEnded:(UIEventSubtype)motion withEvent:(UIEvent *)event{
    if (motion ==UIEventSubtypeMotionShake ){
        NSLog(@"摇一摇结束");
    }
    return ;
}

#pragma mark - lazy load
-(UIImageView *)imageView{
    if (!_imageView) {
        _imageView = [[UIImageView alloc] initWithFrame:CGRectMake(0, 0, BoundWidth, BoundHeight-60)];
        _imageView.image = [UIImage imageNamed:@"shake_news_bgVPic"];
    }
    return _imageView;
}

-(UILabel *)label{
    if (!_label) {
        _label = [[UILabel alloc] initWithFrame:CGRectMake(BoundWidth/2-200/2, BoundHeight - 60 -270, 200, 21)];
        _label.textColor = [UIColor whiteColor];
        _label.numberOfLines = 2;
        _label.font = [UIFont systemFontOfSize:15];
        _label.text = @"摇一摇，为您随机推荐酒店";
        [_label sizeToFit];
    }
    return _label;
}

-(UIView *)bgView{
    if (!_bgView) {
        _bgView = [[UIView alloc] initWithFrame:CGRectMake(15, BoundHeight-260, BoundWidth-30, 120)];
        _bgView.backgroundColor = [UIColor whiteColor];
        _bgView.layer.cornerRadius = 10;
        _bgView.clipsToBounds = YES;
        _bgView.alpha = 0;
        
        UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(watchDetail)];
        tap.cancelsTouchesInView = YES;
        _bgView.userInteractionEnabled = YES;
        [_bgView addGestureRecognizer:tap];
    }
    return _bgView;
}

-(UIImageView *)hotelImage{
    if (!_hotelImage) {
        _hotelImage = [[UIImageView alloc] initWithFrame:CGRectMake(10, 10, 100, 100)];
        _hotelImage.contentMode = UIViewContentModeScaleAspectFit;
    }
    return _hotelImage;
}

-(UILabel *)hotelLabel{
    if (!_hotelLabel) {
        _hotelLabel = [[UILabel alloc] initWithFrame:CGRectMake(135, 40, BoundWidth - 30- 140, 40)];
        _hotelLabel.textColor = [UIColor blackColor];
        _hotelLabel.font = [UIFont systemFontOfSize:20];
    }
    return _hotelLabel;
}
@end


```



效果图
![](/assets/IMG_5735.PNG)
![](/assets/IMG_5736.PNG)


###随机推荐酒店后台实现###

    实现思路：
    1、接收客户端传来的参数：城市名称、App key；
    2、根据城市名称模糊搜索出酒店数据；
    3、根据搜索出的酒店数据数组长度，生成1个随机数，随机数范围[1,数组长度]；    4、利用封装好的Responese将数据返给客户端

```
<?php

/**
 * Created by PhpStorm.
 * User: geek
 * Date: 2017/3/9
 * Time: 上午9:24
 */

header('content-type:text.html;charset=utf-8');
error_reporting(0);
require_once '../../model/PdoMySQL.class.php';
require_once '../../model/config.php';
require_once 'Response.php';


class RandomHotel
{
    private  $tableName = "hotel";
    private  $key = "";
    private  $city = "";

    protected static  $_instance = null;

    private  function  __construct()
    {
    }

    private  function  __clone()
    {
        // TODO: Implement __clone() method.
    }

    public function  sharedInstance(){
        if(self::$_instance == null){
            self::$_instance = new self();
        }
        return self::$_instance;
    }



    private function random($start,$end){
        $tmp=array();
        while(count($tmp)<1){
            $tmp[]=mt_rand($start,$end);
            $tmp=array_unique($tmp);
        }
        return $tmp[0]-1;
    }

    public function getHotel(){
        $mysqlPdo = new PdoMySQL();

        self.$this->key = $_REQUEST["key"];
        self.$this->city = $_REQUEST["city"];

        if($this->key == "" || $this->key !== "TheHotelReversationApplication" ){
            Response::show(201,"fail","非安全的数据请求","json");
        }

        $pdo=new PdoMySQL();
        $res = $pdo->find($this->tableName,"address like '%".$this->city."%'");

        $random = $this->random(1,count($res));

        if($res){
            //随机酒店获取成功
            Response::show(200,"随机酒店获取成功",$res[$random],"json");
        }else{
            //随机酒店获取失败
            Response::show(201,"随机酒店获取失败","json");
        }
    }
}

$hotel = RandomHotel::sharedInstance();
$hotel->getHotel();
?>
```

想看完整项目献上传送门：[住哪儿客户端](https://github.com/FantasticLBP/Hotels)、：[住哪儿服务端](https://github.com/FantasticLBP/Hotels_Server)




