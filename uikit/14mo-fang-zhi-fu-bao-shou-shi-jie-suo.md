# 模仿支付宝手势解锁

> 支付宝的手势解锁主要利用图形绘制和手势2个知识点即可完成
>
> c语言声明的枚举值是以k开头的

主要思路：

* 手指滑过屏幕上的按钮有颜色变化，这个主要通过给按钮设置2种不同状态下的背景图片即可实现
* 滑动过程中判断当前的手势是否在按钮区域内，这个可以通过CGRectContainsPoint实现。返回一个布尔值
* 因为要添加滑动手势因此将按钮的userInteractionEnabled设置为NO
* 在手势移动出发的touchesMoved方法中判断当前手势的点是否在按钮区域内，如果在，声明一个全局可变数组，将该按钮添加到数组中
* 添加到数组中去调用setNeedsDisplay方法开始绘图
* 在手势结束的时候清空数组

```
//控制器的view（自定义view）

#import <UIKit/UIKit.h>


@interface VCView : UIView

@end


#import "VCView.h"
#import "GestureView.h"

@interface VCView()

@property (nonatomic, strong) GestureView *bgView;

@end

@implementation VCView

-(void)drawRect:(CGRect)rect{
    UIImage *image = [UIImage imageNamed:@"Home_refresh_bg"];
    [image drawInRect:rect];
}


-(void)awakeFromNib{
    [super awakeFromNib];
    [self addSubview:self.bgView];
}

-(void)layoutSubviews{
    [super layoutSubviews];
    self.bgView.frame = CGRectMake((self.frame.size.width - BGViewWidth)/2, (self.frame.size.height - BGViewWidth)/2, BGViewWidth, BGViewWidth);
}



#pragma mark -- lazy load

-(GestureView *)bgView{
    if (!_bgView) {
        _bgView = [GestureView new];
        _bgView.backgroundColor = [UIColor clearColor];
    }
    return _bgView;
}

@end


//GestureView（手势view）

#import <UIKit/UIKit.h>
#define BGViewWidth 300

@interface GestureView : UIView

@end

#import "GestureView.h"

//c语言的枚举值需要以K开头
@interface GestureView()

@property (nonatomic, strong) NSMutableArray *selectedButttons;

@property (nonatomic, assign) CGPoint freePoint;

@end

@implementation GestureView


-(instancetype)initWithFrame:(CGRect)frame{
    if (self = [super initWithFrame:frame]) {
        [self setupUI];
    }
    return self;
}

-(void)setupUI{
    CGFloat btnWH = 80;
    int columns = 3;
    int rows = 3;


    for (int i=0; i<9; i++) {
        UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
        CGFloat x = (i%3)*btnWH +  (i%3)*((BGViewWidth- columns * btnWH)/2) ;
        CGFloat y = (i/3)*btnWH +  (i/3)*((BGViewWidth- rows * btnWH)/2) ;
        button.frame = CGRectMake(x, y, btnWH, btnWH);
        button.tag = i;
        [button setImage:[UIImage imageNamed:@"gesture_node_normal"] forState:UIControlStateNormal];
        [button setImage:[UIImage imageNamed:@"gesture_node_highlighted"] forState:UIControlStateSelected];
        button.userInteractionEnabled = NO;
        [self
         addSubview:button];
    }


}


-(void)awakeFromNib{
    [super awakeFromNib];
    [self setupUI];
}

-(void)drawRect:(CGRect)rect{
    UIBezierPath *path = [UIBezierPath bezierPath];

    if (self.selectedButttons.count <= 0) {
        return ;
    }
    for (NSUInteger index = 0; index < self.selectedButttons.count; index++) {
        UIButton *button = self.selectedButttons[index];
        if (index == 0) {
            [path moveToPoint:button.center];
        }else{
            [path addLineToPoint:button.center];
        }
    }
    [path addLineToPoint:self.freePoint];

    [[UIColor greenColor] set];
    [path setLineWidth:10];
    [path setLineJoinStyle:kCGLineJoinRound];
    [path stroke];
}




//需求：手指移到按钮上或滑过的时候按钮要有颜色区分
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    CGPoint currentPoint = [self touchPointInCurrentView:touches];
    UIButton *button = [self btnRectContainsPoint:currentPoint];
    if (button) {
        button.selected = YES;
        [self.selectedButttons addObject:button];
    }
}

-(void)touchesMoved:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    CGPoint currentPoint = [self touchPointInCurrentView:touches];
    UIButton *button = [self btnRectContainsPoint:currentPoint];
    self.freePoint = currentPoint;
    if (button && !button.selected) {
        button.selected = YES;
        [self.selectedButttons addObject:button];
    }
    [self setNeedsDisplay];
}

-(void)touchesEnded:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{
    NSMutableString *string = [NSMutableString string];
    for (UIButton *button in self.selectedButttons) {
        [string appendString:[NSString stringWithFormat:@"%zd",button.tag]];
        button.selected = NO;
    }
    NSLog(@"选中的是->%@",string);
    [self.selectedButttons removeAllObjects];
    [self setNeedsDisplay];
}

#pragma mark -- private method

//拆分函数：按照函数的功能划分

-(CGPoint)touchPointInCurrentView:(NSSet *)touches{
    UITouch *touch = [touches anyObject];
    return [touch locationInView:self];
}

-(UIButton *)btnRectContainsPoint:(CGPoint)point{
    for (UIButton *button in self.subviews) {
        if (CGRectContainsPoint(button.frame, point)) {
            return button;
        }
    }
    return nil;
}

#pragma mark -- lazy load

-(NSMutableArray *)selectedButttons{
    if (!_selectedButttons) {
        _selectedButttons = [NSMutableArray array];
    }
    return _selectedButttons;
}


@end
```



