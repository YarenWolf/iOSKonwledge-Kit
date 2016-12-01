###自定义picker思路：自定义view继承自UIView。封装内部实现，定力协议，将用户滚动picker时选中的数据实时传递到ViewController中。
    直接上干货。WeightAndHeightPickerView实现选择身高、体重并计算BMI。
```
#import <UIKit/UIKit.h>
@class WeightAndHeightPickerView;
@protocol WeightAndHeightPickerViewDelegate <NSObject>
-(void)weightAndHeightPickerView:(WeightAndHeightPickerView *)weightAndHeightPickerView didChooseWeightAndHeight:(NSDictionary *)weightAndHeightDic;
@end
@interface WeightAndHeightPickerView : UIView
@property (nonatomic ,weak) id<WeightAndHeightPickerViewDelegate> delegate; /** 实现点击按钮代理*/
@end
```