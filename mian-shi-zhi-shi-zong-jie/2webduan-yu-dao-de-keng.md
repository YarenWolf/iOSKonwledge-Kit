# Web端开发遇到的坑

1、用 MUI 开发H5页面的时候用到了 MUI 提供的1个picker **mui.listpicker.js、mui.picker.all.js **其中有一个很神奇的bug。当一个页面有 input 文本框和 picker 的时候，你用过 picker 之后再在 input 中输入文本的时候会在弹出键盘的时候底部再弹出 picker 影响操作

解决办法：

```
var picker = new mui.PopPicker({
    layer: 1
});
picker.setData(datasource);
picker.show(function(selectItems) {
    var data = selectItems[0];
    $this.val(data.text);
    //销毁控件
    picker.dispose();
});
```



