###App引导页制作
```

#import <UIKit/UIKit.h>
@interface GuideViewComtroller : UIViewController
@end


#import "GuideViewComtroller.h"
#import "HLLoginVC.h"
#import "ProjectUtil.h"

@interface GuideViewComtroller ()
@property (nonatomic, strong) UIImageView *rootImageView;
@property (nonatomic, strong) UIScrollView *scrollView;
@end
@implementation GuideViewComtroller

- (void)viewDidLoad {
 [super viewDidLoad];
 self.view.backgroundColor = [UIColor whiteColor];
 [[UIApplication sharedApplication].keyWindow addSubview:self.scrollView];
}

-(void)viewDidAppear:(BOOL)animated{
 [super viewDidAppear:animated];
 [ProjectUtil installed];
}

-(void)viewWillAppear:(BOOL)animated{
 [super viewWillAppear:animated];
 [self.navigationController setNavigationBarHidden:YES animated:YES];
 [[UIApplication sharedApplication] setStatusBarHidden:YES withAnimation:UIStatusBarAnimationFade];
}

-(void)viewWillDisappear:(BOOL)animated{
 [super viewWillDisappear:animated];
 [[UIApplication sharedApplication] setStatusBarHidden:NO withAnimation:UIStatusBarAnimationFade];
}

#pragma mark - private method
-(void)enterLoginView{
 [self.scrollView removeFromSuperview];
 UIStoryboard *sb = [UIStoryboard storyboardWithName:@"User" bundle:nil];
 HLLoginVC *vc = [sb instantiateViewControllerWithIdentifier:@"HLLoginVC"];
 [self.navigationController pushViewController:vc animated:YES];
}

#pragma mark - lazy load
-(UIImageView *)rootImageView{
 if (!_rootImageView) {
 UIImageView *rootImageView = [[UIImageView alloc] initWithFrame:self.view.bounds];
 rootImageView.image = [UIImage imageNamed:@"Welcome_first"];
 _rootImageView = rootImageView;
 }
 return _rootImageView;
}

-(UIScrollView *)scrollView{
 if (!_scrollView) {
 UIScrollView *scrollView = [[UIScrollView alloc] initWithFrame:CGRectMake(0, 0, BoundWidth, self.navigationController.view.bounds.size.height)];
 scrollView.backgroundColor = [UIColor clearColor];
 scrollView.contentSize = CGSizeMake(self.navigationController.view.bounds.size.width *3, 0);

 CGFloat width = self.navigationController.view.bounds.size.width
 CGFloat height = self.navigationController.view.bounds.size.height;
 for (int i = 0; i<3; i++) {
 UIImageView *imageView = [[UIImageView alloc] initWithFrame:CGRectMake(i*width, 0, width, height)];
 imageView.image = [UIImage imageNamed:[NSString stringWithFormat:@"Welcome_%zd",i+1]];
 [scrollView addSubview:imageView];

 UITapGestureRecognizer *signalTap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(enterLoginView)];
 signalTap.cancelsTouchesInView = YES;

 UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
 button.backgroundColor = [UIColor clearColor];
 [button setTitle:@"立即进入" forState:UIControlStateNormal];
 [button setTitleColor:[UIColor whiteColor] forState:UIControlStateNormal];
 button.layer.cornerRadius = 5.0f;
 button.layer.borderWidth = 1;
 button.layer.borderColor = [UIColor whiteColor].CGColor;
 button.frame = CGRectMake(110, BoundHeight-130, BoundWidth-220, 40);
 [button addTarget:self action:@selector(enterLoginView) forControlEvents:UIControlEventTouchUpInside];

 if (i == 2) {
 imageView.userInteractionEnabled = YES;
 [imageView addGestureRecognizer:signalTap];
 [imageView addSubview:button];
 }
 }
 [scrollView setPagingEnabled:YES]; //设置分页，否则滚动效果很差
 [scrollView setBounces:NO]; //去掉弹性
 [scrollView setShowsVerticalScrollIndicator:NO];
 [scrollView setShowsHorizontalScrollIndicator:NO];
 _scrollView = scrollView;
 }
 return _scrollView;
}

@end

```