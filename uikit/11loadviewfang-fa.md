# loadView

作用：加载控制器的view

何时调用：当控制器的view第一次使用的时候就会调用

使用场景：只要想自定义控制器的view就调用此方法

访问控制器的View就相当于调用控制器中的view get方法

```
-(UIView *)view{
if(_view == nil){
    [self loadView];
    [self viewDidload];
    }
    return _view;
}
```

# 控制器加载view的流程![](/assets/2017-7-16-01.png)

* 控制器的init方法底层会调用initWithNibName方法

MyViewController \*vc = \[\[MyViewController alloc\] init\];

注意点：

* 系统做判断的前提提条件：没有指定nibName；没有自定义loadView方法；控制器以...Controller命名
* 判断原则：
  * 1、判断下有没有指定nibName，如果指定了就去加载nib
  * 2、判断有没有跟控制器同名的xib，但是xib的名称不带Controller的xib，如果有就去加载
  * 3、如果第二步没有指定，就判断有没有跟控制器类名同名的xib，如果有就去加载
  * 4、如果没有任何xib描述控制器的view，就不加载xib

MyViewController加载view的处理

* 判断有没有指定xibName，如果有就去加载指定的xib
* 判断有没有跟控制器类名同名的xib，但是名字不带controller
* 判断有没有跟控制器类名同名的xib，有就去加载
* 直接创建一个空的xib

例子

```
//在Appdelegate中
ViewController *vc = [[ViewController alloc] init];
vc.view.backgroundColkor = [UIColor redColor];
self.window.rootViewController = vc;
[pself.window makeKeyAndVisable];


//ViewController

-(UIView *)view{
    if(!_view){
        [self loadView];
        [self viewDidLoad];
    }

}

-(void)loadView{
    UIView*view = [[UIView alloc] initWithFrame:[UIScreen mainScreen].bounds];
    view.backgroundColor = [UIColor greenColor];
    self.view = view;
}

-(void)viewDidload{
    [super viewDidload];
    self.view.backgroundColor = [UIColor brownColor];
}
```

请问此时界面颜色是什么？

可能很多人会回到绿色。其实答案是 红色

why？在AppDelegate中vc.view.backgroundColor就是调用vc的view的getter方法，在getter方法内部判断\_view是否存在，不存在则新建一个UIView，新建view是通过\[self loadView\]方法创建，创建成功直接调用viewdidload方法；存在则直接返回，所以界面先是绿色，再是棕色最后是红色



