# hittest和pointinside方法

### hittest方法

* 就是用来寻找最合适的view
* 当一个事件传递给一个控件，就会调用这个控件的hitTest方法
* 点击了白色的view： 触摸事件 -&gt; UIApplication -&gt; UIWindow 调用 \[UIWindow hitTest\] -&gt; 白色view \[WhteView hitTest\]



