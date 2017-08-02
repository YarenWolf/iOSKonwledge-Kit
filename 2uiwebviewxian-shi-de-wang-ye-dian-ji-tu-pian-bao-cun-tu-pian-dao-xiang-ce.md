# UIWebView上的内容长按保存到相册

> 不知道各位对于这个需求要如何解决？
>
> 可能有些人会想到js与原生交互，js监听图片点击事件，然后将图片的url传递给原生App端，然后原生App将图片保存到相册，这样子麻烦吗？超麻烦。（1）、js监听图片长按事件；（2）、js将图片url传递给原生；（3）、原生通过图片的url生成UIImage；（4）、保存UIImage到系统相册，巨麻烦啊，大哥，我很懒的好不好

#### 那么问题跑出来了，怎么办最简单？

* 鉴于个人道行尚浅，我就将自己的想法说出来

* 有个js的api：`Document.elementFromPoint()`

> The`elementFromPoint()`method of the[`Document`](https://developer.mozilla.org/en-US/docs/Web/API/Document)interface returns the topmost element at the specified coordinates.

所以根据这个提示，我们完全可以只在App原生端做一些代码开发，实现这个需求

#### 开发步骤

* 给UIWebView添加长按手势
* 监听手势动作，拿到坐标点\(x,y\)
* 弹出对话框，是否保存到相册
* UIWebView注入js:Document.elementFromPoint\(x,y\).src拿到img标签的src
* 拿到图片的url，生成UIImage
* 图片保存到相册

#### 有巨坑

* 长按手势事件不能每次都响应，据我猜测UIWebView本身就有很多事件，所以实现下UIGestureRecognizerDelegate代理方法。长按手势准确率100%

```
- (BOOL)gestureRecognizer:(UIGestureRecognizer *)gestureRecognizer shouldRecognizeSimultaneouslyWithGestureRecognizer:(UIGestureRecognizer *)otherGestureRecognizer{
    return YES;
}
```

附上关键的js官方文档：[Document.elementFromPoint()](https://developer.mozilla.org/en-US/docs/Web/API/Document/elementFromPoint)

如果需要下载Demo：[Demo](https://github.com/FantasticLBP/SaveImageToAlbumsFromUIWebView)

