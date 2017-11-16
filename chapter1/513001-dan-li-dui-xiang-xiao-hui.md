# 单例对象销毁，还有这种需求？



> 前言：最近在开发一个功能，就是UIImageView播放图片再加上UIImageView本身的位移、旋转动画来实现一个复杂的动画。
>
> 因为这个动画的view是悬浮在UIWindow上的，所以我单例构造了一个UIImageView对象，且这个对象对外提供机构方法用来开始动画，暂停动画。等等因为我们的App具有篇章切换功能，所以当我测试动画的时候奇葩的事情发生了，动画变形了，确切的说也就是UIImageView对象的frame发生了意外改变





1、那么问题发生了，我排查了很久，由于时间问题没有深究为什么会改变frame，但是我本能告诉我这个对象销毁后再重新创建加载肯定是没有问题的。

2、顺着思路一看这货是个单例对象啊，我怎么销毁，我以前没有处理过啊。

3、后来再一想，在创建的时候将对象保存起来，对外暴露一个销毁tearDown的方法，将对象置为nil



```
#import <UIKit/UIKit.h>

#define XLRobotAnimationDuration 4


@class XLRobotImageView;

@protocol XLRobotImageViewDelegate<NSObject>

-(void)xlRobotImageView:(XLRobotImageView *)xlRobotImageView didClickToConsult:(UITapGestureRecognizer *)gesture;

@end

@interface XLRobotImageView : UIImageView

/**
 单例构造

 @return 返回单例实例
 */
+(XLRobotImageView *)sharedInstance;


/**
 销毁单例对象
 */

+(void)tearDown;
/**
 初始化小乐机器人动画及其位置

 @param duration 动画执行时间
 */
-(void)initiateAnimationWithDuration:(NSTimeInterval)duration;

/**
 停止动画，回归初始位置
 */
-(void)stopAnimation;

@property (nonatomic, weak) id<XLRobotImageViewDelegate> delegate;

@end


#import "XLRobotImageView.h"

@interface XLRobotImageView()

@property (nonatomic, assign) NSTimeInterval duration;

@property (nonatomic, assign) BOOL cancelAnimation;

@end

@implementation XLRobotImageView


static XLRobotImageView *instance = nil;

static dispatch_once_t onceToken;

+(XLRobotImageView *)sharedInstance{
    
    dispatch_once(&onceToken, ^{
        instance = [[self alloc] init];
        [instance initate];
    });

    return instance;
}



+(void)tearDown{
    
    instance = nil;
    onceToken = 0l;
}

-(void)initate{
    
    UITapGestureRecognizer *tapGesture = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(showAnimation:)];
    self.userInteractionEnabled = YES;
    [self addGestureRecognizer:tapGesture];

}

-(void)showAnimation:(UITapGestureRecognizer *)gesture{
    
    CGRect rect = self.frame;
    
    if (rect.origin.x == 20) {
        
        if (self.delegate && [self.delegate respondsToSelector:@selector(xlRobotImageView:didClickToConsult:)]) {
            [self.delegate xlRobotImageView:self didClickToConsult:gesture];
        }
    }else{
        [self startAnimationWithDuration:self.duration];
    }
}

-(void)initiateAnimationWithDuration:(NSTimeInterval)duration{
    
    self.duration = duration>0?duration:XLRobotAnimationDuration;
    
    self.frame = CGRectMake(20, BoundHeight/2 - 115/4, 36, 115/2);
    
    [self showRobotWithDuration:self.duration];
}


-(void)startAnimationWithDuration:(NSTimeInterval)duration{
    
    [UIView animateWithDuration:1 animations:^{
        self.transform = CGAffineTransformIdentity;
    }];
    
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        if (!self.cancelAnimation) {
             [self showRobotWithDuration:duration];
        }
    });
    
}


-(void)showRobotWithDuration:(NSTimeInterval)duration{
    
    //1、先播放眨眼睛、摆手动画
    NSMutableArray *images = [NSMutableArray array];
    for (NSUInteger index = 1; index < 10; index++) {
        NSString *imagename = [NSString stringWithFormat:@"XLRobot_%zd",index];
        [images addObject:[UIImage imageNamed:imagename]];
    }
    
    self.animationImages = images;
    self.animationRepeatCount = 0;
    self.animationDuration = 2;
    [self startAnimating];
    
    //2、暂停眨眼睛、摆手动画
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.1 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        if (!self.cancelAnimation) {
            [self stopAnimating];
            self.image = [UIImage imageNamed:@"XLRobot_9"];
        }
    });
    
    
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)( duration   * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        if (!self.cancelAnimation) {
            [UIView animateWithDuration:1 animations:^{
                self.transform = CGAffineTransformMakeTranslation(-28, 0);
                self.transform = CGAffineTransformRotate(self.transform, M_2_PI);
            }];
        }
    });
    
}

-(void)stopAnimation{
    [self stopAnimating];
    self.cancelAnimation = YES;
    self.image = [UIImage imageNamed:@"XLRobot_9"];
    self.transform = CGAffineTransformIdentity;
    self.transform = CGAffineTransformMakeTranslation(-28, 0);
    self.transform = CGAffineTransformRotate(self.transform, M_2_PI);
}


@end



```



