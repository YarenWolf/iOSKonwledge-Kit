### 监听UIWebView的滚动

想用kvo来监听webview滚动到了哪个位置，发现webview是遵循UISCrollViewDelegate代理，且有一个  
scrollView属性的。代码如下：（self.navBar是自定义的导航栏View）

```
[web.scrollView addObserver:self forKeyPath:@"contentOffset" options:NSKeyValueObservingOptionNew context:nil];
```

```
-(void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<<span style="font-variant-ligatures: no-common-ligatures; color: #703daa">NSString *,id> *)change context:(void *)context
{
    if ([keyPath isEqualToString:@"contentOffset"])
    {
        CGFloat y = _webView.scrollView.contentOffset.y;
        if (y>=0 && y<=64) {
            CGFloat nav_alpha = y/64;
            NSLog(@"透明度%f",nav_alpha);
            self.navBar.alpha = nav_alpha;
        }else if(y>64){
            self.navBar.alpha = 1.0;
        }else{
            self.navBar.alpha = 0.0;
        }
    }
}
```



