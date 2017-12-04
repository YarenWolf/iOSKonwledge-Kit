# 点击按钮给按钮底部出现阴影效果

```
#import <UIKit/UIKit.h>

@interface UIButton (ClickShadow)

@end


#import "UIButton+ClickShadow.h"

@implementation UIButton (ClickShadow)


-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{

    self.layer.shadowColor = [UIColor blackColor].CGColor;
    self.layer.shadowOpacity = 0.8f;
    self.layer.shadowRadius = 4.f;
    self.layer.shadowOffset = CGSizeMake(0, 0);
    [super touchesBegan:touches withEvent:event];
}

-(void)touchesEnded:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event{

    self.layer.shadowRadius = 0;
    self.layer.shadowColor = [UIColor clearColor].CGColor;
    self.layer.shadowOpacity = 0;
    self.layer.shadowOffset =  CGSizeMake(0, -3);
    [super touchesEnded:touches withEvent:event];
}

@end
```



