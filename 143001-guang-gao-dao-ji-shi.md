# 广告倒计时

```
//LBPCircleProgressButton.h

#import <UIKit/UIKit.h>

typedef void(^DrawCircleProgressBlock)(void);

@interface LBPCircleProgressButton : UIButton

//track color
@property (nonatomic,strong)UIColor    *trackColor;

//progress color
@property (nonatomic,strong)UIColor    *progressColor;

//track background color
@property (nonatomic,strong)UIColor    *fillColor;

//progress line width
@property (nonatomic,assign)CGFloat    lineWidth;

//progress duration
@property (nonatomic,assign)CGFloat    animationDuration;


/**
 animation completed bloack

 @param duration <#duration description#>
 @param block <#block description#>
 */
- (void)startAnimationDuration:(CGFloat)duration withBlock:(DrawCircleProgressBlock )block;


@end

//LBPCircleProgressButton.m
#import "LBPCircleProgressButton.h"

#define degreesToRadius(x) ((x) * M_PI / 180.0)

@interface LBPCircleProgressButton()<CAAnimationDelegate>

@property (nonatomic, strong) CAShapeLayer *trackLayer;
@property (nonatomic, strong) CAShapeLayer *progressLayer;
@property (nonatomic, strong) UIBezierPath *bezierPath;
@property (nonatomic, copy)   DrawCircleProgressBlock myBlock;

@end

@implementation LBPCircleProgressButton

- (instancetype)initWithFrame:(CGRect)frame
{
    if (self == [super initWithFrame:frame]) {
        self.backgroundColor = [UIColor clearColor];

        [self.layer addSublayer:self.trackLayer];

    }
    return self;
}

- (UIBezierPath *)bezierPath
{
    if (!_bezierPath) {

        CGFloat width = CGRectGetWidth(self.frame)/2.0f;
        CGFloat height = CGRectGetHeight(self.frame)/2.0f;
        CGPoint centerPoint = CGPointMake(width, height);
        float radius = CGRectGetWidth(self.frame)/2;

        _bezierPath = [UIBezierPath bezierPathWithArcCenter:centerPoint
                                                     radius:radius
                                                 startAngle:degreesToRadius(-90)
                                                   endAngle:degreesToRadius(270)
                                                  clockwise:YES];
    }
    return _bezierPath;
}

- (CAShapeLayer *)trackLayer
{
    if (!_trackLayer) {
        _trackLayer = [CAShapeLayer layer];
        _trackLayer.frame = self.bounds;
        _trackLayer.fillColor = self.fillColor.CGColor ? self.fillColor.CGColor : [UIColor colorWithRed:0.0/255.0 green:0.0/255.0 blue:0.0/255.0 alpha:0.3].CGColor ;
        _trackLayer.lineWidth = self.lineWidth ? self.lineWidth : 2.f;
        _trackLayer.strokeColor = self.trackColor.CGColor ? self.trackColor.CGColor : [UIColor redColor].CGColor ;
        _trackLayer.strokeStart = 0.f;
        _trackLayer.strokeEnd = 1.f;

        _trackLayer.path = self.bezierPath.CGPath;
    }
    return _trackLayer;
}

- (CAShapeLayer *)progressLayer
{
    if (!_progressLayer) {
        _progressLayer = [CAShapeLayer layer];
        _progressLayer.frame = self.bounds;
        _progressLayer.fillColor = [UIColor clearColor].CGColor;
        _progressLayer.lineWidth = self.lineWidth ? self.lineWidth : 2.f;
        _progressLayer.lineCap = kCALineCapRound;
        _progressLayer.strokeColor = self.progressColor.CGColor ? self.progressColor.CGColor  : [UIColor grayColor].CGColor;
        _progressLayer.strokeStart = 0.f;

        CABasicAnimation *pathAnimation = [CABasicAnimation animationWithKeyPath:@"strokeEnd"];
        pathAnimation.duration = self.animationDuration;
        pathAnimation.fromValue = @(0.0);
        pathAnimation.toValue = @(1.0);
        pathAnimation.removedOnCompletion = YES;
        pathAnimation.delegate = self;
        [_progressLayer addAnimation:pathAnimation forKey:nil];

        _progressLayer.path = _bezierPath.CGPath;
    }
    return _progressLayer;
}

#pragma mark -- CAAnimationDelegate
- (void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag{
    if (flag) {
        self.myBlock();
    }
}

#pragma mark ---
- (void)startAnimationDuration:(CGFloat)duration withBlock:(DrawCircleProgressBlock )block{
    self.myBlock = block;
    self.animationDuration = duration;
    [self.layer addSublayer:self.progressLayer];
}


@end

//AdvertisementView
#import <UIKit/UIKit.h>
#import "LBPCircleProgressButton.h"

typedef void(^AnimationCompleted)(BOOL flag);

@interface AdvertisementView : UIView

//背景图片
@property (nonatomic, strong) UIImage *image;

//动画完成回调
@property (nonatomic, copy) AnimationCompleted completedBlock;

-(instancetype)initWithFrame:(CGRect)frame;

//开始倒计时
-(void)startAnimationWithSeconds:(int)seconds;

@end



//#import "AdvertisementView.h"

@interface AdvertisementView()
@property (nonatomic, strong) UIImageView *imageView;
@property (nonatomic, strong) LBPCircleProgressButton *button;
@property (nonatomic, strong) NSTimer *timer;
@property (nonatomic, assign) int seconds;
@property (nonatomic, assign) int currentSeconds;

@end

@implementation AdvertisementView

-(instancetype)initWithFrame:(CGRect)frame{
    if (self = [super initWithFrame:frame]) {
        [self setupUI];
    }
    return self;
}

-(void)setupUI{
    self.backgroundColor = [UIColor whiteColor];
    [self addSubview:self.imageView];
    [self addSubview:self.button];
}

-(void)startAnimationWithSeconds:(int)seconds{
    UIWindow *window = [UIApplication sharedApplication].keyWindow;
    [window addSubview:self];
    __weak AdvertisementView *weakSelf = self;
    self.seconds = seconds;
    if (self.currentSeconds == 0) {
        self.currentSeconds = self.seconds;
    }
    [self.button setTitle:[NSString stringWithFormat:@"跳过\n  %ds  ",self.currentSeconds] forState:UIControlStateNormal];
    self.timer = [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(secondsCount) userInfo:nil repeats:YES];
    [self.button startAnimationDuration:5 withBlock:^{
        [weakSelf removeFromSuperview];
        [weakSelf removeProgress];
    }];
}

-(void)removeProgress{
    self.imageView.transform = CGAffineTransformMakeScale(1, 1);
    self.imageView.alpha = 1;
    
    self.button.transform = CGAffineTransformMakeScale(1, 1);
    self.button.alpha = 1;
    
    [UIView animateWithDuration:0.7 animations:^{
        self.imageView.alpha = 0;
        self.imageView.transform = CGAffineTransformMakeScale(5, 5);
        self.button.alpha = 0;
        self.button.transform = CGAffineTransformMakeScale(1, 1);
    } completion:^(BOOL finished) {
        [self.button removeFromSuperview];
        [self.imageView removeFromSuperview];
        [self removeFromSuperview];
        if (self.completedBlock) {
            self.completedBlock(YES);
        }
    }];
}

- (void)secondsCount{
    if(self.currentSeconds > 0){
        self.currentSeconds--;
    }
    [self.button setTitle:[NSString stringWithFormat:@"跳过\n  %ds  ",self.currentSeconds] forState:UIControlStateNormal];
}

#pragma mark -- setter
-(void)setImage:(UIImage *)image{
    _image = image;
    self.imageView.image = image;
}

#pragma mark -- lazy load
-(UIImageView *)imageView{
    if (!_imageView) {
        _imageView = [[UIImageView alloc] initWithFrame:CGRectMake(0, 0, BoundWidth, BoundHeight)];
    }
    return _imageView;
}

-(LBPCircleProgressButton *)button{
    if (!_button) {
        _button = [[LBPCircleProgressButton alloc] initWithFrame:CGRectMake(BoundWidth - 60, 30, 40, 40)];
        _button.lineWidth = 2;
        [_button setTitleColor:[UIColor whiteColor] forState:UIControlStateNormal];
        _button.titleLabel.font = [UIFont systemFontOfSize:12];
        _button.titleLabel.numberOfLines  = 2;
        [_button addTarget:self action:@selector(removeProgress) forControlEvents:UIControlEventTouchUpInside];
    }
    return _button;
}

@end

//test
#import "AViewController.h"
#import "AdvertisementView.h"

@interface AViewController ()
@property (nonatomic, strong) AdvertisementView *adView;

@end

@implementation AViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    self.title = @"倒计时动画";
    [self.adView startAnimationWithSeconds:5];
}



#pragma mark -- lazy load
-(AdvertisementView *)adView{
    if (!_adView) {
        _adView = [[AdvertisementView alloc] initWithFrame:[UIScreen mainScreen].bounds];
        _adView.image = [UIImage imageNamed:@"secondsCounting"];
    }
    return _adView;
}

@end

```



