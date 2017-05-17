\#\#\# UILabel-富文本



1  NSFontAttributeName\\(字体\\)  

 该属性所对应的值是一个 UIFont 对象。该属性用于改变一段文本的字体。如果不指定该属性，则默认为12-point Helvetica\\(Neue\\)。



2  NSParagraphStyleAttributeName\\(段落\\)  

 该属性所对应的值是一个 NSParagraphStyle 对象。该属性在一段文本上应用多个属性。如果不指定该属性，  

 则默认为 NSParagraphStyle 的defaultParagraphStyle 方法返回的默认段落属性。



3  NSForegroundColorAttributeName\\(字体颜色\\)  

 该属性所对应的值是一个 UIColor 对象。该属性用于指定一段文本的字体颜色。如果不指定该属性，则默认为黑色。



4  NSBackgroundColorAttributeName\\(字体背景色\\)  

 该属性所对应的值是一个 UIColor 对象。该属性用于指定一段文本的背景颜色。如果不指定该属性，则默认无背景色。



5  NSLigatureAttributeName\\(连字符\\)  

 该属性所对应的值是一个 NSNumber 对象\\(整数\\)。连体字符是指某些连在一起的字符，  

 它们采用单个的图元符号。0 表示没有连体字符。1 表示使用默认的连体字符。2表示使用所有连体符号。默认值为 1（注意，iOS 不支持值为 2）。



6  NSKernAttributeName\\(字间距\\)  

 该属性所对应的值是一个 NSNumber 对象\\(整数\\)。字母紧排指定了用于调整字距的像素点数。字母紧排的效果依赖于字体。值为 0 表示不使用字母紧排。默认值为0。



7  NSStrikethroughStyleAttributeName\\(删除线\\)  

 该属性所对应的值是一个 NSNumber 对象\\(整数\\)。该值指定是否在文字上加上删除线，该值参考“Underline Style Attributes”。默认值是NSUnderlineStyleNone。



8  NSUnderlineStyleAttributeName\\(下划线\\)  

 该属性所对应的值是一个 NSNumber 对象\\(整数\\)。该值指定是否在文字上加上下划线，该值参考“Underline Style Attributes”。默认值是NSUnderlineStyleNone。



9  NSStrokeColorAttributeName\\(边线颜色\\)  

 该属性所对应的值是一个 UIColor 对象。如果该属性不指定（默认），则等同于 NSForegroundColorAttributeName。  

 否则，指定为删除线或下划线颜色。



10  NSStrokeWidthAttributeName\\(边线宽度\\)  

 该属性所对应的值是一个 NSNumber 对象\\(小数\\)。该值改变描边宽度（相对于字体size 的百分比）。默认为 0，即不改变。正数只改变描边宽度。  

 负数同时改变文字的描边和填充宽度。例如，对于常见的空心字，这个值通常为3.0。



11  NSShadowAttributeName\\(阴影\\)  

 该属性所对应的值是一个 NSShadow 对象。默认为 nil。



12  NSVerticalGlyphFormAttributeName\\(横竖排版\\)  

 该属性所对应的值是一个 NSNumber 对象\\(整数\\)。0 表示横排文本。1 表示竖排文本。在 iOS 中，总是使用横排文本，0 以外的值都未定义。



直接上代码



\`\`\`

- \(void\)viewDidLoad {

    \[super viewDidLoad\];

    \[self.view addSubview:self.label\];

}





\#pragma mark - lazy load

-\(UILabel \*\)label{

    if \(!\_label\) {

        \_label = \[\[UILabel alloc\] initWithFrame:CGRectMake\(0, 100,\[UIScreen mainScreen\].bounds.size.width, \[UIScreen mainScreen\].bounds.size.height - 100\)\];

        \_label.backgroundColor = \[UIColor grayColor\];

        NSMutableAttributedString \*attrbuitedStr = \[\[NSMutableAttributedString alloc\] initWithString:@"人生若只如初见，何事秋风悲画扇。\n等闲变却故人心，却道故人心易变。\n骊山语罢清宵半，泪雨霖铃终不怨。\n何如薄幸锦衣郎，比翼连枝当日愿。"\];



        //前7个字为红色

        \[attrbuitedStr addAttribute:NSForegroundColorAttributeName value:\[UIColor redColor\] range:NSMakeRange\(0, 7\)\];

        //前7个字加下划线

        \[attrbuitedStr addAttribute:NSUnderlineStyleAttributeName value:\[NSNumber numberWithInt:NSUnderlineStyleSingle\] range:NSMakeRange\(0, 7\)\];

        //前7个字字体大小为30px

        \[attrbuitedStr addAttribute:NSFontAttributeName value:\[UIFont systemFontOfSize:30\] range:NSMakeRange\(0, 7\)\];





        //设置第二行前半句文本背景颜色为黄色

        \[attrbuitedStr addAttribute:NSBackgroundColorAttributeName value:\[UIColor yellowColor\] range:NSMakeRange\(16, 8\)\];

        //设置第二行前半句文本间距为10

        \[attrbuitedStr addAttribute:NSKernAttributeName value:@10  range:NSMakeRange\(16, 8\)\];

        //设置边框颜色

        \[attrbuitedStr addAttribute:NSStrokeColorAttributeName value:\[UIColor greenColor\] range:NSMakeRange\(16, 8\)\];

        //设置边框宽度

        \[attrbuitedStr addAttribute:NSStrokeWidthAttributeName value:\[NSNumber numberWithInt:1\] range:NSMakeRange\(16, 8\)\];



        \_label.attributedText = attrbuitedStr;

        \_label.textAlignment = NSTextAlignmentCenter;

        \_label.numberOfLines = 0;

        \[\_label sizeToFit\];

    }

    return \_label;

}

\`\`\`









