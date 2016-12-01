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

```
#import "WeightAndHeightPickerView.h"



@interface WeightAndHeightPickerView ()<UIPickerViewDelegate,UIPickerViewDataSource>

@property (nonatomic ,strong) UIPickerView * weightAndHeightPickerView;/*< 选择器*/

@property (nonatomic ,strong) NSMutableArray * heights;

@property (nonatomic ,strong) NSMutableArray * weights;

@property (nonatomic, strong) NSString *selectHeight;

@property (nonatomic, strong) NSString *selectWeight;

@end



@implementation WeightAndHeightPickerView



- (instancetype)initWithFrame:(CGRect)frame{

 self = [super initWithFrame:frame];

 if (self) {

 [self loadData];

 [self loadPickerView];

 }

 return self;

}



#pragma mark 加载PickerView

- (void)loadPickerView{

 [self addSubview:self.weightAndHeightPickerView];

 [self.weightAndHeightPickerView selectRow:165 inComponent:1 animated:YES];

 [self.weightAndHeightPickerView selectRow:58 inComponent:4 animated:YES];

}



#pragma mark - 加载数据

- (void)loadData{

 for (int i = 0; i<=250;i++) {

 [self.heights addObject:@(i)];

 }

 for (int i = 0; i<=200;i++) {

 [self.weights addObject:@(i)];

 }

 self.selectWeight = @"58.0";

 self.selectHeight = @"165.0";

 [self.weightAndHeightPickerView reloadAllComponents];

}



#pragma mark - UIPickerDatasource

- (NSInteger)numberOfComponentsInPickerView:(UIPickerView *)pickerView{

 return 6;

}



- (NSInteger)pickerView:(UIPickerView *)pickerView

numberOfRowsInComponent:(NSInteger)component{

 if (1 == component) {

 return self.heights.count;

 }else if(4 == component){

 return self.weights.count;

 }else{

 return 1;

 }

}



- (CGFloat)pickerView:(UIPickerView *)pickerView rowHeightForComponent:(NSInteger)component{

 return 60;

}



#pragma mark - UIPickerViewDelegate

#pragma mark 填充文字

- (NSString *)pickerView:(UIPickerView *)pickerView

 titleForRow:(NSInteger)row

 forComponent:(NSInteger)component{

 if (1 == component) {

 return self.heights[row];

 }else if(4 == component){

 return self.weights[row];

 }else{

 return @"";

 }

}



#pragma mark pickerView被选中

- (void)pickerView:(UIPickerView *)pickerView

 didSelectRow:(NSInteger)row

 inComponent:(NSInteger)component{

 if (1 == component) {

 self.selectHeight = self.heights[row];

 }else if(4 == component){

 self.selectWeight = self.weights[row];

 }

 if ([self.delegate respondsToSelector:@selector(weightAndHeightPickerView:didChooseWeightAndHeight:)]) {

 [self.delegate weightAndHeightPickerView:self didChooseWeightAndHeight:@{@"height":self.selectHeight,@"weight":self.selectWeight}];

 }

}



- (UIView *)pickerView:(UIPickerView *)pickerView

 viewForRow:(NSInteger)row forComponent:(NSInteger)component

 reusingView:(UIView *)view{

 UILabel* pickerLabel = [[UILabel alloc] init];

 pickerLabel.textColor = [UIColor colorWithRed:51.0/255

 green:51.0/255

 blue:51.0/255

 alpha:1.0];

 pickerLabel.frame = CGRectMake(0, 0, self.frame.size.width/2, 40);

 pickerLabel.backgroundColor = [UIColor redColor];

 [pickerLabel setTextAlignment:NSTextAlignmentCenter];

 [pickerLabel setBackgroundColor:[UIColor clearColor]];

 if (component == 0) {

 pickerLabel.font = [UIFont systemFontOfSize:14];

 pickerLabel.text = @"身高";

 return pickerLabel;

 }else if(1 == component){

 pickerLabel.font = [UIFont systemFontOfSize:20];

 pickerLabel.text = [NSString stringWithFormat:@"%@",[self pickerView:pickerView

 titleForRow:row forComponent:component]];

 return pickerLabel;

 }else if (2 == component){

 pickerLabel.font = [UIFont systemFontOfSize:14];

 pickerLabel.text = @"cm";

 return pickerLabel;

 }else if (component == 3){

 pickerLabel.font = [UIFont systemFontOfSize:14];

 pickerLabel.text = @"体重";

 return pickerLabel;

 }else if(4 == component){

 pickerLabel.font = [UIFont systemFontOfSize:20];

 pickerLabel.text = [NSString stringWithFormat:@"%@",[self pickerView:pickerView

 titleForRow:row

 forComponent:component]];;

 return pickerLabel;

 }else{

 pickerLabel.font = [UIFont systemFontOfSize:14];

 pickerLabel.text = @"kg";

 return pickerLabel;

 }

}



#pragma mark - lazy load

-(NSMutableArray *)heights{

 if (!_heights) {

 _heights = [NSMutableArray array];

 }

 return _heights;

}



-(NSMutableArray *)weights{

 if (!_weights) {

 _weights = [NSMutableArray array];

 }

 return _weights;

}



- (UIPickerView *)weightAndHeightPickerView{

 if (!_weightAndHeightPickerView) {

 _weightAndHeightPickerView = [[UIPickerView alloc]initWithFrame:

 CGRectMake(0, 0, BoundWidth, 210)];

 _weightAndHeightPickerView.backgroundColor = [UIColor whiteColor];

 _weightAndHeightPickerView.delegate = self;

 _weightAndHeightPickerView.dataSource = self;

 }

 return _weightAndHeightPickerView;

}



@end


```